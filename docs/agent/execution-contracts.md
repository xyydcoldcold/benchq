# Execution Contracts

Confirmed input/output contracts for BenchQ's key functions, suitable for agent-facing dispatch.

---

## `AlgorithmImplementation.from_circuit`

```
Input:
  circuit: orquestra Circuit (QiskitCircuit | CirqCircuit | OrquestraCircuit via import_circuit)
  error_budget: ErrorBudget
  n_shots: int = 1

Output:
  AlgorithmImplementation
    .program: QuantumProgram
    .error_budget: ErrorBudget
    .n_shots: int
```

---

## `ErrorBudget.from_even_split`

```
Input:
  total_failure_tolerance: float

Output:
  ErrorBudget
    .algorithm_failure_tolerance   = total / 3
    .transpilation_failure_tolerance = total / 3
    .hardware_failure_tolerance    = total / 3
    .total_failure_tolerance       = total
```

*(Equal split is inferred from naming; exact split formula not confirmed in source)*

---

## `GraphResourceEstimator.compile_and_estimate`

```
Input:
  algorithm_implementation: AlgorithmImplementation
  algorithm_implementation_compiler: Callable (from get_implementation_compiler)
  logical_architecture_model: GraphBasedLogicalArchitectureModel
  hw_model: BasicArchitectureModel | DetailedArchitectureModel
  decoder_model: DecoderModel | None = None
  magic_state_factory_iterator: Iterable[MagicStateFactoryInfo] | None = None

Output:
  GraphResourceInfo = ResourceInfo[GraphExtra]
```

---

## `get_fast_graph_estimate` / `get_precise_graph_estimate`

```
Input:
  algorithm_implementation: AlgorithmImplementation
  logical_architecture_model: GraphBasedLogicalArchitectureModel
  hardware_model: BasicArchitectureModel
  optimization: str  # "Space" | "Time"
  decoder_model: DecoderModel | None = None

Output:
  GraphResourceInfo
```

---

## `openfermion_estimator`

```
Input:
  num_logical_qubits: int
  num_toffoli: int = 0
  num_t: int = 0
  architecture_model: BasicArchitectureModel = BASIC_SC_ARCHITECTURE_MODEL
  routing_overhead_proportion: float = 0.5
  hardware_failure_tolerance: float = 1e-3
  factory_count: int = 4
  magic_state_factory_iterator: Iterable[MagicStateFactoryInfo] | None = None
  decoder_model: DecoderModel | None = None

Output:
  OpenFermionResourceInfo = ResourceInfo[OpenFermionExtra]
```

---

## `ResourceInfo` structure

```
ResourceInfo[TExtra]:
  physical_qubit_count: int
  (timing fields)
  failure_rates: LogicalFailureRateInfo
    .rotation_failure_rate: float
    .distillation_failure_rate: float
    .qec_failure_rate: float
    .total_circuit_failure_rate: float   [property]
  extra: TExtra
```

For `GraphExtra`: includes compiled algorithm details and `LogicalArchitectureResourceInfo`.
For `OpenFermionExtra`: includes OpenFermion-specific metrics.

---

## `DecoderModel.from_csv`

```
Input:
  file_path: str  # path to CSV with power, area, delay tables by distance

Output:
  DecoderModel
    .power_in_nanowatts(distance: int) -> float
    .area_in_micrometers_squared(distance: int) -> float
    .delay_in_nanoseconds(distance: int) -> float
    .highest_calculated_distance: int = 31
```

---

## `find_optimal_factory`

```
Input:
  per_t_gate_failure_tolerance: float | Decimal
  magic_state_factory_iterator: Iterable[MagicStateFactoryInfo]
  optimization: str = "Time"  # "Time" | "Space"

Output:
  MagicStateFactoryInfo | None
  # None if no factory meets the tolerance
```

---

## `import_circuit`

```
Input:
  circuit: QiskitCircuit | CirqCircuit | OrquestraCircuit
  original_n_qubits: int | None = None

Output:
  OrquestraCircuit
```

---

## `get_pyliqtr_operator`

```
Input:
  hamiltonian: PauliSum | PauliTerm | QubitOperator | IsingOperator | Hamiltonian | InteractionOperator

Output:
  pyLIQTR Hamiltonian
```

---

## `MolecularHamiltonianGenerator`

```
Methods:
  .get_pyscf_molecule() -> pyscf Mol
  .get_active_space_hamiltonian() -> openfermion.InteractionOperator
  .get_active_space_meanfield_object() -> pyscf scf.hf.SCF

Raises:
  SCFConvergenceError  — if PySCF SCF fails to converge
```

---

## `QuantumProgram`

```
Static factory:
  QuantumProgram.from_circuit(circuit) -> QuantumProgram

Properties:
  .subroutine_sequence: Sequence[int]
  .full_circuit: Circuit
  .n_t_gates: int
  .n_rotation_gates: int
  .n_c_gates: int
  .min_n_nodes: int
  .multiplicities: ...

Methods:
  .transpile_to_clifford_t(transpilation_failure_tolerance) -> QuantumProgram
  .compile_to_native_gates(verbose=False) -> QuantumProgram
  .combine_subroutines() -> QuantumProgram
  .split_into_smaller_subroutines(max_size: int) -> QuantumProgram
```
