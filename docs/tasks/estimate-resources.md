# Estimate Resources

Two confirmed estimation paths exist in BenchQ: graph-state compilation and footprint/OpenFermion.

---

## Path 1 — Graph-state estimation (`GraphResourceEstimator`)

### Step 1: Build an `AlgorithmImplementation`

Either from a circuit:

```python
from benchq.algorithms.data_structures import AlgorithmImplementation, ErrorBudget

error_budget = ErrorBudget.from_even_split(1e-3)
algo = AlgorithmImplementation.from_circuit(circuit, error_budget, n_shots=1)
```

Or from an algorithm constructor (e.g. QSP time evolution):

```python
from benchq.algorithms import qsp_time_evolution_algorithm

algo = qsp_time_evolution_algorithm(hamiltonian, time=1.0, failure_tolerance=1e-3)
```

### Step 2: Choose a circuit compiler

```python
from benchq.compilation.graph_states import (
    get_ruby_slippers_circuit_compiler,
    get_implementation_compiler,
)

circuit_compiler = get_ruby_slippers_circuit_compiler(
    takes_graph_input=True,
    gives_graph_output=True,
    max_num_qubits=1,
    teleportation_threshold=40,
)
implementation_compiler = get_implementation_compiler(circuit_compiler, ...)
```

### Step 3: Choose hardware and logical architecture

```python
from benchq.quantum_hardware_modeling import BASIC_SC_ARCHITECTURE_MODEL
from benchq.logical_architecture_modeling import AllToAllArchitectureModel

hw_model = BASIC_SC_ARCHITECTURE_MODEL          # SCModel: 1e-3 error rate, 1µs cycle
logical_arch = AllToAllArchitectureModel()
```

### Step 4: Optionally add a decoder model

```python
from benchq.decoder_modeling import DecoderModel

decoder = DecoderModel.from_csv("path/to/sample_decoder_data.csv")
```

### Step 5: Run the estimator

**Full compile + estimate:**

```python
from benchq.resource_estimators import GraphResourceEstimator

estimator = GraphResourceEstimator(optimization="Time")  # or "Space"
result = estimator.compile_and_estimate(
    algo, implementation_compiler, logical_arch, hw_model, decoder_model=decoder
)
```

**Estimate from pre-compiled implementation:**

```python
result = estimator.estimate_resources_from_compiled_implementation(
    compiled_algo, logical_arch, hw_model
)
```

**Fast estimate (skips full compilation):**

```python
from benchq.resource_estimators import get_fast_graph_estimate

result = get_fast_graph_estimate(algo, logical_arch, hw_model, optimization="Time")
```

**Precise estimate:**

```python
from benchq.resource_estimators import get_precise_graph_estimate

result = get_precise_graph_estimate(algo, logical_arch, hw_model, optimization="Space")
```

### Output: `GraphResourceInfo`

`GraphResourceInfo = ResourceInfo[GraphExtra]` — contains:
- Physical qubit count
- Timing
- Failure rates (`LogicalFailureRateInfo`)
- `GraphExtra`: compiled algorithm details

---

## Path 2 — Footprint / OpenFermion estimate (`openfermion_estimator`)

Does not require full graph-state compilation. Accepts abstract logical counts directly.

```python
from benchq.resource_estimators import openfermion_estimator
from benchq.quantum_hardware_modeling import BASIC_SC_ARCHITECTURE_MODEL

result = openfermion_estimator(
    num_logical_qubits=50,
    num_toffoli=1000,
    num_t=0,
    architecture_model=BASIC_SC_ARCHITECTURE_MODEL,
    routing_overhead_proportion=0.5,
    hardware_failure_tolerance=1e-3,
    factory_count=4,
)
```

Returns `OpenFermionResourceInfo = ResourceInfo[OpenFermionExtra]`.

---

## Optimization modes

| Mode | Effect |
|---|---|
| `"Space"` | Minimize physical qubit count |
| `"Time"` | Minimize wall-clock execution time |

---

## MLflow logging

From `examples/ex_6_mlflow.py` — confirmed pattern for iterating over tolerances and logging:

```python
from benchq.mlflow import log_input_objects_to_mlflow, log_resource_info_to_mlflow

log_input_objects_to_mlflow(algo, algorithm_name, hw_model, decoder_model=decoder)
log_resource_info_to_mlflow(result)
```
