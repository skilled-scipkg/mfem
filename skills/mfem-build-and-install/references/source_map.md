# mfem source map: Build and Install

Use this map after `doc_map.md` when docs are insufficient.

## Fast probes
- `rg -n "MFEM_USE_(MPI|CUDA|HIP|RAJA|OCCA|CEED|METIS)|MFEM_FETCH_TPLS|MFEM_ENABLE_MINIAPPS" CMakeLists.txt config/defaults.cmake config/defaults.mk`
- `rg -n "MFEM_MPIEXEC|MFEM_MPIEXEC_NP|test-par|check" makefile config/defaults.mk config/test.mk CMakeLists.txt`
- `rg -n "find_package|HYPRE|METIS|GSLIB" config/cmake/modules/*.cmake`

## Verified source entry points

### Build orchestration and option gates
- `CMakeLists.txt` - top-level option handling and target registration.
- `makefile` - make-based build/install/test flow.
- `config/defaults.cmake` - CMake option defaults.
- `config/defaults.mk` - make option defaults.
- `config/test.mk` - common smoke/regression test macros.

### TPL/backend discovery and helper logic
- `config/cmake/modules/MfemCmakeUtilities.cmake` - shared CMake helper functions.
- `config/cmake/modules/FindHYPRE.cmake` - HYPRE discovery and integration checks.
- `config/cmake/modules/FindMETIS.cmake` - METIS detection and link setup.
- `config/cmake/modules/FindGSLIB.cmake` - GSLIB detection and link setup.
- `miniapps/contact/tribol.cmake` - Tribol/Axom-specific CMake wiring.

### Target activation boundaries
- `examples/CMakeLists.txt` - examples target enablement.
- `miniapps/CMakeLists.txt` - miniapps target enablement.
- `tests/CMakeLists.txt` - tests target enablement.

## Function-level behavior checks
- Check feature-gated branches and target creation points in `CMakeLists.txt` with:
  `rg -n "add_mfem_target|MFEM_ENABLE_MINIAPPS|MFEM_USE_MPI|MFEM_USE_CUDA|MFEM_USE_HIP" CMakeLists.txt`.
- Check launcher propagation into tests with:
  `rg -n "MFEM_MPIEXEC|MFEM_MPIEXEC_NP|RUN_MPI" makefile config/defaults.mk config/test.mk`.
- Check TPL detection behavior with:
  `rg -n "find_package|set\(|if \(" config/cmake/modules/FindHYPRE.cmake config/cmake/modules/FindMETIS.cmake config/cmake/modules/FindGSLIB.cmake`.
