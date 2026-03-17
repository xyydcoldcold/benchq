# Configuration

BenchQ has no global configuration file. All configuration is passed as constructor arguments or function parameters.

---

## `GraphResourceEstimator`

```python
GraphResourceEstimator(
    optimization: str = "Space",  # "Space" or "Time"
    verbose: bool = False,
)
```

---

## `get_ruby_slippers_circuit_compiler` parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `takes_graph_input` | bool | `True` | Whether input is a graph state |
| `gives_graph_output` | bool | `True` | Whether output is a graph state |
| `max_num_qubits` | int | `1` | Max qubits per compilation batch |
| `optimal_dag_density` | int | `1` | DAG density target (`10` used in utility-scale example) |
| `use_fully_optimized_dag` | bool | `False` | Use fully optimized DAG |
| `teleportation_threshold` | int | `40` | Gate threshold for teleportation (`80` in utility-scale example) |
| `teleportation_distance` | int | `4` | Teleportation distance |
| `min_neighbor_degree` | int | `6` | Minimum neighbor degree |
| `max_num_neighbors_to_search` | — | — | Unknown from current repository evidence |
| `decomposition_strategy` | int | `0` | Decomposition strategy index |
| `max_graph_size` | — | — | Unknown from current repository evidence |

---

## `get_jabalizer_circuit_compiler` parameters

| Parameter | Type | Default |
|---|---|---|
| `space_optimal_timeout` | int | `60` (seconds) |

---

## `ErrorBudget` factory methods

```python
ErrorBudget.from_even_split(total_failure_tolerance: float)
# Splits total evenly across algorithm, transpilation, hardware

ErrorBudget.from_weights(total, algo_weight, transpilation_weight, hardware_weight)
```

---

## Hardware model parameters

```python
SCModel(
    physical_qubit_error_rate: float = 1e-3,
    surface_code_cycle_time_in_seconds: float = 1e-6,
)

IONTrapModel(
    physical_qubit_error_rate: float = 1e-4,
    surface_code_cycle_time_in_seconds: float = 1e-3,
)
```

---

## `GraphBasedLogicalArchitectureModel` code distance range

| Parameter | Value |
|---|---|
| `min_d` | `3` |
| `max_d` | `200` |

---

## `DecoderModel.from_csv`

```python
DecoderModel.from_csv(file_path: str) -> DecoderModel
```

A sample decoder data file is included at `examples/data/sample_decoder_data.csv` (inferred from example usage — exact path **unknown from current repository evidence**).

The model covers distances up to `highest_calculated_distance=31` by default.

---

## `openfermion_estimator` parameters

| Parameter | Default |
|---|---|
| `routing_overhead_proportion` | `0.5` |
| `hardware_failure_tolerance` | `1e-3` |
| `factory_count` | `4` |

---

## `QuantumProgram.split_into_smaller_subroutines`

```python
program.split_into_smaller_subroutines(max_size: int)
```

Splits subroutines that exceed `max_size` gates.
