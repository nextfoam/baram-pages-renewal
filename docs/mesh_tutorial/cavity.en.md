# Cavity

[Download geometry file](https://drive.google.com/file/d/1eJMxycnNdELkT0YJRAqvojJDZTsTstOE/view?usp=sharing) 

[Download BaramMesh folder](https://drive.google.com/file/d/1skTN_uD0E8Gh7_Ku9ggTvKUnDp2cWCTo/view?usp=sharing)

## Introduction 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavity/intro.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavity/intro.png)

* This example is an example of mesh generation for a three-dimensional transonic cavity flow analysis.
* The cavity model is inside a wind tunnel and is the geometry from the following paper.
 
  _Numerical Simulation of High-Speed Turbulent Cavity Flows, G.N.Barakos, S.J.Lawson, R.Steijil, P.Nayyar, Flow Turbulence Combust, 2009_

## Geometry

Use given 7 stl files as geometry.

Click the [Import] button at the bottom to select the stl files. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cavity/cavity-importSTL.png"  >
    <br> 
</p>

It consists of a cavity model and six surfaces of a farfield boundary. Rename the volume to Cavity. Right-click and select [Edit/View] to rename the faces as follows. 

+ Visualization\_Toolkit\_generated\_SLA\_file\_surface : right
+ Visualization\_Toolkit\_generated\_SLA\_file\_surface1 : forward
+ Visualization\_Toolkit\_generated\_SLA\_file\_surface2 : down
+ Visualization\_Toolkit\_generated\_SLA\_file\_surface3 : left
+ Visualization\_Toolkit\_generated\_SLA\_file\_surface4 : up
+ Visualization\_Toolkit\_generated\_SLA\_file\_surface5 : wall
+ Visualization\_Toolkit\_generated\_SLA\_file\_surface6 : back

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cavity/cavity-geom.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cavity/cavity-geom.png)

Create a hexahedron in the area where you want to create a dense mesh. Create two hexahedra in the cavity and near the walls of the model as follows.

Click the [Add] button and select [Hex] and set values as follows

+ Name : Hex\_1 
+ Type : None 
+ MIn. : (1.75 1.1 -0.2)
+ Max. : (2.5 1.3 0.1)

In Geometry, select Hex\_1\_surface and right-click and select Edit/View. In the window that opens, change the type as [None]. 

Once more, click the [Add] button and select [Hex] and set values as follows

+ Name : Hex\_2 
+ Type : None 
+ MIn. : (1 1 0)
+ Max. : (3 1.5 0.05)

In Geometry, select Hex\_2\_surface and right-click and select Edit/View. In the window that opens, change the type as [None]. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cavity/cavity-refineZone.png"  >
    <br> 
</p>

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Region

Click the (+) icon at the top to create a region. Move the mouse to the intersection of the lines that appear in light green color in the graphics window and position it inside the computation domain. Click the [Add] button to complete the setup.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cavity/cavity-region.png"  >
    <br> 
</p>

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Base Grid

Set the number of grids to 36, 24, and 26. Click the [Generate] button to generate the base grid.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cavity/cavity-baseGrid.png"  >
    <br> 
</p>

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Castellation

### Surface/Feature Refinement

Click the (+) icon under [Surface/Feature Refinement] and set the following settings 

+ Surface Refinement
    + Minimum Level : 2
    + Maximum Level : 2
+ Feature Edge refinement Level : 2
+ Surfaces : wall

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cavity/cavity-wall.png"  >
    <br> 
</p>

### Volume Refinement

Click the (+) icon under [Volume Refinement] and set the following settings 

+ Volume Refinement Level : 5
+ Volume : Hex\_1

Once more, click the (+) icon under [Volume Refinement] and set the following settings 

+ Volume Refinement Level : 4
+ Volume : Hex\_2

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cavity/cavity-hex1.png"  >
    <br> 
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cavity/cavity-hex2.png"  >
    <br> 
</p>

Click the [Refine] button and the castellation step starts. 

The result looks like this

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cavity/cavity-refine.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cavity/cavity-refine.png)


Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Snap

Change the [Feature Snap Type] under [Feature Snapping] to [implicit] and click the [Snap] button to start snap.

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Boundary Layer

Click the (+) icon under [Configuration] to add a [Setting], and set it as follows

+ Number of Layers : 5
+ Thickness Model Specification : First and Expansion
+ Size Specification : Relative
+ First Layer Thickness : 0.1
+ Expansion Ratio : 1.2
+ Min. Total Thickness : 0.3
+ Boundary : wall

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cavity/cavity-layer-setup.png"  >
    <br> 
</p>

Use default values for [Advanced] Configuration.

Click [Apply] button to start boundary layer step.

The final mesh looks like this

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cavity/cavity-layer.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cavity/cavity-layer.png)

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Export

Click [Export as BaramFlow project] to export the mesh to the desired location. 

