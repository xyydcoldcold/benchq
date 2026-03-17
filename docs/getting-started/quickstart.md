# Quickstart

This page shows the shortest confirmed path to a resource estimate, drawn from `examples/ex_1_from_qasm.py`.

## Estimate resources from a QASM circuit

```python
from benchq.algorithms.data_structures import AlgorithmImplementation, ErrorBudget
from benchq.compilation.graph_states import get_implementation_compiler
from benchq.logical_architecture_modeling import AllToAllArchitectureModel
from benchq.quantum_hardware_modeling import BASIC_SC_ARCHITECTURE_MODEL
from benchq.resource_estimators import GraphResourceEstimator

# 1. Define error budget
error_budget = ErrorBudget.from_even_split(1e-3)

# 2. Load a circuit and wrap it
#    AlgorithmImplementation.from_circuit accepts an orquestra Circuit object
algorithm_implementation = AlgorithmImplementation.from_circuit(circuit, error_budget, n_shots=1)

# 3. Choose a logical architecture
logical_architecture = AllToAllArchitectureModel()

# 4. Set up the compiler
compiler = get_implementation_compiler(
    circuit_compiler=...,   # e.g. default_ruby_slippers_circuit_compiler
    ...
)

# 5. Run the estimator
estimator = GraphResourceEstimator(optimization="Time")
resource_info = estimator.compile_and_estimate(
    algorithm_implementation,
    compiler,
    logical_architecture,
    BASIC_SC_ARCHITECTURE_MODEL,
)

print(resource_info)
```

## Estimate resources from a Hamiltonian

From `examples/ex_2_time_evolution.py`:

```python
from benchq.problem_ingestion.solid_state_hamiltonians import generate_1d_heisenberg_hamiltonian
from benchq.algorithms import qsp_time_evolution_algorithm
from benchq.compilation.graph_states import get_ruby_slippers_circuit_compiler, get_implementation_compiler
from benchq.logical_architecture_modeling import TwoRowBusArchitectureModel
from benchq.quantum_hardware_modeling import BASIC_SC_ARCHITECTURE_MODEL
from benchq.resource_estimators import GraphResourceEstimator

hamiltonian = generate_1d_heisenberg_hamiltonian(N=4)
algorithm = qsp_time_evolution_algorithm(hamiltonian, time=1.0, failure_tolerance=1e-3)

circuit_compiler = get_ruby_slippers_circuit_compiler()
compiler = get_implementation_compiler(circuit_compiler, ...)

estimator = GraphResourceEstimator(optimization="Space")
result = estimator.compile_and_estimate(
    algorithm, compiler, TwoRowBusArchitectureModel(), BASIC_SC_ARCHITECTURE_MODEL
)
```

## Fast estimate (no full compilation)

From `examples/ex_4_fast_graph_estimates.py`:

```python
from benchq.resource_estimators import get_fast_graph_estimate

result = get_fast_graph_estimate(
    algorithm_implementation,
    TwoRowBusArchitectureModel(),
    DETAILED_ION_TRAP_ARCHITECTURE_MODEL,
    optimization="Time",
)
```

## Optimization modes

`GraphResourceEstimator(optimization=...)` accepts:

- `"Space"` — minimize physical qubit count
- `"Time"` — minimize wall-clock time
