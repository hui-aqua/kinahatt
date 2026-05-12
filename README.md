# Flow Over Hat (OpenFOAM)

This repository is prepared for an OpenFOAM CFD project to simulate airflow over a hat geometry.

The geometry file is **not added yet**. Once you upload it, this project is ready for meshing and solving.

## Project Layout

```text
flow-over-hat/
├── 0.orig/                # Initial fields (U, p, k, omega, nut, etc.)
├── constant/
│   ├── geometry/          # Optional CAD staging (STEP/IGES/STL source copies)
│   ├── triSurface/        # STL/OBJ used by snappyHexMesh
│   └── polyMesh/          # Generated mesh appears here after meshing
├── system/                # Control dictionaries (controlDict, fvSchemes, fvSolution...)
├── scripts/               # Helper scripts (mesh, run, clean)
├── results/               # Post-processing exports, sampled data, figures
└── docs/                  # Notes, assumptions, setup decisions
```

## Intended Workflow

1. Add hat geometry to `constant/triSurface/` (typically `hat.stl`).
2. Add/update OpenFOAM dictionaries in `system/`.
3. Create base mesh (`blockMesh`).
4. Refine around geometry (`snappyHexMesh`).
5. Run solver (likely `simpleFoam` for steady external flow).
6. Post-process in ParaView.

## Geometry Requirements (when you upload)

- Preferred: watertight **STL** in meters.
- Ensure outward-facing normals.
- Keep model near origin for easier domain setup.
- Recommended filename: `hat.stl`.

## Suggested OpenFOAM Setup (external aerodynamics)

- Solver: `simpleFoam`
- Turbulence model: `kOmegaSST` (good default for separated external flows)
- Boundary style:
  - Inlet: fixed velocity
  - Outlet: pressure outlet
  - Sides/top: slip or symmetry (depends on domain size)
  - Ground: wall (no-slip) if ground effects are modeled
  - Hat: wall (no-slip)

## Next Steps After Geometry Upload

When you upload the geometry, we can immediately add:

- `system/blockMeshDict`
- `system/snappyHexMeshDict`
- `system/controlDict`, `fvSchemes`, `fvSolution`
- `constant/transportProperties`, `constant/turbulenceProperties`
- `0.orig/` field files (`U`, `p`, `k`, `omega`, `nut`)
- Optional run script for one-command meshing + solve

## Notes

- This repo currently contains structure and documentation only.
- Case dictionaries are intentionally not guessed yet because they depend on hat size/orientation and target Reynolds number.
