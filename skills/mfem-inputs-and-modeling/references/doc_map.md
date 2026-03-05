# mfem documentation map: Inputs and Modeling

Use docs first for parameter semantics, then source for implementation details.

## Primary docs
- `miniapps/spde/README.md` - SPDE model, parameter meanings, and run options.
- `miniapps/solvers/lor_docs.md` - LOR basis/solver guidance and spectral-equivalence notes.
- `miniapps/solvers/README` - solver miniapp context and executable targets.

## Supporting docs
- `INSTALL` - backend/MPI prerequisites for modeling workflows.
- `README.md` - top-level project context.

## Minimal runnable commands
```bash
# SPDE random field (MPI)
cd miniapps/spde
make generate_random_field -j4
mpirun -np 4 ./generate_random_field -o 1 -r 3 -rp 3 -nu 2 -l1 0.02 -l2 0.02 -l3 0.02 -s 0.01 -t 0.08 -top 1 -rs -no-vis

# LOR solver baseline
cd ../solvers
make lor_solvers -j4
./lor_solvers -m ../../data/star.mesh -o 2 -r 1 -fe h1 -no-vis
```

## Validation checkpoints
- SPDE run exports expected fields and completes without solver abort.
- LOR run converges and reports stable residual/iteration behavior.
- Refinement or order changes produce consistent qualitative trends.
