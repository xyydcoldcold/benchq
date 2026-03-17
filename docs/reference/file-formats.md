# File Formats

Confirmed file formats used in BenchQ, based on source code and example files.

---

## QASM (`.qasm`)

**Usage:** Quantum circuit input.

**Evidence:** Example files in `examples/data/`:
- `ghz_circuit.qasm`
- `h_chain_circuit.qasm`
- `example_circuit.qasm`
- `single_rotation.qasm`

**Loading:** Via `orquestra-qiskit` or `orquestra-cirq` conversion utilities (BenchQ's
`benchq.conversions.import_circuit` accepts `QiskitCircuit` or `CirqCircuit`).

---

## JSON (`.json`)

**Usage:** Hamiltonian input and resource estimate output.

**Loading:**
```python
from benchq.problem_ingestion import get_hamiltonian_from_file
h = get_hamiltonian_from_file("hamiltonian.json")
```

**Schema:** Unknown from current repository evidence.

**Output:** `save_to_file()` in `examples/ex_10_utility_scale.py` produces two JSON files per
lattice type. Exact schema unknown from current repository evidence.

---

## HDF5 (`.hdf5`)

**Usage:** Hamiltonian input.

**Two sub-formats detected by key presence:**

| Variant | Required keys |
|---|---|
| QAOA | `q_matrix` |
| Molecular | `one_body_tensor`, `two_body_tensor` |

**Loading:**
```python
h = get_hamiltonian_from_file("hamiltonian.hdf5")
```

**Library:** `h5py` (core dependency).

---

## CSV (`.csv`)

**Usage:** Decoder model data.

**Loading:**
```python
from benchq.decoder_modeling import DecoderModel
decoder = DecoderModel.from_csv("sample_decoder_data.csv")
```

**Content:** Tables of power (nanowatts), area (micrometers²), and delay (nanoseconds)
indexed by code distance. Maximum pre-calculated distance: 31.

**Example file:** `sample_decoder_data.csv` exists in the examples data directory (exact
path unconfirmed).

---

## ZIP (`.zip`)

**Usage:** Compressed molecule data in examples.

**Evidence:** `examples/data/small_molecules.zip`.

**Contents:** Unknown from current repository evidence.

---

## Julia source (`.jl`)

**Usage:** Ruby Slippers compiler implementation and lookup tables.

**Files:**
- `src/benchq/compilation/graph_states/jabalizer_wrapper.jl` — main Julia wrapper
- `src/benchq/compilation/graph_states/table_generation/graph_sim_data.jl` — lookup tables

**Runtime:** Loaded at runtime via `juliacall`; managed by `juliapkg`.

---

## Jupyter Notebook (`.ipynb`)

**Usage:** Interactive demonstration.

**File:** `examples/toy_problem_demo.ipynb`.

---

## Summary table

| Format | Extension | Direction | Used for |
|---|---|---|---|
| OpenQASM | `.qasm` | Input | Quantum circuits |
| JSON | `.json` | Input/Output | Hamiltonians, resource estimates |
| HDF5 | `.hdf5` | Input | Hamiltonians (QAOA or molecular) |
| CSV | `.csv` | Input | Decoder model tables |
| ZIP | `.zip` | Input | Compressed molecule data |
| Julia source | `.jl` | Internal | Graph state compilation |
| Jupyter | `.ipynb` | Example | Interactive demo |
