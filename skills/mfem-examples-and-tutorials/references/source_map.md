# mfem source map: Examples and Tutorials

Use this map after `doc_map.md` when transfer behavior needs source-level checks.

## Fast probes
- `rg -n "AddOption|Transfer\(|ComputeL2Error|Transfer failed|main\(" examples/moonolith/ex1.cpp examples/moonolith/ex1p.cpp examples/moonolith/ex2p.cpp`
- `rg -n "class (MortarAssembler|ParMortarAssembler)|Assemble\(|Transfer\(|Apply\(|Update\(" fem/moonolith/mortarassembler.hpp fem/moonolith/mortarassembler.cpp fem/moonolith/pmortarassembler.hpp fem/moonolith/pmortarassembler.cpp`
- `rg -n "class .*MortarIntegrator|AssembleElementMatrix" fem/moonolith/mortarintegrator.hpp fem/moonolith/mortarintegrator.cpp`

## Verified source entry points

### Example driver behavior
- `examples/moonolith/ex1.cpp` - serial transfer driver and option parsing.
- `examples/moonolith/ex1p.cpp` - parallel transfer driver and MPI path.
- `examples/moonolith/ex2p.cpp` - nonconforming transfer path and diagnostics.
- `examples/moonolith/example_utils.hpp` - shared helper routines used by examples.
- `examples/moonolith/makefile` - build targets and test-style invocations.
- `examples/moonolith/CMakeLists.txt` - CMake target wiring for Moonolith examples.

### Transfer core implementation
- `fem/moonolith/transfer.hpp`
- `fem/moonolith/transfer.cpp`
- `fem/moonolith/transferutils.hpp`
- `fem/moonolith/transferutils.cpp`
- `fem/moonolith/mortarassembler.hpp`
- `fem/moonolith/mortarassembler.cpp`
- `fem/moonolith/pmortarassembler.hpp`
- `fem/moonolith/pmortarassembler.cpp`
- `fem/moonolith/mortarintegrator.hpp`
- `fem/moonolith/mortarintegrator.cpp`

## Function-level behavior checks
- Check driver option-to-action mapping in `ex1.cpp`, `ex1p.cpp`, and `ex2p.cpp` via `args.AddOption` and `assembler.Transfer` call sites.
- Check transfer assembly/update/apply lifecycle via `MortarAssembler::Assemble`, `Transfer`, `Apply`, and `Update`.
- Check integrator matrix behavior via `L2MortarIntegrator::AssembleElementMatrix` and related vector variants.
