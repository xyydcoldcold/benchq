# Public vs. Internal API

Distinction between public API (stable, intended for external use) and internal
implementation details in BenchQ.

> **Note:** BenchQ does not explicitly mark symbols as `@public` or `_private` in all
> modules. This page is inferred from `__init__.py` exports and the `__all__` declarations
> found in the codebase.

---

## Confirmed public (explicitly exported via `__all__` or `__init__.py`)

### `benchq.problem_ingestion.molecular_hamiltonians` — confirmed `__all__`

```python
__all__ = [
    "MolecularHamiltonianGenerator",
    "SCFConvergenceError",
    "get_hydrogen_chain_hamiltonian_generator",
    "compute_lambda_sf",
    "compute_lambda_df",
    "WATER_MOLECULE",
    "CYCLIC_OZONE_MOLECULE",
    "get_active_space_hamiltonian",
    "get_active_space_meanfield_object",
    "MoleculeSpecification",
    "ActiveSpaceSpecification",
]
```

---

## Inferred public (exported by subpackage `__init__.py`)

The following symbols are accessible from their subpackage `__init__.py` and are used
in example files — treated as public API:

| Symbol | Package |
|---|---|
| `AlgorithmImplementation`, `ErrorBudget` | `benchq.algorithms.data_structures` |
| `qsp_time_evolution_algorithm`, `trotter_time_evolution_algorithm` | `benchq.algorithms` |
| `qpe_gsee_algorithm` | `benchq.algorithms.gsee` |
| `get_qaoa_optimization_algorithm` | `benchq.algorithms` |
| `QuantumProgram` | `benchq.problem_embeddings` |
| `get_hamiltonian_from_file`, `get_vlasov_hamiltonian` | `benchq.problem_ingestion` |
| `generate_1d_heisenberg_hamiltonian`, `generate_ising_hamiltonian_on_*` | `benchq.problem_ingestion.solid_state_hamiltonians` |
| `GraphResourceEstimator`, `get_fast_graph_estimate`, `get_precise_graph_estimate`, `openfermion_estimator` | `benchq.resource_estimators` |
| `AllToAllArchitectureModel`, `TwoRowBusArchitectureModel` | `benchq.logical_architecture_modeling` |
| `BASIC_SC_ARCHITECTURE_MODEL`, `BASIC_ION_TRAP_ARCHITECTURE_MODEL`, `DETAILED_ION_TRAP_ARCHITECTURE_MODEL` | `benchq.quantum_hardware_modeling` |
| `DecoderModel` | `benchq.decoder_modeling` |
| `find_optimal_factory`, `iter_litinski_factories` | `benchq.magic_state_distillation` |
| `import_circuit`, `export_circuit`, `get_pyliqtr_operator` | `benchq.conversions` |
| `log_input_objects_to_mlflow`, `log_resource_info_to_mlflow` | `benchq.mlflow` |
| `measure_time`, `TimingInfo` | `benchq.timing` |
| `ResourceAllocation`, `QECCycleAllocation` | `benchq.visualization_tools` |

---

## Internal / implementation detail

| Symbol / Path | Reason |
|---|---|
| `benchq.__init__.py` | Contains only copyright header; no re-exports — top-level package is not a stable import path |
| `compilation/graph_states/initialize_julia.py` | Julia runtime setup; not intended for direct use |
| `compilation/graph_states/jabalizer_wrapper.jl` | Julia source; internal to compiler |
| `compilation/graph_states/table_generation/graph_sim_data.jl` | Lookup tables; internal |
| `compilation/graph_states/pauli_tracker/` | Internal submodule of Ruby Slippers |
| `compilation/graph_states/ruby_slippers/` | Internal submodule of Ruby Slippers |
| `compilation/graph_states/substrate_scheduler/` | Internal submodule |
| `benchq.algorithms.profolio_optimization` module name | Typo in module name (`profolio` vs `portfolio`) — treat as internal detail |
| `get_algorithmic_graph_and_icm_output` | Low-level compilation primitive; internal |
| `default_ruby_slippers_circuit_compiler` | Lower-level than `get_ruby_slippers_circuit_compiler`; possibly internal |

---

## Naming convention note

Modules prefixed with `_` (e.g. `_circuit_translations.py`, `_qsp.py`, `_trotter.py`,
`_qaoa.py`) follow Python convention for internal/private modules. Their symbols are
re-exported through the parent `__init__.py` if intended for public use.
