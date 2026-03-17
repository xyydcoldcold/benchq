# PySCF to BenchQ

Workflow for estimating resources from a molecular Hamiltonian generated via PySCF.

**Requires:** `pip install '.[pyscf]'`

---

## A. Evidence summary

- `benchq.problem_ingestion.molecular_hamiltonians`: `MolecularHamiltonianGenerator`, `WATER_MOLECULE`, `CYCLIC_OZONE_MOLECULE`, `get_active_space_hamiltonian`, `get_hydrogen_chain_hamiltonian_generator`
- `MolecularHamiltonianGenerator.get_active_space_hamiltonian()` returns `openfermion.InteractionOperator`
- `benchq.conversions.get_pyliqtr_operator` converts `InteractionOperator` → `pyLIQTR Hamiltonian`
- `benchq.algorithms.qsp_time_evolution_algorithm` accepts `SUPPORTED_OPERATORS` (includes `InteractionOperator`)
- Pre-built instances: `WATER_MOLECULE` (H₂O, 6-31g, multiplicity 1), `CYCLIC_OZONE_MOLECULE` (O₃, cc-pvtz, multiplicity 3)

---

## B. Workflow

### Step 1 — Generate the Hamiltonian

Using a pre-built molecule:

```python
from benchq.problem_ingestion.molecular_hamiltonians import WATER_MOLECULE

hamiltonian = WATER_MOLECULE.get_active_space_hamiltonian()
# Returns: openfermion.InteractionOperator
```

Using a custom specification:

```python
from benchq.problem_ingestion.molecular_hamiltonians import (
    MolecularHamiltonianGenerator,
    MoleculeSpecification,
    ActiveSpaceSpecification,
)

mol = MolecularHamiltonianGenerator(
    mol_spec=MoleculeSpecification(...),
    active_space_spec=ActiveSpaceSpecification(...),
)
hamiltonian = mol.get_active_space_hamiltonian()
```

### Step 2 — Build an AlgorithmImplementation

```python
from benchq.algorithms import qsp_time_evolution_algorithm

algo = qsp_time_evolution_algorithm(
    hamiltonian=hamiltonian,
    time=1.0,
    failure_tolerance=1e-3,
)
```

### Step 3 — Compile and estimate

```python
from benchq.compilation.graph_states import (
    get_ruby_slippers_circuit_compiler,
    get_implementation_compiler,
)
from benchq.logical_architecture_modeling import AllToAllArchitectureModel
from benchq.quantum_hardware_modeling import BASIC_SC_ARCHITECTURE_MODEL
from benchq.resource_estimators import GraphResourceEstimator

circuit_compiler = get_ruby_slippers_circuit_compiler()
compiler = get_implementation_compiler(circuit_compiler, ...)

result = GraphResourceEstimator(optimization="Time").compile_and_estimate(
    algo,
    compiler,
    AllToAllArchitectureModel(),
    BASIC_SC_ARCHITECTURE_MODEL,
)
print(result)
```

### Step 4 — Lambda computation (optional, for QPE cost estimation)

```python
from benchq.problem_ingestion.molecular_hamiltonians import compute_lambda_sf, compute_lambda_df
from benchq.problem_embeddings.qpe import (
    get_single_factorized_qpe_toffoli_and_qubit_cost,
    get_double_factorized_qpe_toffoli_and_qubit_cost,
)

toffoli, qubits = get_single_factorized_qpe_toffoli_and_qubit_cost(h1, eri, rank=10)
toffoli, qubits = get_double_factorized_qpe_toffoli_and_qubit_cost(h1, eri, threshold=1e-3)
```

---

## C. Open uncertainties

- Specific `MoleculeSpecification` and `ActiveSpaceSpecification` fields: **unknown from current repository evidence** (frozen dataclasses — field names not inspected)
- Whether `get_hydrogen_chain_hamiltonian_generator` accepts bond-length or N-atom parameters: **unknown**
- Exact `get_implementation_compiler` required arguments: **unknown** (only partial signature confirmed)
