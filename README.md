# smoothMesh

OpenFOAM mesh smoothing tool to improve mesh quality.

Warning: Work in progress.

## Current restrictions

- Smoothing of internal mesh points only
- Applies centroidal smoothing algorithm with given number of iterations (`centroidalIters` option)
- Encourages orthogonal side edges on the boundary cell layer (`orthogonalBlendingFraction` option)
- Developed on OpenFOAM.com v2312

## Compilation instructions

```
. /usr/lib/openfoam/openfoam2312/etc/bashrc
cd smoothMesh/src
wclean
wmake
```

## Command line options

- `-centroidalIters` specifies the number of smoothing iterations (default 20)
- `-maxStepLength` is the maximum length for moving a point in one iteration (default 0.01)
- `-orthogonalBlendingFraction` is the fraction by which the edges touching
  the boundary faces are forced towards orthogonal direction (default 0.3)

The following parameters are applied to limit squishing of cells near concave features:

- `-minEdgeLength` defines edge length below which edge points are fully frozen at their current location (default 0.02)
- `-maxEdgeLength` defines edge length above which edge vertices are fully free to move, without any constraints (default 1.001 * minEdgeLength)

## Usage examples

- Parallel run example: `mpirun -np 3 smoothMesh -centroidalIters 20 -parallel`
- Serial run example: `smoothMesh -centroidalIters 20`
