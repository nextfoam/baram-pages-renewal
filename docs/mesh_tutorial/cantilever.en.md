# Cantilever Beam

[Download geometry file](https://drive.google.com/file/d/1WsbkUpeVhtj8RlEXlhHEVLmwsnjyem5v/view?usp=sharing) 

[Download BaramMesh folder](https://drive.google.com/file/d/1-Gb9mxY1H6_ywJWfwfcqVFMCb8UFBfzA/view?usp=sharing)

## Introduction 

* This example uses the stl file of a Cantilever Beam to generate a mesh around the cantilever.

## Geometry

Use given cantileverBeam.stl file as geometry.

Click the [Import] button at the bottom to select the stl file. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cantileverBeam/1.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cantileverBeam/2.png"><br>
</p>

### Create computational domain

Click the [Add] button and select [Hex6] and set values as follows

+ Name : Hex6_1 
+ Type : None 
+ MIn. : (-0.4 0 0)
+ Max. : (1.2 0.2 0.5) 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cantileverBeam/cantilever_hex6.png"><br>
</p>

### Create hexahedron for refinement

Click the [Add] button and select [Hex] and set values as follows

+ Name : Hex
+ Type : None 
+ MIn. : (-0.05 0 0)
+ Max. : (0.2 0.04 0.2)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cantileverBeam/3.png"><br>
</p>

In Geometry, select Hex\_1\_surface and right-click and select Edit/View. In the window that opens, change the type as [None].

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cantileverBeam/4.png"><br>
</p>

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Region

Click the (+) icon at the top to create a region. Move the mouse to the intersection of the lines that appear in light green color in the graphics window and position it inside the computation domain. Click the [Add] button to complete the setup.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cantileverBeam/5.png"><br>
</p>

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Base Grid

Select [Use Hex6] option and set the number of grids to 100, 12, and 30. Click the [Generate] button to generate the base grid.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cantileverBeam/7.png"><br>
</p>

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Castellation

### Surface/Feature Refinement

Click the (+) icon under [Surface/Feature Refinement] and set the following settings

+ Surface Refinement
    + Minimum Level : 3
    + Maximum Level : 3
+ Feature Edge refinement Level : 3
+ Surfaces : cantileverBeam\_surface\_0

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cantileverBeam/cantilever_refine_surface.png"><br>
</p>

### Volume Refinement

Click the (+) icon under [Volume Refinement] and set the following settings 

+ Volume Refinement Level : 2
+ Volume : Hex\_1
    
Click the [Refine] button and the castellation step starts. 

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Snap

Use the default settings and click the [Snap] button to start snap.

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Boundary Layer

Click the (+) icon under [Configuration] to add a [Setting], and set it as follows

+ Number of Layers : 5
+ Thickness Model Specification : First and Expansion
+ Size Specification : Relative
+ First Layer Thickness : 0.1
+ Expansion Ratio : 1.2
+ Min. Total Thickness : 0.01
+ Boundary : cantileverBeam

Use default values for [Advanced] Configuration.

Click [Apply] button to start boundary layer step.

The final mesh looks like this

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cantileverBeam/cantilever_mesh.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cantileverBeam/cantilever_mesh_1.png"><br>
</p>

Click the [Next] button to move on to the next step.
<!-------------------------------------------------------------------------------------------------->
## Export

Click [Export as BaramFlow project] to export the mesh to the desired location.  
