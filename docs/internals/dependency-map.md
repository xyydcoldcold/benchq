# Dependency Map

All confirmed dependencies from `setup.cfg`.

---

## Core dependencies (always installed)

| Package | Role in BenchQ |
|---|---|
| `orquestra-quantum` | Core quantum types: `Circuit`, `PauliSum`, `PauliTerm`, `PauliRepresentation` |
| `orquestra-vqa` | Variational quantum algorithms (QAOA support) |
| `orquestra-qiskit` | Qiskit circuit conversion (`import_circuit`/`export_circuit`) |
| `orquestra-cirq` | Cirq circuit conversion |
| `orquestra-opt` | Optimization utilities |
| `orquestra-sdk` | Parallelized compilation (requires `orq start`) |
| `networkx` | Graph manipulation in graph state compilation |
| `matplotlib` | Plotting in `visualization_tools` |
| `numpy` | Numerical computations throughout |
| `more-itertools` | Utility iteration |
| `pandas` | `ResourceAllocation.to_pandas_dataframe()` |
| `pyLIQTR` | Hamiltonian types; `get_pyliqtr_operator` conversion target |
| `openfermion` | `InteractionOperator`; QPE cost functions; `openfermion_estimator` |
| `graph-state-generation` | Graph state algorithms in compilation |
| `juliapkg` | Julia package management (Ruby Slippers runtime) |
| `juliacall` | Python↔Julia bridge (Ruby Slippers runtime) |
| `h5py` | HDF5 file reading (`get_hamiltonian_from_file`) |
| `mlflow` | `benchq.mlflow` experiment tracking |
| `stim` | Stabilizer simulation |
| `optuna` | Hyperparameter optimization |
| `cvxpy` | Convex optimization |
| `aiohttp` | Async HTTP (orquestra-sdk dependency) |
| `setuptools` | Package utilities |
| `UpSetPlot` | `ResourceAllocation.plot_upset_plot()` |

---

## Optional extras

### `[pyscf]`

```bash
pip install '.[pyscf]'
```

| Package | Role |
|---|---|
| `pyscf` | SCF calculation backend for `MolecularHamiltonianGenerator` |
| `scipy` | Scientific computing (PySCF dependency) |
| `openfermionpyscf` | PySCF↔OpenFermion bridge |

### `[jabalizer]`

```bash
pip install '.[jabalizer]'
```

| Package | Role |
|---|---|
| `pauli_tracker` | Pauli tracking for Jabalizer compiler |
| `mbqc_scheduling` | MBQC scheduling for Jabalizer compiler |

**Requires:** Rust toolchain for building native extensions.

### `[azure]`

Includes all `[pyscf]` dependencies plus:

| Package | Role |
|---|---|
| `azure-quantum` | Azure Quantum Resource Estimator integration |
| `pyqir` | QIR (Quantum Intermediate Representation) |
| `qiskit_qir` | Qiskit→QIR conversion |
| `qiskit_ionq` | IonQ backend for Qiskit |

### `[dev]`

Includes all other extras plus:

| Package | Role |
|---|---|
| `orquestra-python-dev` | Zapata dev tooling (linting, style) |
| `pytest-benchmark` | Benchmark testing |
| `numba` | JIT compilation for benchmarks |
| `pyright` | Static type checking |

---

## Runtime: Julia

The Ruby Slippers compiler requires a Julia runtime. Julia is auto-installed via `juliapkg`
at first use. No manual Julia installation is required under normal conditions.

Julia packages used (via `juliapkg`): unknown from current repository evidence (managed by
`jabalizer_wrapper.jl`).

---

## Dependency graph (simplified)

```
benchq
├── orquestra-quantum      (circuit + operator types)
├── orquestra-{vqa,qiskit,cirq,opt,sdk}
├── openfermion            (molecular operators)
├── pyLIQTR                (Hamiltonian representation)
├── graph-state-generation (GSC algorithms)
├── juliacall + juliapkg   (Ruby Slippers → Julia runtime)
├── h5py                   (HDF5 file I/O)
├── mlflow                 (experiment tracking)
├── networkx               (graph operations)
├── stim                   (stabilizer simulation)
└── [pyscf] pyscf + openfermionpyscf  (molecular Hamiltonians)
    [jabalizer] pauli_tracker + mbqc_scheduling
    [azure] azure-quantum + pyqir + qiskit_qir + qiskit_ionq
```
