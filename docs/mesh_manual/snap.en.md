# Snap

Snap is the process of accurately fitting a mesh to a geometric surface, moving points near a surface onto the surface to create the shape. During this process, adjustments such as smoothing and relaxation are made to maintain the quality of the mesh.

The settings are as follows

## Iteration Count

Set the number of interation for various operations.

### Smoothing for Surface

Specifies the number of smoothing iterations for the surface mesh.\\

Smoothing is a post-processing step to improve the quality of a mesh, making irregular cells or surfaces smoother. Smoothing can improve the quality of the grid, but too many iterations can cause distortion or deformation of the geometry. The figure below shows the mesh at 0 and 3 values of surface smoothing. You can see that at 0, the grid is still fine-grained, but at 3 days, it has changed. This mesh is not computationally problematic, but it does have an impact when create boundary layer mesh. 

This value is applied to _snapControls.nSmoothPatch_ in _snappyHexMeshDict_ file

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_snap_smoothing.png" width="800" height="800"><br>Effect of Smoothing for Surface</center>

### Smoothing for Internal

The Number of iteration for smoothing the internal mesh to reduce non-orthogonality of the refined surface. 
This makes the interior faces non-planar.

This value is applied to _snapControls.nSmoothInternal_ in _snappyHexMeshDict_ file
 
### Mesh Displacement Relaxation

The number of iterations to relax the mesh movement. Relaxation is an adjustment step during mesh generation, a way to control the position and size changes of cells so that they gradually adapt to the target shape. 

This value is applied to _snapControls.nSolveIter_ in _snappyHexMeshDict_ file.
 
### Snapping Relaxation

The number of relaxations performed during snapping.

This value is applied to _snapControls.nRelaxIter_ in _snappyHexMeshDict_ file.
 
## Feature Snapping

Settings related to feature line snapping.

### Snapping Relaxation

The number of relaxation iterations performed when snapping to a feature line. 

This value is applied to _snapControls.nFeatureSnapIter_ in _snappyHexMeshDict_ file.

### Feature Snap Type

There are two methods: explicit and implicit. The explicit method uses feature lines (obj file) that are automatically created when reading the stl file, and the implicit method uses feature angles to find features. 
 
### Multi-Surface Feature Snap

This option is for finding feature lines between multiple surfaces. Useful for creating multi-region mesh. Only applies when the feature line snapping method is explicit. 

This value is applied to _snapControls.multiRegionFeatureSnap_ in _snappyHexMeshDict_ file.

## Tolerance

This value is used to find points to move to the surface and is multiplied by the length of the local cell edge. If the value is too small, no suitable point will be found, and if it is too large, multiple points will be selected, resulting in poor geometry. The left side of figure below is an example of a geometry that was not properly realized due to a tolerance that was too small.

This value is applied to _snapControls.tolerance_ in _snappyHexMeshDict_ file.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mehs_snap_tolerance.png" width="800" height="800"><br>Effect of Tolerance</center>

## Concave Angle

This parameter determines how accurately the mesh will follow the geometry when processing geometric parts with concave angles. If this angle is small, the concave part can be handled more accurately, but at the cost of a larger number of mesh. Conversely, a large value may result in a less accurate reflection of the geometry. 

This value is applied to _snapControls.concaveAngle_ in _snappyHexMeshDict_ file.

## Min. Area Ratio

This parameter limits the area ratio of cells to improve grid quality. If the area ratio becomes smaller than this value, no refinement is performed. Small values allow complex shapes to be realized well, but may result in poor quality. Larger values improve the quality of the grid but can cause geometric distortion.

This value is applied to _snapControls.minAreaRatio_ in _snappyHexMeshDict_ file.




