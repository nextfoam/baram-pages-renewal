# Fan

[Download geometry file](https://drive.google.com/file/d/1Z_PLXsIe-niyzrYEHUTkhZkboLs5eeSP/view?usp=sharing) 

[Download BaramMesh folder](https://drive.google.com/file/d/1tGAmauRlTQr1k_YRxYIMAeVFqLozNoS8/view?usp=sharing)

## Introduction 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/fan/intro.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/fan/intro.png)

* This is an example of creating a simple geometry centrifugal fan mesh with BaramMesh. The blades are very thin and the gap between the case and the blades is very small, so great care must be taken to get the geometry right.
* To calculate with MRF, we have a cylinder surrounding the blade and set its interior as a cellzone.


## Geometry

Use given 4 stl files as geometry - casing.stl, blade.stl, inlet.stl, outlet.stl.

Click the [Import] button at the bottom to select stl files. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/fan/importSTL.png"  >
    <br> 
</p>

To create Cell zone, click [Add] button and select ]Cylinder]. Select [CellZone] for [Type] and set values as follows

+ Axis Point 1 : (0 0 -0.001)
+ Axis Point 2 : (0 0 0.021)
+ Radius : 0.07

In Geometry, select cylinder\_1\_surface and right-click and select Edit/View. In the window that opens, change type to [None] 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/fan/createCylinder.png"  >
    <br> Add cell zone geometry
</p>

The final geometry looks like this

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/fan/geom1.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/fan/geom1.png)

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Region

Click the (+) icon at the top to create a region. Move the mouse to the intersection of the lines that appear in light green color in the graphics window and position it inside the computation domain. Click the [Add] button to complete the setup.

[![intro](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/fan/region.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/fan/region.png)

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Base Grid

Set the number of grids to 50, 50, and 20. Click the [Generate] button to generate the base grid.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/fan/baseGrid.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/fan/baseGrid.png)

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Castellation

### Surface/Feature Refinement

Click the (+) icon under [Surface/Feature Refinement] and set the following settings for blade

+ Surface Refinement
    + Minimum Level : 1
    + Maximum Level : 4
+ Feature Edge refinement Level : 1
+ Surfaces : blade\_surface

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/fan/fan_refine_blade.png"><br> 
</p>

Click the (+) icon under [Surface/Feature Refinement] and set the following settings for case and cell zone

+ Surface Refinement
    + Minimum Level : 1
    + Maximum Level : 2
+ Feature Edge refinement Level : 1
+ Surfaces : casing\_surface, Cylinder\_1\_surface

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/fan/fan_refine_case.png"><br> 
</p>

### Volume Refinement

Click the (+) icon under [Volume Refinement] and set the following settings for casing 

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/fan/volumeRefine.png"><br> 
</p>

Click the [Refine] button and the castellation step starts. 

When you're done setting up, your screen should look like this

[![intro](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/fan/refineResult.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/fan/refineResult.png)

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Snap

Turn on the [Multi-Surface Feature Snap] option and click the [Snap] button to start snap.

[![intro](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/fan/snap.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/fan/snap.png)

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Boundary Layer

Click the (+) icon under [Configuration] to add a [Setting], and set it as follows

+ Number of Layers : 3
+ Thickness Model Specification : First and Expansion
+ Size Specification) : Relative
+ First Layer Thickness : 0.1
+ Expansion Ratio : 1.2
+ Min. Total Thickness : 0.3
+ Boundary : blade\_surface, casing\_surface

Use default values for [Advanced] Configuration.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/fan/fan_layer.png"><br> 
</p>

Click [Apply] button to start boundary layer step.

The final mesh looks like this

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/fan/fan_final.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/fan/fan_final.png)

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Export

Click [Export as BaramFlow project] to export the mesh to the desired location. 
