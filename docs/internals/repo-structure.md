# Repository Structure

Confirmed layout of the BenchQ repository at `zapatacomputing/benchq`.

```
benchq/
├── src/
│   └── benchq/               # Main package (namespace package)
│       ├── __init__.py        # Copyright header; no re-exports
│       ├── timing.py
│       ├── algorithms/
│       ├── compilation/
│       ├── conversions/
│       ├── decoder_modeling/
│       ├── logical_architecture_modeling/
│       ├── magic_state_distillation/
│       ├── mlflow/
│       ├── problem_embeddings/
│       ├── problem_ingestion/
│       ├── quantum_hardware_modeling/
│       ├── resource_estimators/
│       └── visualization_tools/
├── tests/
│   └── benchq/               # Mirrors src/benchq/ structure exactly
│       ├── algorithms/
│       ├── compilation/
│       ├── conversions/
│       ├── decoder_modeling/
│       ├── examples/          # Integration tests that run example scripts
│       ├── logical_architecture_modeling/
│       ├── magic_state_distillation/
│       ├── mlflow/
│       ├── problem_embeddings/
│       ├── problem_ingestion/
│       ├── quantum_hardware_modeling/
│       ├── resource_estimators/
│       └── visualization_tools/
├── examples/
│   ├── ex_1_from_qasm.py
│   ├── ex_2_time_evolution.py
│   ├── ex_4_fast_graph_estimates.py
│   ├── ex_6_mlflow.py
│   ├── ex_10_utility_scale.py
│   ├── plot_time_data.py
│   ├── toy_problem_demo.ipynb
│   └── data/
│       ├── ghz_circuit.qasm
│       ├── h_chain_circuit.qasm
│       ├── example_circuit.qasm
│       ├── single_rotation.qasm
│       └── small_molecules.zip
├── benchmarks/               # Performance benchmarks
├── pyproject.toml
├── setup.cfg                 # Package metadata, dependencies, extras
└── README.md
```

---

## Notes

- No `docs/` directory exists in the upstream repository.
- `src/benchq/__init__.py` contains only a copyright header — no public re-exports at the top level.
- All 12 source subpackages have corresponding test directories under `tests/benchq/`.
- `tests/benchq/examples/` runs example scripts as integration tests.
- `compilation/graph_states/` contains a Julia source file (`jabalizer_wrapper.jl`) and lookup tables (`table_generation/`).
