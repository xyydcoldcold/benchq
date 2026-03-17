# Module Map

All confirmed modules under `src/benchq/`, with key symbols per module.

---

## `benchq.algorithms`

| Module | Key symbols |
|---|---|
| `algorithms/data_structures/algorithm_implementation.py` | `AlgorithmImplementation` |
| `algorithms/data_structures/error_budget.py` | `ErrorBudget` |
| `algorithms/time_evolution.py` | `qsp_time_evolution_algorithm`, `trotter_time_evolution_algorithm` |
| `algorithms/profolio_optimization.py` | `get_qaoa_optimization_algorithm` |
| `algorithms/gsee/qpe_gsee.py` | `qpe_gsee_algorithm` |
| `algorithms/gsee/ld_gsee.py` | `get_ff_ld_gsee_max_evolution_time`, `get_ff_ld_gsee_num_circuit_repetitions` |

---

## `benchq.compilation`

| Module | Key symbols |
|---|---|
| `compilation/graph_states/circuit_compilers.py` | `get_ruby_slippers_circuit_compiler`, `default_ruby_slippers_circuit_compiler`, `get_jabalizer_circuit_compiler` |
| `compilation/graph_states/implementation_compiler.py` | `get_implementation_compiler` |
| `compilation/graph_states/compiled_data_structures.py` | `GSCInfo`, `CompiledQuantumProgram`, `CompiledAlgorithmImplementation` |
| `compilation/graph_states/initialize_julia.py` | Julia runtime initialization |
| `compilation/graph_states/jabalizer_wrapper.jl` | Julia: Ruby Slippers wrapper |
| `compilation/graph_states/table_generation/graph_sim_data.jl` | Julia: lookup tables |
| `compilation/graph_states/pauli_tracker/` | Pauli tracking submodule |
| `compilation/graph_states/ruby_slippers/` | Ruby Slippers submodule |
| `compilation/graph_states/substrate_scheduler/` | Substrate scheduling submodule |

---

## `benchq.conversions`

| Module | Key symbols |
|---|---|
| `conversions/_circuit_translations.py` | `import_circuit`, `export_circuit`, `SUPPORTED_CIRCUITS` |
| `conversions/_openfermion_pyliqtr.py` | `get_pyliqtr_operator` |
| `conversions/_operator_translations.py` | Operator conversion utilities |

---

## `benchq.decoder_modeling`

| Module | Key symbols |
|---|---|
| `decoder_modeling/decoder.py` | `DecoderModel` |
| `decoder_modeling/decoder_resource_estimator.py` | Decoder resource estimation utilities |

---

## `benchq.logical_architecture_modeling`

| Module | Key symbols |
|---|---|
| `logical_architecture_modeling/basic_logical_architectures.py` | Unknown from current repository evidence |
| `logical_architecture_modeling/graph_based_logical_architectures.py` | `GraphBasedLogicalArchitectureModel`, `AllToAllArchitectureModel`, `TwoRowBusArchitectureModel` |

---

## `benchq.magic_state_distillation`

| Module | Key symbols |
|---|---|
| `magic_state_distillation/litinski_factories.py` | `iter_litinski_factories` (error rates 1e-3 and 1e-4) |
| `magic_state_distillation/factory_selection.py` | `find_optimal_factory` |
| `magic_state_distillation/autoccz_factories.py` | Unknown from current repository evidence |
| `magic_state_distillation/small_footprint_factories.py` | Unknown from current repository evidence |

---

## `benchq.mlflow`

| Module | Key symbols |
|---|---|
| `mlflow/data_logging.py` | `log_input_objects_to_mlflow`, `log_resource_info_to_mlflow`, `create_mlflow_scf_callback` |

---

## `benchq.problem_embeddings`

