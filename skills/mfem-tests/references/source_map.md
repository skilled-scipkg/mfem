# mfem source map: Tests

Use this map after `doc_map.md` for function-level investigation of failures.

## Fast probes
- `rg -n "check|test|test-par|RUN_MPI|MFEM_MPIEXEC" makefile config/test.mk tests/CMakeLists.txt`
- `rg -n "int main|OptionsParser|Compute.*Error|gradu_exact|curlu_exact" tests/convergence/rates.cpp tests/convergence/prates.cpp`
- `rg -n "TEST_CASE|OperatorJacobiSmoother|LOR|CG|GMRES" tests/unit/fem/test_operatorjacobismoother.cpp tests/unit/fem/test_lor.cpp tests/unit/linalg/test_chebyshev.cpp`

## Verified source entry points

### Build/test wiring
- `makefile`
- `config/test.mk`
- `tests/CMakeLists.txt`
- `tests/unit/CMakeLists.txt`
- `tests/convergence/makefile`

### Unit-test execution and high-value behavioral checks
- `tests/unit/unit_test_main.cpp`
- `tests/unit/punit_test_main.cpp`
- `tests/unit/fem/test_operatorjacobismoother.cpp`
- `tests/unit/fem/test_lor.cpp`
- `tests/unit/linalg/test_chebyshev.cpp`

### Convergence behavior checks
- `tests/convergence/rates.cpp`
- `tests/convergence/prates.cpp`

### Core solver behavior references used by tests
- `linalg/solvers.hpp`
- `linalg/solvers.cpp`

## Function-level behavior checks
- Verify smoke/regression target expansion via `makefile` + `config/test.mk` (`test-par`, `check`, launcher wiring).
- Verify convergence metric implementation in `rates.cpp`/`prates.cpp` (`gradu_exact`, `vector_u_exact`, `curlu_exact`).
- Verify solver/preconditioner behavior touched by failing tests via `OperatorJacobiSmoother`, `CGSolver`, and related symbols in unit tests and `linalg/solvers.cpp`.
