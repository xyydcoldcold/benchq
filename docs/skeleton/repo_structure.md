## Repository Structure Commands Output

### pwd
/Users/xiaomaodou/Desktop/benchq

### ls -la
total 120
drwxr-xr-x  21 xiaomaodou  staff    672 Feb 17 10:19 .
drwx------@  6 xiaomaodou  staff    192 Feb 17 01:56 ..
-rw-r--r--@  1 xiaomaodou  staff   8196 Feb 17 10:18 .DS_Store
drwxr-xr-x  12 xiaomaodou  staff    384 Feb 17 02:05 .git
drwxr-xr-x   5 xiaomaodou  staff    160 Feb 17 01:56 .github
-rw-r--r--   1 xiaomaodou  staff   2108 Feb 17 01:56 .gitignore
drwxr-xr-x   4 xiaomaodou  staff    128 Feb 17 01:56 benchmarks
-rw-r--r--   1 xiaomaodou  staff    392 Feb 17 01:56 CODEOWNERS
drwxr-xr-x   3 xiaomaodou  staff     96 Feb 17 01:56 docs
drwxr-xr-x   7 xiaomaodou  staff    224 Feb 17 01:56 examples
-rw-r--r--   1 xiaomaodou  staff  11357 Feb 17 01:56 LICENSE
-rw-r--r--   1 xiaomaodou  staff   1169 Feb 17 01:56 Makefile
drwxr-xr-x   3 xiaomaodou  staff     96 Feb 17 10:19 mirror_docs
-rw-r--r--   1 xiaomaodou  staff     54 Feb 17 01:56 mkdocs.yml
-rw-r--r--   1 xiaomaodou  staff   1939 Feb 17 01:56 pyproject.toml
-rw-r--r--   1 xiaomaodou  staff    231 Feb 17 01:56 pytest.ini
-rw-r--r--   1 xiaomaodou  staff   5616 Feb 17 01:56 README.md
-rw-r--r--   1 xiaomaodou  staff   1824 Feb 17 01:56 setup.cfg
drwxr-xr-x   3 xiaomaodou  staff     96 Feb 17 01:56 src
drwxr-xr-x   3 xiaomaodou  staff     96 Feb 17 01:56 subtrees
drwxr-xr-x   3 xiaomaodou  staff     96 Feb 17 01:56 tests

### tree -L 4 -d src/benchq (tree command not available, using find instead)
src/benchq
src/benchq/algorithms
src/benchq/algorithms/data_structures
src/benchq/algorithms/gsee
src/benchq/compilation
src/benchq/compilation/circuits
src/benchq/compilation/graph_states
src/benchq/compilation/graph_states/pauli_tracker
src/benchq/compilation/graph_states/ruby_slippers
src/benchq/compilation/graph_states/substrate_scheduler
src/benchq/compilation/graph_states/table_generation
src/benchq/conversions
src/benchq/decoder_modeling
src/benchq/logical_architecture_modeling
src/benchq/magic_state_distillation_modeling
src/benchq/mlflow
src/benchq/problem_embeddings
src/benchq/problem_embeddings/block_encodings
src/benchq/problem_ingestion
src/benchq/problem_ingestion/molecular_hamiltonians
src/benchq/quantum_hardware_modeling
src/benchq/resource_estimators
src/benchq/rotation_synthesis_modeling
src/benchq/visualization_tools

