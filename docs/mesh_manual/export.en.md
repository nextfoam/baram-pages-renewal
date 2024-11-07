# Export

Export the created mesh to a folder in a format readable by BaramFlow. The cell zone and region settings are made at this stage. 2D Exports exports as a two-dimensional and axisymmetric mesh.

## 2D Plane

In OpenFOAM, two-dimensional problems are handled with a single volume mesh in one direction and _empty_ boundary conditions on both sides. Since the _snappyHexMesh_ used by baramMesh is locally refined, it is not easy to create only one volume mesh in one direction.

When exporting to a two-dimensional mesh, the surface mesh on one side is extruded to the unlit side to create a single layer of volume mesh. And the existing three-dimensional mesh is deleted. Therefore, the user can select the desired side and enter the thickness to be extruded. 

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/2d-export.png" width="400" height="400"><br>Export 2D Mesh</center>

In the figure below, the left figure is a three-dimensional mesh created in baramMesh. The middle figure is a one-layer mesh created by extruding the zMin faces in the -z direction. On the right, the existing three-dimensional mesh has been deleted and only a single layer of mesh has been exported.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/2d-export-1.png" width="800" height="800"><br>Steps of export 2D mesh</center>

## Axi-Symmetry

In OpenFOAM, axisymmetric problems are handled by using _wedge_ boundary conditions on both sides of a wedge-shaped mesh with one volume mesh in that direction. 

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/axi-export.png" width="400" height="400"><br>Export axi-symmetry mesh</center>

When exporting to an axisymmetric mesh, the surface mesh of one side is rotated to create a one-layer volume mesh. And the existing three-dimensional mesh is deleted. Therefore, the user selects the side to be rotated (Source boundary) and the corresponding side (Exposed boundary), and enters the rotation angle, rotation center, and rotation axis. The rotation direction is determined by the right-hand rule.

Figure below shows the difference between the mesh created by rotating the left and right faces of a three-dimensional mesh in red color. Since the direction of rotation is determined by the right-hand rule, when the left face (zMin) is given as the source boundary to rotate, the axis of rotation must be in the -x direction, (-1 0 0). When the right side (maxZ) is given as the source boundary to rotate, the rotation axis should be (1 0 0) in the +x direction.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/axi-export-1.png" width="400" height="400"><br>Effect of Source boundary</center>

Figure below shows the result as a function of the center of rotation. This is the result when the y coordinate of the center of rotation is given as (0 -0.2 0), which is less than 0, the minimum y value for a three-dimensional mesh. 

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/axi-export-2.png" width="400" height="400"><br>Effect of Rotation Center</center>







