# mfem documentation map: Getting Started

Use docs first, then source map only for unresolved behavior.

## Core onboarding docs
- `README.md` - project overview and first-entry context.
- `INSTALL` - build prerequisites and run-mode toggles.
- `doc/README` - where to find additional documentation.
- `tests/unit/README.md` - quick unit-test execution and filtering.

## Practical run references
- `tests/scripts/README` - test driver conventions and execution wrappers.
- `examples/jupyter/README.md` - notebook path for interactive onboarding.

## Minimal startup workflow
```bash
# from repo root
make serial -j4
make -C examples ex1 -j4
./examples/ex1 -m data/star.mesh -no-vis

# MPI variant (if built with MPI)
make parallel -j4
make -C examples ex1p -j4
mpirun -np 4 ./examples/ex1p -m data/star.mesh -no-vis
```

## Validation checkpoints
- `ex1` runs successfully on a known mesh.
- `ex1p` runs successfully when MPI build is enabled.
- `make check` passes in the current mode.