### find src/benchq -type f -name "*.py" | sort
src/benchq/__init__.py
src/benchq/algorithms/__init__.py
src/benchq/algorithms/data_structures/__init__.py
src/benchq/algorithms/data_structures/algorithm_implementation.py
src/benchq/algorithms/data_structures/error_budget.py
src/benchq/algorithms/gsee/__init__.py
src/benchq/algorithms/gsee/ld_gsee.py
src/benchq/compilation/__init__.py
src/benchq/compilation/circuits/__init__.py
src/benchq/compilation/circuits/compile_to_native_gates.py
src/benchq/compilation/graph_states/__init__.py
src/benchq/compilation/graph_states/circuit_compilers.py
src/benchq/compilation/graph_states/compiled_data_structures.py
src/benchq/compilation/graph_states/implementation_compiler.py
src/benchq/compilation/graph_states/initialize_julia.py
src/benchq/compilation/graph_states/substrate_scheduler/python_substrate_scheduler.py
src/benchq/compilation/graph_states/table_generation/generate_cz_table.py
src/benchq/compilation/graph_states/table_generation/graph_states_equivalent_to_bell_state.py
src/benchq/conversions/__init__.py
src/benchq/conversions/_circuit_translations.py
src/benchq/decoder_modeling/__init__.py
src/benchq/decoder_modeling/decoder_resource_estimator.py
src/benchq/decoder_modeling/decoder.py
src/benchq/logical_architecture_modeling/__init__.py
src/benchq/logical_architecture_modeling/basic_logical_architectures.py
src/benchq/logical_architecture_modeling/graph_based_logical_architectures.py
src/benchq/magic_state_distillation_modeling/__init__.py
src/benchq/magic_state_distillation_modeling/autoccz_factories.py
src/benchq/magic_state_distillation_modeling/factory_selection.py
src/benchq/magic_state_distillation_modeling/litinski_factories.py
src/benchq/magic_state_distillation_modeling/small_footprint_factories.py
src/benchq/mlflow/__init__.py
src/benchq/mlflow/data_logging.py
src/benchq/problem_embeddings/__init__.py
src/benchq/problem_embeddings/block_encodings/double_factorized_hamiltonian.py
src/benchq/problem_embeddings/qpe.py
src/benchq/problem_embeddings/quantum_program.py
src/benchq/problem_ingestion/__init__.py
src/benchq/problem_ingestion/hamiltonian_from_file.py
src/benchq/problem_ingestion/molecular_hamiltonians/__init__.py
src/benchq/problem_ingestion/molecular_hamiltonians/_common_molecules.py
src/benchq/problem_ingestion/molecular_hamiltonians/_compute_lambda.py
src/benchq/problem_ingestion/molecular_hamiltonians/_hamiltonian_generation.py
src/benchq/quantum_hardware_modeling/__init__.py
src/benchq/quantum_hardware_modeling/devitt_surface_code.py
src/benchq/quantum_hardware_modeling/fowler_surface_code.py
src/benchq/quantum_hardware_modeling/hardware_architecture_models.py
src/benchq/resource_estimators/__init__.py
src/benchq/resource_estimators/graph_estimator.py
src/benchq/resource_estimators/openfermion_estimator.py
src/benchq/resource_estimators/resource_info.py
src/benchq/rotation_synthesis_modeling/gridsynth.py
src/benchq/timing.py
src/benchq/visualization_tools/plot_graph_state.py
src/benchq/visualization_tools/plot_substrate_scheduling.py
src/benchq/visualization_tools/resource_allocation.py

### find examples -type f -name "*.py" | sort
examples/__init__.py
examples/data/get_icm.py
examples/ex_1_from_qasm.py
examples/ex_6_mlflow.py
examples/plot_time_data.py

### find tests -type f -name "*.py" | sort
tests/benchq/algorithms/gsee/test_ld_gsee.py
tests/benchq/compilation/test_asg_stitching.py
tests/benchq/compilation/test_rbs_with_pauli_tracking.py
tests/benchq/compilation/test_ruby_slippers.py
tests/benchq/compilation/test_substrate_scheduler_components.py
tests/benchq/compilation/test_transpile_to_native_gates.py
tests/benchq/conversions/test_circuit_translations.py
tests/benchq/decoder_modeling/test_decoder_resource_estimator.py
tests/benchq/decoder_modeling/test_decoder.py
tests/benchq/examples/test_examples.py
tests/benchq/logical_architecture_modeling/test_graph_based_logical_architectures.py
tests/benchq/magic_state_distillation/test_autoccz_factories.py
tests/benchq/magic_state_distillation/test_litinski_factories.py
tests/benchq/mlflow/test_data_logging.py
tests/benchq/problem_embeddings/block_encodings/test_double_factorized.py
tests/benchq/problem_embeddings/test_qpe.py
tests/benchq/problem_ingestion/test_hamiltonian_from_file.py
tests/benchq/problem_ingestion/test_molecular_hamiltonians.py
tests/benchq/quantum_hardware_modeling/test_hardware_architecture_models.py
tests/benchq/resource_estimators/test_graph_estimator.py
tests/benchq/resource_estimators/test_openfermion_estimator.py
tests/benchq/visualization_tools/test_resource_allocation.py
