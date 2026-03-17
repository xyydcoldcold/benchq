# Architecture

BenchQ's internal architecture follows a linear pipeline of three stages, each implemented as an independent subpackage group.

---

## Pipeline overview

```
┌──────────────────────┐
│   Problem Ingestion  │  benchq.problem_ingestion
│  Hamiltonian source  │  → molecular, solid-state, plasma, file
└──────────┬───────────┘
           │ openfermion.InteractionOperator / PauliSum / PauliRepresentation
           ▼
┌──────────────────────┐
│  Problem Embedding   │  benchq.problem_embeddings + benchq.algorithms
│  Algorithm wrapping  │  → QuantumProgram + ErrorBudget
└──────────┬───────────┘
           │ AlgorithmImplementation
           ▼
┌──────────────────────────────────────────────────────────┐
│                  Resource Estimation                      │
│  Path A: Graph-state  │  Path B: Footprint / OpenFermion │
│  benchq.compilation   │  benchq.resource_estimators       │
│  benchq.resource_     │  openfermion_estimator()          │
│  estimators           │                                   │
└──────────┬────────────┴───────────────────────────────────┘
           │ ResourceInfo[GraphExtra] or ResourceInfo[OpenFermionExtra]
           ▼
┌──────────────────────┐
│   Output / Logging   │  benchq.mlflow, benchq.visualization_tools
└──────────────────────┘
```

---

## Stage 1 — Problem Ingestion

Sources:
- **Molecular**: `MolecularHamiltonianGenerator` (PySCF) → `openfermion.InteractionOperator`
- **Solid-state**: `generate_*_hamiltonian` functions → `PauliRepresentation`
- **Plasma**: `get_vlasov_hamiltonian` → `PauliRepresentation`
- **File**: `get_hamiltonian_from_file` → `PauliSum | None`

---

## Stage 2 — Algorithm wrapping

Algorithm constructors each accept a Hamiltonian and produce `AlgorithmImplementation`:

| Constructor | Embedding type |
|---|---|
| `qsp_time_evolution_algorithm` | QSP |
| `trotter_time_evolution_algorithm` | Trotter |
| `qpe_gsee_algorithm` | QPE |
| `get_qaoa_optimization_algorithm` | QAOA |
| `AlgorithmImplementation.from_circuit` | Arbitrary circuit |

`AlgorithmImplementation` holds `QuantumProgram + ErrorBudget + n_shots`.

`QuantumProgram` models structured repetition: a set of subroutines + a sequence
function mapping step indices to subroutine indices.

---

## Stage 3A — Graph-state compilation path

```
AlgorithmImplementation
    → transpile_to_clifford_t()
    → compile_to_native_gates()
    → circuit_compiler (Ruby Slippers or Jabalizer)
    → CompiledAlgorithmImplementation (GSCInfo per subroutine)
    → GraphBasedLogicalArchitectureModel.generate_minimal_code_distance_resources()
    → find_optimal_factory()
    → ResourceInfo[GraphExtra]
```

**Compiler backends:**

| Backend | Language | Strength | Extra |
|---|---|---|---|
| Ruby Slippers | Julia | Large circuits, fast | core |
| Jabalizer | Rust | Higher-quality ASGs | `[jabalizer]` |

---

## Stage 3B — Footprint / OpenFermion path

```
(logical qubit count, Toffoli count, T count)
    → openfermion_estimator()
    → find_optimal_factory()
    → ResourceInfo[OpenFermionExtra]
```

No circuit compilation required.

---

## Cross-cutting concerns

| Concern | Module |
|---|---|
| Circuit conversion (Qiskit/Cirq ↔ Orquestra) | `benchq.conversions` |
| Operator conversion (openfermion ↔ pyLIQTR) | `benchq.conversions` |
| Timing measurement | `benchq.timing` |
| MLflow experiment tracking | `benchq.mlflow` |
| Resource allocation visualization | `benchq.visualization_tools` |
| Magic state factory selection | `benchq.magic_state_distillation` |
| Decoder modeling | `benchq.decoder_modeling` |

---

## Key data flow types

| Type | Origin | Consumed by |
|---|---|---|
| `openfermion.InteractionOperator` | PySCF ingestion | Algorithm constructors |
| `PauliSum` / `PauliRepresentation` | Built-in ingestion | Algorithm constructors |
| `AlgorithmImplementation` | Algorithm constructors | `GraphResourceEstimator`, `openfermion_estimator` |
| `QuantumProgram` | Algorithm constructors | Compilation |
| `CompiledAlgorithmImplementation` | Compiler backends | `GraphResourceEstimator` |
| `ResourceInfo[TExtra]` | Estimators | MLflow logging, visualization |
