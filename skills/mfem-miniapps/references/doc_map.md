# mfem documentation map: Miniapps

Use docs first, then inspect source for unresolved runtime behavior.

## Core miniapp docs
- `miniapps/diag-smoothers/README.md` - smoothers miniapp options and theory context.
- `miniapps/fluids/schrodinger-flow/README.md` - Schrödinger flow workflow and options.
- `miniapps/solvers/README` - solver miniapps overview.
- `miniapps/spde/README.md` - SPDE miniapp usage and parameter meaning.
- `miniapps/contact/README.md` - contact miniapp setup and usage.
- `miniapps/tribol/README.md` - Tribol-coupled miniapp setup and commands.
- `INSTALL` - dependency and backend requirements.

## Minimal startup commands
```bash
# diag-smoothers
cd miniapps/diag-smoothers
make abs-l1-jacobi -j4
mpirun -np 4 ./abs-l1-jacobi -m ../../data/ref-cube.mesh -rs 2 -rp 2 -s 1 -i 1 -a 3 -pc 1 -no-mon -no-vis

# schrodinger flow
cd ../fluids/schrodinger-flow
make schrodinger_flow -j4
./schrodinger_flow -lf -nx 8 -ny 8 -ms 32 -no-vis
```

## Validation checkpoints
- Executables start and finish without runtime aborts.
- Output fields/log summaries are generated as expected by each miniapp.
- Refined runs preserve qualitative behavior.
