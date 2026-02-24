# BenchQ Entry Points

## Project Files

### README.md
Path: `README.md`

BenchQ provides tools for estimating the hardware resources required for fault-tolerant quantum computation. It includes a graph-state compiler, distillation factory models, decoder performance models, an ion-trap architecture model, implementations of selected quantum algorithms, and more.

Developed as part of DARPA Quantum Benchmarking program.

Installation: `pip install benchq`

### pyproject.toml
Path: `pyproject.toml`

Contains build system configuration using setuptools and setuptools_scm.

### setup.cfg
Path: `setup.cfg`

Package name: `benchq`
Description: "BenchQ platform for resource estimation"
GitHub: https://github.com/isi-usc-edu/benchq
Python requirements: >=3.9, !=3.9.7

### src/benchq/__init__.py
Path: `src/benchq/__init__.py`

Empty module (only copyright header).

## CLI Entry Points

No console scripts or CLI entry points are defined in `pyproject.toml` or `setup.cfg`.

## Primary User Entry Points

Based on README.md and package structure:

### Main Resource Estimators

1. **GraphResourceEstimator**
   - File: `src/benchq/resource_estimators/graph_estimator.py:32`
   - Primary class for graph-based resource estimation
   - Supports optimization for "Time" or "Space"

2. **openfermion_estimator** (function)
   - File: `src/benchq/resource_estimators/openfermion_estimator.py:182`
   - OpenFermion-based resource estimation

### Core Data Structures

3. **AlgorithmImplementation**
   - File: `src/benchq/algorithms/data_structures/algorithm_implementation.py:9`
   - Encapsulates algorithm implementation including program, error budget, and n_shots

4. **ErrorBudget**
   - File: `src/benchq/algorithms/data_structures/error_budget.py:5`
   - Manages error sources for resource estimation

5. **QuantumProgram**
   - File: `src/benchq/problem_embeddings/quantum_program.py:15`
   - Represents a quantum circuit as a sequence of subroutine invocations

### Primary Example Scripts

6. **ex_1_from_qasm.py**
   - File: `examples/ex_1_from_qasm.py`
   - Basic example of resource estimation from a QASM file
   - Demonstrates the complete pipeline: circuit loading, error budget setup, compilation, and estimation

7. **ex_6_mlflow.py**
   - File: `examples/ex_6_mlflow.py`
   - Demonstrates MLflow integration for logging parameters and metrics

### Hardware Models

8. **BASIC_SC_ARCHITECTURE_MODEL**
   - Module: `benchq.quantum_hardware_modeling`
   - Imported in examples as the default hardware architecture model

9. **DetailedIonTrapModel**
   - File: `src/benchq/quantum_hardware_modeling/hardware_architecture_models.py:69`
   - Detailed ion trap architecture model

### Compilers

10. **get_implementation_compiler** (function)
    - File: `src/benchq/compilation/graph_states/implementation_compiler.py:42`
    - Returns a parallelized compiler for algorithm implementations

11. **get_ruby_slippers_circuit_compiler** (function)
    - File: `src/benchq/compilation/graph_states/circuit_compilers.py:37`
    - Returns the Ruby Slippers circuit compiler (graph state compilation)

### Logical Architectures

12. **AllToAllArchitectureModel**
    - File: `src/benchq/logical_architecture_modeling/graph_based_logical_architectures.py:412`

13. **TwoRowBusArchitectureModel**
    - File: `src/benchq/logical_architecture_modeling/graph_based_logical_architectures.py:447`

### Magic State Factories

14. **find_optimal_factory** (function)
    - File: `src/benchq/magic_state_distillation_modeling/factory_selection.py:7`
    - Finds the optimal magic state factory for given parameters

## Usage Pattern

According to README.md and examples, typical usage follows this pattern:

1. Load or create a quantum circuit
2. Define an ErrorBudget
3. Create an AlgorithmImplementation from the circuit and error budget
4. Select a hardware architecture model
5. Create a GraphResourceEstimator or use openfermion_estimator
6. Optionally select a compiler (implementation compiler + circuit compiler)
7. Call estimator methods to get resource estimates
