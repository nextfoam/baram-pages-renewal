# Compressible subsonic flow

## High speed train 

[Download mesh](https://drive.google.com/file/d/1eL9zqfmXct3zMtJIJUuoap27afeMkdq2/view?usp=sharing)

[Download simulation](https://drive.google.com/file/d/1FnEN0tfTE5Pg2Co61oMx8VfsrBI1Oz7q/view?usp=sharing)

### Introduction 

[![intro](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/train/intro.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/train/intro.png)

High-speed trains run in the subsonic compressible flow regime with Mach numbers in the range of 0.3 to 0.4. In CFD, the pressure-based solver of SIMPLE algorithm is often used for low-speed flows and the density-based solver for high-speed flows. This challenge aims to validate the stability of Baram's incompressible solver, buoyantSimpleNFoam, in the subsonic compressible flow regime.

We use a high-speed train model with simplified vehicle connections, bogies(wheels), and pantographs, and the train speed is 400 km/h. 

With a mesh of about 2 millions, the convergence was achieved in about 300 iterations(about 20 minutes on an 8-core CPU). The convergence is much better compared to the results obtained using OpenFOAM's standard solvers buoyantSimpleFoam and rhoSimpleFoam.

The computational conditions are as follows 

+ solver : buoyantSimpleNFoam 
+ turbulence model : $SST$ $k-\omega$ 모델
+ density : Perfect Gas
+ viscosity : 1.79e-5 $kg/ms$
+ flow condition : 400 $km/h$(111.11 $m/s$) at inlet

### Start BaramFlow and load mesh

Run the program and select [New Case] from the launcher. In the launcher, select [Pressure-based] for [Solver Type] and [None] for [Multiphase Model].

Use the given polyMesh folder. In the top tab, click [File]-[Load Mesh]-[OpenFOAM] in that order and select the polyMesh folder. 

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/train/mesh.png"><br>
</p>

### General

For this example, we'll use default conditions.

### 모델(Models)

For this example, we'll use $SST$ $k-\omega$  model for turbulence.

Include Energy.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/train/tur.png"><br>
</p>

### Materials

Material properties of air is as follows 

+ Density : Perfect Gas
+ Specific heat : 1006
+ Viscosity : 1.79e-05
+ Thermal Conductivity : 0.0245
+ Molecular Weight : 28.966

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/train/mat.png"><br>
</p>

### Boundary Conditions

Set the boundary type and values as shown below.

+ Hex6_1_xMin : Velocity Inlet
    + Velocity Specification Method : Magnitude, Normal to Boundary
    + Velocity Magnitude : 111.11
    + Turbulence Specification Method : Intensity and Viscosity Ratio
    + Turbulent Intensity : 1
    + Viscosity ratio : 100
    + Temperature : 300

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/train/inletbc.png">
</p>

+ Hex6_1_xMax : Pressure Outlet
    + Pressure  : 0

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/train/outletbc.png">
</p>

+ Hex6_1_zMin : Wall
    + Velocity Condition : Translational Moving Wall
    + Velocity : (111.11 0 0)

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/train/groundbc.png">
</p>

+ train_surface_0 : Wall

+ Hex6_1_yMin, Hex6_1_yMax, Hex6_1_zMax : Symmetry

### Reference Values

The Reference Value is used to calculate the aerodynamic coefficient. Since this is not a problem with experimental data, you do not need to enter an exact value and set the velocity to 111.11.

### Numerical Conditions

In this example, we'll change the settings as shown below.

+ Pressure-Velocity Coupling : SIMPLEC

+ Discretization Schemes
    + Pressure : Momentum Weighted Reconstruct
    + Momentum, Energy, Turbulence :Second Order Wpwind

+ Under-Relaxation Factors
    + Pressure : 0.8
    + Momentum, Energy, Turbulence : 0.9
    + Density : 1.0

+ Convergence Criteria
    + Pressure, Momentum, Turbulence : 1e-4
    + Energy : 1e-6

### Monitor

Monitor the force coefficients of train.

Select [Monitors]-[Add]-[Forces] and select train\_surface\_0 at [Boundaries].

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/train/monitor.png"><br>
</p>

### Initialization

Enter the value and click the Initialize button at the bottom. Then click the [File]-[Save] menu to save the case file.

+ Velocity : (111.11 0 0)
+ Pressure : 0
+ Temperature : 300
+ Scale of velocity : 111.11
+ Turbulent Intensity : 1
+ Turbulent Viscosity Ratio : 10

### Run 

Selct [Parallel]-[Environment] in menu. Set Number of Cores as you want and select [Local Machine] for [Parallel Type].

Change the values as shown below, and click [Start Calculation] button.

+ Number of Iterations : 1000
+ Save Interval : 1000

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/train/run.png"><br>
</p>

When the calculation is started, you can see the graphs of Residuals and Force monitor as shown below.

[![intro](https://github.com/nextfoam/baram-pages/raw/main/screenshots/train/residual.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/train/residual.png)

[![intro](https://github.com/nextfoam/baram-pages/raw/main/screenshots/train/force.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/train/force.png)


### Post-processing

Click the parview button in [External tools] to open the paraview.

Change the [Case Type] to [Decomposed Case].

To draw the pressure distribution around the vehicle, select train\_surface\_0, ground, symmetry from Mesh Regions and Coloring as p\_rgh.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/train/contour.png"><br>
</p>
<!------------------------------------------------------------------------------------------------------------->
## Hot subsonic jet 

[Download mesh](https://drive.google.com/file/d/1fq6KLFt2mpidQchHwQ9j9mRgty5rlr3G/view?usp=sharing)

[Download simulation](https://drive.google.com/file/d/1I2zMwTUwXwLbYIoBFZ4UDt8m86tWS9Hr/view?usp=sharing)

### Introduction 

[![intro](https://github.com/nextfoam/baram-pages/raw/main/screenshots/jet/nozzle3d.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/jet/nozzle3d.png)

[![intro](https://github.com/nextfoam/baram-pages/raw/main/screenshots/jet/y0.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/jet/y0.png)

This is an example flow simulation of a high-temperature subsonic nozzle with a temperature of 260$^o C$ and a Mach number of 0.376 at the nozzle throat.

The geometry and experimental conditions are provided by NASA Langley Research Center.

[https://turbmodels.larc.nasa.gov/jetsubsonichot_val.html](https://turbmodels.larc.nasa.gov/jetsubsonichot_val.html)

Experimental results from NASA ARN2 (Acoustic Research Nozzle 2) with the following geometry and conditions

* Radius of the nozzle neck: 1 inch
* Pressure ratio, $p/p{ref}$ = 1.10203, $p{ref}$ = 14.3 psi
* Temperature ratio, $T/T{ref}$ = 1.81388, $T{ref}$ = 530 R
* Nozzle exit Mach number: 0.376

Along with the experimental results, we provide calculations from the $SST$ k-$omega$ model and the Spalart-Allmaras model from the NASA WIND code.

+ solver : buoyantSimpleNFoam
+ turbulence model : $standard$ $k-\epsilon$
+ density : Perfect Gas
+ viscosity and thermal conductivity : Sutherland law 
+ nozzle inlet condition : 10059.65 Pa, 534.086 K
 

### Start BaramFlow and load mesh

Run the program and select [New Case] from the launcher. In the launcher, select [Pressure-based] for [Solver Type] and [None] for [Multiphase Model].

Use the given polyMesh folder. In the top tab, click [File]-[Load Mesh]-[OpenFOAM] in that order and select the polyMesh folder.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/jet/nozzleMesh.png"><br>
</p>

### General

Set Time as Steady, Gravity as (0 0 0).

Set Operating Pressure as  98595.03.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/jet/general.png"><br>
</p>

### Models

For this example, we'll use $Standard$ $k-\epsilon$ model for turbulence.

Include Energy.


### Materials

Material properties of air is as follows 

+ Density : Perfect Gas
+ Specific heat : 1006
+ Viscosity : Sutherland, As = 1.46e-6, Ts = 110.4
+ Molecular Weight : 28.966

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/jet/mat.png"><br>
</p>

### Boundary Conditions

Set the boundary type and values as shown below.

+ inlet : Pressure Inlet
    + Total Pressure : 10059.65
    + Turbulence Specification Method : Intensity and Viscosity Ratio
    + Turbulent Intensity : 1
    + Viscosity ratio : 10
    + Temperature : 534.086

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/jet/bc-inlet.png">
</p>

+ outlet : Pressure Outlet
    + Pressure : 0
    + Specify Backflow Properties : on
        + Backflow Total Temperature : 294.4444
        + Turbulence Specification Method : Intensity and Viscosity Ratio
        + Turbulent Intensity : 1
        + Viscosity ratio : 10

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/jet/bc-outlet.png">
</p>

+ farfield : Velocity Inlet
    + Velocity Specification Method : Component (3.44 0 0)
    + Turbulence Specification Method : Intensity and Viscosity Ratio
    + Turbulent Intensity : 1
    + Viscosity ratio : 10
    + Temperature : 294.4444

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/jet/bc-far.png">
</p>

+ nozzle : Wall
    + Velocity : No Slip
    + Temperature : Adiabatic

+ frontAndBackPlanes_pos, frontAndBackPlanes_neg : Wedge


### Numerical Conditions

In this example, we'll change the settings as shown below.

+ Pressure-Velocity Coupling : SIMPLEC

+ Discretization Schemes
    + Pressure : Momentum Weighted Reconstruct
    + Momentum, Energy, Turbulence :Second Order Wpwind

+ Under-Relaxation Factors
    + Pressure : 0.1
    + Momentum : 0.3
    + Energy : 0.9
    + Turbulence : 0.2

+ Convergence Criteria
    + Pressure : 0
    + Momentum, Energy, Turbulence : 0.001

+ Advanced
    + Minimum Static Temperature : 100
    + Maximum Static Temperature : 1000

### Monitor

In this example, we will monitor the axial velocity of a point with x/D of 20 on the axis. Select [Add]-[Points]. 

Enter X-Velocity for Field and (1.016 0 0) for Coordinate.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/jet/monitor.png"><br>
</p>

### Initialization

Enter the value and click the Initialize button at the bottom. Then click the [File]-[Save] menu to save the case file.

+ Velocity : (3.44 0 0)
+ Pressure : 0
+ Temperature : 294.4444
+ Scale of velocity : 100
+ Turbulent Intensity : 1
+ Turbulent Viscosity Ratio : 200


### Run 

Selct [Parallel]-[Environment] in menu. Set Number of Cores as you want and select [Local Machine] for [Parallel Type].

Change the values as shown below, and click [Start Calculation] button.

+ Number of Iterations : 20000
+ Save Interval : 1000

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/jet/run.png"><br>
</p>

When the calculation is started, you can see the graphs of Residuals and point monitor as shown below.

[![intro](https://github.com/nextfoam/baram-pages/raw/main/screenshots/jet/residual.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/jet/residual.png)


[![intro](https://github.com/nextfoam/baram-pages/raw/main/screenshots/jet/point.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/jet/point.png)


### Post-processing

Click the parview button in [External tools] to open the paraview.

Change the [Case Type] to [Decomposed Case].


Change [Coloring] to U.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/jet/contour.png"><br>
</p>

<!------------------------------------------------------------------------------------------------------------->
## Subsonic cavity flow

[Download mesh](https://drive.google.com/file/d/1u__XyUIi_xiL5-LmuT9OaBNda7s7qpDk/view?usp=sharing)

[Download simulation](https://drive.google.com/file/d/1rfwtg18SiB2Bh50VE4vnXEYeOg0U0qMs/view?usp=sharing)

### Introduction 

[![include surface's own gap](https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavity/2d-pressureContour.png)](https://youtu.be/NI_MVr7M7dI){:target="_blank"}

This example computes subsonic two-dimensional cavity flow. 

The geometry and flow conditions are as follows

+ L/D = 2 (the ratio of the length and height of the cavity)
+ Mach Number = 0.6
+ Reynolds Number = 2.75e+5
+ Prandtl Number = 0.7

The mesh was converted from a plot3d format created in matlab. 

The solver is buoyantPimpleNFoam, a pressure-based thermal flow solver developed by NEXTfoam.

The boundary conditions at the outlet and the top surface are waveTransmissive (non-reflecting) with no reflection of pressure waves.

The computational conditions are as follows 

+ solver : buoyantPimpleNFoam 
+ turbulence model : $SST$ $k-\omega$
+ density : Perfect Gas
+ inlet temperature : 300 K
+ inlet pressure : 101325 Pa

### Start BaramFlow and load mesh

Run the program and select [New Case] from the launcher. In the launcher, select [Pressure-based] for [Solver Type] and [None] for [Multiphase Model].

Use the given polyMesh folder. In the top tab, click [File]-[Load Mesh]-[OpenFOAM] in that order and select the polyMesh folder. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/launcher.png"><br>
</p>

[![include surface's own gap](https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavity/2d-mesh.png "include surface's own gap")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavity/2d-mesh.png)

### General

Change Time to Transient. 

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavity/2d-general.png"><br>
</p>

### Models

For this example, we'll use $SST$ $k-\omega$ model for turbulence.

Include Energy.

### Materials

Material properties of air is as follows 

+ Density : Perfect Gas
+ Specific heat : 1006
+ Viscosity : 0.00178 (value for the Reynolds number)
+ Thermal Conductivity : 2.562 (value for the Prandtl number)
+ Molecular Weight : 28.966

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavity/2d-material.png"><br>
</p>

### Boundary Conditions

Set the boundary type and values as shown below.

+ inlet : Velocity Inlet
    + Velocity Specification Method : Magnitude, Normal to Boundary
    + Velocity Magnitude : 208.31
    + Turbulence Specification Method : Intensity and Viscosity Ratio
    + Turbulent Intensity : 1
    + Turbulent Viscosity Ratio : 10
    + Temperature : 300

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavity/2d-inletbc.png">
</p>

+ outlet, top : Pressure Outlet
    + Pressure  : 0
    + with Non-Reflecting Boundary option

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavity/2d-outletbc.png">
</p>

+ cavityFront, cavityBottom, cavityRear, frontBottom, rearBottom : Wall
    + Velocity Condition : No Slip

+ frontPlane, backPlane : Empty


### Numerical Conditions

In this example, we'll change the settings as shown below.

+ Pressure-Velocity Coupling : SIMPLE

+ Discretization Schemes
    + Time : Second Order Implicit
    + Pressure : Momentum Weighted Reconstruct
    + Momentum, Energy, Turbulence : Second Order Wpwind

+ Max Iteration per Time Step : 20

+ Number of Correctors : 2

+ Under-Relaxation Factors
    + Pressure : 0.3 / 1
    + Momentum, Turbulence : 0.7 / 1
    + Energy, Density : 1 / 1


### Monitor

Monitor pressure at cavity center point. Click [Add]-[Point]. 

Set coordinate as (0 -0.5 0.25).

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavity/2d-monitor.png"><br>
</p>

### Initialization

Set values as follows

+ Velocity : (208.31 0 0)
+ Pressure : 0
+ Temperature : 300
+ Scale of velocity : 208.31  
+ Turbulent Intensity : 1
+ Turbulent Viscosity Ratio : 10

Initialize the velocity inside the cavity to zero. Under [Advanced]-[Sections], select [Create]-[Hex] and enter the Min/Max coordinates as (-1 -1 -1), (1 0 1). Select Velcity and give it a value of (0 0 0).

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavity/2d-setField.png"><br>
</p>

Enter the value and click the Initialize button at the bottom. Then click the [File]-[Save] menu to save the case file.

### Run

Selct [Parallel]-[Environment] in menu. Set Number of Cores as you want and select [Local Machine] for [Parallel Type].

Change the values as shown below, and click [Start Calculation] button.

+ Time Stepping Method : Fixed
+ Time Step Size : 1e-5
+ End Time : 0.5
+ Save Interval : Every 0.002 sec

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavity/2d-run.png"><br>
</p>


### Post-processing

Click the parview button in [External tools] to open the paraview.

Change the [Case Type] to [Decomposed Case].

Change [Coloring] to U.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavity/2d-contour.png"><br>
</p>

