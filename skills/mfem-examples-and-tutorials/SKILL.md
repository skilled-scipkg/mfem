---
name: mfem-examples-and-tutorials
description: This skill should be used when users ask about examples and tutorials in mfem; it prioritizes documentation references and then source inspection only for unresolved details.
---

# mfem: Examples and Tutorials

## High-Signal Playbook

### Route Conditions
- Route to `mfem-getting-started` for baseline `ex1`/`ex1p` onboarding.
- Route to `mfem-build-and-install` when example prerequisites (for example `MFEM_USE_MOONOLITH`) are unmet.
- Route to `mfem-miniapps` when the task is application miniapps rather than example codes.
- Route to `mfem-tests` for regression/convergence planning around examples.

### Triage Questions
- Is this a standard MFEM example request or Moonolith transfer workflow?
- Serial only or MPI-parallel execution?
- Is MFEM built with required feature flags (`MFEM_USE_MOONOLITH`, `MFEM_USE_MPI`)?
- Which source/destination meshes and refinement levels are intended?
- Scalar transfer, vector FE transfer, or nonconforming AMR transfer (`ex2p`)?
- Is visualization required or should runs be batch-safe (`-no-vis`)?

### Canonical Workflow
1. Confirm feature flags and compile target (`examples/moonolith/makefile`).
2. Run serial Moonolith example (`ex1`) as baseline transfer.
3. Run parallel transfer (`ex1p`) for MPI workflows.
4. Use `ex2p` when nonconforming AMR transfer behavior matters.
5. Adjust refinement/order options gradually.
6. Validate transfer quality from reported errors and successful completion.

### Minimal Working Example
```bash
cd examples/moonolith
make ex1 ex1p ex2p -j4

# serial transfer baseline
./ex1 -s ../../data/inline-tri.mesh -d ../../data/inline-quad.mesh -no-vis

# parallel transfer baseline
mpirun -np 4 ./ex1p --source_refinements 1 --dest_refinements 2 -no-vis
```

### Pitfalls And Fixes
- Build fails with explicit message when Moonolith is not enabled: rebuild with `MFEM_USE_MOONOLITH=YES`.
- Parallel examples fail without MPI-enabled MFEM: rebuild with MPI support.
- Mesh path mismatches are common when running from nested example directories.
- `ex2p` transfer quality can degrade for inconsistent source/destination FE order choices.
- Headless environments should disable visualization sockets using `-no-vis`.
- Vector FE transfer paths in parallel are marked experimental; start with scalar paths first.

### Convergence/Validation Checks
- Transfer run completes without `Transfer failed!`.
- Reported destination error decreases or stabilizes under refinement adjustments.
- Serial (`ex1`) and MPI (`ex1p`) behave consistently for equivalent meshes/options.
- Nonconforming scenario (`ex2p`) produces physically plausible transferred field and error trend.

## Scope
- Handle questions about worked examples, tutorials, and code-backed usage patterns.
- Keep responses example-centric and executable.

## Primary Documentation References
- `README.md`
- `INSTALL`
- `examples/moonolith/README.md`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for the full topic inventory.
- Use example source headers and makefiles for authoritative run options.
- If ambiguity remains after docs, inspect `references/source_map.md` and linked source entry points.
- Cite exact file paths in responses.

## Tutorials And Examples
- `examples`
- `examples/moonolith`

## Test References
- `examples/makefile`
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
- `examples/moonolith/makefile`
- `examples/moonolith/ex1.cpp`
- `examples/moonolith/ex1p.cpp`
- `examples/moonolith/ex2p.cpp`
- `examples/moonolith/example_utils.hpp`
- `fem/moonolith/transfer.hpp`
- `fem/moonolith/transfer.cpp`
- `fem/moonolith/transferutils.hpp`
- `fem/moonolith/transferutils.cpp`
- `fem/moonolith/mortarintegrator.hpp`
- `fem/moonolith/mortarassembler.cpp`
- `fem/moonolith/pmortarassembler.hpp`
- `fem/moonolith/pmortarassembler.cpp`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" config fem general linalg mesh miniapps examples`).
