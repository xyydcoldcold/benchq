# N2 Scan Example

> **Unknown from current repository evidence:** No N₂ molecule example, N₂ scan workflow,
> or N₂-specific test was found in the BenchQ repository. No `n2`, `nitrogen`, or
> `diatomic` references were confirmed in examples, tests, or source code.

This page documents the closest confirmed analog: a multi-system parameter scan using
solid-state Hamiltonians, from `examples/ex_10_utility_scale.py`.

---

## A. Evidence summary

- `examples/ex_10_utility_scale.py` — confirmed example that iterates over three lattice types
- Three Ising lattice generators: triangular, Kitaev, cubic
- Outputs two JSON files per lattice type via `save_to_file()`
- Compiler parameters: `teleportation_threshold=80`, `optimal_dag_density=10`
- Also uses CSV decoder data and both graph-state and OpenFermion estimation paths

---

## B. Multi-system scan (confirmed, from `ex_10_utility_scale.py`)

```python
from benchq.problem_ingestion.solid_state_hamiltonians import (
    generate_ising_hamiltonian_on_triangular_lattice,
    generate_ising_hamiltonian_on_kitaev_lattice,
    generate_ising_hamiltonian_on_cubic_lattice,
)
from benchq.algorithms import qsp_time_evolution_algorithm
from benchq.compilation.graph_states import get_ruby_slippers_circuit_compiler, get_implementation_compiler
from benchq.decoder_modeling import DecoderModel
from benchq.resource_estimators import GraphResourceEstimator, openfermion_estimator

decoder = DecoderModel.from_csv("path/to/sample_decoder_data.csv")

circuit_compiler = get_ruby_slippers_circuit_compiler(
    teleportation_threshold=80,
    optimal_dag_density=10,
)
compiler = get_implementation_compiler(circuit_compiler, ...)

for lattice_fn in [
    generate_ising_hamiltonian_on_triangular_lattice,
    generate_ising_hamiltonian_on_kitaev_lattice,
    generate_ising_hamiltonian_on_cubic_lattice,
]:
    hamiltonian = lattice_fn(lattice_size=...)
    algo = qsp_time_evolution_algorithm(hamiltonian, time=..., failure_tolerance=...)

    # Graph-state path
    result_graph = GraphResourceEstimator(optimization="Time").compile_and_estimate(
        algo, compiler, logical_arch, hw_model, decoder_model=decoder
    )

    # OpenFermion footprint path
    result_oe = openfermion_estimator(
        num_logical_qubits=...,
        num_toffoli=...,
        architecture_model=hw_model,
    )

    # save_to_file() — outputs two JSON files per lattice type
```

---

## C. How to adapt for a molecular scan

If doing a scan over molecular geometries (e.g. N₂ bond length), the pattern would use
`MolecularHamiltonianGenerator` from the `[pyscf]` extra. The exact parameter interface
for varying geometry is **unknown from current repository evidence**.

---

## D. Open uncertainties

- No N₂-specific workflow exists in the repository
- `save_to_file()` exact API: **unknown from current repository evidence**
- `lattice_size` parameter range used in `ex_10_utility_scale.py`: **unknown**
