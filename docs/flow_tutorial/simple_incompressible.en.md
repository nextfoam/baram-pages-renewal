# Simple incompressible flow examples

## Mixing Pipe 

[Download mesh](https://drive.google.com/file/d/12CyYZO_FSl7Baz-MmbtXnBst02EGg5Tg/view?usp=sharing)

[Downlaod simulation](https://drive.google.com/file/d/19ge2_LwDOPlN2tYopBoOD5OGSadkzo65/view?usp=sharing)

### Introduction 

[![Mesh and velocity](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/1.1.png "Mesh and velocity")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/1.1.png)

This is Steady incompressible flow example. Predict the mixing of the flow inside a circular pipe with two inlets and one outlet.

Simulation conditions are as follows. 

+ solver : buoyantSimpleNFoam
+ turbulence model : Standard $k-\epsilon$ model
+ density : 1.225 $kg/m^3$
+ viscosity : 1.79e-5 $kg/ms$
+ flow condition : velocity of large inlet(in-1) is 5 m/s, small inlet(in-2) is 10 m/s, outlet pressure is 0

### Start BaramFlow

Run the program and select [New Case] from the launcher. In the launcher, select [Pressure-based] for [Solver Type] and [None] for [Multiphase Model].

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/launcher.png"><br>
</p>

### Mesh

Use the given polyMesh folder. In the top tab, click [File]-[Load Mesh]-[OpenFOAM] in that order and select the polyMesh folder. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/1.2.png"><br>
</p>

_※  Note: When reading OpenFOAM mesh, select the “polyMesh” or “constant” folder. OpenFOAM's mesh is a folder named polyMesh under the constant folder for a single region, or multiple polyMesh folders under the constant folder for multiple regions._

### General

For this example, we'll use default conditions.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/1.3.png"><br>
</p>

### Models

For this example, we'll use Standard $k-\epsilon$ model for turbulence.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/1.4.png"><br>
</p>

### Materials

For this example, we will use the properties of air.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/1.5.png"><br>
</p>

### Boundary Conditions

You can set boundary values for multiple boundaries. Each boundary will turn red when selected. 

Right-clicking on a boundary allows you to change the boundary type, and double-clicking or clicking the 'Edit' button below opens a window where you can set the value.

Each boundary condition is set as follows

+ in-1 : Velocity Inlet
    + Velocity Magnitude : 5 (m/s)
    + Turbulent Intensity : 1 (%)
    + Turbulent Viscosity Ratio : 10
      
+ in-2 : Velocity Inlet
    + Velocity Magnitude : 10 (m/s)
    + Turbulent Intensity : 1 (%)
    + Turbulent Viscosity Ratio : 10
      
+ out : Pressure Outlet
    + Pressure : 0 (Pa)
      
+ wall
    + Velocity Condition : No Slip

### Numerical Conditions

In this example, we'll change the settings as shown below.

+ Pressure-Velocity Coupling Scheme : SIMPLEC

+ Discretization Scheme
    + Pressure : Momentum Weighted Reconstruct
    + Momentum : Second Order Upwind
    + Turbulence : Second Order Upwind

+ Under-Relaxation Factors
    + Pressure, Momentum, Turbulence : 0.9

+ Convergence Criteria
    + Pressure : 0.0001
    + Momentum : 0.001
    + Turbulence : 0.001

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/1.7.1.png"><br>
</p>


### Monitor

Monitor the pressure at the location (0, 0, 1).

Select [Solution]-[Monitors] and click [Add]-[Points] at the bottom of the window to set it up as shown below.

+ Point Monitor
    + Write Interval : 1
    + Field : Pressure
    + Coordinate : (0, 0, 1)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/1.8.png"><br>
</p>

### Initialization

Initial Condition allows you to enter the velocity and pressure in x, y, and z as initial values.

If you are using a turbulence model, you can enter the Velocity Scale, Turbulent Intensity, and Viscosity Ratio values, and the $k$ and $\epsilon$ values will be calculated and used.

+ Velocity
    + X-Velocity : 0 (m/s)
    + Y-Velocity : 0 (m/s)
    + Z-Velocity : 0 (m/s)

+ Pressure
    + 0 (Pa)

+ Turbulence
    + Scale of Velocity : 5 (m/s)
    + Turbulent Intensity : 1 (%)
    + Turbulent Viscosity Ratio : 10

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/1.9.png"><br>
</p>

Enter the value and click the Initialize button at the bottom. Then click the [File]-[Save] menu to save the case file.

### Run

For this example, we'll use default values.

Click [Run]-[Start Calculation] button.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/residual.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/residual.png)
<p align='center'>residual & monitoring graph</p>

### Post-processing

BARAM uses ParaView for post-processing. To start post-processing, click the ParaView button in [External tools].

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/1.10.png"><br>
</p>

When running ParaView, the following features are required

+ Skip Zero Time: Shows the results excluding the initial value.

+ Case Type: Set according to the number of CPUs.
    + Reconstructed Case: Single core simulation
    + Decomposed Case : Parallel simulation case

+ Mesh Regions: You can set the internal mesh, boundary surface, etc. you want to see.

+ Cell Arrays: You can set the physical quantities you want to see.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/1.11.png"><br>
</p>

#### Scalar distribution at boundary

Plot the pressure distribution on the wall. The initial settings are as follows.

+ Skip Zero Time: Disabled
+ Mesh Regions: internalMesh - enabled
+ Rest : Default

$p_rgh$ is the pressure minus the term due to gravity ($\rho gh$), which is the same as the pressure when gravity is not considered, such as in this problem. $p_rgh$ is the relative pressure relative to the operating pressure and $p$ is the absolute pressure.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/1.12.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/1.13.png"><br>
</p>


#### Axial cross-sectional Scalar distributions

Check the pressure distribution inside the pipe. Click the slice button and change the orientation to Y-normal to see the pressure inside the pipe.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/1.14.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/1.15.png"><br>
</p>

<!------------------------------------------------------------------------------------------------------------->
## Cantilever Beam 

[Download mesh](https://drive.google.com/file/d/1bk5svPtGY4So9_1C18lgWcAy6lOsb524/view?usp=sharing)

### Introduction 

This example is a steady-state incompressible flow analysis example. This is a 1-way Fluid Structure Interaction(FSI) example to determine how a cantilever beam is deformed by air entering from the inlet region.

The cantilever beam is 5 mm thick, 50 mm long, and 150 mm high. We model only half of it and impose a symmetry boundary condition.

The geometry and mesh are shown in the figure below.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/1.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/2.png"><br>
</p>

Simulation conditions are as follows. 

+ solver : buoyantSimpleNFoam
+ turbulence model : $Standard$ $k-\epsilon$
+ density : 1.225 $kg/m^3$
+ viscosity : 1.79e-5 $kg/ms$
+ flow condition : 80 $m/s$ at inlet

### Start BaramFlow and load mesh

Run the program and select 'New Case' from the launcher. In the launcher, select Pressure-based for 'Solver Type' and None for 'Multiphase Model'.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/launcher.png"><br>
</p>

Use the given polyMesh folder. In the top tab, click [File]-[Load Mesh]-[OpenFOAM] in that order and select the polyMesh folder. 

### General

For this example, we'll use default conditions.

### Models

For this example, we'll use Standard $k-\epsilon$ model for turbulence.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/3.png"><br>
</p>

### Materials

For this example, we will use the properties of air.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/4.png"><br>
</p>

### Boundary Conditions

Each boundary condition is set as follows

+ Hex6_1_xMin : velocity Inlet
    + Velocity Specfication Method : Magnitude, Normal to Boundary
    + Profile Type : Constant
    + Velocity Magnitude : 80 (m/s)
    + Turbulent Intensity : 1 (%)
    + Turbulent Viscosity Ratio : 10

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/5.png"><br>
</p>

+ Hex6_1_zMax : velocity Inlet
    + Velocity Specfication Method : Component
    + Profile Type : Constant
    + X-Velocity : 80 (m/s)
    + Y-Velocity : 0 (m/s)
    + Z-Velocity : 0 (m/s)
    + Turbulent Intensity : 1 (%)
    + Turbulent Viscosity Ratio : 10

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/Hex6_1_zMax.png"><br>
</p>

+ Hex6_1_xMax : Pressure Outlet
    + Pressure : 0 (Pa)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/6.png"><br>
</p>

+ Hex6_1_yMin, Hex6_1_yMax : Symemtry

+ cantileverBeam_surface_0, Hex6_1_zMin : Wall
    + Velocity Condition : No Slip


### Numerical Conditions

In this example, we'll change the settings as shown below.

+ Pressure-Velocity Coupling Scheme : SIMPLEC

+ Discretization Scheme
    + Momentum : Second Order Upwind
    + Turbulence : First Order Upwind

+ Under-Relaxation Factors
    + Pressure : 0.9
    + Momentum : 0.9
    + Turbulence : 0.9

+ Convergence Criteria
    + Pressure : 0.001
    + Momentum : 0.001
    + Turbulence : 0.001

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/9.png"><br>
</p>


### Monitor

In this example, we will monitor the force on the Cantilever. Go to [Monitors]-[Add]-[Forces] and select cantileverBeam\_surface\_0.

Then, set up the Surface Montior as shown below.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/7.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/8.png"><br>
</p>

### Initialization

Change the values as shown below

+ Velocity
    + X-Velocity : 80 (m/s)
    + Y-Velocity : 0 (m/s)
    + Z-Velocity : 0 (m/s)

+ Pressure
    + 0 (Pa)

+ Turbulence
    + Scale of Velocity : 80 (m/s)
    + Turbulent Intensity : 0.1 (%)
    + Turbulent Viscosity Ratio : 10

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/11.png"><br>
</p>

Enter the value and click the Initialize button at the bottom. Then click the [File]-[Save] menu to save the case file.

### Run

Change the values as shown below, and click [Start Calculation] button.

+ Number of Iterations : 1,000
    + Save Interval : 100
    + Data Write Format : Binary
+ Selct [Parallel]-[Environment] in menu. Set Number of Cores as you want and select [Local Machine] for [Parallel Type].

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/12.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/13.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/14.png"><br>
</p>

When the calculation is started, you'll see a graph of Residuals as shown below.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/15.png"><br>
</p>

<!--
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/16.png"><br>
</p>
-->

### Post-processing

To start post-processing, click the ParaView button in [External tools].

We will plot the pressure field around a cantilever. 

Change the Case Type to Decomposed Case. 

Click the Slice button to cut a cross section.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/17.png"><br>
</p>

Change the Axis Direction to Y Normal and the Origin to (200, 115, 125).


<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/18.png"><br>
</p>

Then change the p at the top to p\_rgh.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/19.png"><br>
</p>

The pressure distribution around the cantilever is shown in the figure below.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/20.png"><br>
</p>
