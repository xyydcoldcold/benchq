# Installation

## Requirements

- Python 3.9–3.11 (`!=3.9.7`)
- Linux recommended; Windows: use WSL

## Standard install

```bash
pip install benchq
```

## Install from source

```bash
pip install .
```

## Editable dev install (all extras)

```bash
pip install -e '.[dev]'
```

## Optional extras

| Extra | Command | Adds |
|---|---|---|
| PySCF (molecular Hamiltonians) | `pip install '.[pyscf]'` | `pyscf`, `scipy`, `openfermionpyscf` |
| Jabalizer compiler (Rust) | `pip install '.[jabalizer]'` | `pauli_tracker`, `mbqc_scheduling` |
| Azure QRE | `pip install '.[azure]'` | pyscf extra + `azure-quantum`, `pyqir`, `qiskit_qir`, `qiskit_ionq` |
| Dev tools | `pip install '.[dev]'` | `orquestra-python-dev`, `pytest-benchmark`, `numba`, `pyright` + all extras |

## Julia runtime

The Ruby Slippers compiler (default) requires Julia. Julia is auto-installed at runtime via `juliapkg` — no manual Julia installation is needed unless the auto-install fails.

The Julia wrapper is located at `src/benchq/compilation/graph_states/jabalizer_wrapper.jl`.

## Verify installation

```python
import benchq
```

No CLI entry points are registered; all interaction is via the Python API.

## Running tests

```bash
pytest .
```

With coverage:

```bash
make coverage
```

Benchmarks:

```bash
pytest benchmarks/
# Extended benchmarks:
SLOW_BENCHMARKS=1 pytest benchmarks/
```
