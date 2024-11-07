# Rotating machinery

## Fan(MRF) 

[Download mesh](https://drive.google.com/file/d/1GL268zuyYtKNtp2sKrgOujRIUMROU_Ij/view?usp=sharing)

[Download simulation](https://drive.google.com/file/d/1KkySXAnSB0DRb_2hwBreKQEd5xkKnKYG/view?usp=sharing)

### Introduction 

[![Mesh and velocity](https://github.com/nextfoam/baram-pages/raw/main/screenshots/fan/intro.png "Mesh and velocity")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/fan/intro.png)

This is a steady-state incompressible flow analysis example. The problem is to predict the flow using multiple reference frames (MRFs) as the impeller rotates inside a fan.

The computational conditions are as follows 

+ solver : buoyantSimpleNFoam 
+ turbulence model : $Standard$ $k-\epsilon$ model
+ density : 1.225 $kg/m^3$
+ viscosity : 1.79e-5 $kg/ms$
+ Rotation velocity : 1,000 RPM
+ flow condition : 10 $m/s$ at inelt

### Start BaramFlow and load mesh

Run the program and select [New Case] from the launcher. In the launcher, select [Pressure-based] for [Solver Type] and [None] for [Multiphase Model].

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/launcher.png"><br>
</p>

Use the given polyMesh folder. In the top tab, click [File]-[Load Mesh]-[OpenFOAM] in that order and select the polyMesh folder. 

### General

For this example, we'll use default conditions.

### Models

For this example, we'll use default conditions.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/4.3.png"><br>
</p>

### Materials

For this example, we will use the properties of air.

### Cell zone Conditions

Cell Zone Conditions allow you to set MRF, Sliding Mesh, Source, and more. In this example, we are using a Multiple Reference Frame, MRF condition for the 'rotating' Cell Zone.

Select Multiple Reference Frame, MRF and set values as follows

+ Multiple Reference Frame, MRF
    + Rotating Speed : 1000(RPM)
    + Rotation-Axis Origin : (0 0 0)
    + Rotation-Axis Direction : (0 0 1)
    + Static Boundary : casing

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/fan/mrf.png"><br>
</p>

### Boundary Conditions

Each boundary condition is set as follows

+ blade, casing
    + Velocity Condition : No Slip

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/4.8.png"><br>
</p>

+ inlet : Velocity Inlet
    + Velocity Specification Method : Magnitudde, Normal to Boundary
    + Profile Type : Constant
    + Velocity Magnitude : 10 (m/s)
    + Turbulent Intensity : 1 (%)
    + Turbulent Viscosity Ratio : 10

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/fan/inletBC.png"><br>
</p>

+ outlet : Pressure Outlet
    + Total Pressure : 0 (Pa)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/4.10.png"><br>
</p>

### Numerical Conditions

In this example, we'll change the settings as shown below.

+ Pressure-Velocity Coupling Scheme : SIMPLE

+ Discretization Scheme
    + Pressure : Linear
    + Momentum : Second Order Upwind
    + Turbulence : Second Order Upwind

+ Under-Relaxation Factors
    + Pressure : 0.3
    + Momentum : 0.5
    + Turbulence : 0.7

+ Convergence Criteria
    + Pressure : 0.001
    + Momentum : 0.001
    + Turbulence : 0.001

### Initialization

Use default vaues.

Enter the value and click the Initialize button at the bottom. Then click the [File]-[Save] menu to save the case file.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/4.11.png"><br>
</p>

### Run

Change the values as shown below, and click [Start Calculation] button.

+ Number of Iterations : 1000
+ Save Interval : 100
+ Retain Only the Most Recent Files, 1
+ Data Write Format : Binary
+ Selct [Parallel]-[Environment] in menu. Set Number of Cores as you want and select [Local Machine] for [Parallel Type].

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/fan/runCondition.png"><br>
</p>

When the calculation is started, you'll see a graph of Residuals and Force monitor as shown below.

|[![residual](https://github.com/nextfoam/baram-pages/raw/main/screenshots/fan/run.png "residual")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/fan/run.png){:target="_blank"}|

### Post-processing

Draw the pressure distribution in the fan.

Click the parview button in [External tools] to open the paraview.

Change the [Case Type] to [Decomposed Case].

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/fan/pv1.png"><br>
</p>

Use the [Slice] function to cut a cross-section inside the domain.

Click the Z-normal button and enter 0.01 for the z value in Origin.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/fan/pv2.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/fan/pv3.png"><br>
</p>

<!-------------------------------------------------------------------------------------------------------->
## Mixer(MRF)

[Download mesh](https://drive.google.com/file/d/1hDPj81oeEhsQD86Uu9fUZmnIaglgth7c/view?usp=sharing)

[Download simulation](https://drive.google.com/file/d/1UBi_opGRIYnGxDFeFihTukVEDOI52h2m/view?usp=sharing)

### Introduction 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixer/intro.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixer/intro.png)

This example is an MRF simulation of a mixer with a simple geometry. The MRF model enables steady-state calculations and allows the use of rotational periodic conditions based on one impeller blade, which can significantly reduce the computational cost. 

The simulation conditions are as follows 

+ solver : buoyantPimpleNFoam 
+ turbulence model : $Standard$ $k-\epsilon$ model
+ density : 1000 $kg/m^3$
+ viscosity : 0.001 $kg/ms$
+ rotation velocity : 100 RPM

### Start BaramFlow and load mesh

Run the program and select [New Case] from the launcher. In the launcher, select [Pressure-based] for [Solver Type] and [None] for [Multiphase Model].

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/launcher.png"><br>
</p>

Use the given polyMesh folder. In the top tab, click [File]-[Load Mesh]-[OpenFOAM] in that order and select the polyMesh folder. 


### General

For this example, we'll use default conditions.

### Models

For this example, we'll use default conditions.

### Materials

Change the Name to water and enter a Density of 1000 and a viscosity of 0.001.

### Cell zone Conditions

Select [Multiple Reference Frame, MRF] at [Cell Zone Conditions] and set values as follows

+ Multiple Reference Frame
    + Rotating Speed : 100(RPM)
    + Rotation-Axis Origin : (0 0 0)
    + Rotation-Axis Direction : (0 0 1)
    + Static Boundary : periodic1, periodic2

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixer/cellZone.png"><br>
</p>

### Boundary Conditions

Each boundary condition is set as follows

+ periodic1, periodic2 : Interface - Rotational Periodic
    + periodic1 : Change to Rotational Periodic, then select periodic2 as [Coupled Boundary]
    + Set [Rotation-Axis Origin] as (0 0 0), [Rotation-Axis Direction] as (0 0 1)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixer/interface.png"><br>
</p>

+ impeller, impeller_slave : Thermo-Coupled Wall
    + impeller : Change to Thermo-Coupled Wall, then, select impeller\_slave as [Coupled Boundary]

+ wall, hub, hub_rot : Wall
    + Velocity Condition : noSlip

+ top : symmetry

### Numerical Conditions

For this example, we'll use default conditions.

### Initialization

For this example, we'll use default conditions.

Click the Initialize button at the bottom. Then click the [File]-[Save] menu to save the case file.

### Run

Change [Number of Iteration] as 3000 and click [Start Calculation] button.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixer/residual.png"><br>
</p>

<!-------------------------------------------------------------------------------------------------------->
## Sirocco Fan(Sliding mesh) 

[Download mesh](https://drive.google.com/file/d/1479KBQebn3GnVG_X5KlY9JBejJn4dX3Q/view?usp=sharing)

[Download simulation](https://drive.google.com/file/d/1n7kY5ZSO4I9PsH1TVDLI0nTRLrl8lRhh/view?usp=sharing)

### Introduction 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/intro.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/intro.png)

This is an example of transient incompressible flow. The problem is to predict the flow inside a sirocco fan when the impeller  is rotating.

First steady-state simulations is performed using the MRF method. Then using the steady results as initial condition, the transient simulation is performed using the sliding mesh.

The simulations conditions are as follows 

+ solver : buoyantPimpleNFoam
+ turbulence model : $Standard$ $k-\epsilon$ model
+ density : 1.225 $kg/m^3$
+ viscosity : 1.79e-5 $kg/ms$
+ Rotation velocity : 2,000 RPM

### Start BaramFlow

Run the program and select [New Case] from the launcher. In the launcher, select [Pressure-based] for [Solver Type] and [None] for [Multiphase Model].

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/launcher.png"><br>
</p>

### Mesh

Use fluent format mesh. Select [File]-[Load Mesh]-[Fluent (ASCII)] from the menu to open the file selection window. Select the downloaded siroccofan.msh file and the following window will open.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/fluentToFoam.png"><br>
</p>

This mesh has two cell zones, fluid and rotating. These two cell zones and two regions, region1 and region2, are shown. Click the (+) icon to the right of region2 to add a region. This is an option to convert to a multi-region grid. Since this is not a multi-region problem, you can leave both cell zones as region1. You can also delete region2 by pressing the trash icon below region2. 

### Steady-state simulation

#### General

Change Time to Steady. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/general-steady.png"><br>
</p>

#### Models

For this example, we'll use $Standard$ $k-\epsilon$ model for turbulence.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/4.3.png"><br>
</p>

#### Materials

For this example, we will use the properties of air.

#### Cell zone Conditions

Double-click rotating in the [Cell Zone Conditions] to open a new window. Select [MUltiple Reference Frame, MRF] and enter the values below.

+ Rotating Speed : 2,000(RPM)
+ Rotation-Axis Origin : (0, 0, 0)
+ Rotation-Axis Direction : (0, 0, 1)
+ Static Boundary : interface-rotating, interface-stat
    + Select the non-rotating boundaries in the cell zone to use MRF. One of the two interface boundary is inside the cell zone and the other is outside. It is usually hard to tell which one is the case, so you can select both. It is okay to include boundary faces that are outside the cell zone.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/mrf.png"><br>
</p>

#### Boundary Conditions

Each boundary condition is set as follows

+ interface-stat, interface-rotating : Interface - Internal Interface
    + interface-stat : Change to Internal Interface, then select interface-rotating as [Coupled Boundary]

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/4.5.png"><br>
</p>

+ axis : Wall
    + Velocity Condition : Rotational Moving Wall
    + Speed : 2000 (RPM)
    + Rotation-Axis Origin : 0 0 0
    + Rotation-Axis Direction : 0 0 1

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/4.6.png"><br>
</p>

+ axis-r, blades, externalwalls, walls : Wall
    + Velocity Condition : No Slip

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/4.8.png"><br>
</p>

+ inlet : Velocity Inlet
    + Velocity Specification Method : Magnitudde, Normal to Boundary
    + Profile Type : Constant
    + Velocity Magnitude : 1 (m/s)
    + Turbulent Intensity : 1 (%)
    + Turbulent Viscosity Ratio : 10

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/4.9.png"><br>
</p>

+ outlet : Pressure Outlet
    + Total Pressure : 0 (Pa)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/4.10.png"><br>
</p>

#### Numerical Conditions

For this example, we'll use default conditions.

#### Initialization

For this example, we'll use default conditions.

Enter the value and click the Initialize button at the bottom. Then click the [File]-[Save] menu to save the case file.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/initial.png"><br>
</p>

#### Run

Change the values as shown below, and click [Start Calculation] button.

+ Number of Iteration : 1000
+ Save Interval : 100
+ Selct [Parallel]-[Environment] in menu. Set Number of Cores as you want and select [Local Machine] for [Parallel Type].

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/run.png"><br>
</p>

Because the flow is unsteady, the residual does not converge, but it is fine to use as an initial condition for the transient simulation.

### Transient simulation

#### General

Change Time to Transient. 

The following window appears. It asks if you want to use the steady-state results as initial conditions for the transient simulation. If you select Yes, the last saved data will be set as the initial condition and the calculation will start from time 0.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/toTransient.png"><br>
</p>

#### Cell zone Conditions

Change rotating cell zone to [Sliding Mesh]. Setting of [Static Boundary] disappears. 

#### Boundary Conditions

Change the boundary conditions for the moving wall to the following

+ axis-r, blades : Wall
    + Velocity Condition : Moving Wall

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/4.7.png"><br>
</p>

#### Numerical Conditions

For this example, we'll use default conditions.

#### Run

Change the values as shown below, and click [Start Calculation] button.

+ Time Stepping Method : Fixed
+ Time Step Size : 0.0001
+ End Time : 0.3

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/4.12.png"><br>
</p>

When the calculation is started, you'll see a graph of Residuals and Force monitor as shown below.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/4.13.png"><br>
</p>

### Post-processing

Draw the pressure distribution in the fan.

Click the parview button in [External tools] to open the paraview.

Change the [Case Type] to [Decomposed Case].

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/4.14.png"><br>
</p>

Use the [Slice] function to cut a cross-section inside the domain.

Click the Z-normal button and enter values as follow

+ Origin : (0.06 -0.017 0.05)
+ Normal : (0 0 1)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/4.15.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/4.16.png"><br>
</p>

<!-------------------------------------------------------------------------------------------------------->
## Propeller 

[Download simulation](https://drive.google.com/file/d/1DD4Ba9iBaVpgORhmzyh1ppTtHYXIYX1t/view?usp=sharing)

### Introduction 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/propeller/intro.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/propeller/intro.png)

This is an example of the propeller from the pimpleFoam tutorial of OpenFOAM. This is to predict the flow over the rotation of a propeller using the sliding mesh.

The simulation conditions are as follows 

+ solver : buoyantPimpleNFoam
+ turbulence model : $Realizable$ $k-\epsilon$ model
+ density : 1000 $kg/m^3$
+ viscosity : 0.001 $kg/ms$
+ rotating velocity : 1432 $RPM$
+ flow condition : 5 $m/s$ at inlet

### Start BaramFlow and load mesh

Run the program and select [New Case] from the launcher. In the launcher, select [Pressure-based] for [Solver Type] and [None] for [Multiphase Model].

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/launcher.png"><br>
</p>

Use the given constant/polyMesh folder of [Download simulation] file. In the top tab, click [File]-[Load Mesh]-[OpenFOAM] in that order and select the constant/polyMesh folder. 

### General

Change Time to Transient.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/4.2.png"><br>
</p>

### Models

For turbulence, use $Realizable$ $k-\epsilon$ model and [Enhanced Wall Treatment].

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/propeller/tur.png"><br>
</p>

### Materials

Change the Name to water and enter a Density of 1000 and a viscosity of 0.001.

### Cell zone Conditions

Select [Sliding Mesh] at [Cell Zone Conditions] and set values as follows

+ Rotating Speed : 1432(RPM)
+ Rotation-Axis Origin : (0 0 0)
+ Rotation-Axis Direction : (-1 0 0)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/propeller/cellZone.png"><br>
</p>

### Boundary Conditions

Set the boundary type and values as shown below.

+ cellZone\_surface, cellZone\_surface\_slave : Interface - Internal Interface
    + cellZone\_surface : Change to Internal Interface, then select cellZone\_surface\_slave as [Coupled Boundary]

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/propeller/interface.png"><br>
</p>

+ propeller, propellerStem : Wall
    + Velocity Condition : Moving Wall

+ far_surface : Wall
    + Velocity Condition : Slip

+ inlet : Velocity Inlet
    + Velocity Specification Method : Magnitudde, Normal to Boundary
    + Profile Type : Constant
    + Velocity Magnitude : 5 (m/s)
    + Turbulent Intensity : 1 (%)
    + Turbulent Viscosity Ratio : 10

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/propeller/inlet.png"><br>
</p>

+ outlet : Pressure Outlet
    + Total Pressure : 0 (Pa)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/propeller/outlet.png"><br>
</p>

### Numerical Conditions

Change [Max Iterations per Time Step] to 20, [Number of Correctors] to 2. Use default value for the rest.

### Initialization

Set [X-Velocity] as 5, [Scale of Velocity] as 5. 

Enter the value and click the Initialize button at the bottom. Then click the [File]-[Save] menu to save the case file.

### Run

In Run Conditions, set the following settings and proceed with the calculation.

+ Time Stepping Method : Fixed
+ Time Step Size : 1e-5
+ End Time : 0.1
+ Save Interval : 0.001
+ Data Write Format : Binary
+ Selct [Parallel]-[Environment] in menu. Set Number of Cores as you want and select [Local Machine] for [Parallel Type].

<!-------------------------------------------------------------------------------------------------------->
## Fan in Room

### Introduction 

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/room/room-intro.png" ><br>
</p>

This is an example of a simulation using a sliding mesh.

It is a simple problem with a fan rotating at 100 RPM in a room with air flowing through a window.

The simulation conditions are as follows

+ solver : buoyantSimpleNFoam 
+ turbulence model : $Standard$ $k-\epsilon$
+ rotating velocity : 100 RPM

### Start BaramFlow and load mesh

Run the program and select 'Open' from the launcher. 

Select the folder created from [BaramMesh tutorial](https://baramcfd.org/mesh/2024/03/21/fanInRoomMesh-post/).

Then select Pressure-based for 'Solver Type' and None for 'Multiphase Model'.

### General

For this example, we'll use default conditions.

### Models

For this example, we'll use default conditions.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixer/3.3.png"><br> turbulence model
</p>

### Materials

Material properties of air is as follows 

### Cell zone Conditions

Double-click AMI in the [Cell Zone Conditions] to open a new window. Select [Sliding Mesh] and enter the values below.

+ Rotating Speed : 100
+ Rotation Axis Origin : (-3 2 2.6)
+ Rotation Axis Direction : (0 0 1)

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/room/room-cellZone.png" >
    <br> Cell Zone setup
</p>

### Boundary Conditions

Set the boundary type and values as shown below.

+ desk\_surface_0, door, room : Wall - No Slip

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/room/room-noSlip.png" >
</p> 

+ fan_surface_0 : Wall - Moving Wall

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/room/room-movingWall.png" >
</p>

+ outlet : Pressure Outlet - Total Pressure = 0

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/room/room-outlet.png" >
</p>

+ AMI\_surface\_0, AMI\_surface\_0\_slave : interface - Internal Interface

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/room/room-interface.png" >
</p>


<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/room/room-bc.png" >
    <br> boundary condition setup
</p> 

### Numerical Conditions

In this example, we'll change the settings as shown below.

+ Pressure-Velocity Coupling Scheme : SIMPLE

+ Use Momentum Predictor : On

+ Discretization Schemes
    * Time : First Order Implicit
    * Pressure : Linear
    * Momentum : Second Order Upwind
    * Turbulence : First Order Upwind

+ Under-Relaxation Factors : 1 for all

* Max Iteration per Time Step : 3

* Number of Correctors : 1

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/room/room-nume.png" >
    <br> Numerical conditions setup
</p> 

### Initialization

For this example, we'll use default values.

Enter the value and click the Initialize button at the bottom. Then click the [File]-[Save] menu to save the case file.

### Run

Change the values as shown below, and click [Start Calculation] button.

+ Time Stepping Method : Adaptive
+ Couraant Number : 1
+ EndTime : 1
+ Save Interval : 0.02

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/room/room-residual.png">
    <br> Residual plot
</p>


