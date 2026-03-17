# Terminology

Definitions for terms used throughout BenchQ, grounded in source code and README.

## AlgorithmImplementation

Dataclass (`benchq.algorithms.data_structures`). Combines:
- `program: QuantumProgram`
- `error_budget: ErrorBudget`
- `n_shots: int`

The top-level input to resource estimation.

## QuantumProgram

Dataclass (`benchq.problem_embeddings`). Represents a structured quantum computation with:
- `subroutines: Sequence[Circuit]` — distinct circuit fragments
- `steps: int`
- `calculate_subroutine_sequence: Callable[[int], Sequence[int]]` — maps step index to subroutine index sequence
- Properties: `multiplicities`, `subroutine_sequence`, `full_circuit`, gate counts (`n_t_gates`, `n_rotation_gates`, `n_c_gates`)

## ErrorBudget

Dataclass. Partitions total allowed failure probability:
- `algorithm_failure_tolerance`
- `transpilation_failure_tolerance`
- `hardware_failure_tolerance`
- Property `total_failure_tolerance`

Factory methods: `from_even_split(total)`, `from_weights(total, algo_w, transp_w, hw_w)`.

## Graph State Compilation (GSC)

Compilation path that converts a `QuantumProgram` into a graph state representation (`GSCInfo` / `CompiledQuantumProgram`) using either the Ruby Slippers or Jabalizer backend. Enables full physical resource estimation via `GraphResourceEstimator`.

## GSCInfo

Dataclass. Output of graph state compilation for one subroutine:
- `num_logical_qubits`
- `num_layers`
- `graph_creation_tocks_per_layer: List[int]`
- `t_states_per_layer: List[int]`
- `rotations_per_layer: List[int]`

## Ruby Slippers

Default graph state compiler. Python interface backed by a Julia runtime. Fast on large circuits; lower-quality ASGs than Jabalizer. Configurable via Ruby Slippers hyperparameters and Pauli tracking parameters.

## Jabalizer

Alternative compiler. Rust-based (via `[jabalizer]` extra). Slower; produces higher-quality adaptive syndrome graphs.

## Adaptive Syndrome Graph (ASG)

The graph representation produced by compilation. Quality affects resource efficiency.

## Logical Architecture Model

Specifies how logical qubits are connected and routed. BenchQ provides:
- `AllToAllArchitectureModel` — all-to-all connectivity
- `TwoRowBusArchitectureModel` — two-row bus topology

## Hardware Model

Specifies physical qubit parameters. Two tiers:
- **Basic** (`SCModel`, `IONTrapModel`): parameterized by `physical_qubit_error_rate` and `surface_code_cycle_time_in_seconds`
- **Detailed** (`DetailedIonTrapModel`): full ELU-level breakdown

## Magic State Distillation

Process of producing high-fidelity T states from noisy physical qubits. BenchQ models factories from Litinski's catalog; `find_optimal_factory` selects the best factory given a per-T-gate failure tolerance and optimization target.

## Decoder Model

Models a quantum error correction decoder. `DecoderModel` (loaded from CSV) provides power, area, and delay as a function of code distance.

## QEC Cycle Allocation

`QECCycleAllocation` — breakdown of surface code QEC cycles across operations. Returned as part of resource estimation output.

## Optimization modes

`"Space"` — minimize physical qubit count.
`"Time"` — minimize total wall-clock execution time.

## Supported operator types

`SUPPORTED_OPERATORS` — union of `PauliSum`, `PauliTerm`, `QubitOperator`, `IsingOperator`, `Hamiltonian`, `InteractionOperator` (from orquestra-quantum and openfermion).

## Supported circuit types

`SUPPORTED_CIRCUITS = QiskitCircuit | CirqCircuit | OrquestraCircuit`
