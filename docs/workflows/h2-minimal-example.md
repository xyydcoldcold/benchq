# H2 Minimal Example

A minimal end-to-end workflow using the hydrogen Hamiltonian.

---

## A. Evidence summary

- `get_hydrogen_chain_hamiltonian_generator()` — confirmed symbol in `benchq.problem_ingestion.molecular_hamiltonians`
- `WATER_MOLECULE` and `CYCLIC_OZONE_MOLECULE` are confirmed pre-built generators (H₂O and O₃)
- H₂ specifically: **unknown from current repository evidence** — no example file or test
  was confirmed to use an H₂ molecule by name. The hydrogen chain generator is the closest confirmed analog.
- `examples/data/h_chain_circuit.qasm` — confirmed QASM file exists for a hydrogen chain circuit
- `AlgorithmImplementation.from_circuit` — confirmed path from circuit to estimation

---

## B. Workflow — Hydrogen chain via QASM (confirmed)

The repository includes `h_chain_circuit.qasm`. The QASM-to-estimation workflow from
`examples/ex_1_from_qasm.py` applies directly:

```python
# Assumes orquestra-qiskit or cirq conversion to load QASM into a Circuit object
from benchq.algorithms.data_structures import AlgorithmImplementation, ErrorBudget
from benchq.logical_architecture_modeling import AllToAllArchitectureModel
from benchq.quantum_hardware_modeling import BASIC_SC_ARCHITECTURE_MODEL
from benchq.resource_estimators import GraphResourceEstimator
from benchq.compilation.graph_states import get_implementation_compiler

# Load QASM circuit (conversion step depends on orquestra-qiskit)
# circuit = load_qasm("h_chain_circuit.qasm")

error_budget = ErrorBudget.from_even_split(1e-3)
algo = AlgorithmImplementation.from_circuit(circuit, error_budget, n_shots=1)

estimator = GraphResourceEstimator(optimization="Time")
result = estimator.compile_and_estimate(
    algo,
    get_implementation_compiler(...),
    AllToAllArchitectureModel(),
    BASIC_SC_ARCHITECTURE_MODEL,
)
print(result)
```

## C. Workflow — Hydrogen chain via PySCF (requires `[pyscf]` extra)

```python
from benchq.problem_ingestion.molecular_hamiltonians import get_hydrogen_chain_hamiltonian_generator
from benchq.algorithms import qsp_time_evolution_algorithm
from benchq.resource_estimators import get_fast_graph_estimate
from benchq.logical_architecture_modeling import AllToAllArchitectureModel
from benchq.quantum_hardware_modeling import BASIC_SC_ARCHITECTURE_MODEL

generator = get_hydrogen_chain_hamiltonian_generator()
# generator parameters (e.g. chain length, bond distance): unknown from current repository evidence

hamiltonian = generator.get_active_space_hamiltonian()
algo = qsp_time_evolution_algorithm(hamiltonian, time=1.0, failure_tolerance=1e-3)

result = get_fast_graph_estimate(
    algo,
    AllToAllArchitectureModel(),
    BASIC_SC_ARCHITECTURE_MODEL,
    optimization="Space",
)
```

---

## D. Open uncertainties

- `get_hydrogen_chain_hamiltonian_generator` constructor parameters (chain length, bond distance): **unknown from current repository evidence**
- Whether H₂ (2-atom chain) is a supported configuration: **unknown**
- Exact QASM loading API (which orquestra package): **unknown from current repository evidence**
