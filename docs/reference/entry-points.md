# Entry Points

## CLI entry points

**None.** BenchQ does not register any `console_scripts` or CLI entry points in `setup.cfg`.
All interaction is via the Python API.

---

## Python API entry points

The canonical top-level entry points for the three pipeline stages:

### Problem Ingestion

```python
# From file
from benchq.problem_ingestion import get_hamiltonian_from_file

# Molecular (pyscf extra)
from benchq.problem_ingestion.molecular_hamiltonians import MolecularHamiltonianGenerator, WATER_MOLECULE

# Solid-state
from benchq.problem_ingestion.solid_state_hamiltonians import generate_1d_heisenberg_hamiltonian

# Plasma
from benchq.problem_ingestion import get_vlasov_hamiltonian
```

### Problem Embedding / Algorithm Construction

```python
from benchq.algorithms import qsp_time_evolution_algorithm, trotter_time_evolution_algorithm
from benchq.algorithms.gsee import qpe_gsee_algorithm
from benchq.algorithms import get_qaoa_optimization_algorithm
from benchq.algorithms.data_structures import AlgorithmImplementation  # .from_circuit() for arbitrary circuits
```

### Resource Estimation

```python
# Full graph-state compilation + estimation
from benchq.resource_estimators import GraphResourceEstimator

# Convenience wrappers
from benchq.resource_estimators import (
    get_precise_graph_estimate,
    get_fast_graph_estimate,
    get_precise_stitched_estimate,
    get_fast_stitched_estimate,
    get_footprint_estimate,
    estimate_full_graph_size,
    openfermion_estimator,
)
```

### Compiler construction

```python
from benchq.compilation.graph_states import (
    get_ruby_slippers_circuit_compiler,   # default
    get_jabalizer_circuit_compiler,        # [jabalizer] extra required
    get_implementation_compiler,
)
```

---

## Development commands

| Command | Purpose |
|---|---|
| `pip install '.[dev]'` | Editable install with all extras |
| `pytest .` | Run all tests |
| `make coverage` | Tests with coverage report |
| `pytest benchmarks/` | Performance benchmarks |
| `SLOW_BENCHMARKS=1 pytest benchmarks/` | Extended benchmarks |
| `make style` | Style checks |
| `orq start` | Start orquestra-sdk daemon (required for parallelized compilation) |
