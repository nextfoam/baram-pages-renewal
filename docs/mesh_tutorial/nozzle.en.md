# Axi-symmetric nozzle

[Download geometry file](https://drive.google.com/file/d/1mWnujK9XPQyt5gnSD0Z4cF3hP67tHVA1/view?usp=sharing) 

[Download BaramMesh folder](https://drive.google.com/file/d/1h3VUvhbEv6gdD8tPpWSNXwshjvHYwYch/view?usp=sharing)

## Introduction 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/blood/intro.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/blood/intro.png)

* This is an example of mesh generation for an axisymmetric nozzle analysis.
* Geometry from the following paper used in FDA's Nozzle Challenge, a benchmark test program for simulation validation of non-Newtonian fluids hosted by the U.S. Food and Drug Administration.

_Trias, Miquel, Antonio Arbona, Joan Massó, Borja Miñano, and Carles Bona. “FDA’s nozzle numerical simulation challenge: non-Newtonian fluid effects and blood damage.” PloS one 9, no. 3 (2014): e92638._ 

## Geometry

Use given fda-nozzle-scaled.stl file as geometry. This file is a three-dimensional geometry with axisymmetric faces in the x-y plane extruded in the z direction. BaramMesh does not directly create an axisymmetric mesh, but rather creates a three-dimensional mesh, rotates one of its faces (extrudes), and exports it as an axisymmetric lattice.

Click the [Import] button at the bottom to select the stl file.

Turn on the [Split Surface] option to separate boundary surfaces and pressing the OK button, a window like the one on the right in the image below will appear. You can see that eight boundary surfaces are created. Click the OK button to load the geometry.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/nonNewtonianNozzle/importSTL.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/nonNewtonianNozzle/importSTL.png)

When you read the stl files, you'll see eight faces numbered like zone0\_surface, zone0\_surface1, .... We'll rename them to distinguish them. 

In Geometry, select surfaces and right-click and select Edit/View. In the window that opens, change the name as follows 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/nonNewtonianNozzle/changeName.png"  >
    <br> 
</p>

+ zone0\_surface : back
+ zone0\_surface1 : front
+ zone0\_surface2 : bottom
+ zone0\_surface3 : wall
+ zone0\_surface4 : wall1
+ zone0\_surface5 : out
+ zone0\_surface6 : in
+ zone0\_surface7 : wall2

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Region

Click the (+) icon at the top to create a region. Move the mouse to the intersection of the lines that appear in light green color in the graphics window and position it inside the computation domain. Click the [Add] button to complete the setup.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/nonNewtonianNozzle/region.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/nonNewtonianNozzle/region.png)

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Base Grid

Set the number of grids to 600, 50, and 2. Click the [Generate] button to generate the base grid.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/nonNewtonianNozzle/baseGrid.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/nonNewtonianNozzle/baseGrid.png)

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Castellation

Use the default settings.Click the [Refine] button and the castellation step starts.

The result looks like this

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/nonNewtonianNozzle/castel.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/nonNewtonianNozzle/castel.png)

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
+ First Layer Thickness : 0.15
+ Expansion Ratio : 1.2
+ Min. Total Thickness : 0.3
+ Boundary : wall, wall1, wall2

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/nonNewtonianNozzle/nozzle_layer.png"  >
    <br> 
</p>

Use default values for [Advanced] Configuration.

Click [Apply] button to start boundary layer step.

The final mesh looks like this 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/nonNewtonianNozzle/layer.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/nonNewtonianNozzle/layer.png)

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Export

To export to an axisymmetric mesh, click the [Axi-Symmetry] button under [2D Exports] to open the following window.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/nonNewtonianNozzle/nozzle_export.png"  >
    <br> 
</p>

Select path and enter folder name. And set values as follows
+ Source Boundary : back
+ Exposed Boundary : front
+ Angle : 5
+ Axis Origin : (0 0 0)
+ Direction : (-1 0 0)

Pressing the OK button will create a folder that BaramFlow can read. 

Exported mesh looks like the image below

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/nonNewtonianNozzle/nozzle_exported_mesh.png"  >
    <br> 
</p>


