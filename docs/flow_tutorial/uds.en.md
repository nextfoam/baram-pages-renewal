# User defined Scalar

## Fire in room 

[Download mesh](https://drive.google.com/file/d/1ySpMPSdtioU4DSJrWAKJEsCT0wihKo44/view?usp=sharing)

[Download simulation](https://drive.google.com/file/d/1NH75is3AIN0Kl1nSYRWy58OgKT8UTRCJ/view?usp=sharing)

### Introduction 

[![intro](https://github.com/nextfoam/baram-pages/raw/main/screenshots/fireInRoom/intro.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/fireInRoom/intro.png)

This problem simulates an indoor fire. Without calculating the chemical species and combustion reactions, calculate the calorific value and flue gas production over time using the fire growth curve as a source for the energy equation and a user-defined scalar equation. Give the source term in the hexagonal cell zone at the bottom of the table.

The fire growth curve is calculated for 25 seconds using the following quadratic function 

$S_h = 1000 \cdot t^2$

$S_{gas} = 5 \cdot 10^{-6} \cdot t^2$

The simulation conditions are as follows 

+ solver : buoyantSimpleNFoam
+ turbulence model : $standard$ $k-\epsilon$
+ density : Perfect Gas
+ viscosity : 1.79e-5 $kg/ms$

### Start BaramFlow and load mesh

Run the program and select [New Case] from the launcher. In the launcher, select [Pressure-based] for [Solver Type] and [None] for [Multiphase Model].

Use the given polyMesh folder. In the top tab, click [File]-[Load Mesh]-[OpenFOAM] in that order and select the polyMesh folder. 

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/fireInRoom/mesh.png"><br>
</p>

### General

Change Time to Transient.

Set Gravity as (0 0 -9.81).


### Models

For this example, we'll use $Standard$ $k-\epsilon$ model for turbulence.

Include Energy.

Add User-defined Scalar. Set the [Field Name] as gas and select diffusivity as [Laminar and Turbulent Viscosity].

Set viscosity coefficient for laminar and turbulence as 1 for both.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/fireInRoom/uds.png"><br>
</p>

### Materials

Set material properties as follows 

+ Density : Perfect Gas
+ Specific heat : 1006
+ Viscosity : 1.79e-05
+ Thermal Conductivity : 0.0245
+ Molecular Weight : 28.966

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/train/mat.png"><br>
</p>

### Cell zone Conditions

Set energy and gas source term at Hex_1. 

+ Specification Method : Value for Entire Cell Zone
+ Temporal Profile Type : Polynomial. Click [Edit] button and set values at (0 1 2)
    + For energy : (0 0 1000)
    + for gas : (0 0 5e-6)

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/fireInRoom/cellZone.png"><br>
</p>

### Boundary Conditions

Set the boundary type and values as shown below.

+ room, desk_surface : Wall
    + Velocity Condition : No Slip
    + Temperature : Adiabatic

+ door_surface, outlet : Pressure Outlet
    + Pressure  : 0

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/train/outletbc.png">
</p>

### Numerical Conditions

Set 20 for [Max Iteration per Time Step] and 2 for [Number of Correctors]. Use default values for the rest.

### Initialization

Set values as follows

+ Velocity : (0 0 0)
+ Pressure : 0
+ Temperature : 300
+ Scale of velocity : 1 
+ Turbulent Intensity : 1
+ Turbulent Viscosity Ratio : 10
+ User-defined Scalars - gas : 0

Enter the value and click the Initialize button at the bottom. Then click the [File]-[Save] menu to save the case file.

### Run

Change the values as shown below, and click [Start Calculation] button.

+ Time Stepping Method : Adaptive
+ Courant Number : 4
+ End Time : 25
+ Save Interval : 0.2

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/fireInRoom/run.png"><br>
</p>


### Post-processing

Click the parview button in [External tools] to open the paraview.

Change [Coloring] to gas. Click Play icon.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/fireInRoom/contour.png"><br>
</p>

