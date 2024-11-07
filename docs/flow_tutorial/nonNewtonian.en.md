# Non-Newtonian Fluid

## Blood flow of FDA Nozzle 

[Download mesh](https://drive.google.com/file/d/1fq6KLFt2mpidQchHwQ9j9mRgty5rlr3G/view?usp=sharing)

[Download simulation](https://drive.google.com/file/d/1GT4riUP0E9Niwrw6LWrh0ZV0l49YlZJG/view?usp=sharing)

### Introduction 

[![include surface's own gap](https://github.com/nextfoam/baram-pages/raw/main/screenshots/blood/intro.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/blood/intro.png)

FDA's Nozzle Challenge is a benchmark testing program for simulation validation organized by the U.S. Food and Drug Administration. In this program, experimental and simulation studies were conducted on small nozzles that reflect the characteristics of blood-carrying medical devices, and the following papers by Trias et al. were referenced.

_Trias, Miquel, Antonio Arbona, Joan Massó, Borja Miñano, and Carles Bona. “FDA’s nozzle numerical simulation challenge: non-Newtonian fluid effects and blood damage.” PloS one 9, no. 3 (2014): e92638._

Based on the nozzle throat diameter, the Reynolds number is 500, and the non-Newtonian fluid viscosity uses the Carreau model.

### Start BaramFlow and load mesh

Run the program and select [New Case] from the launcher. In the launcher, select [Pressure-based] for [Solver Type] and [None] for [Multiphase Model].

Use the given polyMesh folder. In the top tab, click [File]-[Load Mesh]-[OpenFOAM] in that order and select the polyMesh folder. 

Geometry and mesh are as follows

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/blood/fda-diagram.png"><br>
</p>

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/blood/fda-mesh.png"><br>
</p>


### General

For this example, we'll use default conditions.

### Models

For this example, we'll use Laminar for turbulence.

### Materials

Click the (+) in the Materials Configuration to add a waterLiquid. 

Open the waterLiquid edit window and set the following settings.

+ Name : blood
+ Density : 1056
+ Viscosity : Bird-Carreau

Click the Edit button next to Bird-Carreau and enter the coefficients as follows

+ Zero shear viscosity : 0.0001515
+ Infinite shear viscosity : 3.3144e-6
+ Relaxation time = 8,2
+ Power-law index = 0.2128
+ Linearity deviation, a = 0.64

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/blood/material.png"><br>
</p>

### Cell zone Conditions

Double-click region0 to bring up the settings window. Select Material as blood.

### Boundary Conditions

Set the boundary type and values as shown below.

+ in : Velocity Inlet
    + Velocity magnitude : 0.04607

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/blood/bc-in.png">
</p>

+ outlet : Pressure Outlet
    + Pressure : 0

+ wall, wall1, wall2 : Wall
    + Velocity : No Slip

+ back, front : Wedge


### Numerical Conditions

Set [Convergence Criteria]-[Pressure] and [Momentum] as 1e-6 for both

### Initialization

Set the value for velocity and pressure as follows

+ Velocity : (0.04607 0 0)
+ Pressure : 0

Enter the value and click the Initialize button at the bottom. Then click the [File]-[Save] menu to save the case file.

### Run

Change the values as shown below, and click [Start Calculation] button.

+ Number of Iterations : 2000
+ Save Interval : 500

When the calculation is started, you can see the graphs of Residuals as shown below.

[![intro](https://github.com/nextfoam/baram-pages/raw/main/screenshots/blood/residual.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/blood/residual.png)


### Post-processing

Click the parview button in [External tools] to open the paraview.

Change [Coloring] to U.

To see a graph of velocity on an axis, select [Plot Over Line] from the Fileters menu.

For Line Parameters, enter (-0.1 0 0.001) for Point1 and (0.1 0 0.001) for Point2. 

In the [X Axis Parameters], disable [Use Index for XAxis] and select [Point_X for the X Array Name].

In [Series Parameters], select U_X and click the Apply button to see the graph below.

[![intro](https://github.com/nextfoam/baram-pages/raw/main/screenshots/blood/paraview.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/blood/paraview.png)

Run 'Save Data' from the File menu to get the velocity along the axis as a data file. The figure below plots this data against the experimental results and the results for a constant viscosity.

[![intro](https://github.com/nextfoam/baram-pages/raw/main/screenshots/blood/result.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/blood/result.png)

