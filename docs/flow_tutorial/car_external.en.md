# Car External Flow 

## Ahmed body

[Downlaod mesh](https://drive.google.com/file/d/1qVQqF6oavui3NCoAQ2QCpSmo824CVbx_/view?usp=sharing)

[Downlaod simulation](https://drive.google.com/file/d/1bvY3-XtcuimHAnliKtSojHWPNmcYnmZr/view?usp=sharing)

### Introduction 

[![intro](https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/intro.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/intro.png)

S.R. Ahmed used a simplified automobile model to experimentally observe the change in flow structure as a function of rearward inclination angle. Since then, this problem has been used to validate automotive external aerodynamic analyses. This example uses steady-state incompressible flow conditions for a velocity of 40 m/s at a rearward tilt angle of 25°. 

ref : _S.R. Ahmed, G. Ramm, Some Salient Features of the Time-Averaged Ground Vehicle Wake, SAE-Paper 840300, 1984_

In the paper Drag coefficient(Cd) is 0.285. Simulation result is 0.287, 0.7% difference.

Simulation conditions are as follows. 

+ solver : buoyantSimpleNFoam
+ turbulence model : $Realizable$ $k-\epsilon$ model
+ density : 1.2 $kg/m^3$
+ viscosity : 1.8e-5 $kg/ms$
+ flow condition : 40 $m/s$ at inlet

### Start BaramFlow and load mesh

Run the program and select [New Case] from the launcher. In the launcher, select [Pressure-based] for [Solver Type] and [None] for [Multiphase Model].

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/launcher.png"><br>
</p>

Use the given polyMesh folder. In the top tab, click [File]-[Load Mesh]-[OpenFOAM] in that order and select the polyMesh folder. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.1.1.png"><br>
</p>

### General

For this example, we'll use default conditions.

### 모델(Models)

For this example, we'll use $Realizable$ $k-\epsilon$ model for turbulence.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.2.png"><br>
</p>

### Materials

Material properties of air is as follows 

+ air
    + Density : 1.2 𝑘𝑔/㎥ 
    + Viscosity : 1.8e-5 𝑘𝑔/𝑚s

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.3.png"><br>
</p>

### Boundary Conditions

Set the boundary type and values as shown below.

+ minx : velocity Inlet
    + Velocity Specfication Method : Magnitude, Normal to Boundary
    + Profile Type : Constant
    + Velocity Magnitude : 40 (m/s)
    + Turbulent Intensity : 1 (%)
    + Turbulent Viscosity Ratio : 10

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.4.png"><br>
</p>

+ maxx : Pressure Outlet
    + Pressure : 0 (Pa)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.5.png"><br>
</p>

+ miny : Wall (Velocity Condition : Translation Moving Wall)
    + Velocity : (40, 0, 0) (m/s)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.6.png"><br>
</p>

+ bottom, leg, nose1, nose2, nose3, nose4, nose5, rear, side, slant, top : Wall
    + Velocity Condition : No Slip

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.7.png"><br>
</p>

+ minz, maxz, maxy : symmetry

### Reference Values

Set the Reference Value for the aerodynamic coefficient calculation as follows.

+ Area : 0.056(kg/m<sup>2</sup>, (50% of the cross-sectional area perpendicular to the flow direction)
+ Density : 1.2 (kg/m<sup>3</sup>)
+ Length : 1 (m)
+ Pressure : 0 (Pa)
+ Velocity : 40 (m/s)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.8.png"><br>
</p>

### Numerical Conditions

In this example, we'll change the settings as shown below.

+ Pressure-Velocity Coupling Scheme : SIMPLE

+ Discretization Scheme
    + Pressure : Momentum Weighted Reconstruct
    + Momentum : Second Order Upwind
    + Turbulence : Second Order Upwind

+ Under-Relaxation Factors
    + Pressure : 0.3
    + Momentum : 0.7
    + Turbulence : 0.7

+ Convergence Criteria
    + Pressure : 0.001
    + Momentum : 0.001
    + Turbulence : 0.001

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.10.1.png"><br>
</p>


### Monitor

Monitor the force coefficients of car.

Select [Monitors]-[Add]-[Forces] and set values as shown below.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.9.png"><br>
</p>

### Initialization

Set values as follows

+ Velocity
    + X-Velocity : 40 (m/s)
    + Y-Velocity : 0 (m/s)
    + Z-Velocity : 0 (m/s)

+ Pressure
    + 0 (Pa)

+ Turbulence
    + Scale of Velocity : 40 (m/s)
    + Turbulent Intensity : 1 (%)
    + Turbulent Viscosity Ratio : 10

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.11.png"><br>
</p>

Enter the value and click the Initialize button at the bottom. Then click the [File]-[Save] menu to save the case file.

### Run

In Run Conditions, set the following settings and proceed with the calculation.

+ Number of Iterations : 2000
+ Save Interval : 300
+ Data Write Format : Binary
+ Selct [Parallel]-[Environment] in menu. Set Number of Cores as you want and select [Local Machine] for [Parallel Type].

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.12.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.25.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.26.png"><br>
</p>

When the calculation is started, you can see the graphs of Residuals and Force monitor as shown below.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/residual.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/force.png"><br>
</p>

### Post-processing

#### Scalar distribution at boundary

BARAM uses ParaView for post-processing. To start post-processing, click the ParaView button in [External Tools]. In this example, we will draw the pressure distribution and streamlines.

Change the Case Type to Decomposed Case.

+ In [Mesh Regions] select boundaries
    + Bottom, internalMesh, leg, miny, nose1, nose2, nose3, nose5, rear, side, slant, top

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.20.png"><br>
</p>

Change solid color to p\_rgh.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.15.png"><br>
</p>

#### Streamline

Draw the streamline of the flow around the vehicle.

Utilize the [Extract block] function to extract the geometry of the vehicle and floor as shown below.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/21.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.24.png"><br>
</p>

Change p to [Solid Color].

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.21.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.22.png"><br>
</p>

In the Pipeline Browser on the left, click baram.foam once to activate it.


<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.23.png"><br>
</p>

Click [Stream Tracer] icon.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.16.png"><br>
</p>

Change settings as follows

+ Seed Type : Point Cloud
+ Center : (0.7, 0.1, 0.1)
+ Radius :  0.2
+ Number of Points : 100
+ Coloring : Vorticity

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.17.png"><br>
</p>

You can see streamline as shown below.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.18.png"><br>
</p>


## DriveAer


[Download mesh](https://drive.google.com/file/d/1uL_roJ1xnBsmG87HTwQ4fie4txDhjCjW/view?usp=sharing)

[Download simulation](https://drive.google.com/file/d/1sug9iCm2bMCYez2AsrI164yn_xAm1IOX/view?usp=sharing)

### Introduction

[![intro](https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/intro.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/intro.png)

DrivAer is a full-scale vehicle model for exterior design and aerodynamic testing used in the field of automotive engineering to simulate and evaluate the exterior shape and aerodynamic characteristics of vehicles. The model was introduced to bridge the gap between simplified models and highly complex production vehicles, and CAD files and experimental results for many types of geometries are publicly available.

This example uses a fastback model with side mirrors and wheels.

[https://www.epc.ed.tum.de/en/aer/research-groups/automotive/drivaer/](https://www.epc.ed.tum.de/en/aer/research-groups/automotive/drivaer/)

Use steady-state incompressible flow conditions with moving ground and rotating wheel.

The experimental results in the paper show that drag coefficient(Cd) is 0.247 for ASME and 0.243 for SAE, and the simulation results show that Cd = 0.243, which is consistent with the SAE experimental results.

The simulation conditions are as follows 

+ solver : buoyantSimpleNFoam 
+ turbulence model : $Realizable$ $k-\epsilon$ model
+ density : 1.205 $kg/m^3$
+ viscosity : 1.82e-5 $kg/ms$
+ flow condition : 30 $m/s$ at inlet

### Start BaramFlow and load mesh

Run the program and select [New Case] from the launcher. In the launcher, select [Pressure-based] for [Solver Type] and [None] for [Multiphase Model].

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/launcher.png"><br>
</p>

Use the given polyMesh folder. In the top tab, click [File]-[Load Mesh]-[OpenFOAM] in that order and select the polyMesh folder. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/1.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/2.png"><br>
</p>

### General

For this example, we'll use default conditions.

### Models

For this example, we'll use $Realizable$ $k-\epsilon$ model for turbulence.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/3.png"><br>
</p>

### Materials

For this example, we will use the properties of air.

+ air
    + Density : 1.205 $kg/m^3$
    + Viscosity : 1.82e-5 $kg/ms$

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/4.png"><br>
</p>

### Boundary Conditions

Each boundary condition is set as follows

+ minX : velocity Inlet
    + Velocity Specfication Method : Magnitude, Normal to Boundary
    + Profile Type : Constant
    + Velocity Magnitude : 30 (m/s)
    + Turbulent Intensity : 1 (%)
    + Turbulent Viscosity Ratio : 10

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/5.png"><br>
</p>

+ maxX : Pressure Outlet
    + Pressure : 0 (Pa)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/6.png"><br>
</p>

+ minY, maxY, maxZ : Symemtry

+ minZ : Wall
    + Velocity Condition : Translational Moving Wall
    + Velocity : (30, 0, 0)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/7.png"><br>
</p>

+ body\_no\_wheel : Wall
    + Velocity Condition : No Slip

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/8.png"><br>
</p>

+ Wheels\_Front\_Smooth : Wall
    + Velocity Condition : Rotational Moving Wall
    + Speed (RPM) : 898.8
    + Rotation-Axis Origin : (0.007, 0, 0)
    + Rotation-Axis Direction : (0, -1, 0)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/9.png"><br>
</p>

+ Wheels\_Rear\_Smooth : Wall
    + Velocity Condition : Rotational Moving Wall
    + Speed (RPM) : 898.8
    + Rotation-Axis Origin : (2.7932, 0, 0)
    + Rotation-Axis Direction : (0, -1, 0)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/10.png"><br>
</p>

### Reference Values

Set the Reference Value for the aerodynamic coefficient calculation as follows.

+ Area : 1.08
+ Density : 1.205
+ Length : 4.6132
+ Pressure : 0
+ Velocity : 30

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/11.png"><br>
</p>

### Numerical Conditions

In this example, we'll change the settings as shown below.

+ Pressure-Velocity Coupling Scheme : SIMPLEC

+ Discretization Scheme
    + Pressure : Momentum Weighted Reconstruct
    + Momentum : Second Order Upwind
    + Turbulence : Second Order Upwind

+ Under-Relaxation Factors
    + Pressure : 0.9
    + Momentum : 0.9
    + Turbulence : 0.9
    + Density : 0.9

+ Convergence Criteria
    + Pressure : 0.00001
    + Momentum : 0.001
    + Turbulence : 0.001

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/13.png"><br>
</p>


### Monitor

Monitor the force coefficients of car.

Select [Monitors]-[Add]-[Forces] and set values as shown below.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/12.png"><br>
</p>

### Initialization

Change the values as shown below

+ Velocity
    + X-Velocity : 30 (m/s)
    + Y-Velocity : 0 (m/s)
    + Z-Velocity : 0 (m/s)

+ Pressure
    + 0 (Pa)

+ Turbulence
    + Scale of Velocity : 30 (m/s)
    + Turbulent Intensity : 1 (%)
    + Turbulent Viscosity Ratio : 10

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/15.png"><br>
</p>

Enter the value and click the Initialize button at the bottom. Then click the [File]-[Save] menu to save the case file.

### Run

Change the values as shown below, and click [Start Calculation] button.

+ Number of Iterations : 2,000
+ Save Interval : 100
+ Data Write Format : Binary
+ Selct [Parallel]-[Environment] in menu. Set Number of Cores as you want and select [Local Machine] for [Parallel Type].

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/16.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/28.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/29.png"><br>
</p>

When the calculation is started, you'll see a graph of Residuals and Force monitor as shown below.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/residual.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/force.png"><br>
</p>

### Post-processing

#### Scalar distribution at boundary

BARAM uses ParaView for post-processing. To start post-processing, click the ParaView button in [External Tools]. In this example, we will draw the pressure distribution and streamlines.

Change the Case Type to Decomposed Case.

+ In [Mesh Regions] select boundaries
    + Wheels\_Front\_Smooth, Wheels\_Rear\_Smooth, body\_no\_wheels

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/19.png"><br>
</p>

Utilize the [Extract block] function to extract the geometry of the vehicle and floor as shown below.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/21.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/22.png"><br>
</p>

Change [Solid Color] to p\_rgh.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/20.png"><br>
</p>

#### Streamline

Draw the streamline of the flow around the vehicle.

Change p to [Solid Color].

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/23.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/24.png"><br>
</p>

In the Pipeline Browser on the left, click baram.foam once to activate it.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/25.png"><br>
</p>

Click [Stream Tracer] icon.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.16.png"><br>
</p>

Change settings as follows

+ Seed Type : Point Cloud
+ Center : (-1.5, 0.1, 0.1)
+ Radius :  0.2
+ Number of Points : 100
+ Coloring : U

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/26.png"><br>
</p>

You can see streamline as shown below.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/27.png"><br>
</p>




