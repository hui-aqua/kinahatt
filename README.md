# Kinahatt OpenFOAM Case

CFD case for an immersed hat-shaped structure inside a cylindrical water domain using OpenFOAM v2506.

---

# OpenFOAM Version

```text id="6m6q5i"
OpenFOAM v2506
```

---

# Project Structure

```text id="6d8fgn"
kinahatt/
├── 0.orig/                    # Initial field templates
├── constant/
│   ├── polyMesh/              # Generated mesh
│   ├── triSurface/            # STL geometry files
│   └── extendedFeatureEdgeMesh/
├── ORG_geo/                   # Original geometry archive
├── system/
│   ├── blockMeshDict
│   ├── controlDict
│   ├── fvSchemes
│   ├── fvSolution
│   ├── meshQualityDict
│   ├── snappyHexMeshDict
│   └── surfaceFeatureExtractDict
├── scripts/
├── results/
└── README.md
```

---

# Geometry Structure

The CFD domain is built using separated STL surfaces.

```text id="c5kxqv"
outsideWall.stl
    Open outer cylindrical boundary

pipeInlet.stl
    Pipe inflow boundary

pipeWall.stl
    Solid pipe wall

A_buet_hat.stl
    Internal immersed solid structure
```

---

# Mesh Strategy

Mesh generation uses:

* `blockMesh`
* `surfaceFeatureExtract`
* `snappyHexMesh`

The final fluid region is:

```text id="8nnwzn"
inside outsideWall
AND
outside A_buet_hat
```

---

# Boundary Patch Strategy

| Patch    | Type          |
| -------- | ------------- |
| inlet    | inflow        |
| outside  | open boundary |
| pipeWall | wall          |
| hatt     | wall          |

---

# Geometry Placement

Working STL files are located in:

```text id="9e5j0k"
constant/triSurface/
```

Original geometry archive:

```text id="utwn1j"
ORG_geo/
```

---

# Current Mesh Workflow

## Clean mesh

```bash id="dr4tbv"
rm -rf constant/polyMesh
rm -rf 0
cp -r 0.orig 0
```

## Generate feature edges

```bash id="cmup2w"
surfaceFeatureExtract
```

## Generate background mesh

```bash id="m5e6ws"
blockMesh
```

## Generate snappy mesh

```bash id="vzb1pw"
snappyHexMesh -overwrite | tee log.snappy
```

## Check mesh quality

```bash id="0w8xrk"
checkMesh
```

---

# Visualization

```bash id="avq0z0"
paraFoam
```

or

```bash id="x2prht"
touch case.foam
paraview case.foam
```

---

# Notes

* Mesh generated successfully with OpenFOAM v2506
* Immersed solid subtraction working correctly
* Structured STL workflow simplifies patch assignment
* Large/generated files excluded using `.gitignore`
