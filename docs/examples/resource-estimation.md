# Resource Estimation Examples

Annotated examples for all confirmed resource estimation paths in BenchQ.

---

## A. Evidence summary

- `examples/ex_2_time_evolution.py` — Hamiltonian → QSP → Ruby Slippers → both architectures
- `examples/ex_4_fast_graph_estimates.py` — fast graph estimate with `DetailedIonTrapModel`
- `examples/ex_6_mlflow.py` — tolerance sweep with MLflow logging
- `examples/ex_10_utility_scale.py` — multi-lattice scan, both estimation paths, decoder model

---

## B. Full graph-state estimation with both architectures

From `examples/ex_2_time_evolution.py`:

```python
from benchq.problem_ingestion.solid_state_hamiltonians import generate_1d_heisenberg_hamiltonian
from benchq.algorithms import qsp_time_evolution_algorithm
from benchq.compilation.graph_states import get_ruby_slippers_circuit_compiler, get_implementation_compiler
from benchq.logical_architecture_modeling import AllToAllArchitectureModel, TwoRowBusArchitectureModel
from benchq.quantum_hardware_modeling import BASIC_SC_ARCHITECTURE_MODEL
from benchq.resource_estimators import GraphResourceEstimator

hamiltonian = generate_1d_heisenberg_hamiltonian(N=4)
algo = qsp_time_evolution_algorithm(hamiltonian, time=1.0, failure_tolerance=1e-3)

circuit_compiler = get_ruby_slippers_circuit_compiler()
compiler = get_implementation_compiler(circuit_compiler, ...)
estimator = GraphResourceEstimator(optimization="Space")

for arch in [AllToAllArchitectureModel(), TwoRowBusArchitectureModel()]:
    result = estimator.compile_and_estimate(algo, compiler, arch, BASIC_SC_ARCHITECTURE_MODEL)
    # result.extra contains QEC cycle allocation breakdown
```

---

## C. Fast estimate with detailed ion trap model

From `examples/ex_4_fast_graph_estimates.py`:

```python
from benchq.problem_ingestion.solid_state_hamiltonians import generate_ising_hamiltonian_on_triangular_lattice
from benchq.algorithms import qsp_time_evolution_algorithm
from benchq.logical_architecture_modeling import TwoRowBusArchitectureModel
from benchq.quantum_hardware_modeling import DETAILED_ION_TRAP_ARCHITECTURE_MODEL
from benchq.resource_estimators import get_fast_graph_estimate

hamiltonian = generate_ising_hamiltonian_on_triangular_lattice(3)
algo = qsp_time_evolution_algorithm(hamiltonian, time=1.0, failure_tolerance=1e-3)

result = get_fast_graph_estimate(
    algo,
    TwoRowBusArchitectureModel(),
    DETAILED_ION_TRAP_ARCHITECTURE_MODEL,
    optimization="Time",
)
```

---

## D. Tolerance sweep with MLflow logging

From `examples/ex_6_mlflow.py`:

```python
from benchq.algorithms.data_structures import AlgorithmImplementation, ErrorBudget
from benchq.mlflow import log_input_objects_to_mlflow, log_resource_info_to_mlflow
from benchq.resource_estimators import GraphResourceEstimator

for tolerance in [1e0, 1e-1, 1e-2, 1e-3, 1e-4]:
    error_budget = ErrorBudget.from_even_split(tolerance)
    algo = AlgorithmImplementation.from_circuit(circuit, error_budget)
    result = GraphResourceEstimator(optimization="Time").compile_and_estimate(
        algo, compiler, logical_arch, hw_model
    )
    log_input_objects_to_mlflow(algo, "my_algorithm", hw_model)
    log_resource_info_to_mlflow(result)
```

---

## E. Utility-scale: two estimation paths + decoder model

From `examples/ex_10_utility_scale.py`:

```python
from benchq.decoder_modeling import DecoderModel
from benchq.resource_estimators import GraphResourceEstimator, openfermion_estimator

decoder = DecoderModel.from_csv("sample_decoder_data.csv")

circuit_compiler = get_ruby_slippers_circuit_compiler(
    teleportation_threshold=80,
    optimal_dag_density=10,
)

# Graph-state path
result_graph = GraphResourceEstimator(optimization="Time").compile_and_estimate(
    algo, compiler, logical_arch, hw_model, decoder_model=decoder
)

# OpenFermion footprint path
result_oe = openfermion_estimator(
    num_logical_qubits=...,
    num_toffoli=...,
    architecture_model=hw_model,
    decoder_model=decoder,
)

# Outputs two JSON files per lattice type via save_to_file()
```

---

## F. OpenFermion estimator (no compilation)

```python
from benchq.resource_estimators import openfermion_estimator
from benchq.quantum_hardware_modeling import BASIC_SC_ARCHITECTURE_MODEL

result = openfermion_estimator(
    num_logical_qubits=50,
    num_toffoli=10000,
    num_t=0,
    architecture_model=BASIC_SC_ARCHITECTURE_MODEL,
    routing_overhead_proportion=0.5,
    hardware_failure_tolerance=1e-3,
    factory_count=4,
)
# Returns: OpenFermionResourceInfo = ResourceInfo[OpenFermionExtra]
```

---

## G. Timing measurement

```python
from benchq.timing import measure_time

with measure_time() as t:
    result = estimator.compile_and_estimate(...)

print(f"Estimation took {t.total:.2f}s")
```
