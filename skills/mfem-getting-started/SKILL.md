---
name: mfem-getting-started
description: This skill should be used when users ask about getting started in mfem; it prioritizes documentation references and then source inspection only for unresolved details.
---

# mfem: Getting Started

## High-Signal Playbook

### Route Conditions
- Route to `mfem-build-and-install` if library/examples are not building or MPI/GPU is not enabled.
- Route to `mfem-miniapps` when user moves from baseline examples to domain miniapps.
- Route to `mfem-tests` for regression/convergence workflows beyond first-run smoke checks.

### Triage Questions
- Is MFEM already built, and with which mode (serial vs MPI)?
- Are you running from repo root, `examples/`, or an out-of-source build dir?
- Which mesh file should be used, and does the path exist under `data/`?
- Do you want serial (`ex1`) or MPI (`ex1p`) first?
- Is GLVis available, or should we force `-no-vis`?
- Are you testing CPU only or a device backend (`-d cuda|hip|raja-*|occa-*|ceed-*`)?

### Canonical Workflow
1. Build MFEM in the desired mode (`serial` or `parallel`).
2. Build example binaries (`ex1`/`ex1p`).
3. Start with a known mesh in `data/` (for example `star.mesh`).
4. Run a serial baseline (`ex1`) without visualization first.
5. Run MPI baseline (`ex1p`) if parallel build is enabled.
6. Add backend/runtime flags only after CPU baseline succeeds.
7. Enable GLVis visualization when interactive graphics are available.
8. Use `make check` for repeatable smoke validation.

### Minimal Working Example
```bash
cd examples
make ex1 ex1p -j4

# serial baseline
./ex1 -m ../data/star.mesh -no-vis

# mpi baseline
mpirun -np 4 ./ex1p -m ../data/star.mesh -no-vis
```

### Pitfalls And Fixes
- Mesh path errors are usually relative-path mistakes: from `examples/` use paths like `../data/star.mesh`, from repo root use `data/star.mesh`.
- `ex1p` fails when MFEM was built without MPI: rebuild with parallel configuration.
- Headless environments hang on visualization sockets: add `-no-vis`.
- Device options fail if backend was not compiled into MFEM: verify build flags first.
- Data-heavy unit tests tagged `[MFEMData]` are skipped unless `--data <path-to-mfem-data>` is provided.

### Convergence/Validation Checks
- `ex1`/`ex1p` complete with no runtime errors on a known mesh.
- Output files (for example `refined.mesh` and solution files) are produced for post-checks.
- `make check` succeeds in the selected build mode.
- MPI and serial baselines show consistent qualitative behavior on the same mesh/options.

## Scope
- Handle questions about initial setup, quickstarts, and core concepts.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary Documentation References
- `README.md`
- `INSTALL`
- `tests/unit/README.md`
- `doc/README`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for the complete topic document list.
- Use examples as executable usage patterns.
- Use tests as behavior references when available.
- If ambiguity remains after docs, inspect `references/source_map.md` and start with ranked source entry points.
- Cite exact documentation file paths in responses.

## Tutorials And Examples
- `examples`
- `miniapps`

## Test References
- `tests`
- `makefile`
- `config/test.mk`

## Optional Deeper Inspection
- `config`
- `fem`
- `general`
- `linalg`
- `mesh`
- `miniapps`
- `examples`

## Source Entry Points For Unresolved Issues
- `examples/ex1.cpp`
- `examples/ex1p.cpp`
- `examples/makefile`
- `config/test.mk`
- `makefile`
- `data`
- `tests/unit/README.md`
- `linalg/solvers.hpp`
- `linalg/solvers.cpp`
- `linalg/filteredsolver.hpp`
- `linalg/filteredsolver.cpp`
- `linalg/amgxsolver.hpp`
- `linalg/amgxsolver.cpp`
- `mesh/wedge.hpp`
- `mesh/vtk.hpp`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" config fem general linalg mesh miniapps examples tests`).
