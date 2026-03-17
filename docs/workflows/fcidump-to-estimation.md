# File-Based Hamiltonian to Estimation

> **Note:** BenchQ does not use the FCIDUMP format. Supported file formats for Hamiltonian
> input are JSON and HDF5. This page documents the confirmed file-to-estimation workflow.

---

## A. Evidence summary

- `benchq.problem_ingestion.get_hamiltonian_from_file(file_name, allow_unsupported_files=True)`
- `benchq.problem_ingestion.get_all_hamiltonians_in_folder(folder_name, allow_unsupported_files=True)`
- HDF5 variant detection: QAOA (`q_matrix` key) vs molecular (`one_body_tensor`, `two_body_tensor` keys)
- JSON format also supported (exact schema: **unknown from current repository evidence**)
- `examples/data/small_molecules.zip` — compressed molecule data present in examples

---

## B. Workflow

### Load a single Hamiltonian from file

```python
from benchq.problem_ingestion import get_hamiltonian_from_file

# Returns PauliSum or None (if allow_unsupported_files=True and format unrecognized)
hamiltonian = get_hamiltonian_from_file("hamiltonian.json")
hamiltonian = get_hamiltonian_from_file("hamiltonian.hdf5")
```

### Load all Hamiltonians from a directory

```python
from benchq.problem_ingestion import get_all_hamiltonians_in_folder

hamiltonians = get_all_hamiltonians_in_folder("./hamiltonians/")
```

### HDF5 format notes

Two HDF5 sub-formats are distinguished by key presence:

| Format | Required key(s) |
|---|---|
| QAOA | `q_matrix` |
| Molecular | `one_body_tensor`, `two_body_tensor` |

### Proceed to estimation

After loading a Hamiltonian, the workflow is identical to any other algorithm path:

```python
from benchq.algorithms import qsp_time_evolution_algorithm
from benchq.algorithms.data_structures import AlgorithmImplementation, ErrorBudget
from benchq.resource_estimators import get_fast_graph_estimate
from benchq.logical_architecture_modeling import AllToAllArchitectureModel
from benchq.quantum_hardware_modeling import BASIC_SC_ARCHITECTURE_MODEL

# From Hamiltonian via algorithm constructor
algo = qsp_time_evolution_algorithm(hamiltonian, time=1.0, failure_tolerance=1e-3)

# Or, if the file contains a circuit representation — load QASM separately and wrap:
error_budget = ErrorBudget.from_even_split(1e-3)
algo = AlgorithmImplementation.from_circuit(circuit, error_budget)

result = get_fast_graph_estimate(
    algo,
    AllToAllArchitectureModel(),
    BASIC_SC_ARCHITECTURE_MODEL,
    optimization="Space",
)
```

---

## C. Open uncertainties

- JSON Hamiltonian file schema: **unknown from current repository evidence**
- Whether `small_molecules.zip` contains HDF5 or JSON files: **unknown**
- Return type of `get_hamiltonian_from_file` for each format (PauliSum vs InteractionOperator): stated as `Union[PauliSum, None]` in signature but exact per-format behavior **unknown**
