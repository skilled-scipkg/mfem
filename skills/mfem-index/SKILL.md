---
name: mfem-index
description: Route MFEM requests to the correct topic skill before deep inspection. Use this skill when the user request spans build/configuration, first-run simulation setup, miniapps, modeling parameters, examples, or testing/validation workflows.
---

# mfem Skills Index

## Route the request
- Build, installation, compiler/toolchain setup, MPI/GPU/TPL enablement, and CI-style environment setup -> `mfem-build-and-install`.
- First successful run (serial/MPI), mesh/data path setup, and baseline visualization path -> `mfem-getting-started`.
- Worked examples and tutorial-style transfers (especially Moonolith examples) -> `mfem-examples-and-tutorials`.
- Miniapp selection and physics-specific execution flows (fluids, contact, EM, SPDE, solvers) -> `mfem-miniapps`.
- Modeling/parameterization questions (SPDE Matérn parameters, LOR basis/solver setup) -> `mfem-inputs-and-modeling`.
- Validation/regression/convergence strategy, test commands, and pass/fail interpretation -> `mfem-tests`.
- Python bindings (`PyMFEM`) -> out-of-repo; route to PyMFEM docs.

## Docs-First Protocol (Required)
1. Start in the routed skill's **Primary documentation references**.
2. If details are missing, open that skill's `references/doc_map.md`.
3. Use source only after docs: open that skill's `references/source_map.md`.
4. If the request spans multiple domains, use `references/doc_map.md` in this index skill to sequence the handoff.

## Cross-Skill Triage
- Build fails before executable exists -> `mfem-build-and-install`.
- Executable exists but first run fails on mesh/path/launcher -> `mfem-getting-started`.
- Baseline works but application miniapp or domain behavior fails -> `mfem-miniapps` or `mfem-inputs-and-modeling`.
- Numerical trend/regression unclear -> `mfem-tests`.

## Local References
- `references/doc_map.md`
- `references/source_map.md`
