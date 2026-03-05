# mfem source map: Getting Started

Use this map after `doc_map.md` when run behavior is unclear.

## Fast probes
- `rg -n "AddOption|Device device|Mesh mesh|ParMesh|FormLinearSystem|PCG|CG|HypreBoomerAMG" examples/ex1.cpp examples/ex1p.cpp`
- `rg -n "ex1|ex1p|check|test-print" examples/makefile makefile config/test.mk`
- `rg -n "class CGSolver|void PCG|OperatorJacobiSmoother|UMFPackSolver" linalg/solvers.hpp linalg/solvers.cpp`

## Verified source entry points

### First-run option parsing and baseline solve path
- `examples/ex1.cpp` - serial option parsing, assembly, and solve workflow.
- `examples/ex1p.cpp` - MPI/HYPRE initialization and parallel solve workflow.
- `examples/makefile` - executable targets and example test hooks.

### Root-level build/test wiring for startup flows
- `makefile` - top-level target orchestration.
- `config/test.mk` - smoke test definitions used by `make check` and `make test`.

### Solver internals commonly touched by startup options
- `linalg/solvers.hpp` - solver interfaces used by examples.
- `linalg/solvers.cpp` - implementations for `CG`, `PCG`, and smoothing paths.

## Function-level behavior checks
- Verify option-to-behavior mapping in `examples/ex1.cpp` and `examples/ex1p.cpp` by searching `args.AddOption`, `Device`, and `FormLinearSystem`.
- Confirm selected iterative solver path by searching `PCG(`, `CG(`, and `OperatorJacobiSmoother` in `examples/ex1.cpp`, `examples/ex1p.cpp`, and `linalg/solvers.cpp`.
- Confirm startup test target wiring with `rg -n "check|test-print|ex1|ex1p" makefile examples/makefile config/test.mk`.
