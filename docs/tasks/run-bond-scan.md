# Run a Parameter Scan

> **Unknown from current repository evidence:** A dedicated "bond scan" workflow is not
> confirmed in BenchQ's examples, tests, or source code. This page documents confirmed
> parameter sweep patterns that could support such a scan.

---

## Confirmed: lattice-size sweep (from `examples/ex_10_utility_scale.py`)

The utility-scale example iterates over multiple Hamiltonian types and saves results to JSON files:

```python
from benchq.problem_ingestion.solid_state_hamiltonians import (
    generate_ising_hamiltonian_on_triangular_lattice,
    generate_ising_hamiltonian_on_kitaev_lattice,
    generate_ising_hamiltonian_on_cubic_lattice,
)
from benchq.algorithms import qsp_time_evolution_algorithm

for hamiltonian_fn in [
    generate_ising_hamiltonian_on_triangular_lattice,
    generate_ising_hamiltonian_on_kitaev_lattice,
    generate_ising_hamiltonian_on_cubic_lattice,
]:
    hamiltonian = hamiltonian_fn(lattice_size=...)
    algo = qsp_time_evolution_algorithm(hamiltonian, time=..., failure_tolerance=...)
    # ... estimate resources ...
    # save_to_file() outputs two JSON files per lattice type
```

Compiler parameters used in this example:
- `teleportation_threshold=80`
- `optimal_dag_density=10`

---

## Confirmed: tolerance sweep (from `examples/ex_6_mlflow.py`)

Iterating over 5 failure tolerance values (10⁰–10⁻⁴):

```python
for tolerance in [1e0, 1e-1, 1e-2, 1e-3, 1e-4]:
    algo = AlgorithmImplementation.from_circuit(circuit, ErrorBudget.from_even_split(tolerance))
    result = estimator.compile_and_estimate(...)
    log_resource_info_to_mlflow(result)
```

---

## Hydrogen chain (variable length)

`get_hydrogen_chain_hamiltonian_generator()` exists in
`benchq.problem_ingestion.molecular_hamiltonians`. Confirmed symbol — parameters
controlling bond length are **unknown from current repository evidence** (source not
fully inspected for argument signatures).

---

## QPE Toffoli/qubit cost scan

`benchq.problem_embeddings.qpe` provides functions that accept decomposition rank or threshold as parameters, enabling manual scans:

```python
from benchq.problem_embeddings.qpe import (
    get_single_factorized_qpe_toffoli_and_qubit_cost,
    get_double_factorized_qpe_toffoli_and_qubit_cost,
)

# Scan over rank
for rank in [10, 20, 50]:
    toffoli_count, qubit_count = get_single_factorized_qpe_toffoli_and_qubit_cost(
        h1, eri, rank=rank
    )
```
