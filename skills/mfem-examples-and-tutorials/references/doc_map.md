# mfem documentation map: Examples and Tutorials

Use docs first. This skill currently centers on Moonolith transfer examples.

## Primary docs
- `examples/moonolith/README.md` - transfer examples, options, and expected workflows.
- `INSTALL` - Moonolith build prerequisites and enabling flags.
- `README.md` - global project context and entry points.

## Supplemental tutorial path
- `examples/jupyter/README.md` - interactive tutorial workflow when notebooks are preferred.

## Minimal runnable commands
```bash
cd examples/moonolith
make ex1 ex1p ex2p -j4

./ex1 -s ../../data/inline-tri.mesh -d ../../data/inline-quad.mesh -no-vis
mpirun -np 4 ./ex1p --source_refinements 1 --dest_refinements 2 -no-vis
mpirun -np 4 ./ex2p -s ../../data/star.mesh -d ../../data/star.mesh -no-vis
```

## Validation checkpoints
- Runs complete without `Transfer failed!`.
- Error output remains stable or improves under refinement.
- Serial and MPI transfer behavior is consistent on equivalent setups.
