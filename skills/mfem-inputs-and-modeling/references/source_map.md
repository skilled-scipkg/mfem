# mfem source map: Inputs and Modeling

Use this map after `doc_map.md` to inspect parameter-to-behavior implementation.

## Fast probes
- `rg -n "AddOption|SPDESolver|GenerateRandomField|Boundary::|ComputeRationalCoefficients" miniapps/spde/generate_random_field.cpp miniapps/spde/spde_solver.hpp miniapps/spde/spde_solver.cpp`
- `rg -n "MaterialTopology|ParticleTopology|OctetTrussTopology|ComputeMetric" miniapps/spde/material_metrics.hpp miniapps/spde/material_metrics.cpp`
- `rg -n "LORSolver|BasisType|lor|AddOption|main\(" miniapps/solvers/lor_solvers.cpp miniapps/solvers/plor_solvers.cpp miniapps/solvers/lor_elast.cpp miniapps/solvers/lor_mms.hpp`

## Verified source entry points

### SPDE parameterization and solver core
- `miniapps/spde/generate_random_field.cpp`
- `miniapps/spde/spde_solver.hpp`
- `miniapps/spde/spde_solver.cpp`
- `miniapps/spde/material_metrics.hpp`
- `miniapps/spde/material_metrics.cpp`
- `miniapps/spde/transformation.hpp`
- `miniapps/spde/transformation.cpp`
- `miniapps/spde/util.hpp`
- `miniapps/spde/util.cpp`
- `miniapps/spde/visualizer.hpp`
- `miniapps/spde/visualizer.cpp`
- `miniapps/spde/makefile`

### LOR modeling and solver behavior
- `miniapps/solvers/lor_solvers.cpp`
- `miniapps/solvers/plor_solvers.cpp`
- `miniapps/solvers/lor_elast.cpp`
- `miniapps/solvers/lor_mms.hpp`
- `miniapps/tools/lor-transfer.cpp`
- `miniapps/solvers/makefile`

## Function-level behavior checks
- Map SPDE CLI options to solver setup in `generate_random_field.cpp` (`args.AddOption`, `SPDESolver` constructor, `SetupRandomFieldGenerator`, `GenerateRandomField`).
- Verify boundary handling in `Boundary::VerifyDefinedBoundaries`, `Boundary::UpdateIntegrationCoefficients`, and `Boundary::ComputeBoundaryError`.
- Verify SPDE solve path in `SPDESolver::Solve`, `SolveLinearSystem`, and `ComputeRationalCoefficients`.
- Verify LOR basis/preconditioner behavior via `LORSolver<...>` call sites in `lor_solvers.cpp`, `plor_solvers.cpp`, and `lor_elast.cpp`.
