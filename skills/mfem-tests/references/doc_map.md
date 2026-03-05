# mfem documentation map: Tests

Use docs first, then source map for test-behavior internals.

## Primary test docs
- `tests/unit/README.md` - unit test executable usage, filtering, and data-path handling.
- `tests/scripts/README` - test helper scripts and local driver conventions.
- `tests/gitlab/README.md` - CI test matrix context.
- `tests/gitlab/reproduce-ci-jobs-interactively.md` - CI job reproduction workflow.
- `INSTALL` - build/test option dependencies (MPI/device/TPL).

## Test execution baseline
```bash
# smoke
make check

# unit tests
cd tests/unit
./unit_tests -l
./unit_tests "[NCMesh]"

# convergence
cd ../convergence
make rates prates -j4
./rates -m ../../data/inline-quad.mesh -sr 3 -prob 0 -o 2 -no-vis
mpirun -np 4 ./prates -m ../../data/inline-quad.mesh -sr 1 -pr 3 -prob 0 -o 2 -no-vis
```

## Validation checkpoints
- Smoke checks pass in the selected build mode.
- Targeted unit subset reports zero failures.
- Convergence examples show decreasing error with refinement.
