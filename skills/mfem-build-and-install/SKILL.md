---
name: mfem-build-and-install
description: This skill should be used when users ask about build and install in mfem; it prioritizes documentation references and then source inspection only for unresolved details.
---

# mfem: Build and Install

## High-Signal Playbook

### Route Conditions
- Route to `mfem-getting-started` after build succeeds and the user needs first serial/MPI runs.
- Route to `mfem-miniapps` for miniapp-specific runtime options and physics workflows.
- Route to `mfem-tests` for regression, convergence, or CI-equivalent validation strategy.

### Triage Questions
- Are you using GNU make or CMake?
- Do you need serial only, MPI, GPU, or hybrid MPI+GPU?
- Which optional backends are required (`CUDA`, `HIP`, `RAJA`, `OCCA`, `CEED`)?
- Are TPLs preinstalled, or should CMake fetch them (`MFEM_FETCH_TPLS`)?
- Do you need installed artifacts (`make install` / `cmake --target install`) for downstream builds?
- Are you building just the library, or also examples/miniapps/tests?
- What launcher should tests use (`mpirun` vs `srun` via `MFEM_MPIEXEC*`)?

### Canonical Workflow
1. Choose build system (`make` or `CMake`) and keep build dirs separate by configuration.
2. Build a baseline library (`make serial` or `cmake .. && cmake --build .`).
3. Enable MPI explicitly (`make parallel` or `-DMFEM_USE_MPI=YES`).
4. Enable one device backend path at a time (`MFEM_USE_CUDA` or `MFEM_USE_HIP`).
5. Build executables (`make all` or CMake target `exec`) when examples/miniapps are needed.
6. Run smoke validation (`make check` / CMake `check`).
7. Run broader validation only after smoke passes (`make test` / CMake `test`).
8. Install when downstream reuse is needed (`make install` or CMake `install`).

### Minimal Working Example
```bash
# GNU make baseline (from repo root)
make serial -j4
make -C examples ex1 -j4
./examples/ex1 -m data/star.mesh -no-vis
make check

# CMake MPI + fetched core TPLs
mkdir -p build && cd build
cmake .. -DMFEM_USE_MPI=YES -DMFEM_FETCH_TPLS=YES
cmake --build . -j4
cmake --build . --target check
```

### Pitfalls And Fixes
- Parallel runs fail in many examples/miniapps when `MFEM_USE_METIS=NO` with MPI: enable METIS for non-Cartesian partitioning.
- CUDA with CMake fails on old CMake: MFEM requires CMake >= 3.17 when `MFEM_USE_CUDA=YES`.
- CUDA and HIP are mutually exclusive in one CMake configure: build separate trees.
- HIP builds fail to find toolchain: set `ROCM_PATH` and ensure `$ROCM_PATH/bin` is in `PATH`.
- RAJA/OCCA CUDA usage fails unless MFEM CUDA support is enabled as documented.
- Batch systems fail with default launcher: set `MFEM_MPIEXEC` and `MFEM_MPIEXEC_NP` (for example `srun` and `-n`).
- Tribol/Moonolith miniapps do not build by default: enable `MFEM_USE_TRIBOL` / `MFEM_USE_MOONOLITH` and required deps.
- Downstream builds cannot locate MFEM if not installed: run `make install` or CMake `install` and use installed config files.

### Convergence/Validation Checks
- `make info` or CMake cache confirms intended toggles (`MFEM_USE_MPI`, backend flags, TPL flags).
- `make check`/`check` passes (`ex1` for serial or `ex1p` for MPI builds).
- A no-visualization smoke run succeeds (`-no-vis`) before interactive GLVis usage.
- `make test` (or CMake `test`) passes required subset after smoke checks.
- Installed tree contains expected outputs: library, headers, and MFEM config files.

## Scope
- Handle questions about build, installation, compilation, and environment setup.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary Documentation References
- `README.md`
- `INSTALL`
- `CMakeLists.txt`
- `config/defaults.mk`
- `config/defaults.cmake`
- `tests/gitlab/README.md`
- `tests/gitlab/reproduce-ci-jobs-interactively.md`
- `examples/jupyter/README.md`
- `miniapps/contact/README.md`
- `miniapps/tribol/README.md`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for the complete topic document list.
- Use examples/tests as executable validation patterns.
- If ambiguity remains after docs, inspect `references/source_map.md` and start with ranked source entry points.
- Cite exact documentation file paths in responses.

## Tutorials And Examples
- `examples`
- `miniapps`

## Test References
- `tests`
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
- `makefile`
- `CMakeLists.txt`
- `config/defaults.mk`
- `config/defaults.cmake`
- `config/test.mk`
- `miniapps/CMakeLists.txt`
- `examples/CMakeLists.txt`
- `tests/CMakeLists.txt`
- `miniapps/hdiv-linear-solver/CMakeLists.txt`
- `miniapps/hdiv-linear-solver/makefile`
- `miniapps/solvers/CMakeLists.txt`
- `miniapps/toys/CMakeLists.txt`
- `miniapps/spde/CMakeLists.txt`
- `miniapps/plasma/CMakeLists.txt`
- `miniapps/electromagnetics/CMakeLists.txt`
- `miniapps/contact/CMakeLists.txt`
- `miniapps/tribol/CMakeLists.txt`
- `miniapps/contact/tribol.cmake`
- `config/cmake/modules/FindHYPRE.cmake`
- `config/cmake/modules/FindMETIS.cmake`
- `config/cmake/modules/FindGSLIB.cmake`
- `config/cmake/modules/MfemCmakeUtilities.cmake`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" config fem general linalg mesh miniapps examples tests`).
