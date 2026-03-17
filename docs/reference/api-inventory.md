# API Inventory

Complete inventory of confirmed public symbols in BenchQ, organized by subpackage.

---

## `benchq.algorithms.data_structures`

| Symbol | Type | Description |
|---|---|---|
| `AlgorithmImplementation` | dataclass | Top-level algorithm spec: `program`, `error_budget`, `n_shots`; methods: `from_circuit`, `transpile_to_clifford_t`, `compile_to_native_gates` |
| `ErrorBudget` | dataclass | Failure budget: `algorithm_failure_tolerance`, `transpilation_failure_tolerance`, `hardware_failure_tolerance`; factories: `from_even_split`, `from_weights` |

---

## `benchq.algorithms`

| Symbol | Type | Signature |
|---|---|---|
| `qsp_time_evolution_algorithm` | function | `(hamiltonian, time, failure_tolerance) -> AlgorithmImplementation` |
| `trotter_time_evolution_algorithm` | function | `(hamiltonian: PauliRepresentation, time, failure_tolerance) -> AlgorithmImplementation` |
| `get_qaoa_optimization_algorithm` | function | `(hamiltonian: PauliSum, n_layers=1, failure_tolerance=1e-3) -> AlgorithmImplementation` |

---

## `benchq.algorithms.gsee`

| Symbol | Signature |
|---|---|
| `qpe_gsee_algorithm` | `(hamiltonian, precision, failure_tolerance) -> AlgorithmImplementation` |
| `get_ff_ld_gsee_max_evolution_time` | `(alpha, energy_gap, square_overlap, precision) -> float` |
| `get_ff_ld_gsee_num_circuit_repetitions` | `(alpha, energy_gap, square_overlap, precision, failure_probability) -> float` |

---

## `benchq.problem_ingestion`

| Symbol | Signature |
|---|---|
| `get_hamiltonian_from_file` | `(file_name, allow_unsupported_files=True) -> Union[PauliSum, None]` |
| `get_all_hamiltonians_in_folder` | `(folder_name, allow_unsupported_files=True) -> List[PauliSum]` |
| `get_vlasov_hamiltonian` | `(k, alpha, nu, N) -> PauliRepresentation` |
| `generate_fermi_hubbard_jw_qubit_hamiltonian` | `(...)` |

---

## `benchq.problem_ingestion.molecular_hamiltonians`

| Symbol | Type |
|---|---|
| `MolecularHamiltonianGenerator` | class — methods: `get_pyscf_molecule`, `get_active_space_hamiltonian`, `get_active_space_meanfield_object` |
| `MoleculeSpecification` | frozen dataclass |
| `ActiveSpaceSpecification` | frozen dataclass |
| `SCFConvergenceError` | exception |
| `get_hydrogen_chain_hamiltonian_generator` | function |
| `get_active_space_hamiltonian` | function — `(mol_spec, active_space_spec, ...) -> InteractionOperator` |
| `get_active_space_meanfield_object` | function — `(mol_spec, active_space_spec, ...) -> scf.hf.SCF` |
| `compute_lambda_sf` | function — `(h1, eri_full, sf_factors)` |
| `compute_lambda_df` | function — `(h1, eri_full, df_factors)` |
| `WATER_MOLECULE` | constant — H₂O, 6-31g, multiplicity 1 |
| `CYCLIC_OZONE_MOLECULE` | constant — O₃, cc-pvtz, multiplicity 3 |

---

## `benchq.problem_ingestion.solid_state_hamiltonians`

| Symbol | Signature |
|---|---|
| `generate_1d_heisenberg_hamiltonian` | `(N)` |
| `generate_ising_hamiltonian_on_kitaev_lattice` | `(lattice_size, weight_prob=1)` |
| `generate_ising_hamiltonian_on_triangular_lattice` | `(lattice_size, longitudinal_weight_prob=1, transverse_weight_prob=1)` |
| `generate_ising_hamiltonian_on_cubic_lattice` | `(lattice_size, longitudinal_weight_prob=0.5, transverse_weight_prob=1)` |

---

## `benchq.problem_embeddings`

| Symbol | Type |
|---|---|
| `QuantumProgram` | dataclass — fields: `subroutines`, `steps`, `calculate_subroutine_sequence`; methods: `from_circuit`, `transpile_to_clifford_t`, `compile_to_native_gates`, `combine_subroutines`, `split_into_smaller_subroutines`; properties: `multiplicities`, `subroutine_sequence`, `full_circuit`, `n_rotation_gates`, `n_c_gates`, `n_t_gates`, `min_n_nodes` |

