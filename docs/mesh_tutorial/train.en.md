# High speed train

[Download geometry file](https://drive.google.com/file/d/1r9S6y9TAx7m2eyg59KGejgYFvIazv1Px/view?usp=sharing) 

[Download BaramMesh folder](https://drive.google.com/file/d/1qZzOj1Jhs8KEwuUTqY5qBIFdF3GGnwq5/view?usp=sharing)

## Introduction 

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/train/intro.png"><br>
</p>

High-speed trains travel in the subsonic compressible flow regime with Mach numbers in the range of 0.3 to 0.4. In CFD, the pressure-based solver of SIMPLE algorithm is often used for low-speed flows and the density-based solver for high-speed flows. This challenge aims to validate the stability of Baram's incompressible solver, buoyantSimpleNFoam, in the subsonic compressible flow regime.

We use a high-speed train model with simplified vehicle connections, bogies (wheels), and pantographs, and the train speed is 400 km/h. 

With a mesh of about 2 million mesh, the convergence was achieved in about 300 iterations (about 20 minutes on an 8-core CPU). The convergence is much better compared to the results obtained using OpenFOAM's standard solvers buoyantSimpleFoam and rhoSimpleFoam.

<!-------------------------------------------------------------------------------------------------->
## Geometry

Use given stl file for train geometry.

Using [Hex6] for the computational domain and one hexahedron to create a dense mesh around the vehicle, set it up as shown in the following image. 

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/train/geom.png"><br> 
</p>

Click the [Import] button at the bottom to select the train.stl file. 

You will have a volume called Visualization\_Toolkit\_generated\_SLA\_File and a surface called Visualization\_Toolkit\_generated\_SLA\_File\_surface.

In Geometry, select a face and right-click and select Edit/View. In the window that opens, change the name as  train\_surface로 변경한다. 

### Create hexahedron for computational domain
 
Click the [Add] button and select [Hex6] and set values as follows

+ far : Use [Hex6] to define inlet, outlet, bottom, etc.
    + Type : None
    + Min./Max. : (-75 -40 0) / (260 0 40) 
    + Since it is symmetrical, we only create half of it in the y-axis. 

In Geometry, select surfaces and right-click and select Edit/View. In the window that opens, change the name as follows 

+ far\_xMin : inlet
+ far\_xMax : outlet
+ far\_yMin : side
+ far\_yMax : symmetry
+ far\_zMin : ground
+ far\_zMax : top

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/train/far.png"><br> 
</p>

### Create hexahedron for mesh refinement

Click the [Add] button and select [Hex] and set values as follows

* refine box : Use [Hex]
    + Type : None 
    + Min./Max. : (-18 -5 0) / (100 5 7) 
    
In Geometry, select Hex\_1\_surface and right-click and select Edit/View. In the window that opens, change the type to [None]

  <p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/train/hex.png"><br> 
</p>
   
Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Region

Click the (+) icon at the top to create a region. Move the mouse to the intersection of the lines that appear in light green color in the graphics window and position it inside the computation domain. Click the [Add] button to complete the setup.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/train/region.png"><br> 
</p>

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Base Grid

Select [Use Hex6] option and set the number of grids to 100, 20, and 20. Click the [Generate] button to generate the base grid.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/train/baseGrid.png"><br> 
</p>

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Castellation

Use default conditions for [Configuration] and [Advanced].

Define mesh refinement for the car.

Click the (+) icon under [Surface/Feature Refinement] and set the following settings for train\_surface

+ Surface Refinement
    + Minimum Level : 5
    + Maximum Level : 5
+ Feature Edge refinement Level : 5 
+ Surfaces : train\_surface

<p style="text-align: center">
     <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/train/train_refine_surface.png"><br> 
</p>

Click the (+) icon under [Volume Refinement] and set the following settings for Hex\_1

[공간 세분화(Volume Refinement)]의 (+) 아이콘을 누르고 Hex\_1에 대해 다음과 같이 설정한다. 

+ Volume Refinement Level : 4
+ Volume : Hex\_1

<p style="text-align: center">
     <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/train/train_refine_volume.png"><br> 
</p>

When you're done setting up, your screen should look like this

<p style="text-align: center">
     <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/train/train_refine_level.png"><br>
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

Create boundary layer at car body.

Click the (+) icon under [Configuration] to add a [Setting], and set it as follows

+ Number of Layers : 5
+ Thickness Model Specification : First and Expansion
+ Size Specification) : Relative
+ First Layer Thickness : 0.1
+ Expansion Ratio : 1.2
+ Min. Total Thickness : 0.3
+ Boundary : train\_surface

<p style="text-align: center">
     <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/train/train_blayer.png"><br> </p>

Use default settings for [Advanced Configuration].

<p style="text-align: center">
     <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/train/boundary.png"><br> 
</p>

Click [Apply] button to start boundary layer step.

The final mesh looks like this

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/train/train_final.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/train/train_final.png)

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Export

Click [Export as BaramFlow project] to export the mesh to the desired location. 



