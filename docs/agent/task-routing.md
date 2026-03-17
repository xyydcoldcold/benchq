# Task Routing

Decision guide for routing agent tasks to the correct BenchQ entry point.

---

## Route by task type

### "Estimate resources for a quantum algorithm"

→ `GraphResourceEstimator.compile_and_estimate()` (graph-state path)
→ or `openfermion_estimator()` (footprint path, no compilation required)

**Decision:** Use graph-state path when a circuit or `QuantumProgram` is available.
Use footprint path when only logical qubit count + Toffoli/T counts are known.

---

### "Build a Hamiltonian from a molecule"

→ Requires `[pyscf]` extra
→ `MolecularHamiltonianGenerator.get_active_space_hamiltonian()` → `openfermion.InteractionOperator`
→ Pre-built: `WATER_MOLECULE`, `CYCLIC_OZONE_MOLECULE`

---

### "Build a Hamiltonian from a lattice model"

→ No extra required
→ `generate_1d_heisenberg_hamiltonian(N)`
→ `generate_ising_hamiltonian_on_triangular_lattice(lattice_size)`
→ `generate_ising_hamiltonian_on_kitaev_lattice(lattice_size)`
→ `generate_ising_hamiltonian_on_cubic_lattice(lattice_size)`
→ `generate_fermi_hubbard_jw_qubit_hamiltonian(...)`
→ `get_vlasov_hamiltonian(k, alpha, nu, N)`

---

### "Load a Hamiltonian from a file"

→ `get_hamiltonian_from_file(file_name)` — JSON or HDF5
→ `get_all_hamiltonians_in_folder(folder_name)` — batch loading

---

### "Run time evolution"

→ QSP: `qsp_time_evolution_algorithm(hamiltonian, time, failure_tolerance)`
→ Trotter: `trotter_time_evolution_algorithm(hamiltonian, time, failure_tolerance)`

---

### "Run ground-state energy estimation"

→ `qpe_gsee_algorithm(hamiltonian, precision, failure_tolerance)`
→ `get_ff_ld_gsee_max_evolution_time(...)` / `get_ff_ld_gsee_num_circuit_repetitions(...)` — fast LDGSEE

---

### "Run combinatorial optimization (QAOA)"

→ `get_qaoa_optimization_algorithm(hamiltonian: PauliSum, n_layers, failure_tolerance)`

---

### "Estimate resources from an existing circuit"

→ `AlgorithmImplementation.from_circuit(circuit, error_budget, n_shots=1)`
→ Then: `GraphResourceEstimator.compile_and_estimate(...)`

---

### "Use a specific hardware model"

| Target hardware | Model constant |
|---|---|
| Superconducting (basic) | `BASIC_SC_ARCHITECTURE_MODEL` |
| Ion trap (basic) | `BASIC_ION_TRAP_ARCHITECTURE_MODEL` |
| Ion trap (detailed, ELU) | `DETAILED_ION_TRAP_ARCHITECTURE_MODEL` |

---

### "Compare Space vs. Time optimization"

→ `GraphResourceEstimator(optimization="Space")` or `optimization="Time"`
→ or pass `optimization=` to `get_fast_graph_estimate` / `get_precise_graph_estimate`

---

### "Log results to MLflow"

→ `log_input_objects_to_mlflow(algo, name, hw_model)`
→ `log_resource_info_to_mlflow(result)`

---

### "Get QPE Toffoli/qubit cost without full compilation"

→ `get_single_factorized_qpe_toffoli_and_qubit_cost(h1, eri, rank, ...)`
→ `get_double_factorized_qpe_toffoli_and_qubit_cost(h1, eri, threshold, ...)`

---

### "Use a decoder model"

→ `DecoderModel.from_csv(file_path)`
→ Pass as `decoder_model=` to any estimator that accepts it

---

## Compiler selection

| Scenario | Compiler |
|---|---|
| Default, large circuits | `get_ruby_slippers_circuit_compiler()` |
| Higher-quality ASG needed | `get_jabalizer_circuit_compiler()` (requires `[jabalizer]`) |
| Parallelized compilation | `get_implementation_compiler(..., orquestra-sdk)` + `orq start` |
