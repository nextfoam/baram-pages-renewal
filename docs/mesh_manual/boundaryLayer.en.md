# Boundary Layer

Create boundary layer.

## Configurations

Clicking the (+) on the right opens a window to set the conditions for generating the boundary layer mesh.

Number of Layers : Determine how many layers to create

Thickness Model Specification : Select how you want to set the height of the boundary layer. You have the option to specify the height as the height of the first, the last, or the entire. You can specify a rate of increase for the height, or you can specify the total height. And you have the option to specify the first height and the last height.

You can choose between Absolute and Relative to represent the height of the layer. The relative value is relative to the mesh size of the boundary where the boundary layer will be stacked, while the absolute value is in meters.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_layerConfig.png" width="400" ><br>Boundary Layer setup</center>

* Final and Expansion
* Final and Total
* First and Expansion
* First and Total
* Total and Expansion
* First and Relative Final : When this option is selected, the height specification method is automatically set to relative

Min. Total Thickness is the minimum value of the total thickness. If the total height is less than this value, no boundary layer will be created for that part. This is to prevent poor mesh quality when the boundary layer height is too small.

## Advanced Configuration

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_layerAdv.png" width="300" ><br>Advanced Configuration</center>

### Number of Grow

Number of layers of connected faces that are not grown if points do not get extruded. Helps convergence of layer addition close to features.

This value is applied to _addLayersControls.nGrow_ in _snappyHexMeshDict_ file.

The figure below shows the shape of the boundary layer as a function of the value of Number of Grow, when the boundary layer is not stacked at the points in the corners due to the influence of the Feature Angle Threshold.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/ngrow.png" width="800" ><br>Effect of Number of Grow, 0(left), 1(center), 2(right)</center>

### Static Analysis of Starting Mesh

#### Feature Angle Threshold

If the angle formed by the normal vectors on each side is greater than this value, no boundary layer is created at the corners. 

The left side of the figure below is when this value is 60. Because the angle between the normal vectors of the two sides is 90, no boundary layer grid is created at the corners when the value is less than 60, and boundary layers are created at the corners when the value is greater than 120, as shown on the right.

It is important to note that values as large as 120 can cause problems with the quality of the mesh as well as distortion of the shape of the boundary.

This value is applied to _addLayersControls.featureAngle_ in _snappyHexMeshDict_ file.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_layerFeatureAngle.png" width="800" ><br>Feature Angle Threshold, 60(left), 120(right)</center>

#### Max. Thickness Ratio

Parameter to avoid creating boundary layers from warped cells. Values greater than this will not create boundary layers. Larger values allow for freer mesh generation, but may result in poorer quality mesh. 

This value is applied to _addLayersControls.maxFaceThicknessRatio_ in _snappyHexMeshDict_ file.

### Patch Displacement smoothing

#### Number of Iterations

The number of smoothing iterations perpendicular to the faces. The default is 1. Increase this value if the desired mesh is not being created. 

This value is applied to _addLayersControls.nSmoothSurfaceNormals_ in _snappyHexMeshDict_ file.

#### Smooth Layer Thickness

The number of iterations to smooth the layer thickness across the surface. We recommend a value of around 10; larger values can improve the quality of the grid, but are more time consuming. 

The figure below shows the shape of the boundary layer as a function of the maximum number of layer thickness smoothing for a Feature Angle Threshold of 60 when adding four boundary layers.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/smoothLayerThickness.png" width="600" ><br> The effect of Smooth Layer Thickness (left) 10, (right) 100</center>


This value is applied to _addLayersControls.nSmoothThickness_ in _snappyHexMeshDict_ file

### Medial Axis

_SnappyHexMesh_ provides _displacementMedialAxis_ and _displacementMotionSolver_ as mesh shrinking algorithms, while BaramMesh only supports the _displacementMotionSolver_ method. The settings below are the inputs for this. 

