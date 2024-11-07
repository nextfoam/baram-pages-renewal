# Propeller

[Download geometry file](https://drive.google.com/file/d/1Y0-PdoUDE6MFPLRlPVwE1Q54BMvOkn2N/view?usp=sharing) 

[Download BaramMesh folder](https://drive.google.com/file/d/1_q807VRQmB4kM47IfswJOvukThvjwI9X/view?usp=sharing)

## Introduction 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/propeller/intro.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/propeller/intro.png)

* This example uses BaramMesh to create the mesh of a propeller from the pimpleFoam tutorial on OpenFOAM.
* To compute as a sliding mesh, we have a cylinder surrounding the propeller and set its interior as a cellzone. The surfaces surrounding the cellzone must have two interfaces at the same location.

## Geometry

Use given 3 stl files of propeller.stl, propelerStem.stl, far.stl as geometry.

far.stl is a cylinder for computational domain, but the flow in and out is not separated, so you need to separate the boundaries when reading it. 

Click the [Import] button at the bottom and select propeller.stl and propellerStem.stl files. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/importSTL.png">
    <br>
</p>

Click the [Import] button at the bottom to select far.stl file and turn on [Split Surface] option.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/importSTL1.png">
    <br> 
</p>

With the default Feature Angle of 60, you can see that the surface is divided into three parts: entrance, exit, and farfield. Press the OK button. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/importSTL2.png"  >
    <br> split surface
</p>

In Geometry, select far\_surface1 and right-click and select Edit/View. In the window that opens, change the name as outlet. 

In Geometry, select far\_surface2 and right-click and select Edit/View. In the window that opens, change the name as inlet. 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/geom.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/geom.png)

### Create cylinder for cell zone

To create cell zone for sliding mesh, click [Add] button. Select [Cylinder] and set values as follows

+ Name : cellZone
+ Type : CellZone
+ Cylinder Geometry
    + Axis Point1 : (-0.06 0 0)
    + Axis Point2 : (0.08 0 0) 
+ Radius : 0.12

In Geometry, select cellZone\_surface and right-click and select Edit/View. In the window that opens, change the type as [Interface]-[Non-Conformal]. 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/cellZone.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/cellZone.png)

### Create cylinder for refinement

Click [Add] button and select [Cylinder] and set values as follows

+ Name : refine
+ Type : None
+ Cylinder Geometry
    + Axis Point1 : (-0.1 0 0)
    + Axis Point2 : (0.6 0 0) 
+ Radius : 0.16

In Geometry, select refine\_surface and right-click and select Edit/View. In the window that opens, change the type as [None]. 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/refine.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/refine.png)

The final geometry looks like this

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/geom1.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/geom1.png)

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Region

Click the (+) icon at the top to create a region. Move the mouse to the intersection of the lines that appear in light green color in the graphics window and position it inside the computation domain. Click the [Add] button to complete the setup.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/region.png"  >
    <br> 
</p>

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Base Grid

Set the number of grids to 20, 120, and 12. Click the [Generate] button to generate the base grid.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/baseGrid.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/baseGrid.png)


Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Castellation

### Surface/Feature Refinement

Use level 4~6 for propeller, 4 for stem and cellZone, and 3 for refine.

Click the (+) icon under [Surface/Feature Refinement] and set the following settings for propeller

+ Surface Refinement
    + Minimum Level : 4
    + Maximum Level : 6
+ Feature Edge refinement Level : 4
+ Surfaces : propeller

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/propeller_refine_propeller.png"  >
    <br> 
</p>

Click the (+) icon under [Surface/Feature Refinement] and set the following settings for propellerStem and cellZone

+ Surface Refinement
    + Minimum Level : 4
    + Maximum Level : 4
+ Feature Edge refinement Level : 4
+ Surfaces : propellerStem, cellZone\_surface

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/propeller_refine_stem.png"  >
    <br> 
</p>

Click the (+) icon under [Surface/Feature Refinement] and set the following settings for refine

+ Surface Refinement
    + Minimum Level : 3
    + Maximum Level : 3
+ Feature Edge refinement Level : 3
+ Surfaces : refine\_surface

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/propeller_refine_refine.png"  >
    <br> 
</p>

### Volume Refinement

Set the level 4 for cellZone and 3 for refine.

Click the (+) icon under [Volume Refinement] and set the following settings for cellZone 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/propeller_refine_v1.png"  >
    <br> 
</p>

Click the (+) icon under [Volume Refinement] and set the following settings for refine

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/propeller_refine_v2.png"  >
    <br> 
</p>

Click the [Refine] button and the castellation step starts. 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/refineResult.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/refineResult.png)

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Snap

Change the [Feature Snap Type] under [Feature Snapping] to [implicit] and click the [Snap] button to start snap.

[![intro](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/snap.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/snap.png)

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Boundary Layer

Add boundary layer at propeller and propellerStem.

Click the (+) icon under [Configuration] to add a [Setting], and set it as follows

+ Number of Layers : 3
+ Thickness Model Specification : First and Expansion
+ Size Specification : Relative
+ First Layer Thickness : 0.1
+ Expansion Ratio : 1.2
+ Min. Total Thickness : 0.2
+ Boundary : propeller, propellerStem

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/layer.png"  >
    <br> 
</p>

Use default settings for [Advanced Configuration].

Click [Apply] button to start boundary layer step.

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Export

Click [Export as BaramFlow project] to export the mesh to the desired location. 

