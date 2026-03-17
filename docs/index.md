# BenchQ Documentation

BenchQ is a Python library for quantum resource estimation developed under the DARPA Quantum Benchmarking program by Zapata Computing. Latest release: **v0.7.0** (2024-04-17).

## What BenchQ does

Given a quantum algorithm specification, BenchQ estimates the physical resources (qubits, time, power) required to execute it fault-tolerantly on a target hardware model.

The pipeline follows three stages:

1. **Problem Ingestion** — load or generate a Hamiltonian from chemistry, solid-state physics, or plasma physics sources
2. **Problem Embedding** — encode the Hamiltonian into a `QuantumProgram` via an algorithm (QSP, Trotter, QPE, QAOA)
3. **Resource Estimation** — compile the program to a graph state or footprint representation and estimate physical resources

## Supported platforms

- Python 3.9–3.11 (`!=3.9.7` excluded)
- Tested on Linux; Windows users should use WSL

## Navigation

| Section | Contents |
|---|---|
| [Getting Started](getting-started/overview.md) | Installation, quickstart, terminology |
| [Tasks](tasks/estimate-resources.md) | How-to guides for common operations |
| [Workflows](workflows/pyscf-to-benchq.md) | End-to-end worked examples |
| [Reference](reference/api-inventory.md) | Full API, file formats, config |
| [Internals](internals/repo-structure.md) | Architecture and module map |
| [Examples](examples/minimal.md) | Annotated example scripts |
| [Agent](agent/task-routing.md) | Agent-facing contracts and routing |
