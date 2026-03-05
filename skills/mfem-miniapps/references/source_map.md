# mfem source map: Miniapps

Use this map after `doc_map.md` for function-level runtime diagnosis.

## Fast probes
- `rg -n "AddOption|main\(" miniapps/diag-smoothers/*.cpp miniapps/fluids/schrodinger-flow/*.cpp miniapps/contact/*.cpp miniapps/tribol/*.cpp`
- `rg -n "AbsL1GeometricMultigrid|AssembleElementLpqJacobiDiag|OperatorJacobiSmoother" miniapps/diag-smoothers/ds-common.hpp miniapps/diag-smoothers/ds-common.cpp miniapps/diag-smoothers/abs-l1-jacobi.cpp`
- `rg -n "NavierSolver::|Step\(|PressureProject|PoissonSolve" miniapps/fluids/navier/navier_solver.hpp miniapps/fluids/navier/navier_solver.cpp`

## Verified source entry points

### Build and target wiring
- `miniapps/CMakeLists.txt`
- `miniapps/diag-smoothers/CMakeLists.txt`
- `miniapps/fluids/schrodinger-flow/CMakeLists.txt`
- `miniapps/contact/CMakeLists.txt`
- `miniapps/tribol/CMakeLists.txt`

### Diag-smoothers behavior
- `miniapps/diag-smoothers/abs-l1-jacobi.cpp`
- `miniapps/diag-smoothers/mg-abs-l1-jacobi.cpp`
- `miniapps/diag-smoothers/ds-common.hpp`
- `miniapps/diag-smoothers/ds-common.cpp`

### Fluids behavior
- `miniapps/fluids/schrodinger-flow/schrodinger_flow.cpp`
- `miniapps/fluids/schrodinger-flow/pschrodinger_flow.cpp`
- `miniapps/fluids/navier/navier_solver.hpp`
- `miniapps/fluids/navier/navier_solver.cpp`

### Contact and Tribol behavior
- `miniapps/contact/contact.cpp`
- `miniapps/contact/elastoperator.hpp`
- `miniapps/contact/elastoperator.cpp`
- `miniapps/contact/optcontactproblem.hpp`
- `miniapps/contact/optcontactproblem.cpp`
- `miniapps/contact/solver_utils.hpp`
- `miniapps/contact/solver_utils.cpp`
- `miniapps/contact/makefile`
- `miniapps/contact/tribol.cmake`
- `miniapps/tribol/contact-patch-test.cpp`
- `miniapps/tribol/makefile`

## Function-level behavior checks
- Verify CLI option handling in each miniapp via `args.AddOption` blocks.
- Trace smoothing behavior through `AbsL1GeometricMultigrid` and `AssembleElementLpqJacobiDiag` for diag-smoothers.
- Trace flow-step behavior through `NavierSolver::Setup`, `NavierSolver::Step`, and `NavierSolver::PressureProject`.
- Trace contact assembly/solve flow through `elastoperator*`, `optcontactproblem*`, and `solver_utils*` call sites.
