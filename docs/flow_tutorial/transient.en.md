# Transient Flow

## Vortex shedding 

[Download mesh](https://drive.google.com/file/d/1f-eW_euVw3nNLarKX_FCF7tVywuiHDnY/view?usp=sharing)

[Download simulation](https://drive.google.com/file/d/1ez8LCp7MGVcOpT7peHTAzt4G-wROafQa/view?usp=sharing)

### Introduction 

[![Mesh and velocity](https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.1.png "Mesh and velocity")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.1.png)

This example is a two-dimensional steady-state laminar flow simulation of vortex shedding around a two-dimensional cylinder with a Reynolds number of 100.

The simulation conditions are as follows 

+ solver : buoyantPimpleNFoam
+ turbulence model : laminar
+ Reynolds No. : 100

### Start BaramFlow and load mesh

Run the program and select [New Case] from the launcher. In the launcher, select [Pressure-based] for [Solver Type] and [None] for [Multiphase Model].

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/launcher.png"><br>
</p>

Use the given polyMesh folder. In the top tab, click [File]-[Load Mesh]-[OpenFOAM] in that order and select the polyMesh folder. 

### General

Change Time to Transient.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.2.png"><br>
</p>

### Models

For this example, we'll use Laminar for turbulence.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.3.png"><br>
</p>

### Materials

For this example, we use the condition of Reynolds number is 100 and velocity is 1.

Set the density and viscosity as follows

+ density : 1 $kg/m^3$
+ viscosity : 0.01 $kg/ms$ 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.4.png"><br>
</p>

### Boundary Conditions

Each boundary condition is set as follows

+ cylinder : Wall
    + Velocity Condition : No Slip

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.5.png"><br>
</p>

+ sym : Symmetry

+ out : Pressure Outlet
    + Total Pressure : 0 (Pa)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.6.png"><br>
</p>

+ in : Velocity Inlet
    + Velocity Specification Method : Magnitudde, Normal to Boundary
    + Profile Type : Constant
    + Velocity Magnitude : 1 (m/s)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.7.png"><br>
</p>

+ frontAndBackPlanes : Empty

### Reference Values

Set the Reference Value for the aerodynamic coefficient calculation as follows.

+ Area : 1
+ Density : 1
+ Length : 1
+ Pressure : 0
+ Velocity : 1 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.8.png"><br>
</p>

### Numerical Conditions

In this example, we'll change the settings as shown below.

+ Use Momentum Predictor : active

+ Discretization Schemes
    + Time : Second Order Implicit
    + Pressure : Momentum Weighted Reconstruct
    + Momentum : Second Order Upwind

+ Max iterations per Time Step : 10
+ Number of Correctors : 2

Use default conditions for the rest.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.9.png"><br>
</p>

### Monitor

Monitor the Drag/Lift Coefficient acting on the cylinder and the velocity/pressure at a point 1 meter from the center of the cylinder.

#### Drag/Lift Coefficient

+ Select [Add]-[Forces] button.
+ Set the values as follows
    + Write Interval : 1
    + Lift Direction : 0 1 0
    + Drag Direction : 1 0 0
    + Center of Rotation : 0 0 0
    + Boundaries : cylinder

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.10.1.png"><br>
</p>

#### Velocity and Pressure

+ Select [Add]-[Points] button.
+ Set the values as follows
    + Write Interval : 1
    + Field : Pressure
    + Coordinate : 1 0 0

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.10.2.png"><br>
</p>

In the same way, set up Velocity Magnitude monitoring for the same point.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.10.3.png"><br>
</p>

### Initialization

Set X-Velocity as 1 and use default values for the rest. 

Enter the value and click the Initialize button at the bottom. Then click the [File]-[Save] menu to save the case file.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.11.png"><br>
</p>

### Run

Change the values as shown below, and click [Start Calculation] button.

+ Time Stepping Method : Adaptive
+ Max Courant Number : 1
+ End Time : 150
+ Save Interval : 0.5
+ Data Write Format : Binary
+ Number of Cores : 4 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.12.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.18.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.19.png"><br>
</p>


<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.13.1.png"><br>Residuals
</p>


<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.13.2.png"><br>Monitorings
</p>

### Post-processing

Draw the velocity and pressure distribution around the cylinder.

Click the parview button in [External tools] to open the paraview.

Change the [Case Type] to [Decomposed Case].

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.14.png"><br>
</p>

Change [Solid Color] to U or p\_rgh and click [play] icon.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.15.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.16.png"><br>
</p>

<!-------------------------------------------------------------------------------------------->
## Time dependen Boundayr Condition

[Download mesh](https://drive.google.com/file/d/1pzp6DXomC0cyaxn0xzlpcneShrEkRW68/view?usp=sharing)

[Download simulation](https://drive.google.com/file/d/1dXzAUlGhnKvABA1bGgk-Us3JHDh8OyZB/view?usp=sharing)

### Introduction

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/profileBC/10.1.png"><br>
</p>

This example sets up a boundary condition where the velocity and temperature at the inlet change over time.

It uses the pitzDaily mesh from the OpenFOAM tutorial.

The computational conditions are as follows 

+ solver : buoyantPimpleNFoam
+ turbulence model : $Standard$ $k-\epsilon$
+ density : Perfect Gas
+ viscosity : 1.79e-5 $kg/ms$

### Start BaramFlow and load mesh

Run the program and select [New Case] from the launcher. In the launcher, select [Pressure-based] for [Solver Type] and [None] for [Multiphase Model].

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/launcher.png"><br>
</p>

Use the given polyMesh folder. In the top tab, click [File]-[Load Mesh]-[OpenFOAM] in that order and select the polyMesh folder. 

### General

Change Time to Transient.

### Models

For this example, we'll use $Standard$ $k-\epsilon$ model for turbulence.

Change Eergy to Include.

### Materials

For this example, we will use the properties of air.

+ Density : Perfect Gas (m/s)
+ Specific Heat (Cp) : 1004 $J/kgK$ (m/s)
+ Viscosity : 1.79e-05 $kg/ms$
+ Thermal Conductivity : 0.0245 $W/mK$

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/profileBC/Perfect Gas.png"><br>
</p>

### Boundary Conditions

Each boundary condition is set as follows

+ inlet : Velocity Inlet
    + Velocity Specification Method : Magnitude, Normal to Boundary
    + Velocity Profile Type : Temporal Distribution
        + piecewise linear : (0, 1) (0.1 2) (0.2 1.5)
    + Turbulent Intensity : 1 (%)
    + Turbulent Viscosity Ratio : 10
    + Temperature Profile Type : Temporal Distribution
        + piecewise linear : (0, 300) (0.1 400) (0.2 350)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/profileBC/10.3.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/profileBC/10.4.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/profileBC/10.5.png"><br>
</p>

+ outlet : Pressure Outlet
    + Total Pressure : 0 (Pa)

+ upperWall, lowerWall : Wall
    + Velocity Condition : No Slip
    + Temperature : Adiabatic

+ frontAndBack : empty

### Numerical Conditions

For Numerical Conditions, use default values.

### Monitor

Monitor flow rate and temperature at inlet. 

Select [Monitors]-[Add]-[Forces] and set values as shown below.

+ Surface Monitor 1
    + Report Type : Mass Flow Rate
    + Surface : inlet

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/profileBC/10.6.png"><br>
</p>

+ Surface Monitor 2
    + Report Type : Area-Weighted Average
    + Field Variable : Temperature
    + Surface : inlet

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/profileBC/10.7.png"><br>
</p>

### Initialization

Set velocity, pressure, temperature and turbulence as follows.

+ Velocity
    + X-Velocity : 0 (m/s)
    + Y-Velocity : 0 (m/s)
    + Z-Velocity : 0 (m/s)

+ Pressure
    + 0 (Pa)

+ Temperature : 300 (K)

+ Turbulence
    + Scale of Velocity : 1 (m/s)
    + Turbulent Intensity : 1 (%)
    + Turbulent Viscosity Ratio : 10

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/profileBC/10.8.png"><br>
</p>

Enter the value and click the Initialize button at the bottom. Then click the [File]-[Save] menu to save the case file.

### Run

Change the values as shown below, and click [Start Calculation] button.

+ Time Step Size : 0.001
+ End Time : 1
+ Save Interval(Every) : 0.1

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/profileBC/10.9.png"><br>
    그림 11.9
</p>

When the calculation is started, you'll see a graph of Residuals and Monitor as shown below.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/profileBC/10.10.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/profileBC/10.11.png"><br>
</p>

### Post-processing

Check the distribution of temperature and velocity over time. Click the paraview button in [External tools] to open paraview.

Change the [Solid Color] at the top to T.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/profileBC/10.12.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/profileBC/10.13.png"><br>
</p>

Press [Set Range] to adjust the temperature range to 340 - 350K.

Then click the Play icon at the top to see the temperature change over time.

The figure below shows the temperature distribution at the final moment.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/profileBC/10.14.png"><br>
</p>



