# smoothMesh

OpenFOAM mesh smoothing tool to improve mesh quality. Moves internal
mesh points by using the Centroidal smoothing algorithm (a version of the
[Laplacian smoothing algorithm](https://en.wikipedia.org/wiki/Laplacian_smoothing),
which uses surrounding cell centers instead of the neighbouring vertex
locations to calculate the new vertex position). Optional heuristic
quality control options exist to constrain the smoothing, to avoid
self-intersections. No changes to mesh topology are made.

## Current restrictions

- Works on 3D polyhedron meshes
- Requires a consistent (not self-intersecting or tangled) initial mesh
- Smoothes internal mesh points only (boundary points are frozen)
- Developed on OpenFOAM.com v2312

## Compilation instructions

```
. /usr/lib/openfoam/openfoam2312/etc/bashrc
cd smoothMesh/src
wclean; wmake
```

## Command line options

- `-centroidalIters` specifies the number of smoothing iterations (default 20).

- `-maxStepLength` is the maximum length (in metres) for moving a point in one iteration (default 0.01). Adjust this value for your case. Smoothing process seems to be stable when this value is smaller than about one tenth of cell side length.

- `-orthogonalBlendingFraction` is the fraction (0 <= value <= 1) by which the edges touching the boundary faces are forced towards orthogonal direction. Zero value causes no orthogonal direction for boundary edges (default 0).

- `-qualityControl true` enables extra quality constraints for smoothing. The constraints limit the tendency of smoothing to compress cells and create self-intersecting cells near concave geometry features. Without constraints, self-intersections can be created if the mesh contains skewed faces or low determinant cells (`cellDeterminant` according to `checkMesh`). When this option is enabled (default is true), the following options affect the results:

  - `-minEdgeLength` defines edge length below which edge points are fully frozen at their current location (default 0.002). Adjust this value for your case.

  - `-maxEdgeLength` defines edge length above which edge vertices are fully free to move, without any constraints (default 1.001 * minEdgeLength).

  - `-minAngle` defines the minimum edge-edge angle for face corners (in degrees, 0 < value < 180, default 45) below which points are fully frozen in their current location, if the angle would decrease in smoothing. Points are allowed to move if minimum angle value increases with smoothing, regardless of this value.

## Usage examples

- Parallel run example: `mpirun -np 3 smoothMesh -centroidalIters 20 -parallel`
- Serial run example: `smoothMesh -centroidalIters 20`

## Getting help and feedback

Please use Github issues section for asking help. If you like this
tool, please star this repository in Github!