---

## `benchq.problem_embeddings.qpe`

| Symbol | Signature |
|---|---|
| `get_single_factorized_qpe_toffoli_and_qubit_cost` | `(h1, eri, rank, allowable_phase_estimation_error=0.001, bits_precision_coefficients=10) -> Tuple[int, int]` |
| `get_double_factorized_qpe_toffoli_and_qubit_cost` | `(h1, eri, threshold, allowable_phase_estimation_error=0.001, bits_precision_coefficients=10, bits_precision_rotation=20) -> Tuple[int, int]` |

---

## `benchq.compilation.graph_states`

| Symbol | Description |
|---|---|
| `get_ruby_slippers_circuit_compiler` | Returns callable compiler; params: `takes_graph_input`, `gives_graph_output`, `max_num_qubits`, `optimal_dag_density`, `use_fully_optimized_dag`, `teleportation_threshold`, `teleportation_distance`, `min_neighbor_degree`, `max_num_neighbors_to_search`, `decomposition_strategy`, `max_graph_size` |
| `default_ruby_slippers_circuit_compiler` | `(circuit, logical_architecture_name, optimization, verbose) -> GSCInfo` |
| `get_jabalizer_circuit_compiler` | `(space_optimal_timeout=60)` → callable |
| `get_implementation_compiler` | `(circuit_compiler, max_subroutine_size, destination, num_cores, config_name, workspace_id, project_id)` → Callable |
| `get_algorithmic_graph_and_icm_output` | `(circuit)` → tuple |
| `GSCInfo` | dataclass: `num_logical_qubits`, `num_layers`, `graph_creation_tocks_per_layer`, `t_states_per_layer`, `rotations_per_layer` |
| `CompiledQuantumProgram` | dataclass: `subroutines`, `steps`, `calculate_subroutine_sequence`; properties: `n_rotation_gates`, `n_t_gates` |
| `CompiledAlgorithmImplementation` | dataclass: `program`, `algorithm_implementation`; attributes: `n_shots`, `error_budget` |

---

## `benchq.resource_estimators`

| Symbol | Description |
|---|---|
| `GraphResourceEstimator` | class; `optimization: str = "Space"`; methods: `compile_and_estimate`, `estimate_resources_from_compiled_implementation` |
| `get_precise_graph_estimate` | `(algo, logical_arch, hw_model, optimization, decoder_model=None) -> GraphResourceInfo` |
| `get_fast_graph_estimate` | `(algo, logical_arch, hw_model, optimization, decoder_model=None) -> GraphResourceInfo` |
| `get_precise_stitched_estimate` | `(...) -> ResourceInfo` |
| `get_fast_stitched_estimate` | `(...) -> ResourceInfo` |
| `get_footprint_estimate` | `(algo, hw_model, optimization, decoder_model=None)` |
| `estimate_full_graph_size` | `(algo, delayed_gate_synthesis=False) -> int` |
| `openfermion_estimator` | `(num_logical_qubits, num_toffoli=0, num_t=0, architecture_model=BASIC_SC_ARCHITECTURE_MODEL, routing_overhead_proportion=0.5, hardware_failure_tolerance=1e-3, factory_count=4, ...) -> OpenFermionResourceInfo` |

---

## `benchq.resource_estimators.resource_info`

| Symbol | Type |
|---|---|
| `ResourceInfo[TExtra]` | generic dataclass: physical qubits, timing, failure rates, extra |
| `GraphResourceInfo` | `= ResourceInfo[GraphExtra]` |
| `OpenFermionResourceInfo` | `= ResourceInfo[OpenFermionExtra]` |
| `GraphExtra` | dataclass: compiled algorithm details |
| `OpenFermionExtra` | dataclass: OpenFermion-specific metrics |
| `LogicalFailureRateInfo` | dataclass: rotation/distillation/QEC failure rates; property `total_circuit_failure_rate` |
| `LogicalArchitectureResourceInfo` | dataclass: qubit counts, code distance, magic state factories, QEC cycle allocation |
| `AbstractLogicalResourceInfo` | dataclass: abstract logical qubit count, T-gate count |
| `MagicStateFactoryInfo` | dataclass: name, error rates, spatial dims, qubit count, distillation timing |
| `DecoderInfo` | dataclass: energy, power, area, max decodable distance |
| `ELUResourceInfo` | dataclass: power, communication ports, optical cross-connect layers, ion types |
| `DetailedIonTrapArchitectureResourceInfo` | dataclass: data/bus/distillation ELU counts |

