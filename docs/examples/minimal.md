# Minimal Example

Based on `examples/ex_1_from_qasm.py` — the simplest confirmed end-to-end path.

---

## A. Evidence summary

- **File:** `examples/ex_1_from_qasm.py`
- **Circuit files:** `examples/data/ghz_circuit.qasm`, `example_circuit.qasm`
- **Key symbols used:** `AlgorithmImplementation.from_circuit`, `ErrorBudget.from_even_split`, `AllToAllArchitectureModel`, `BASIC_SC_ARCHITECTURE_MODEL`, `GraphResourceEstimator(optimization="Time")`
- **Compiler:** `get_implementation_compiler` with a circuit compiler

---

## B. Annotated example

```python
from benchq.algorithms.data_structures import AlgorithmImplementation, ErrorBudget
from benchq.logical_architecture_modeling import AllToAllArchitectureModel
from benchq.quantum_hardware_modeling import BASIC_SC_ARCHITECTURE_MODEL
from benchq.resource_estimators import GraphResourceEstimator
from benchq.compilation.graph_states import get_implementation_compiler

# Step 1 — Define the total error budget and split it evenly
#   across algorithm, transpilation, and hardware failure tolerances.
error_budget = ErrorBudget.from_even_split(1e-3)

# Step 2 — Wrap the circuit into an AlgorithmImplementation.
#   circuit is an orquestra Circuit (load from QASM via orquestra-qiskit).
algo = AlgorithmImplementation.from_circuit(circuit, error_budget, n_shots=1)

# Step 3 — Choose logical and physical architecture.
logical_arch = AllToAllArchitectureModel()
hw_model = BASIC_SC_ARCHITECTURE_MODEL  # SCModel: 1e-3 error rate, 1µs cycle

# Step 4 — Create the implementation compiler.
#   Exact required args for get_implementation_compiler not fully confirmed.
compiler = get_implementation_compiler(...)

# Step 5 — Estimate resources.
estimator = GraphResourceEstimator(optimization="Time")
result = estimator.compile_and_estimate(algo, compiler, logical_arch, hw_model)

print(result)
```

---

## C. What `result` contains

`result` is `GraphResourceInfo = ResourceInfo[GraphExtra]`:

- Physical qubit count
- Timing breakdown
- `LogicalFailureRateInfo` (rotation, distillation, QEC failure rates; `total_circuit_failure_rate`)
- `LogicalArchitectureResourceInfo` (code distance, qubit counts, QEC cycle allocation)
- `GraphExtra` (compiled algorithm details)

---

## D. Open uncertainties

- Exact `get_implementation_compiler` required arguments: **unknown from current repository evidence**
- QASM loading API (which orquestra call): **unknown from current repository evidence**
- `print(result)` exact output format: **unknown**
