---
name: mfem-miniapps
description: This skill should be used when users ask about miniapps in mfem; it prioritizes documentation references and then source inspection only for unresolved details.
---

# mfem: Miniapps

## High-Signal Playbook

### Route Conditions
- Route to `mfem-build-and-install` for compile-time toggles and TPL enablement issues.
- Route to `mfem-inputs-and-modeling` for parameter-model design (SPDE covariance, LOR modeling choices).
- Route to `mfem-tests` for cross-miniapp validation and convergence/regression checks.
- Route to `mfem-getting-started` for first MFEM execution before miniapp-specific work.

### Triage Questions
- Which miniapp family is relevant (solvers, fluids, electromagnetics, contact/tribol, SPDE, etc.)?
- Is the target run serial, MPI, or MPI+device?
- Which optional dependencies are required (`MFEM_USE_TRIBOL`, `MFEM_USE_GSLIB`, etc.)?
- What mesh/data files are needed and where are they located?
- Should visualization be interactive (`GLVis`) or disabled (`-no-vis`) for batch runs?
- What does success mean: run completion, field output quality, equilibrium constraints, or convergence trend?

### Canonical Workflow
1. Identify the correct miniapp directory and read its README first.
2. Confirm compile-time gates from miniapp CMake/make files.
3. Build the miniapp executable.
4. Run a known-good command (prefer `-no-vis` first).
5. Add problem-specific options (mesh/refinements/device/TPL flags).
6. Validate outputs/constraints (and only then optimize/tune).
7. Escalate to source entry points if option behavior is unclear.

### Minimal Working Example
```bash
# serial fluid miniapp
cd miniapps/fluids/schrodinger-flow
make schrodinger_flow -j4
./schrodinger_flow -lf -nx 8 -ny 8 -ms 32 -no-vis

# mpi diag-smoothers miniapp
cd ../../diag-smoothers
make abs-l1-jacobi -j4
mpirun -np 4 ./abs-l1-jacobi -m ../../data/ref-cube.mesh -rs 2 -rp 2 -s 1 -i 1 -a 3 -pc 1 -no-mon -no-vis
```

### Pitfalls And Fixes
- Many miniapps are MPI-only; serial builds cannot run them.
- Contact/Tribol miniapps require `MFEM_USE_TRIBOL=YES` and external Axom/Tribol setup.
- Missing or wrong relative mesh paths are common in subdirectory runs.
- Interactive visualization can stall CI/batch jobs; use `-no-vis`.
- Runtime backend flags (`-d ...`) fail when backend support was not compiled.
- CMake users may forget `MFEM_ENABLE_MINIAPPS` or explicit `miniapps` target builds.

### Convergence/Validation Checks
- Start from maintained test-like option sets in miniapp `CMakeLists.txt` entries.
- Re-run with increased refinement/order to check stability of qualitative outputs.
- For contact patch workflows, confirm equilibrium/contact-gap checks reported by the app.
- For SPDE/fluids workflows, confirm expected output fields and post-process artifacts are produced.

## Scope
- Handle questions about selecting, building, and running MFEM miniapps.
- Keep responses concise and workflow-oriented; go to source only when docs are insufficient.

## Primary Documentation References
- `miniapps/diag-smoothers/README.md`
- `miniapps/fluids/schrodinger-flow/README.md`
- `miniapps/solvers/README`
- `miniapps/spde/README.md`
- `miniapps/contact/README.md`
- `miniapps/tribol/README.md`
- `INSTALL`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for full topic docs.
- Use miniapp CMake/make test invocations as known-good run patterns.
- If ambiguity remains after docs, inspect `references/source_map.md` and source entry points.
- Cite exact file paths in responses.

## Tutorials And Examples
- `miniapps`
- `examples`

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

## Source Entry Points For Unresolved Issues
- `miniapps/CMakeLists.txt`
- `miniapps/diag-smoothers/CMakeLists.txt`
- `miniapps/fluids/schrodinger-flow/CMakeLists.txt`
- `miniapps/solvers/CMakeLists.txt`
- `miniapps/electromagnetics/CMakeLists.txt`
- `miniapps/spde/CMakeLists.txt`
- `miniapps/contact/CMakeLists.txt`
- `miniapps/tribol/CMakeLists.txt`
- `miniapps/diag-smoothers/abs-l1-jacobi.cpp`
- `miniapps/diag-smoothers/mg-abs-l1-jacobi.cpp`
- `miniapps/diag-smoothers/ds-common.hpp`
- `miniapps/fluids/schrodinger-flow/schrodinger_flow.cpp`
- `miniapps/fluids/schrodinger-flow/schrodinger_flow.hpp`
- `miniapps/fluids/schrodinger-flow/pschrodinger_flow.cpp`
- `miniapps/spde/generate_random_field.cpp`
- `miniapps/fluids/navier/navier_solver.cpp`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" config fem general linalg mesh miniapps`).
