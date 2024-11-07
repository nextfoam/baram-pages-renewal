# Species transport

## Mixing in duct 

[Download mesh](https://drive.google.com/file/d/19BS3wUfZZeh8A8Mqx2B1gSMwbylE-xXS/view?usp=sharing)

[Download simulation](https://drive.google.com/file/d/1VihUsXB47k3qHfJOZ6Lgm4THC3v6jsRw/view?usp=sharing)

### Introduction

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/species/intro1.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/species/intro1.png)

This is an example of an transient chemical species mixing. A two-dimensional duct with two openings is introduced with air at the top opening and water vapor at the bottom opening.

The simulation conditions are as follows 

+ solver : buoyantSimpleNFoam 
+ turbulence model : $Standard$ $k-\epsilon$ model
+ flow condition : 0.5 $m/s$ at inlet

### Start BaramFlow and load mesh

Run the program and select [New Case] from the launcher. In the launcher, select [Pressure-based] for [Solver Type] and [None] for [Multiphase Model].

Use the given polyMesh folder. In the top tab, click [File]-[Load Mesh]-[OpenFOAM] in that order and select the polyMesh folder. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/species/mesh1.png"><br>
</p>

### General

Change Time to Transient.

Set Gravity as (0 -9.81 0).

### (Models

Double click Species and select [Include].

### Materials

Click the (+) in the top right corner of the Materials panel to add a mixture. Select air and waterVapor as shown on the left in the image below and click 'Create Mixture' to create a mixture as shown on the right.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/species/material.png"><br>
</p>

### Cell zone Conditions

Double-click region0 in Cell Zone Conditions and change the Material to mixture.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/species/cellzone.png"><br>
</p>

### Boundary Conditions

Set the boundary type and values as shown below.

+ in-up : Velocity Inlet
    + Velocity Specification Method : Magnitudde, Normal to Boundary
    + Velocity Magnitude : 0.5 (m/s)
    + Turbulent Intensity : 0.1 (%)
    + Turbulent Viscosity Ratio : 1
    + mixture - air : 1
    + mixture - waterVapor : 0
    + Temperature : 300
    
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/species/inletBC1.png"><br>
</p>

+ in-down : Velocity Inlet
    + Velocity Specification Method : Magnitudde, Normal to Boundary
    + Velocity Magnitude : 0.5 (m/s)
    + Turbulent Intensity : 0.1 (%)
    + Turbulent Viscosity Ratio : 1
    + mixture - air : 0
    + mixture - waterVapor : 1
    + Temperature : 300
    
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/species/inletBC2.png"><br>
</p>


+ out : Pressure Outlet
    + Pressure : 0 (Pa)

+ down_surface, up : wall
    + noSlip

+ zMin, zMax : empty


### Numerical Conditions

In this example, we'll change the settings as shown below.

+ Pressure-Velocity Coupling Scheme : SIMPLE

+ Discretization Scheme
    + Time : Second Order Implicit
    + Pressure : Linear
    + Momentum : Second Order Upwind
    + Turbulence : Second Order Upwind
    + Species : Second Order Upwind

+ Under-Relaxation Factors
    + Pressure : 0.3
    + Momentum : 0.7
    + Turbulence : 0.7
    + Species : 1

+ Convergence Criteria : use default values

+ Max Iterations per Time Step : 100

+ Number of Correctors : 2

### Initialization

In this example, we'll change the settings as shown below.

+ Velocity : (0.5 0 0)
+ Pressure : 0
+ Scale of Velocity : 0.5
+ Turbulent intensity : 0.1
+ Turbulent viscosity ratio : 1
+ mixture - air : 1
+ mixture - waterVapor : 0

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/species/init1.png"><br>
</p>

Enter the value and click the Initialize button at the bottom. Then click the [File]-[Save] menu to save the case file.

### Run

Change the values as shown below, and click [Start Calculation] button.

+ Time Stepping Method : Fixed
+ Time Step Size : 0.001
+ End Time : 20
+ Save Interval : 0.1

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/species/runCondition1.png"><br>
</p>


### Post-processing

Click the parview button in [External tools] to open the paraview.

Change [Coloring] to [waterVapor].

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/species/pv2.png"><br>
</p>
