# AhmedBody

[Download geometry file](https://drive.google.com/file/d/1gxuKBcN6puyEF6Yv6VEYSNUz-tlHyAuD/view) 

[Download BaramMesh folder](https://drive.google.com/file/d/1A0bp1w8HKox4DbQBxmaG1kNvZeCbMrxF/view?usp=sharing)

## Introduction 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/intro.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/intro.png)

S.R. Ahmed used a simplified automobile model to experimentally observe how the flow structure changes with the angle of inclination of the rear. Since then, this problem has been used to validate automotive external aerodynamic analyses. In this example, a mesh for external flow analysis is created for a geometry with a 25° rear tilt angle.

_ref : S.R. Ahmed, G. Ramm, Some Salient Features of the Time-Averaged Ground Vehicle Wake, SAE-Paper 840300, 1984_

## Geometry

### Import geometry

Use given ahmed.stl file as geometry.

Click the [Import] button at the bottom to select the ahmed.stl file. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/ahmedBody/1.png"><br>
</p>

When the geometry file is read, a window appears as shown below.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/ahmedBody/ahmed_import.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/ahmedBody/ahmed_import.png)

Create farfield boundary for the external flow analysis of a car. 

Click the [Add] button and select [Hex6] and set values as follows

+ Name : Hex6_1 
+ Type : None 
+ MIn. : (-4.5 -0.05 0)
+ Max. : (10 5 3) 

Since it is a left-right symmetric shape, we give it a minimum z coordinate of 0 to make the mesh only half in the +z direction. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/ahmedBody/ahmed_hex6.png"><br>
</p>

In Geometry, select a face and right-click and select Edit/View. In the window that opens, change the name as follows 

+ Hex6\_1\_xMin → inlet 
+ Hex6\_1\_xMax → outlet 
+ Hex6\_1\_yMin → wall 
+ Hex6\_1\_yMax → sky 
+ Hex6\_1\_zMin → left\_side 
+ Hex6\_1\_zMax → right\_side 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/ahmedBody/4.png"><br>
</p>

### Create hexahedra for refinement

To specify a volume to refine mesh, click the [Add] button and select [Hex] and set values as follows

+ Name : Refinement1
+ Type : None 
+ MIn. : (-0.6 -0.05 0)
+ Max. : (1 0.4 0.27)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/ahmedBody/5.png"><br>
</p>

A Refinement1\_surface is created in [Geometry]. Select this and right-click Edit. Change the [Type] to [None].

Once again, click the [Add] button and select [Hex] and set values as follows

+ Name : Refinement2 
+ Type : None
+ MIn. : (-0.9 -0.05 0)
+ Max. : (1.7 0.6 0.5)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/ahmedBody/6.png"><br>
</p>

A Refinement2\_surface is created in [Geometry]. Select this and right-click Edit. Change the [Type] to [None].

The final geometry looks like this

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/ahmedBody/ahmed_add.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/ahmedBody/ahmed_add.png)

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Region

Click the (+) icon at the top to create a region. Move the mouse to the intersection of the lines that appear in light green color in the graphics window and position it inside the computation domain. Click the [Add] button to complete the setup.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/ahmedBody/ahmed_region.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/ahmedBody/ahmed_region.png)

Click the [Next] button to move on to the next step.
<!-------------------------------------------------------------------------------------------------->
## Base Grid

Select [Use Hex6] option and set the number of grids to 100, 35, and 20. Click the [Generate] button to generate the base grid.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/ahmedBody/ahmed_basegrid.png"><br>
</p>


Click the [Next] button to move on to the next step.
<!-------------------------------------------------------------------------------------------------->
## Castellation

Use default conditions for [Configuration] and [Advanced].

Define mesh refinement for Refinement1.

Click the (+) icon under [Volume Refinement] and set the following settings for Refinement1 

+ Group Name : Refinement1
+ Volume Refinement Level : 5 
+ Volume : refinement1

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/ahmedBody/10.png"><br>
</p>

Once again, click the (+) icon and set the following settings for Refinement2

+ Group Name : Refinement2
+ Volume Refinement Level : 3 
+ Volume : refinement2

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/ahmedBody/11.png"><br>
</p>

To enable parallelization, click [Parallel]-[Environment] in the menu and enter the desired value for [Number of Cores]. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/ahmedBody/parallel.png"><br>
</p>

Click the [Refine] button and the castellation step starts. 

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Snap

Use the default settings and click the [Snap] button to start snap.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/ahmedBody/ahmed_snap.png"><br>
</p>


Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Boundary Layer

Click the (+) icon under [Configuration] to add a [Setting], and set it as follows

+ Number of Layers : 5
+ Thickness Model Specification : First and Expansion
+ 높이 지정 방법(Size Specification : relative
+ First Layer Thickness : 0.1
+ Expansion Ratio : 1.2
+ Min. Total Thickness : 0.3
+ Boundary : nose1, nose2, nose3, nose5, rear, side, slant, top, leg, bottom

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/ahmedBody/ahmed_layer.png"><br>
</p>

Click [Apply] button to start boundary layer step.

The final mesh looks like this

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/ahmedBody/ahmed_final.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/ahmedBody/ahmed_final.png)

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Export

Click [Export as BaramFlow project] to export the mesh to the desired location. 



