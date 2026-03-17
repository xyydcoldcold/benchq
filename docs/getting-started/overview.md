# Overview

BenchQ estimates physical quantum computing resources for fault-tolerant algorithm execution.

## Conceptual pipeline

```
Problem Ingestion → Problem Embedding → Algorithm Implementation → Resource Estimation
```

### Stage 1 — Problem Ingestion (`benchq.problem_ingestion`)

Load or generate a Hamiltonian operator. Sources:

- **Molecular**: via PySCF (`[pyscf]` extra); `MolecularHamiltonianGenerator` produces `openfermion.InteractionOperator`
- **Solid-state**: Fermi-Hubbard, Heisenberg, Ising lattice models (built-in, no extra required)
- **Plasma**: Vlasov Hamiltonian (`get_vlasov_hamiltonian`)
- **File**: JSON or HDF5 via `get_hamiltonian_from_file`

### Stage 2 — Problem Embedding (`benchq.problem_embeddings`, `benchq.algorithms`)

Wrap the Hamiltonian into an `AlgorithmImplementation`, which pairs a `QuantumProgram` with an `ErrorBudget` and shot count.

Algorithm constructors:

| Function | Algorithm |
|---|---|
| `qsp_time_evolution_algorithm` | QSP-based time evolution |
| `trotter_time_evolution_algorithm` | Trotter-based time evolution |
| `qpe_gsee_algorithm` | QPE ground-state energy estimation |
| `get_qaoa_optimization_algorithm` | QAOA portfolio optimization |
| `AlgorithmImplementation.from_circuit` | Arbitrary circuit |

### Stage 3 — Resource Estimation (`benchq.resource_estimators`)

Two estimation paths:

- **Graph-state path** (`GraphResourceEstimator`): compiles the program to a graph state via Ruby Slippers or Jabalizer, then estimates physical resources against a `LogicalArchitectureModel` and `HardwareModel`.
- **Footprint path** (`openfermion_estimator`): abstract estimate from logical qubit and Toffoli/T-gate counts, without full compilation.

## Hardware models

| Model | Class | Default error rate | Cycle time |
|---|---|---|---|
| Superconducting (basic) | `SCModel` | 1e-3 | 1 µs |
| Ion trap (basic) | `IONTrapModel` | 1e-4 | 1 ms |
| Ion trap (detailed) | `DetailedIonTrapModel` | — | — |

## Logical architecture models

- `AllToAllArchitectureModel` — all-to-all qubit connectivity
- `TwoRowBusArchitectureModel` — two-row bus connectivity

## Compiler backends

- **Ruby Slippers** (default): Python interface to a Julia runtime; fast on large circuits; configurable via hyperparameters and Pauli tracking parameters
- **Jabalizer**: Rust-based; slower but produces higher-quality adaptive syndrome graphs (ASGs); requires `[jabalizer]` extra
