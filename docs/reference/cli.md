# CLI Reference

## BenchQ CLI

**BenchQ has no CLI.** No `console_scripts` entry points are registered in `setup.cfg`.
All functionality is accessed via the Python API.

---

## Related tool: `orq` (orquestra-sdk)

When using parallelized implementation compilation via `orquestra-sdk`, the `orq` daemon
must be started separately:

```bash
orq start
```

`orq` is not part of BenchQ itself. It is a separate package (`orquestra-sdk`) that BenchQ
depends on for the parallelized compilation path.

---

## Development commands

These are development/maintenance commands, not BenchQ CLI commands:

```bash
# Install with all extras (editable)
pip install -e '.[dev]'

# Run tests
pytest .

# Run with coverage
make coverage

# Style checks
make style

# Run benchmarks
pytest benchmarks/

# Extended benchmarks
SLOW_BENCHMARKS=1 pytest benchmarks/
```
