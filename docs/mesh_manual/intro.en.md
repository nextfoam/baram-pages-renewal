# Introduction

## What is BaramMesh

BaramMesh is mesh generation module of BARAM. It uses OpenFOAM's utilities _blockMesh_ and _snappyHexMesh_ to generate mesh. Geometry of STL files can be imported. Simple geomertry as hexahedron, sphere, and cylinder can be created in it. It can generate octree-style three-dimensional mesh including boundary layer and can create regions, cell zones, interfaces, and baffles.

OpenFOAM's _snappyHexMesh_ utility create mesh with the following steps

1) Create a structured type background mesh that covers the entire domain with _blockMesh_ utility.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_blockMesh.png" width="400" height="400"><br>배경 격자</center>

2) Refine mesh where you need and remove mesh of outside domain. This process is called castellation.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_castellate.png" width="400" height="400"><br>격자세분화</center>

3) Move mesh points onto the surface to achieve the correct geometry. This process is called snap.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_snap.png" width="400" height="400"><br>형상구현</center>

4) Add boundary layer mesh from surfaces.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_layer.png" width="400" height="400"><br>경계층 격자</center>

After the first step, a mesh is created in the constant/polyMesh folder, and folders 1, 2, and 3 are created after every 2, 3, and 4 steps.

BaramMesh breaks down the above process into seven steps, including

1. Geometry : Import STL file or create primitive geometry
2. Region : Define regions to create mesh
3. Base Grid : Create base mesh
4. Castellation : refine mesh and remove mesh outside domain
5. Snap : Move mesh points onto surface
6. Boundary Layer : Create boundary layer mesh
7. Export : Export mesh in a folder format that BaramFlow can read.

The seven steps above are sequential.

When a step is completed, it is locked and you move on to the next step. If you want to go back to a previous step and work with different settings, you need to go to that step and unlock it. Unlocking delete all work done after that step.




 
  


