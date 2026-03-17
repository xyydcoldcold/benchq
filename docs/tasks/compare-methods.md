# Compare Methods

BenchQ supports multiple algorithm and estimator choices that can be compared on the same problem.

---

## Comparing QSP vs. Trotter time evolution

Both return `AlgorithmImplementation` and feed the same downstream pipeline.

```python
from benchq.algorithms import qsp_time_evolution_algorithm, trotter_time_evolution_algorithm

hamiltonian = generate_1d_heisenberg_hamiltonian(N=4)

qsp_algo   = qsp_time_evolution_algorithm(hamiltonian, time=1.0, failure_tolerance=1e-3)
trot_algo  = trotter_time_evolution_algorithm(hamiltonian, time=1.0, failure_tolerance=1e-3)
```

Both can then be passed to `GraphResourceEstimator.compile_and_estimate()` identically.

| Property | QSP | Trotter |
|---|---|---|
| Algorithm | `qsp_time_evolution_algorithm` | `trotter_time_evolution_algorithm` |
| Input type | `SUPPORTED_OPERATORS` | `PauliRepresentation` |
| Typical gate complexity | Lower T-count | More T gates at large time steps |

> **Note:** Resource comparison outcomes are problem-dependent and not pre-computed in BenchQ.

---

## Comparing graph-state vs. footprint estimation

| Estimator | Function | Requires compilation | Returns |
|---|---|---|---|
| Graph-state (precise) | `get_precise_graph_estimate` | Yes (full) | `GraphResourceInfo` |
| Graph-state (fast) | `get_fast_graph_estimate` | Yes (fast) | `GraphResourceInfo` |
| Footprint / OpenFermion | `openfermion_estimator` | No | `OpenFermionResourceInfo` |

```python
from benchq.resource_estimators import (
    get_precise_graph_estimate,
    get_fast_graph_estimate,
    openfermion_estimator,
)

precise = get_precise_graph_estimate(algo, logical_arch, hw_model, optimization="Space")
fast    = get_fast_graph_estimate(algo, logical_arch, hw_model, optimization="Space")
footprint = openfermion_estimator(num_logical_qubits=50, num_toffoli=1000, ...)
```

---

## Comparing Ruby Slippers vs. Jabalizer compilers

| Compiler | Accessor | Backend | Strength | Weakness |
|---|---|---|---|---|
| Ruby Slippers | `get_ruby_slippers_circuit_compiler` | Julia | Fast on large circuits | Lower ASG quality |
| Jabalizer | `get_jabalizer_circuit_compiler` | Rust | Higher-quality ASGs | Slower; requires `[jabalizer]` extra |

```python
from benchq.compilation.graph_states import (
    get_ruby_slippers_circuit_compiler,
    get_jabalizer_circuit_compiler,
)

rs_compiler  = get_ruby_slippers_circuit_compiler()
jab_compiler = get_jabalizer_circuit_compiler(space_optimal_timeout=60)
```

---

## Comparing hardware models

```python
from benchq.quantum_hardware_modeling import (
    BASIC_SC_ARCHITECTURE_MODEL,
    BASIC_ION_TRAP_ARCHITECTURE_MODEL,
    DETAILED_ION_TRAP_ARCHITECTURE_MODEL,
)

for hw_model in [BASIC_SC_ARCHITECTURE_MODEL, BASIC_ION_TRAP_ARCHITECTURE_MODEL]:
    result = get_fast_graph_estimate(algo, logical_arch, hw_model, optimization="Time")
    print(hw_model, result)
```

---

## Comparing logical architectures

```python
from benchq.logical_architecture_modeling import (
    AllToAllArchitectureModel,
    TwoRowBusArchitectureModel,
)

for arch in [AllToAllArchitectureModel(), TwoRowBusArchitectureModel()]:
    result = estimator.compile_and_estimate(algo, compiler, arch, hw_model)
```

From `examples/ex_2_time_evolution.py`: both architectures are iterated in the same example script.

---

## Comparing "Space" vs. "Time" optimization

```python
for opt in ["Space", "Time"]:
    result = GraphResourceEstimator(optimization=opt).compile_and_estimate(...)
```
