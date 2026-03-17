# Debug Common Failures

> Evidence basis: `benchq.problem_ingestion.molecular_hamiltonians` source, README,
> `compilation/graph_states/README.md`, and test structure. Direct error messages and
> tracebacks are **unknown from current repository evidence** unless noted.

---

## `SCFConvergenceError`

**Location:** `benchq.problem_ingestion.molecular_hamiltonians`

Raised when the PySCF self-consistent field (SCF) calculation fails to converge.

**Confirmed symbol:** `SCFConvergenceError` is exported from the molecular Hamiltonians subpackage.

**Likely causes:**
- Molecule geometry or basis set incompatible with SCF convergence
- Multiplicity mismatch (e.g., wrong spin state for molecule)

**Mitigation:** Adjust `MoleculeSpecification` parameters; try a different basis or initial guess. *(Specific PySCF convergence parameters are unknown from current repository evidence.)*

---

## Julia runtime not available (Ruby Slippers)

Ruby Slippers requires a Julia runtime, auto-installed via `juliapkg`. Failures typically surface at first `get_ruby_slippers_circuit_compiler()` call.

**Check:** Ensure `juliapkg` and `juliacall` are installed (both are in core `install_requires`). If auto-install fails, verify internet access and `juliapkg` configuration.

---

## Jabalizer extra not installed

Calling `get_jabalizer_circuit_compiler()` without the `[jabalizer]` extra raises an import error for `pauli_tracker` or `mbqc_scheduling`.

```bash
pip install '.[jabalizer]'
```

Jabalizer also requires a Rust toolchain for building its dependencies.

---

## PySCF extra not installed

Importing from `benchq.problem_ingestion.molecular_hamiltonians` without `[pyscf]` raises an import error for `pyscf` or `openfermionpyscf`.

```bash
pip install '.[pyscf]'
```

---

## HDF5 file format mismatch

`get_hamiltonian_from_file` distinguishes HDF5 variants by key presence:
- QAOA format: expects `q_matrix` key
- Molecular format: expects `one_body_tensor` and `two_body_tensor` keys

If the file lacks the expected key, the function may return `None` (when `allow_unsupported_files=True`) or raise an error.

---

## `allow_unsupported_files` behavior

```python
get_hamiltonian_from_file("file.xyz", allow_unsupported_files=True)  # returns None on unknown format
get_hamiltonian_from_file("file.xyz", allow_unsupported_files=False) # raises on unknown format
```

---

## Ruby Slippers overhead on small circuits

From `compilation/graph_states/README.md`: Ruby Slippers has overhead on small circuits.
Use `get_jabalizer_circuit_compiler` or `AlgorithmImplementation.from_circuit` with
a simpler compilation path for small test cases.

---

## Code distance limits

`GraphBasedLogicalArchitectureModel` enforces `min_d=3`, `max_d=200`. If the required
code distance to meet the error budget exceeds 200, estimation will fail.
Reduce `hardware_failure_tolerance` or switch to a lower-error hardware model.

---

## `orq start` required for parallelized compilation

If using `orquestra-sdk` for parallelized implementation compilation, the `orq` daemon must be running:

```bash
orq start
```

Without this, `get_implementation_compiler` calls that use the SDK backend will fail.
