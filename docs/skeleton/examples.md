# BenchQ Examples Inventory

## examples/__init__.py
**Path:** `examples/__init__.py`

- Empty file (only copyright header)

---

## examples/data/get_icm.py
**Path:** `examples/data/get_icm.py`

### Summary
- Provides a utility function to convert quantum circuits to ICM (Instantaneous Quantum Polynomial-time) form
- Imports: `orquestra.quantum.circuits` (CNOT, Circuit)
- Main function: `get_icm(circuit, gates_to_decompose)`
- Takes a circuit and decomposes specified gates (T, T_Dagger, RX, RY, RZ by default) into CNOT operations with ancilla qubits
- Returns a new Circuit in ICM form
- Manages qubit index compilation and ancilla qubit allocation

---

## examples/ex_1_from_qasm.py
**Path:** `examples/ex_1_from_qasm.py`

### Summary
- **Purpose:** Basic example demonstrating resource estimation from a QASM file
- **Requirements:** Requires `pyscf` extra (`pip install benchq[pyscf]`)
- **Main imports from benchq:**
  - `benchq.algorithms.data_structures`: AlgorithmImplementation, ErrorBudget
  - `benchq.compilation.graph_states`: get_ruby_slippers_circuit_compiler
  - `benchq.compilation.graph_states.implementation_compiler`: get_implementation_compiler
  - `benchq.logical_architecture_modeling.graph_based_logical_architectures`: AllToAllArchitectureModel, TwoRowBusArchitectureModel
  - `benchq.quantum_hardware_modeling`: BASIC_SC_ARCHITECTURE_MODEL
  - `benchq.resource_estimators.graph_estimator`: GraphResourceEstimator
- **Main flow:**
  1. Loads a quantum circuit from QASM file using qiskit
  2. Creates an ErrorBudget with even split and 1e-3 total failure tolerance
  3. Wraps circuit in AlgorithmImplementation with error budget and n_shots=1
  4. Creates GraphResourceEstimator optimized for "Time"
  5. Gets implementation compiler with Ruby Slippers circuit compiler
  6. Uses AllToAllArchitectureModel for logical architecture
  7. Runs `estimator.compile_and_estimate()` to get resource estimates
- **Output:** Prints resource estimation results
- **Default input:** `data/ghz_circuit.qasm`

---

## examples/ex_6_mlflow.py
**Path:** `examples/ex_6_mlflow.py`

### Summary
- **Purpose:** Copy of example 1 with added MLflow integration for logging params and metrics
- **Main imports from benchq:**
  - `benchq.algorithms.data_structures`: AlgorithmImplementation, ErrorBudget
  - `benchq.compilation.graph_states.implementation_compiler`: get_implementation_compiler
  - `benchq.mlflow.data_logging`: log_input_objects_to_mlflow, log_resource_info_to_mlflow
  - `benchq.problem_embeddings`: QuantumProgram
  - `benchq.quantum_hardware_modeling`: BASIC_SC_ARCHITECTURE_MODEL
  - `benchq.resource_estimators.graph_estimator`: GraphResourceEstimator
- **Main flow:**
  1. Loads circuit from QASM file using qiskit
  2. Converts to QuantumProgram using `import_from_qiskit`
  3. Creates ErrorBudget and AlgorithmImplementation
  4. Creates GraphResourceEstimator with "Time" optimization
  5. Runs compilation and estimation
  6. **MLflow integration:**
     - Starts MLflow run with `mlflow.start_run()`
     - Logs input objects: algorithm_implementation, algorithm name, hardware model
     - Logs resource estimation results
- **Output:** Logs data to MLflow (tracking URI can be configured)
- **Execution:** Runs 5 iterations with different failure tolerances (10^-i for i in range(5))
- **Default input:** `data/example_circuit.qasm`

---

## examples/plot_time_data.py
**Path:** `examples/plot_time_data.py`

### Summary
- **Purpose:** Utility for plotting timing data with log-log scale and polynomial fit
- **Imports:** matplotlib.pyplot, numpy (no benchq imports)
- **Main function:** `plot_log_log_with_fit(times, problem_sizes)`
- **Functionality:**
  - Takes lists of execution times and problem sizes
  - Converts to logarithmic scale and fits a line to the data
  - Creates extended range up to problem size 20
  - Plots original data as scatter points
  - Overlays polynomial fit line (red)
  - Uses log-log scale for both axes
  - Labels: "Lattice Size" (x-axis), "Time" (y-axis)
  - Title: "Log-Log Plot with Linear Fit Extended to Lattice Size 20"
- **Example usage:** Includes sample data for lattice sizes [6, 8, 10, 12] with corresponding execution times
- **Output:** Displays matplotlib plot
