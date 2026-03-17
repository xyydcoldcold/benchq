# Generate Hamiltonians

> **Note:** BenchQ does not use the FCIDUMP file format. Molecular Hamiltonian generation
> uses PySCF and produces `openfermion.InteractionOperator` objects, not FCIDUMP files.
> This page documents confirmed Hamiltonian generation paths.

---

## Molecular Hamiltonians (requires `[pyscf]` extra)

`benchq.problem_ingestion.molecular_hamiltonians`

### Pre-built molecules

```python
from benchq.problem_ingestion.molecular_hamiltonians import WATER_MOLECULE, CYCLIC_OZONE_MOLECULE
```

| Constant | Species | Basis | Multiplicity |
|---|---|---|---|
| `WATER_MOLECULE` | H₂O | 6-31g | 1 |
| `CYCLIC_OZONE_MOLECULE` | O₃ | cc-pvtz | 3 |

Both are `MolecularHamiltonianGenerator` instances.

### Custom molecule

```python
from benchq.problem_ingestion.molecular_hamiltonians import (
    MolecularHamiltonianGenerator,
    MoleculeSpecification,
    ActiveSpaceSpecification,
)

mol = MolecularHamiltonianGenerator(...)
pyscf_mol = mol.get_pyscf_molecule()
hamiltonian: openfermion.InteractionOperator = mol.get_active_space_hamiltonian()
meanfield = mol.get_active_space_meanfield_object()
```

### Hydrogen chain

```python
from benchq.problem_ingestion.molecular_hamiltonians import get_hydrogen_chain_hamiltonian_generator

generator = get_hydrogen_chain_hamiltonian_generator()
```

---

## Solid-state Hamiltonians (built-in, no extra required)

`benchq.problem_ingestion.solid_state_hamiltonians`

```python
from benchq.problem_ingestion.solid_state_hamiltonians import (
    generate_fermi_hubbard_jw_qubit_hamiltonian,
    generate_1d_heisenberg_hamiltonian,
    generate_ising_hamiltonian_on_kitaev_lattice,
    generate_ising_hamiltonian_on_triangular_lattice,
    generate_ising_hamiltonian_on_cubic_lattice,
)

h = generate_1d_heisenberg_hamiltonian(N=4)
h = generate_ising_hamiltonian_on_triangular_lattice(lattice_size=3)
h = generate_ising_hamiltonian_on_cubic_lattice(lattice_size=3, longitudinal_weight_prob=0.5)
```

---

## Plasma Hamiltonians (built-in)

`benchq.problem_ingestion`

```python
from benchq.problem_ingestion import get_vlasov_hamiltonian

h = get_vlasov_hamiltonian(k=2.0, alpha=0.6, nu=0.0, N=2)
```

---

## Loading from file

`benchq.problem_ingestion`

Supported file formats: JSON, HDF5 (`.hdf5`).

```python
from benchq.problem_ingestion import get_hamiltonian_from_file, get_all_hamiltonians_in_folder

h = get_hamiltonian_from_file("hamiltonian.json", allow_unsupported_files=True)
hamiltonians = get_all_hamiltonians_in_folder("./hamiltonians/")
```

HDF5 format variants detected by key presence:
- QAOA: uses `q_matrix` key
- Molecular: uses `one_body_tensor` and `two_body_tensor` keys

---

## Lambda computation (double/single factorization)

```python
from benchq.problem_ingestion.molecular_hamiltonians import compute_lambda_sf, compute_lambda_df

lambda_sf = compute_lambda_sf(h1, eri_full, sf_factors)
lambda_df = compute_lambda_df(h1, eri_full, df_factors)
```
