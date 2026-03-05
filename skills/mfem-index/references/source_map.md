# mfem source map: Index

Use this map only after reading `doc_map.md` and routing to a topic skill.

## Fast probes
- `rg -n "MFEM_USE_(MPI|CUDA|HIP|RAJA|OCCA|CEED)|MFEM_FETCH_TPLS|MFEM_ENABLE_MINIAPPS" CMakeLists.txt config/defaults.cmake config/defaults.mk`
- `rg -n "int main\(" examples/ex1.cpp examples/ex1p.cpp miniapps/spde/generate_random_field.cpp`
- `rg -n "check|test|test-par" makefile config/test.mk tests/CMakeLists.txt`

## Cross-domain entry points
- `CMakeLists.txt` - global feature gates and target registration.
- `makefile` - top-level build/test workflow entry.
- `config/defaults.cmake` - CMake default options.
- `config/defaults.mk` - make default options.
- `config/test.mk` - test target orchestration and MPI launcher plumbing.
- `examples/ex1.cpp` - baseline serial simulation behavior.
- `examples/ex1p.cpp` - baseline MPI simulation behavior.
- `miniapps/spde/generate_random_field.cpp` - advanced simulation parameterization path.
- `tests/convergence/rates.cpp` - convergence/accuracy behavior checks.

## Escalation rule
If this map identifies a domain-specific failure point, switch to that topic skill source map for detailed function-level checks.
