# Mixing pipe 

[Download geometry file](https://drive.google.com/file/d/1WHF4ptAEe0RXVily3eg5GhrAGkNcRCIQ/view?usp=sharing) 

[Download BaramMesh folder](https://drive.google.com/file/d/1AM2EolmBIfDrNDOjVM6CTRSTFPfBFbqm/view?usp=sharing)

## Introduction 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/1.1.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/1.1.png)

This example uses an STL file of a simple pipe to generate an internal mesh. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixingpipe/1.1.png"><br>
</p>

## Geometry

### Import geometry

Use given mixingpipe.stl file as geometry.
 
Click the [Import] button at the bottom to select the mixingpipe.stl file. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixingpipe/import.png"><br>
</p>

The [Split Surface] option is a feature that splits the surfaces of the geometry. When enabled, the surfaces will be split into different names based on the feature angle. Using 60 degrees to split the boundary will split the surface into four surfaces, as shown below.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixingpipe/split.png"><br>
</p>

When the geometry file is read, a window appears as shown below.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixingpipe/1.3.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixingpipe/1.3.png)

On the left side, there are two sections called [Geometry] and [Display Control]. In [Geometry], you can change or delete the type of surfaces or volumes, and in [Display Control], you can set the appearance of the graphics window.

In Geometry, select a face and right-click and select Edit/View. In the window that opens, change the name as follows 

+ mixingpipe\_surface → wall
+ mixingpipe\_surface_1 → outlet 
+ mixingpipe\_surface_2 → inlet1 
+ mixingpipe\_surface_3 → inlet2 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixingpipe/1.4.png"><br>
</p>

Create a hexahedron to densify the mesh around inlet1.

### Create hexahedron for refinement

Click the [Add] button and select [Hex] from the window that appears. A hexahedron is defined by the coordinates of two points, minimum and maximum, as follows

+ Name : Box 
+ Type : None
+ MIn. : (-0.5 -0.3 -1.8)
+ Max. : (0.5 0.3 0) 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixingpipe/1.7.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixingpipe/1.8.png"><br>
</p>

A volume called Box and a surface called Box\_surface have been added to the Geometry.

Right-click on the Box\_surface and select Edit/View. Change the [Type] to [None] because the surface of a hexahedron is not boundary. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixingpipe/1.9.png"><br>
</p>

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Region

Click the (+) icon at the top to create a region. Move the mouse to the intersection of the lines that appear in light green color in the graphics window and position it inside the computation domain. Click the [Add] button to complete the setup.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixingpipe/1.11.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixingpipe/1.12.png"><br>
</p>

Click the [Next] button to move on to the next step.
<!-------------------------------------------------------------------------------------------------->
## Base Grid

Set the number of grids to 20, 5, and 40. Click the [Generate] button to generate the base grid.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixingpipe/1.14.png"><br>
</p>


Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Castellation

In the Castellation step, we will refine the mesh inside the mixingpipe and Box created in the Geometry step. 

Click the (+) icon in [Volume Refinement] to create two groups. Set the inside of the mixingpipe to be refine at level 1 and the inside of the box to be refine at level 3, as shown below. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixingpipe/1.15.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixingpipe/1.16.png"><br>
</p>

Click the [Refine] button. 

Click the [Next] button to move on to the next step.
<!-------------------------------------------------------------------------------------------------->
## Snap

In the Snap step, the geometry is realized by moving grid points along the feature lines and surfaces of the geometry. 

With the default settings, click the [Snap] button. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixingpipe/1.17.png"><br>
</p>

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Boundary Layer

Click the (+) icon under [Configuration] to add a [Setting], and set it as follows

+ Number of Layers : 4
+ Thickness Model Specification : First and Expansion
+ Size Specification : relative
+ First Layer Thickness : 0.1
+ Expansion Ratio : 1.2
+ Min. Total Thickness : 0.3
+ Boundary : wall

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixingpipe/1.18.png"><br>
</p>

Click the [Apply] button. Final mesh is shown below.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixingpipe/1.19.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixingpipe/1.20.png"><br>
</p>

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Export

Click [Export as BaramFlow project] to export the mesh to the desired location. 



