# Batch Run

## Angle of Attack sweep (RAE2822 airfoil)

[Download mesh](https://drive.google.com/file/d/1XfaXhTFvdUD5P3-avf8ShqQpn-5D25iy/view?usp=sharing)

[Download simulation](https://drive.google.com/file/d/1NVP39oK6pboQgZoitJndGW-YSfV85eDD/view?usp=sharing)

### Introduction

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/RAE2822/rae-mesh.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/RAE2822/rae-mesh.png)

This example performs a batch run of the flow analysis of the RAE2822 transonic airfoil as the angle of attack changes. The mesh is used from the RAE2822 transonic airfoil tutorial.

The computational conditions are as follows

+ solver : TSLAeroFoam
+ turbulence model : Inviscid(Euler)
+ Mach number : 1.5
+ farfield pressure : 100000 Pa
+ farfield temperature : 288 K
+ Angle of Attack : -20 to 20 deg, 2 degree interv

### Start BaramFlow and load mesh

Run the program and select 'New Case' from the launcher. In the launcher, select [Density-based] for [Solver Type].

Use the given polyMesh folder. In the top tab, click [File]-[Load Mesh]-[OpenFOAM] in that order and select the polyMesh folder.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/RAE2822/launcher-densityBased.png"> 
    <br> launcher 
</p>


<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/1.2.png"><br>
</p>

### General

Run the program and select 'New Case' from the launcher. In the launcher, select [Density-based] for [Solver Type] and [None] for [Multiphase Model].

Use the given polyMesh folder. In the top tab, click [File]-[Load Mesh]-[OpenFOAM] in that order and select the polyMesh folder.


### Models

For this example, we'll use Inviscid for turbulence.

### Materials

Set Density as Perfect Gas, Viscosity as Sutherland.

### Define user parameter

The user parameter required for a batch run are angle of attack, velocity, drag direction, lift direction, etc. and are defined as follows

+ AOA: Angle of attack, not used as a variable, but used when calculating other variables.
+ UX, UY: Velocity in x,y direction, used to set initial conditions.

Go to [Solution - Run] to declare the variables.

Click the Edit button of [User Parameters] to open the window and click (+) next to [Parameter Values] to add them one by one as shown below. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/batchRun-RAE2822/batchRAE-editParameter.png"> 
    <br> 사Define user parameter
</p>


### Boundary Conditions

Set the boundary type and values as shown below.

* wing
    + Wall - No slip, adiabatic 

* farfield_in, farfield_out
    + Far-Field Riemann
    + Flow Direction
        + Specification Method : AOA and AOA
        + Direction at AOA=0, AOS=0 : Drag direction (1 0 0), Lift direction (0 1 0)
        + Angle of Atteck : $AOA
        + Angle of Sideslip : 0 
    + Mach Number : 1.5
    + Static Pressure : 100000
    + Static Temperature : 288 
  
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/batchRun-RAE2822/batchRAE-farfield.png" > 
    <br> farfield Riemann condition
</p>

+ frontAndBackPlanes
    + Empty
  
### Reference Values

Set values as follows

+ Area, Length : 0.3048(chord of airfoil)
+ Density : 1.21(farfield condition)
+ Pressure : 100000(farfield condition)
+ Velocity : 510.2611(farfield condition)

### Numerical Conditions

Set [Formulation] as [Implicit], [Flux Type] as [Roe-FDS] and [Entropy Fix Coefficient] as 0.5. 

Set [Discretization Schemes] as [Second Order Upwind] for Flow.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/batchRun-RAE2822/rae-nume.png" > 
    <br> Numerical Conditions
</p>

### Monitor

Select [Add]-[Forces] and set values as follows

+ Flow Direction
    + Specification Method : AOA and AOA
    + Direction at AOA=0, AOS=0 : Drag direction (1 0 0), Lift direction (0 1 0)
    + Angle of Atteck : $AOA
    + Angle of Sideslip : 0 
+ Center of Rotation : (0 0 0)
+ Boundaries : wing

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/batchRun-RAE2822/batchRAE-monitor.png" > 
    <br>
</p>

### Initialization

Enter the value and click the Initialize button at the bottom. Then click the [File]-[Save] menu to save the case file.

+ Velocity : ($UX, $UY, 0)
+ Pressure : 100000
+ Temperature : 288

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/batchRun-RAE2822/batchRAE-init.png" > 
    <br> Initialization
</p>

### Run

Change the values as shown below

+ Number of Iterations : 3000
+ Courant Number : 1000
+ Save Interval : 500

Click [Switch To Batch Running Mode] button, then the [Batch Cases] settings section appears, as shown in the image below.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/batchRun-RAE2822/batchRAE-batchCases.png" > 
    <br> batch case setting
</p>

Select the file that sets the conditions for each variable by clicking the import button. The condition setting file can be in csv (comma separated values) or xlsx file format.

In this example, it is an excel file as shown below.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/batchRun-RAE2822/batchRAE-excel.png" > 
    <br> 
</p>

[Downlaod excel file](https://drive.google.com/file/d/1KOb8dQ3D1b2gYoWnwmkhgfGxySfArUBP/view?usp=sharing)

After you click the Import button and select the file above, the [Batch Cases] section will look like the following image.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/batchRun-RAE2822/batchRAE-batchCases1.png" > 
<br> 
</p>

Select the entire case by dragging the mouse and press the right mouse button to select [Schedule Calculation] and a check mark will appear in the [Calc.] column. Only the checked items will be calculated. 

Click [Start Calculation] to start the calculation sequentially. 

An arrow appears on the leftmost column to indicate the case currently being calculated. Completed cases are displayed in green in the Result colume, and cases that diverged during the calculation are displayed in red.

[![residual](https://github.com/nextfoam/baram-pages/raw/main/screenshots/batchRun-RAE2822/batchRAE-run.png "residual")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/batchRun-RAE2822/batchRAE-run.png)

### Post-processing

After the calculation, select a case from Batch Cases and right-click [Load] to activate the results of the case and see the residual and monitor graph. Click the paraview button in External tools to run paraview and select Pressure, and you can see the following distribution.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/batchRun-RAE2822/batchRAE-paraview.png" > 
    <br> pressure at the condition of AoA is -20
</p>


