# mfem documentation map: Index

Use this index map to choose the right topic skill before reading source.

## Top-level docs for global context
- `README.md` - project overview, high-level capabilities, and quick links.
- `INSTALL` - authoritative build/backends/TPL configuration guide.
- `doc/README` - documentation organization and navigation.

## Topic skill handoff map
- `mfem-build-and-install` -> start at `skills/mfem-build-and-install/references/doc_map.md`.
- `mfem-getting-started` -> start at `skills/mfem-getting-started/references/doc_map.md`.
- `mfem-examples-and-tutorials` -> start at `skills/mfem-examples-and-tutorials/references/doc_map.md`.
- `mfem-miniapps` -> start at `skills/mfem-miniapps/references/doc_map.md`.
- `mfem-inputs-and-modeling` -> start at `skills/mfem-inputs-and-modeling/references/doc_map.md`.
- `mfem-tests` -> start at `skills/mfem-tests/references/doc_map.md`.

## Minimal routing smoke path
```bash
# 1) Build baseline library
make serial -j4

# 2) Build + run first example
make -C examples ex1 -j4
./examples/ex1 -m data/star.mesh -no-vis

# 3) Run smoke checks
make check
```

## Routing checkpoints
- If step 1 fails: route to `mfem-build-and-install`.
- If step 2 fails: route to `mfem-getting-started`.
- If step 3 fails: route to `mfem-tests`.
