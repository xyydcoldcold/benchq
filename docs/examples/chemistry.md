# Chemistry Example

Based on confirmed molecular Hamiltonian paths in BenchQ. Requires `[pyscf]` extra.

---

## A. Evidence summary

- **Symbols:** `WATER_MOLECULE`, `CYCLIC_OZONE_MOLECULE`, `MolecularHamiltonianGenerator`, `get_hydrogen_chain_hamiltonian_generator`
- **Pre-built molecules:** H₂O (6-31g, multiplicity 1), O₃ (cc-pvtz, multiplicity 3)
- **Output type:** `openfermion.InteractionOperator`
- **Algorithm constructors:** `qsp_time_evolution_algorithm`, `qpe_gsee_algorithm`
- **QPE cost functions:** `get_single_factorized_qpe_toffoli_and_qubit_cost`, `get_double_factorized_qpe_toffoli_and_qubit_cost`
- **Lambda utilities:** `compute_lambda_sf`, `compute_lambda_df`

---

## B. Time evolution on a pre-built molecule

```python
from benchq.problem_ingestion.molecular_hamiltonians import WATER_MOLECULE
from benchq.algorithms import qsp_time_evolution_algorithm
from benchq.logical_architecture_modeling import AllToAllArchitectureModel
from benchq.quantum_hardware_modeling import BASIC_SC_ARCHITECTURE_MODEL
from benchq.resource_estimators import get_fast_graph_estimate

# Generate active-space Hamiltonian via PySCF
hamiltonian = WATER_MOLECULE.get_active_space_hamiltonian()
# Returns: openfermion.InteractionOperator

# Wrap in algorithm implementation
algo = qsp_time_evolution_algorithm(hamiltonian, time=1.0, failure_tolerance=1e-3)

# Fast resource estimate (no full compilation)
result = get_fast_graph_estimate(
    algo,
    AllToAllArchitectureModel(),
    BASIC_SC_ARCHITECTURE_MODEL,
    optimization="Time",
)
print(result)
```

---

## C. QPE Toffoli and qubit cost (single factorization)

```python
from benchq.problem_ingestion.molecular_hamiltonians import WATER_MOLECULE, compute_lambda_sf
from benchq.problem_embeddings.qpe import get_single_factorized_qpe_toffoli_and_qubit_cost

hamiltonian = WATER_MOLECULE.get_active_space_hamiltonian()
# h1, eri: one- and two-body integrals from InteractionOperator
# sf_factors: single-factorization factors (computation not confirmed in detail)

toffoli_count, qubit_count = get_single_factorized_qpe_toffoli_and_qubit_cost(
    h1, eri, rank=10,
    allowable_phase_estimation_error=0.001,
    bits_precision_coefficients=10,
)
```

---

## D. QPE Toffoli and qubit cost (double factorization)

```python
from benchq.problem_embeddings.qpe import get_double_factorized_qpe_toffoli_and_qubit_cost

toffoli_count, qubit_count = get_double_factorized_qpe_toffoli_and_qubit_cost(
    h1, eri, threshold=1e-3,
    allowable_phase_estimation_error=0.001,
    bits_precision_coefficients=10,
    bits_precision_rotation=20,
)
```

---

## E. Ground-state energy estimation (QPE GSEE)

```python
from benchq.algorithms.gsee import qpe_gsee_algorithm

algo = qpe_gsee_algorithm(
    hamiltonian=hamiltonian,
    precision=1e-3,
    failure_tolerance=1e-3,
)
```

---

## F. Accessing PySCF objects directly

```python
pyscf_mol = WATER_MOLECULE.get_pyscf_molecule()
meanfield  = WATER_MOLECULE.get_active_space_meanfield_object()
# Returns: pyscf scf.hf.SCF object
```

---

## G. Open uncertainties

- `MoleculeSpecification` and `ActiveSpaceSpecification` field names: **unknown from current repository evidence**
- How to extract `h1` (one-body integrals) and `eri` (two-body integrals) from `InteractionOperator` for QPE cost functions: **unknown from current repository evidence**
- `get_hydrogen_chain_hamiltonian_generator` parameters: **unknown**
