# DrivAer

[Download geometry file](https://drive.google.com/file/d/1vgJ6DTyna02EOOMAj8kZMxf6eDoM8mr8/view?usp=sharing) 

[Download BaramMesh folder](https://drive.google.com/file/d/1dawD37cqRtg8aPnHIgEOPY3hRzSS33OA/view?usp=sharing)

## Introduction 

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/drivAer/main.png"><br>
</p>

DrivAer is a real-world vehicle model for vehicle exterior design and aerodynamic testing used in the field of automotive engineering to simulate and evaluate the exterior shape and aerodynamic properties of vehicles. It was introduced to bridge the gap between simplified models and highly complex production vehicles. 

This is an example of how a mesh is created and calculated in BARAM using publicly available CAD files and compared to wind tunnel test results.

The geometry files can be downloaded for the Fastback, Smooth Underbody, with Mirrors, and with Wheels models from the pages linked below.

[https://www.epc.ed.tum.de/en/aer/research-groups/automotive/drivaer/geometry/](https://www.epc.ed.tum.de/en/aer/research-groups/automotive/drivaer/geometry/)


## Geometry

The geometry uses three STL files: the vehicle, front wheels, and rear wheels. Using Hex6 for the computational domain setup and 3 hexahedra to create a dense mesh around the vehicle, we set it up as shown in the following figure. 

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/drivAer/geom.png"><br> 
</p>

Click the [Import] button at the bottom to select stl files. Select body\_no\_wheel.stl, Wheels\_Front\_Smooth.stl, Wheels\_Rear\_Smooth.stl.

Click the [Add] button to create a box for farfield domain and 2 boxes to refine mesh.

* far : Use [Hex6] to define inlet, outlet, bottom, etc.
    + Type : None 
    + Min./Max. : (-14 0 -0.3) / (30 4 6) 
    + Since it is symmetrical, we only create half of it in the y-axis. The minimum value for the z-axis is a little higher because using the minimum value in the stl file would result in too bad a mesh where the circle of the wheel meets the plane of the floor.
* refineBox1 : Use [Hex]
    + Type : None
    + Min./Max. : (-2 0 -0.3) / (15 2 2.5)

In Geometry, select refineBox1\_surfacee and right-click and select Edit/View. In the window that opens, change the type to None
  
* refineBox2 : Use [Hex]
    + Type : None
    + Min./Max. : (-1 0 -0.3) / (7 1.2 1.5)

In Geometry, select refineBox2\_surfacee and right-click and select Edit/View. In the window that opens, change the type to None
   
* refineBox3 : Use [Hex]
    + Type : None
    + Min./Max. : (3 0 -0.3) / (5 1 1)

In Geometry, select refineBox3\_surfacee and right-click and select Edit/View. In the window that opens, change the type to None
  
Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Region

Click the (+) icon at the top to create a region. Move the mouse to the intersection of the lines that appear in light green color in the graphics window and position it inside the computation domain. Click the [Add] button to complete the setup.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/drivAer/region.png"><br> 
</p>

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Base Grid

Select [Use Hex6] option and set the number of grids to 220, 20, and 32. Click the [Generate] button to generate the base grid.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/drivAer/baseGrid.png"><br> 
</p>

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Castellation

Use default conditions for [Configuration] and [Advanced].

Define mesh refinement for car body and wheels.

Click the (+) icon under [Surface/Feature Refinement] and set the following settings for wheels

+ wheels
    + Group Name : wheel
    + Surface Refinement
        + Minimum Level : 5 
        + Maximum Level : 5 
    + Feature Edge refinement Level : 1 
    + Surfaces : Wheels\_Front\_Smooth\_surface\_0, Wheels\_Rear\_Smooth\_surface\_0

<p style="text-align: center">
     <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/drivAer/drivAer_refine_wheel.png"><br> 
</p>

Click the (+) icon under [Surface/Feature Refinement] and set the following settings for car body
  
+ body
    + Group Name : body
    + Surface Refinement
        + Minimum Level : 4 
        + Maximum Level : 4 
    + Feature Edge refinement Level : 5 
    + Surfaces : body\_no\_wheel\_surface

<p style="text-align: center">
     <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/drivAer/driAer_refine_body.png"><br> 
</p>

Click the (+) icon under [Volume Refinement] and set the following settings for refineBox1

+ Group Name : refine1
+ Volume Refinement Level : 1
+ Volume : refineBox1 

Once again, click the (+) icon under [Volume Refinement] and set the following settings for refineBox2

+ Group Name : refine2
+ Volume Refinement Level : 2
+ Volume : refineBox2 

Once again, click the (+) icon under [Volume Refinement] and set the following settings for refineBox3

+ Group Name : refine3
+ Volume Refinement Level : 3
+ Volume : refineBox3

When you're done setting up, your screen should look like this

<p style="text-align: center">
     <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/drivAer/drivAer_refine_level.png"><br>
</p>

To enable parallelization, click [Parallel]-[Environment] in the menu and enter the desired value for [Number of Cores]. 

Click the [Refine] button and the castellation step starts. 


Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Snap

Use the default settings and click the [Snap] button to start snap.

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Boundary Layer

Create boundary layer at car body and wheels.

Click the (+) icon under [Configuration] to add a [Setting], and set it as follows

+ Number of Layers : 4
+ Thickness Model Specification : First and Expansion
+ Size Specification : Relative
+ First Layer Thickness : 0.001
+ Expansion Ratio : 1.2
+ Min. Total Thickness : 0.0001
+ Boundary : body_no_wheel_surface_0, Wheels_Front_Smooth_surface_0, Wheels_Rear_Smooth_surface_0

<p style="text-align: center">
     <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/drivAer/drivAer_blayer.png"><br> 
</p>

Use default settings for [Advanced Configuration].

<p style="text-align: center">
     <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/drivAer/drivAer_boundary.png"><br> 
</p>

Click [Apply] button to start boundary layer step.

The final mesh looks like this

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/drivAer/finalMesh.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/drivAer/finalMesh.png)

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Export

Click [Export as BaramFlow project] to export the mesh to the desired location. 