| Module | Key symbols |
|---|---|
| `problem_embeddings/quantum_program.py` | `QuantumProgram` |
| `problem_embeddings/qpe.py` | `get_single_factorized_qpe_toffoli_and_qubit_cost`, `get_double_factorized_qpe_toffoli_and_qubit_cost` |
| `problem_embeddings/qsp/_qsp.py` | QSP embedding |
| `problem_embeddings/qsp/_lin_and_dong_qsp.py` | Lin and Dong QSP |
| `problem_embeddings/qsp/get_qsp_phases.py` | QSP phase computation |
| `problem_embeddings/qsp/get_qsp_polynomial.py` | QSP polynomial |
| `problem_embeddings/trotter/_trotter.py` | Trotter embedding |
| `problem_embeddings/qaoa/_qaoa.py` | QAOA embedding |
| `problem_embeddings/block_encodings/double_factorized_hamiltonian.py` | Double-factorized block encoding |
| `problem_embeddings/block_encodings/offset_tridiagonal.py` | Offset tridiagonal encoding |
| `problem_embeddings/time_marching/` | Time-marching submodule |

---

## `benchq.problem_ingestion`

| Module | Key symbols |
|---|---|
| `problem_ingestion/hamiltonian_from_file.py` | `get_hamiltonian_from_file`, `get_all_hamiltonians_in_folder` |
| `problem_ingestion/molecular_hamiltonians/_hamiltonian_generation.py` | `MolecularHamiltonianGenerator`, `get_active_space_hamiltonian`, `WATER_MOLECULE`, `CYCLIC_OZONE_MOLECULE` |
| `problem_ingestion/molecular_hamiltonians/_common_molecules.py` | Pre-built molecule constants |
| `problem_ingestion/molecular_hamiltonians/_compute_lambda.py` | `compute_lambda_sf`, `compute_lambda_df` |
| `problem_ingestion/plasma_hamiltonians/vlasov.py` | `get_vlasov_hamiltonian` |
| `problem_ingestion/solid_state_hamiltonians/heisenberg.py` | `generate_1d_heisenberg_hamiltonian` |
| `problem_ingestion/solid_state_hamiltonians/ising.py` | `generate_ising_hamiltonian_on_*` |
| `problem_ingestion/solid_state_hamiltonians/fermi_hubbard.py` | `generate_fermi_hubbard_jw_qubit_hamiltonian` |

---

## `benchq.quantum_hardware_modeling`

| Module | Key symbols |
|---|---|
| `quantum_hardware_modeling/hardware_architecture_models.py` | `BasicArchitectureModel`, `DetailedArchitectureModel`, `BASIC_SC_ARCHITECTURE_MODEL`, `BASIC_ION_TRAP_ARCHITECTURE_MODEL`, `DETAILED_ION_TRAP_ARCHITECTURE_MODEL` |
| `quantum_hardware_modeling/fowler_surface_code.py` | `SCModel` |
| `quantum_hardware_modeling/devitt_surface_code.py` | Unknown from current repository evidence |

---

## `benchq.resource_estimators`

| Module | Key symbols |
|---|---|
| `resource_estimators/graph_estimator.py` | `GraphResourceEstimator`, `get_precise_graph_estimate`, `get_fast_graph_estimate`, `estimate_full_graph_size` |
| `resource_estimators/default_estimators.py` | `get_precise_stitched_estimate`, `get_fast_stitched_estimate`, `get_footprint_estimate` |
| `resource_estimators/openfermion_estimator.py` | `openfermion_estimator` |
| `resource_estimators/resource_info.py` | `ResourceInfo`, `GraphResourceInfo`, `OpenFermionResourceInfo`, `GraphExtra`, `OpenFermionExtra`, `LogicalFailureRateInfo`, `LogicalArchitectureResourceInfo`, `MagicStateFactoryInfo`, `DecoderInfo` |

---

## `benchq.visualization_tools`

| Module | Key symbols |
|---|---|
| `visualization_tools/resource_allocation.py` | `ResourceAllocation`, `QECCycleAllocation` |
| `visualization_tools/plot_graph_state.py` | Graph state plotting |
| `visualization_tools/plot_substrate_scheduling.py` | Substrate scheduling plotting |

---

## `benchq.timing`

| Module | Key symbols |
|---|---|
| `timing.py` | `TimingInfo`, `measure_time` |
