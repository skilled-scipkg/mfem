# mfem documentation map: Build and Install

Use docs in this order before reading source.

## Core build references
- `README.md` - repository layout and top-level build entry points.
- `INSTALL` - canonical make/CMake instructions, backend toggles, and TPL notes.
- `CMakeLists.txt` - supported configure flags and target layout.
- `makefile` - make targets (`serial`, `parallel`, `check`, `test`, `install`).
- `config/defaults.mk` - make defaults for MPI/device/TPL options.
- `config/defaults.cmake` - CMake defaults for MPI/device/TPL options.

## Validation and CI-style execution references
- `config/test.mk` - shared test macros and launcher behavior.
- `tests/gitlab/README.md` - CI pipeline context and job patterns.
- `tests/gitlab/reproduce-ci-jobs-interactively.md` - local CI reproduction workflow.

## Optional dependency references
- `miniapps/contact/README.md` - contact miniapp dependency/setup notes.
- `miniapps/tribol/README.md` - Tribol dependency and build requirements.
- `examples/jupyter/README.md` - notebook-focused setup path.

## Minimal command set
```bash
# make path
make serial -j4
make -C examples ex1 -j4
./examples/ex1 -m data/star.mesh -no-vis
make check

# CMake path
cmake -S . -B build -DMFEM_USE_MPI=YES -DMFEM_FETCH_TPLS=YES
cmake --build build -j4
ctest --test-dir build --output-on-failure
```

## Build validation checkpoints
- Build succeeds with intended toggles (`MFEM_USE_MPI`, backend flags, TPL flags).
- At least one serial smoke run completes (for example: run `ex1` on `data/star.mesh` with `-no-vis`).
- `make check` or `ctest` passes in the active configuration.
