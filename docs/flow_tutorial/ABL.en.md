# Atmospheric Boundary Layer
 
## Validation of boundary conditions

[Download mesh](https://drive.google.com/file/d/19kMYRiWaB84kaUzCoobMRZBKCc_uKxVU/view?usp=drive_link)

[Download simulation](https://drive.google.com/file/d/1wMjgdSaHfAD8GzPNRVkUayhCyOFwK_9C/view?usp=sharing)

### Introduction 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/8.1.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/8.1.png)

This example is an atmospheric boundary layer flow analysis example. The domain is 600 m X 50 m X 600 m. The objective is to verify that the flow distribution at the inlet given by the atmospheric boundary layer conditions is maintained to the outlet.

The simulation conditions are as follows 

+ solver : buoyantSimpleNFoam
+ turbulence model : standard 𝑘 − ε model
+ density : 1.225 𝑘𝑔/㎥
+ viscosity : 1.79e-5 𝑘𝑔/𝑚s

The atmospheric boundary layer conditions are provided by OpenFOAM and use the equations from the following paper by D.M. Hargreaves and N.G. Wright.

*"On the use of the k-epsilon model in commercial CFD software to model the neutral atmospheric boundary layer"*

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/8.2.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/8.3.png"><br>
</p>

### Start BaramFlow and load mesh

Run the program and select [New Case] from the launcher. In the launcher, select [Pressure-based] for [Solver Type] and [None] for [Multiphase Model].

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/launcher.png"><br>
</p>

Use the given polyMesh folder. In the top tab, click [File]-[Load Mesh]-[OpenFOAM] in that order and select the polyMesh folder. 

### General

For this example, we'll use default conditions.

### Models

For this example, we'll use $Standard$ $k-\epsilon$ model for turbulence.


<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/8.4.png"><br>
</p>

### Materials

Use material properties of air. 

### Boundary Conditions

Set the boundary type and values as shown below.

+ inlet : ABL Inlet
    + Flow Direction : 1 0 0
    + Ground-Normal Direction : 0 0 1
    + Reference Flow Speed : 7 (m/s)
    + Reference Height : 9 (m)
    + Surface Roughness Length : 0.0002 (m)
    + Minimum z-coordinate : 0.0 (m)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/8.5.png"><br>
</p>

+ Sea : Wall
    + Velocity Condition : Atmospheric Wall

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/8.6.png"><br>
</p>

+ outlet : Pressure Outlet
    + Total Pressure : 0 (Pa)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/8.7.png"><br>
</p>

+ minY, maxY, sky : Symmetry

### Numerical Conditions

In this example, we'll change the settings as shown below.

+ Pressure-Velocity Coupling Scheme : SIMPLE

+ Discretization Schemes
    + Pressure : Momentum Weighted Reconstruct
    + Momentum : Second Order Upwind
    + Turbulence : First Order Upwind

+ Convergence Criteria : 1e-6 (for all)

+ Advanced
    + Set [Maximum Viscosity Ratio] as 1e7 - Because the computational domain is very large, the distribution of turbulent kinetic energy will not be preserved if we use default value(1e5).

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/8.8.1.png"><br>
</p>


### Initialization

Change the settings as shown below.

+ X-Velocity : 7 (m/s)
+ Pressure : 0 (Pa)
+ Turbulence
    + Scale of Velocity : 7 (m/s)
    + Turbulent Intensity : 1 (%)
    + Turbulent Viscosity Ratio : 10

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/8.9.png"><br>
</p>

Enter the value and click the Initialize button at the bottom. Then click the [File]-[Save] menu to save the case file.

### Run

Change the values as shown below, and click [Start Calculation] button.

+ Number of Iterations : 1000
+ Save Interval : 1000
+ Data Write Format : Binary

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/8.10.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/residual.png"><br>residual plot
</p>

### Post-processing

Check the velocity profile. Click the parview button in [External tools] to open the paraview.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/8.12.png"><br>
</p>

Change [Solid Color] to U.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/8.13.png"><br>
</p>

Change [Slice] icon and set as follows

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/8.16.png"><br>
</p>

Click the [Plot Over Line] icon in the top toolbar and create two lines, one for the entrance and one for the exit, as shown below.

Use the lines at the entrance and exit to quantitatively verify that the velocity profile remains the same.

line 1 (inlet)

+ Point1 : 0 25 0
+ Point2 : 0 25 600
+ X Array Name : U_X
+ Series Parameters : Points_Z

line 2 (exit)

+ Point1 : 600 25 0
+ Point2 : 600 25 600
+ X Array Name : U_X
+ Series Parameters : Points_Z

You can change the color of paramters at [Series Parameters].

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/8.14.1.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/8.14.2.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/8.14.3.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/8.14.4.png"><br>
</p>

You can see the velocity distribution over height in the figure below.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/8.15.png"><br>
</p>

In the figure above, the red line is the velocity profile in the inlet region and the green line is the velocity profile in the outlet region.

