# Mixer - baffle, periodic, cell zone

[Download geometry file](https://drive.google.com/file/d/1K_DUBphQi3Xqhy7E7dTtYR-s_D46DmZQ/view?usp=sharing) 

[Download BaramMesh folder](https://drive.google.com/file/d/1IX5tscEZjO3u0pbtilcwwPb9--RIZzYS/view?usp=sharing)

## Introduction 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixer/intro.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixer/intro.png)

* This is an example of creating a mixer mesh with a simple geometry. It uses a multiple reference frame (MRF) and create a mesh for steady-state simulation. There are no flow inlets or outlets, and only a quarter of the total geometry is modeled to use the rotational periodicity condition. The impeller is treated as a baffle with no thickness.
* After downloading and extracting the geometry files, you will find the mixer geometry (mixer.stl) and the cell zone geometry (cellZone.stl) files.

## Geometry

Use given mixer.stl and cellZone.stl file as geometry.

Click the [Import] button at the bottom to select stl files. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixer/importSTL.png"  >
    <br> 
</p>

The mixer.stl file consists of seven solids: wall, hub, hub_rot, impeller, periodic1, periodic2, and top. 

In Geometry, select cellZone and right-click and select Edit/View. In the window that opens, change the type as [CellZone]. 

In Geometry, select cellZone_surface and right-click and select Edit/View. In the window that opens, change the type as [None]. 

In Geometry, select impeller and right-click and select Edit/View. In the window that opens, change the type as [interface].(Not activate Non-Conformal and Inter-Region option)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixer/geom.png"  >
    <br> 
</p>


Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Region

Click the (+) icon at the top to create a region. Move the mouse to the intersection of the lines that appear in light green color in the graphics window and position it inside the computation domain. Click the [Add] button to complete the setup.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixer/region.png"  >
    <br> 
</p>

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Base Grid

Set the number of grids to 60, 40, and 80. Click the [Generate] button to generate the base grid.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixer/baseGrid.png"  >
    <br> 
</p>

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Castellation

Set level 2 for hub, hub\_rot, impeller surface, 1 for cellZone volume

### Surface/Feature Refinement

Click the (+) icon under [Surface/Feature Refinement] and set the following settings

+ Surface Refinement
    + Minimum Level : 2
    + Maximum Level : 2
+ Feature Edge refinement Level : 2
+ Surfaces : hub, hub\_rot, impeller

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixer/surfaceRefine.png"  >
    <br> 
</p>


### Volume Refinement

Click the (+) icon under [Volume Refinement] and set the following settings for cellZone

+ Volume Refinement Level : 1
+ Volume : cellZone

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixer/volumeRefine.png"  >
    <br> 
</p>

Turn on [Keep Non-Manifold Edges] under [Configuration]

Click the [Refine] button and the castellation step starts. 

The result looks like this

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixer/refineResult.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixer/refineResult.png)


Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Snap

Use the default settings and click the [Snap] button to start snap.


[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixer/snap.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixer/snap.png)

Click the [Next] button to move on to the next step.
<!-------------------------------------------------------------------------------------------------->
## Boundary Layer

Add boundary layer at wall, hub, hub\_rot, impeller, impeller\_slave. 

Click the (+) icon under [Configuration] to add a [Setting], and set it as follows

+ Number of Layers : 3
+ Thickness Model Specification : First and Expansion
+ Size Specification) : Relative
+ First Layer Thickness : 0.1
+ Expansion Ratio : 1.2
+ Min. Total Thickness : 0.2
+ Boundary : wall, hub, hub\_rot, impeller, impeller\_slave

Set [Advanced Configuration] as follows

+ Patch Displacement Smoothing
    + Number of Iteration : 0
    + Smooth Layer Thickness : 0 

Click [Apply] button to start boundary layer step.

The final mesh looks like this

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixer/layer.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixer/layer.png)


Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Export

Click [Export as BaramFlow project] to export the mesh to the desired location. 