---

## `benchq.quantum_hardware_modeling`

| Symbol | Description |
|---|---|
| `SCModel` | frozen dataclass; defaults: `physical_qubit_error_rate=1e-3`, `surface_code_cycle_time_in_seconds=1e-6` |
| `IONTrapModel` | frozen dataclass; defaults: `physical_qubit_error_rate=1e-4`, `surface_code_cycle_time_in_seconds=1e-3` |
| `DetailedIonTrapModel` | concrete class (detailed ELU model) |
| `BASIC_SC_ARCHITECTURE_MODEL` | `= SCModel()` |
| `BASIC_ION_TRAP_ARCHITECTURE_MODEL` | `= IONTrapModel()` |
| `DETAILED_ION_TRAP_ARCHITECTURE_MODEL` | `= DetailedIonTrapModel()` |
| `BasicArchitectureModel` | Protocol: `physical_qubit_error_rate`, `surface_code_cycle_time_in_seconds` |
| `DetailedArchitectureModel` | Protocol: extends `BasicArchitectureModel`; adds `get_hardware_resource_estimates()` |

---

## `benchq.logical_architecture_modeling`

| Symbol | Description |
|---|---|
| `AllToAllArchitectureModel` | name: `"all_to_all"` |
| `TwoRowBusArchitectureModel` | name: `"two_row_bus"` |
| `GraphBasedLogicalArchitectureModel` | base class; methods: `generate_minimal_code_distance_resources`, `generate_spatial_resource_breakdown`, `get_qec_cycle_allocation`, `get_max_parallel_t_states`, `get_max_number_of_data_qubits_from_compiled_program`, `get_total_number_of_physical_qubits`; code distance range: `min_d=3`, `max_d=200` |

---

## `benchq.decoder_modeling`

| Symbol | Description |
|---|---|
| `DecoderModel` | dataclass: `power_table`, `area_table`, `delay_table`, `highest_calculated_distance=31`; methods: `power_in_nanowatts(distance)`, `area_in_micrometers_squared(distance)`, `delay_in_nanoseconds(distance)`, `from_csv(file_path)` |

---

## `benchq.magic_state_distillation`

| Symbol | Description |
|---|---|
| `iter_litinski_factories` | Iterates pre-defined `MagicStateFactoryInfo` for error rates 1e-3 and 1e-4 |
| `find_optimal_factory` | `(per_t_gate_failure_tolerance, magic_state_factory_iterator, optimization="Time") -> MagicStateFactoryInfo \| None` |

---

## `benchq.conversions`

| Symbol | Description |
|---|---|
| `import_circuit` | singledispatch: `QiskitCircuit \| CirqCircuit \| OrquestraCircuit → OrquestraCircuit` |
| `export_circuit` | `(circuit_type, circuit: OrquestraCircuit) -> SUPPORTED_CIRCUITS` |
| `get_pyliqtr_operator` | singledispatch: converts any supported operator type → pyLIQTR `Hamiltonian` |
| `SUPPORTED_CIRCUITS` | `= QiskitCircuit \| CirqCircuit \| OrquestraCircuit` |

---

## `benchq.mlflow`

| Symbol | Description |
|---|---|
| `log_input_objects_to_mlflow` | `(algorithm_implementation, algorithm_name, hardware_model, decoder_model=None)` |
| `log_resource_info_to_mlflow` | `(resource_info: ResourceInfo)` |
| `create_mlflow_scf_callback` | `(mlflow_client, run_id) -> Callable` |

---

## `benchq.timing`

| Symbol | Description |
|---|---|
| `TimingInfo` | dataclass: `start_counter`, `stop_counter`; property `total` |
| `measure_time` | context manager; yields `TimingInfo` |

---

## `benchq.visualization_tools`

| Symbol | Description |
|---|---|
| `ResourceAllocation` | class; methods: `log`, `log_parallelized`, `exclusive`, `inclusive`, `to_pandas_dataframe`, `plot`, `plot_upset_plot`, `plot_stacked_bar_chart`; property `total` |
| `QECCycleAllocation` | class; method `__add__(other, safe=False)` |
