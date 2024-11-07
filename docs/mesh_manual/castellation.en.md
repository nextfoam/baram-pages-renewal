# Castellation

This step prepares you for fitting the mesh to the geometric model. Isolate the mesh by checking if the base grid intersects the geometry, and then remove the cells outside the geometric boundaries. Refine the mesh by splitting the hexahedral cells. The refinement of the mesh is controlled by the size level, which uses the base grid size as a baseline of 0, and then splits the hexahedron into four for every increment of 1.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_castell.png" width="800" height="800"><br>Base Grid to Castellate</center>

There are two options: Configuration and Advance. The mesh refinement can be set for Surface/Feature or Volume.

## Configuration

### Number of Cells between Levels

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_nCellsBetweenLevels.png" width="800" height="800"><br>Number of Cells between Levels (left) 1, (center) 2, (right) 3</center>

Rapid changes in cell size are not good for the mesh system, which is why we have a buffer zone when changing levels. The figure below shows the results when the size level of the circle is 1, 2, and 3.

This value is applied to _castellatedMeshControls.nCellsBetweenLevels_ in _snappyHexMeshDict_  file.

### Feature Angle Threshold

If the angle formed by the normal of two neighboring surface is greater than this value, the maximum level of surface refinement for a given surface is used. The figure below shows the results of mesh refinement based on the feature angle threshold when there are two hexahedra and the size level of the inner hexahedron is given a minimum level of 1 and a maximum level of 2.

This value is applied to _castellatedMeshControls.resolveFeatureAngle_ in _snappyHexMeshDict_  file

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_resolveFeature.png" width="800" height="800"><br>Effect of Feature Angle Threshold (left) geometry, (center) 100, (right) 30</center>

### Keep Non-Manifold Edges

This option is used when extracting feature edges of surfaces and is an option to include edges with three or more connected faces.

### Keep Open Edges

This option is used when extracting feature edges for surfaces, and determines whether edges with only one connected face should be included.

## Advanced

### Max. Global Cell Count

The number of cells after refinement and before removing the cells of outside domain. Cell refinement stops when this value is reached as a limit on the total number of cells. 

This value is applied to _castellatedMeshControls.maxGlobalCells_ in _snappyHexMeshDict_  file

### Max. Local Cell Count

The maximum number of cells on each core in parallel computation. Parallel computations use 'refinement-followed-by balancing', and when this value is reached, the 'weighted balancing method' is used. This can result in slower speeds. 

This value is applied to _castellatedMeshControls.maxLocalCells_ in _snappyHexMeshDict_  file.

### Min. Refinement Cell count

You may have a problem where the mesh refinement process is consuming a lot of time because of a small number of cells. Stop the mesh refinement process when the remaining cells reach the value specified here. This setting is to prevent the mesh refinement process from spending too much time on a small number of unimportant cells. 

This value is applied to _castellatedMeshControls.minRefinementCells_ in _snappyHexMeshDict_  file.

### Max. Load Unbalance

The relative difference in the number of cells per process in parallel computation. Lower values result in more frequent load balancing, while higher values can disable load balancing. 

This value is applied to _castellatedMeshControls.maxLoadUnbalance_ in  _snappyHexMeshDict_  file

### Allow Free Standing Zone Faces

This should be set to true when the faceZone is an independent face, such as a baffle. 

This value is applied to _castellatedMeshControls.allowFreeStandingZoneFaces_ in _snappyHexMeshDict_  file

## Surface/Feature Refinement

Click the (+) on the right to create them.

Specify the level for each surface and feature and select the surfaces to apply. Surfaces are those created in [1.Geometry]. Surface Refinement can give a minimum and maximum level. If the angle formed by the normals of two adjacent surface is greater than the value entered in the 'Feature Angle Threshold', the maximum level is used; otherwise, the minimum level is used.

You do not need to select feature lines as they are created by baramMesh when you create the geometry.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_surfaceRefinement.png" width="400" height="400"><br>Surface/Feature Refinement</center>

## Volume Refinement

Specify the level for each volume and select the volumes to apply. Volumes are those created in [1.Geometry]

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_volumeRefinement.png" width="400" height="400"><br>Volume Refinement</center>

The Gap Refinement option automatically refine the mesh into closely spaced areas. This can be handy if you have multiple surfaces that are very close together, or if you have thin plates or wings. There are five inputs

* Min. Cell Layers in a gap : Enter an integer greater than 3 as the minimum number of mesh to fit in the narrow gap.
* Gap detection start level : Here, gap refinement is applied if the gap is smaller than the mesh size for a given level.
* Max. refinement level : Do not apply levels greater than this value.
* Direction : Inside is an option that applies to narrow gaps inside closed faces, refine the mesh in the direction of the thickness of a thin plate or wing. Outside applies to gaps between surfaces and other surfaces. Mixed applies both inside and outside.
* Include surface's own gap : Choose whether to apply to surface's own gap. In the image below, the left side is not included and the right side is included.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_volumeRefinement-1.png" width="400" height="400"><br>Example of Gap Refinement</center>




