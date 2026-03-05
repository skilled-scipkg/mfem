---
name: mfem-tests
description: This skill should be used when users ask about tests in mfem; it prioritizes documentation references and then source inspection only for unresolved details.
---

# mfem: Tests

## High-Signal Playbook

### Route Conditions
- Route to `mfem-build-and-install` when failures are clearly build/configuration issues.
- Route to `mfem-getting-started` for first-run smoke checks (`ex1`/`ex1p`) before broader testing.
- Route to `mfem-miniapps` for physics-specific runtime behavior questions.
- Route to `mfem-inputs-and-modeling` when failures depend on modeling parameters (SPDE/LOR setup).

### Triage Questions
- Is the goal smoke check, full regression, convergence study, or CI-job reproduction?
- Should validation cover serial, MPI, GPU/HIP, or all configured modes?
- Do you need examples-based checks or miniapp-based checks?
- Are you running from make-based or CMake-based build trees?
- Do you need targeted tests (by tag/test name) or full-suite execution?
- Do any tests require external data (`[MFEMData]`) or external TPLs?

### Canonical Workflow
1. Confirm the active build configuration (`make info` or CMake cache).
2. Run smoke validation first (`make check` / CMake `check`).
3. Choose **examples** when validating baseline solver/device/MPI behavior.
4. Choose **miniapps** when validating application-level workflows or TPL couplings.
5. Run unit tests with focused tags/names before full sweeps.
6. Run convergence tools (`rates`/`prates`) when numerical order or discretization quality matters.
7. Escalate to CI-reproduction scripts only when local workflows are insufficient.

### Minimal Working Example
```bash
# repo root smoke checks
make check
make -C examples test-print

# unit test focus
cd tests/unit
./unit_tests -l
./unit_tests "[NCMesh]"

# convergence check
cd ../convergence
make rates prates -j4
./rates -m ../../data/inline-quad.mesh -sr 3 -prob 0 -o 2 -no-vis
mpirun -np 4 ./prates -m ../../data/inline-quad.mesh -sr 1 -pr 3 -prob 0 -o 2 -no-vis
```

### Pitfalls And Fixes
- Parallel test targets fail when built without MPI: rebuild with `MFEM_USE_MPI=YES`.
- Headless runs fail due visualization sockets: use `-no-vis` for batch tests.
- MPI launcher mismatch on clusters: set `MFEM_MPIEXEC` and `MFEM_MPIEXEC_NP`.
- Unit tests tagged `[MFEMData]` are skipped unless `--data <mfem-data-path>` is provided.
- CI helper scripts are environment-specific (especially LLNL workflows); default to local `make check/test` when outside those systems.
- Comparing noisy runs without fixed random controls can hide regressions; prefer deterministic options where available.

### Convergence/Validation Checks
- Smoke target passes (`make check` or CMake `check`).
- Unit test summary reports zero failing assertions for the selected subset.
- Convergence studies show error decrease with refinement and plausible rates (`rates`/`prates`).
- Examples and miniapps selected for validation execute with the same backend/MPI mode as production intent.
- Contact patch workflows (Tribol) validate equilibrium/constraint behavior as documented.

## Scope
- Handle questions about validation strategy, unit/regression/convergence testing, and reproducible execution patterns.
- Keep responses practical and command-oriented; avoid exhaustive internals unless requested.

## Primary Documentation References
- `tests/unit/README.md`
- `tests/scripts/README`
- `tests/gitlab/README.md`
- `tests/gitlab/reproduce-ci-jobs-interactively.md`
- `INSTALL`
- `README.md`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md`.
- Use example/miniapp test targets for runnable validation patterns.
- If ambiguity remains after docs, inspect `references/source_map.md` and source entry points.
- Cite exact file paths in responses.

## Choosing Examples Vs Miniapps
- Use `examples` for core API/solver/device/MPI sanity checks (broad and fast).
- Use `miniapps` for application-specific behavior, TPL coupling, and physics workflow validation.
- Use `tests/convergence` when the question is about discretization error trends/rates.

## Common Runtime Option Patterns
- Visualization control: `-vis` / `-no-vis`.
- MPI execution wrapper: `${MFEM_MPIEXEC} ${MFEM_MPIEXEC_NP} ${MFEM_MPI_NP}` (make workflows).
- Refinement/order controls often appear as `-r`, `-rp`, `-sr`, `-pr`, `-o`.
- Device/backend selection is typically `-d <backend-string>`.

## Optional Deeper Inspection
- `config`
- `fem`
- `general`
- `linalg`
- `mesh`
- `miniapps`
- `examples`
- `tests`

## Source Entry Points For Unresolved Issues
- `makefile`
- `config/test.mk`
- `examples/makefile`
- `tests/unit/README.md`
- `tests/unit/CMakeLists.txt`
- `tests/convergence/makefile`
- `tests/convergence/rates.cpp`
- `tests/convergence/prates.cpp`
- `miniapps/solvers/CMakeLists.txt`
- `miniapps/electromagnetics/CMakeLists.txt`
- `miniapps/diag-smoothers/CMakeLists.txt`
- `linalg/solvers.hpp`
- `linalg/solvers.cpp`
- `linalg/filteredsolver.hpp`
- `linalg/filteredsolver.cpp`
- `linalg/amgxsolver.hpp`
- `linalg/amgxsolver.cpp`
- `mesh/vtk.hpp`
- `mesh/wedge.hpp`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" config fem general linalg mesh miniapps examples tests`).