Mesh shrinking is a process that pushes the mesh created in the snap step away from the boundary by the height of the boundary layer. SnappyHexMesh provides _displacementMedialAxis_ and _displacementMotionSolver_ as Mesh shrinking algorithms, while BaramMesh only supports the _displacementMedialAxis_ method. Below are the settings for this. A Medial Axis is defined as the collection of points within the polygon that are closest to more than one of the edges. It can also be viewed (equivalently) as the points that can be the center of a circle that is entirely within the polygon and touches the polygon in at least two places.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_medialAxis.png" width="400" ><br>Figure from RJ Donaghy's paper Dimensional Reduction of Analysis Models.</center>

#### Min. Axis Angle

Minimum angle to select the medial axis points

The figure below shows the shape of the boundary layer as a function of the minimum axis angle for a Feature Angle Threshold of 60 when adding four boundary layers. A value that is too small can distort the shape, and a value that is too large can cause problems with the mesh outside the boundary layer.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/minAxisAngle.png" width="800" ><br> The effect of Min. Axis Angle (left) 10, (center) 90, (right) 120</center>


This value is applied to _addLayersControls.minMedialAxisAngle_ in _snappyHexMeshDict_ file

#### Max. Thickness Ratio

The ratio of the boundary layer thickness ($\Delta L$) to the distance to the medial axis ($\Delta M$, medial distance). If thickness ratio is greater than this value, the growth of the boundary layer is limited. Values that are too small can result in small boundary layer heights, and values that are too large can distort the geometry.

$Max. Thickness$ $Ratio = \frac{\Delta L}{\Delta M}$

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/thicknessToMedialRatio.png" width="400" ><br> </center>

The figure below shows the shape of the boundary layer as a function of the maximum thickness ratio when stacking four boundary layers and a Feature Angle Threshold of 60.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/thicknessToMedialRatio1.png" width="800" ><br> The effect of Max. Thickness ratio. (left) 0.1, (center) 0.3, (right) 1.0</center>

This value is applied to _addLayersControls.maxThicknessToMedialRatio_ in _snappyHexMeshDict_ file

#### Number of Smoothing Iter.

Number of smoothing iterations of interior mesh movement direction

This value is applied to _addLayersControls.nSmoothNormals_ in _snappyHexMeshDict_ file

#### Max. Snapping Relaxation Iter.

Number of relaxation steps (where relaxed mesh quality parameters are used)

This value is applied to _addLayersControls.nRelaxIter_ in _snappyHexMeshDict_ file

### Mesh Shrinking

A method of shrinking the mesh in certain areas during the mesh generation process. When generating boundary layers, this is done to reduce the size of cells near the boundary to obtain a good quality mesh.

#### Num. of Buffer Cells

Create buffer region for new layer terminations, i.e. gradually step down number of layers. Set to less than 0 to terminate layer in one go

The figure below shows the shape of the boundary layer as a function of the Num. of Buffer Cells when stacking four boundary layers and using a Feature Angle Threshold of 60.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/buffercell.png" width="600" ><br> The effect of Num. of Buffer Cells. (left) 0, (right) 2</center>

This value is applied to _addLayersControls.nBufferCellsNoExtrude_ in _snappyHexMeshDict_ file

#### Max. Layer Addition Iter

Overall max number of layer addition iterations. The mesher will exit if it reaches this number of iterations; possibly with an illegal mesh.

In the figure below the lack of boundary layer at the corners is the result of the mesh shrinking applied to preserve the mesh quality. In the figure on the right, no mesh shrinking is applied(1), which also creates boundary layer grids at the corners, but results in poorer mesh quality.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_layerShrinking.png" width="800" ><br>Max. Layer Addition Iter, 50(left), 1(right)</center>

This value is applied to _addLayersControls.nLayerIter_ in _snappyHexMeshDict_ file

#### Max. Iter. Before Relax

Max number of iterations after which relaxed mesh quality controls are used. Up to nRelaxedIter it uses the settings in meshQualityControls after nRelaxedIter it uses the relaxed values.

This value is applied to _addLayersControls.nRelaxedIter_ in _snappyHexMeshDict_ file






