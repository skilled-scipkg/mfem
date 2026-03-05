---
name: mfem-inputs-and-modeling
description: This skill should be used when users ask about inputs and modeling in mfem; it prioritizes documentation references and then source inspection only for unresolved details.
---

# mfem: Inputs and Modeling

## High-Signal Playbook

### Route Conditions
- Route to `mfem-miniapps` for full run orchestration and miniapp selection.
- Route to `mfem-build-and-install` when required backends/TPLs are missing.
- Route to `mfem-tests` for convergence/regression checks of modeling choices.
- Route to `mfem-getting-started` for baseline MFEM usage before advanced model setup.

### Triage Questions
- Is the user modeling SPDE random fields or configuring LOR preconditioners?
- Is MPI available (required for the SPDE miniapp)?
- What mesh and dimensionality are being used?
- Which Matérn/SPDE parameters are controlled (`-nu`, `-l1..-l3`, `-e1..-e3`, `-s`, `-t`)?
- Is topology set to particles (`-top 0`) or octet-truss (`-top 1`)?
- Should randomness be fixed (`-rs`) for reproducibility checks?
- For LOR: are basis types and space choices aligned with documented spectral-equivalence guidance?

### Canonical Workflow
1. Select model family: SPDE-based random field vs LOR solver/discretization setup.
2. Read model-specific docs for parameter meaning and constraints.
3. Run a documented baseline case with explicit mesh/options.
4. Check outputs (GLVis/ParaView fields for SPDE, solver behavior for LOR).
5. Tune a small set of dominant parameters (refinement/order/covariance scales).
6. Validate with refinement or repeated-seed checks before broad sweeps.

### Minimal Working Example
```bash
# SPDE random-field baseline (MPI)
cd miniapps/spde
make generate_random_field -j4
mpirun -np 4 ./generate_random_field -o 1 -r 3 -rp 3 -nu 2 \
  -l1 0.02 -l2 0.02 -l3 0.02 -s 0.01 -t 0.08 -top 1 -rs
```

```cpp
// LOR one-liner from miniapps/solvers/lor_docs.md
LORSolver<UMFPackSolver> prec(a, ess_tdof_list);
```

### Pitfalls And Fixes
- SPDE miniapp does not work in non-MPI builds: build MFEM with MPI enabled.
- Uniform-random transform mode (`-urf`) without sensible `-umin/-umax` can produce poor scaling.
- Anisotropy parameters (`-l*`, `-e*`) can overcondition/undercondition fields if chosen inconsistently.
- LOR spectral equivalence degrades when basis choices do not follow the documented requirements.
- Non-reproducible comparisons occur when random seeding controls are not fixed.

### Convergence/Validation Checks
- Increase refinement/order and verify field/statistical behavior is stable.
- Confirm expected exported SPDE fields (support, perturbation, topology, level set) are generated.
- Compare runs with fixed/random seeds to separate stochastic variability from numerical instability.
- For LOR, verify solver iterations/residual trends stay reasonable versus non-LOR baselines.

## Scope
- Handle questions about model parameterization, input controls, and mathematically meaningful runtime settings.
- Keep responses tied to documented options and representative examples.

## Primary Documentation References
- `miniapps/spde/README.md`
- `miniapps/solvers/lor_docs.md`
- `miniapps/solvers/README`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for complete topic docs.
- Use sample commands/snippets from docs as baseline templates.
- If ambiguity remains after docs, inspect `references/source_map.md` and source entry points.
- Cite exact documentation file paths in responses.

## Tutorials And Examples
- `miniapps/spde`
- `miniapps/solvers`
- `examples/ex33.cpp`
- `examples/ex33p.cpp`

## Test References
- `tests/convergence`
- `mfem-tests` skill for broader validation execution

## Optional Deeper Inspection
- `config`
- `fem`
- `general`
- `linalg`
- `mesh`
- `miniapps`

## Source Entry Points For Unresolved Issues
- `miniapps/spde/generate_random_field.cpp`
- `miniapps/spde/spde_solver.cpp`
- `miniapps/spde/spde_solver.hpp`
- `miniapps/spde/material_metrics.cpp`
- `miniapps/spde/material_metrics.hpp`
- `miniapps/spde/transformation.cpp`
- `miniapps/spde/util.cpp`
- `miniapps/spde/visualizer.cpp`
- `miniapps/solvers/lor_docs.md`
- `miniapps/solvers/lor_solvers.cpp`
- `miniapps/solvers/plor_solvers.cpp`
- `miniapps/solvers/lor_elast.cpp`
- `miniapps/solvers/lor_mms.hpp`
- `miniapps/tools/lor-transfer.cpp`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" config fem general linalg mesh miniapps`).
