# Failure Recovery

Known failure modes in BenchQ and confirmed or inferred recovery paths.

---

## `SCFConvergenceError`

**Source:** `benchq.problem_ingestion.molecular_hamiltonians`

**When:** PySCF SCF calculation fails to converge during `MolecularHamiltonianGenerator.get_active_space_hamiltonian()` or `get_active_space_meanfield_object()`.

**Recovery:** Adjust `MoleculeSpecification` parameters (geometry, basis, multiplicity).
`create_mlflow_scf_callback` can be used to monitor SCF convergence per-step during
MLflow-tracked runs.

---

## Julia runtime initialization failure

**When:** First call to `get_ruby_slippers_circuit_compiler()` or any Ruby Slippers function.

**Cause:** `juliapkg` auto-install failed (network, disk, permission).

**Recovery:**
1. Check `juliapkg` and `juliacall` are installed: `pip install juliapkg juliacall`
2. Check network access for Julia package download
3. Inspect `juliapkg` configuration for proxy or custom depot settings

---

## `[jabalizer]` import error

**When:** Calling `get_jabalizer_circuit_compiler()` without the extra installed.

**Recovery:** `pip install '.[jabalizer]'` — also requires Rust toolchain.

---

## `[pyscf]` import error

**When:** Importing from `benchq.problem_ingestion.molecular_hamiltonians` without PySCF.

**Recovery:** `pip install '.[pyscf]'`

---

## Code distance exceeds `max_d=200`

**When:** `GraphBasedLogicalArchitectureModel` cannot find a valid code distance ≤ 200
to meet the error budget.

**Recovery options:**
- Use a hardware model with lower `physical_qubit_error_rate`
- Increase `hardware_failure_tolerance` in `ErrorBudget`
- Use `ErrorBudget.from_weights` to shift budget toward hardware tolerance

---

## `allow_unsupported_files=True` returns `None`

**When:** `get_hamiltonian_from_file` encounters an unrecognized format.

**Recovery:** Check file format. Supported: JSON, HDF5 with correct keys (`q_matrix` for
QAOA; `one_body_tensor` + `two_body_tensor` for molecular). Set `allow_unsupported_files=False`
to raise immediately instead of returning `None`.

---

## `orq start` not running

**When:** `get_implementation_compiler` uses the `orquestra-sdk` parallelized path but
the daemon is not started.

**Recovery:** Run `orq start` in a separate shell before executing the compilation step.

---

## Ruby Slippers poor quality on small circuits

**Symptom:** Excessive overhead or poor ASG quality on small test circuits.

**Recovery:** Per `compilation/graph_states/README.md`:
- Use `get_jabalizer_circuit_compiler()` for small circuits needing high-quality ASGs
- Or use `AlgorithmImplementation.from_circuit` with a simpler downstream path

---

## Magic state factory not found

**When:** `find_optimal_factory(per_t_gate_failure_tolerance, ...)` returns `None`.

**Cause:** No factory in `magic_state_factory_iterator` meets the given tolerance.

**Recovery:**
- Relax per-T-gate failure tolerance
- Check that `iter_litinski_factories()` covers the target error rate (confirmed for 1e-3 and 1e-4)
- Provide a custom `magic_state_factory_iterator` with appropriate factories
