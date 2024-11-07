# Multi-phase Flow

## Weir flow

[Download mesh](https://drive.google.com/file/d/1ODSzqSH9TfIjkmnVNm5p5XLBPX-gjplN/view?usp=sharing)

[Download simulation](https://drive.google.com/file/d/1mZsJla3w9X_95I51-dUixL-jbFXyVT4Z/view?usp=sharing)

### Introduction 

[![intro](https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/main.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/main.png)

In hydraulic engineering, a weir is a structure set in a waterway that allows water to overflow and is used to maintain a specific water level or measure flowrate. In CFD, they are often used for code validation by comparing analyzed results with theoretical flow rates.

This example is for analyzing the flow in a square weir with a constant water level.

The calculation conditions are as follows

+ water height at inlet : 1.6 m, 15696 Pa
+ solver : interFoam
+ turbulence model : $standard$ $k-\epsilon$ 

### Start BaramFlow and load mesh

Run the program and select [New Case] from the launcher. In the launcher, select [Pressure-based] for [Solver Type] and [Volume of Fluid] for [Multiphase Model].

Use the given polyMesh folder. In the top tab, click [File]-[Load Mesh]-[OpenFOAM] in that order and select the polyMesh folder. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/launcher.png"><br>
</p>

### General

Change Time to Transient.

Set Gravity as (0 0 -9.81).

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/general.png"><br>
</p>

### Models

For this example, we'll use $Standard$ $k-\epsilon$ model for turbulence.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/turbulence.png"><br> Turbulence Model
</p>


### Materials

Since this example is two phase flow, two fluids are required. You can add a fluid by pressing the (+) in the top right corner of the Material Configuration section. Add [water-liquid] and rename it to water.

+ water
    + density : 1000
    + viscosity : 0.001
  
+ air
    + density : 1.225
    + viscosity : 1.79e-5

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/material.png"><br> Materials
</p>

### Cell zone Conditions

In Cell Zone Conditions, there is region0 (when multi-region, multiple regions are displayed). Set the fluid of the region. Double-click region0 to open the setting window. Specify air as the primary material and water as the secondary material. 

Use 0 for Surface Tension.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/region.png"><br> Cell Zone Conditions
</p>

### Boundary Conditions

Set the boundary type and values as shown below.

+ water-in
    + type : Pressure Inlet
    + Total Pressure : 15696
    + Turbulent Intensity : 1
    + Turbulent Viscosity Ratio : 10
    + Volume Fraction(water) : 1
  
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/inlet.png"><br> 
</p>

+ air-in
    + type : Pressure Inlet
    + Total Pressure : 0
    + Turbulent Intensity : 1
    + Turbulent Viscosity Ratio : 10
    + Volume Fraction water : 0
  
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/inlet-air.png"><br> 
</p>

+ top
    + type : Pressure Outlet
    + Total Pressure : 0
    + Backflow Turbulent Intensity : 1
    + Backflow Turbulent Viscosity Ratio : 10
    + Backflow Volume Fraction(water) : 0

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/top.png"><br> 
</p>

+ out, out-1
    + type : Outflow
  
+ weir, bottom
    + type : wall
  
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/wall.png"><br> 
</p>

+ front, front-1, back, back-1
    + type : symmetry


### Numerical Conditions

In this example, we'll change the settings as shown below.

+ Pressure-Velocity Coupling Scheme : SIMPLE

+ Use Momentum Predictor : On

+ Discretization Schemes
    + Time : First Order Implicit
    + Pressure : Momentum Weighted Reconstruct
    + Momentum : Second Order Upwind
    + Turbulence : First Order Upwind
    + Volume Fraction : Second Order Upwind

+ Under-Relaxation Factors : 1 for all

+ Improve Stability : Off

+ Max Iteration per Time Step : 1

+ Number of Correctors : 2

+ Multiphase와 Convergence Criteria : use default values
  
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/numerical.png"><br> Numerical Conditions
</p>


### Monitor

Click [Monitor]-[Add]-[Surface]. Select [Volume Flow Rate] for [Report Type] and selct waterin for region.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/monitor.png"><br>
</p>


### Initialization

In this example, we'll change the settings as shown below.

+ velocity : (0 0 0)
+ Pressure : 0
+ Scale of Velocity : 1
+ Turbulent Intensity : 1
+ Turbulent Viscosity Ratio : 10
+ Volume Fraction(water) : 0

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/initial.png"><br> 초기조건 설정
</p>

Create two sections to initialize the water region.

Click [Initialization]-[Advanced]-[Section]-[Create], and then set the following settings

+ region1
    + Min.point : (0.05 -1 0)
    + Max.point : (2 1 0.2)
    + Volume Fraction(water) : 1

+ region2
    + Min.point : (-2 -1 0)
    + Max.point : (-0.05 1 1.6)
    + Volume Fraction(water) : 1
  
Not use [Override Boundary Value] option.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/setFields.png"><br> Section initialize
</p>

Two sections have been created in [Advanced]-[Sections], and you can display the area by clicking the eye-shaped mark on each item.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/setFieldsDisplay.png"><br> Display sections
</p>

Click the Initialize button at the bottom. Then click the [File]-[Save] menu to save the case file.

### Run

Change the values as shown below, and click [Start Calculation] button.

+ Time Stepping Method : Adaptive
+ Max Courant Number : 1
+ Max Courant Number for VoF : 1
+ End time : 20
+ Save Interval : 0.1
+ Selct [Parallel]-[Environment] in menu. Set Number of Cores as you want and select [Local Machine] for [Parallel Type].

When the calculation is started, you can see the graphs of Residuals and flowrate monitor as shown below.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/run.png"><br> monitoring
</p>

### Post-processing

Click the parview button in [External tools] to open the paraview.

Change the [Case Type] to [Decomposed Case].

<p align='left'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/clip.png"> Use the Clip filter to cut out areas with a volume fraction greater than 0.5
</p>

Field to U. In the Pipeline Browser, display baram.foam as Outline, and read the weir.stl file to display it as Solid Color, as shown in the image below.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/pvClip.png"><br> Clip water 
</p>

<p align='left'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/play.png"> Press the Play icon to watch the video.
</p>

<!--------------------------------------------------------------------------------------------------->
## Ship Resistance - KCS(KRISO Container Ship)

[Download mesh](https://drive.google.com/file/d/1GC8NdJuT3Eog9FWQ9M7XyqoCrVOx1TpP/view?usp=sharing)

[Download simulation](https://drive.google.com/file/d/185vzZC9Cc7kboGWYoLyLXaWZawQHKmvr/view?usp=sharing)

### Introduction

[![wave height](https://github.com/nextfoam/baram-pages/raw/main/screenshots/kcs/kcs-wave.png "wave height")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/kcs/kcs-wave.png)

This example is a validation problem for the resistance of a ship with a free surface. The resistance of KCS and is compared to the results in the paper below. The interFoam solver is used for the free surface simulation, a steady-state calculation using the Local Time Step (LTS) technique, and the turbulence model is $SST$ $k-\omega$. 

_Measurement of flows around modern commercial ship models, Kim,W J.Kim, Van, S H, Kim, D H, Experiments in Fluids, 2001_

Simulation and experiment conditions are as follows

+ speed : 2.196 $m/s$
+ reference area(wetted surface area) : 9.5121 $m^2/s$
+ draft : 0.3418 $m$ (z coordinate at mesh is 0)

### Start BaramFlow and load mesh

Run the program and select 'New Case' from the launcher. In the launcher, select [Pressure-based] for [Solver Type] and [Volume of Fluid] for [Multiphase Model].

Use the given polyMesh folder. In the top tab, click [File]-[Load Mesh]-[OpenFOAM] in that order and select the polyMesh folder.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/kcs/launcher.png"><br> launcher
</p>

### General

Set Time as Steady.

Set Gravity as (0 0 -9.81).

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/kcs/general.png"><br> General 
</p>

### Models

For this example, we'll use $SST$ $k-\omega$ model for turbulence.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/kcs/turbulence.png"><br> Turbulence Model
</p>

### Materials

Since this example is two phase flow, two fluids are required. You can add a fluid by pressing the (+) in the top right corner of the Material Configuration section. Add water-liquid and rename it to water.

Set the properties for each fluid as follows

+ water
    + density : 1000
    + viscosity : 0.001
  
+ air
    + density : 1.225
    + viscosity : 1.79e-5

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/kcs/material.png"><br> Materials
</p>

### Cell zone Conditions

In Cell Zone Conditions, there is region0 (when multi-region, multiple regions are displayed). Set the fluid of the region. Double-click region0 to open the setting window. Specify air as the primary material and water as the secondary material. 

Use 0 for Surface Tension.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/kcs/region.png"><br> Cell Zone Conditions
</p>

### Boundary Conditions

Set the boundary type and values as shown below.

+ far\_inlet
    + type : Velocity Inlet
    + Umag : 2.196
    + turbulentIntensity : 1
    + viscosityRatio : 10
    + alpha.liquid : 0
  
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/kcs/inlet.png"><br> 
</p>

+ far\_outlet
    + type : Open Channel Outlet
    + Umean : 2.196
    + turbulentIntensity : 1
    + viscosityRatio : 10
  
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/kcs/outlet.png"><br> 
</p>

+ far\_top
    + type : Pressure Outlet
    + totalPressure : 0
    + inflow turbulentIntensity : 1
    + inflow viscosityRatio : 10
    + inflow alpha.liquid : 0

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/kcs/top.png"><br> 
</p>

+ KCS\_dummy\_hub, KCS\_hub\_aft, KCS\_hub\_cap, KCS\_hull, KCS\_transom, KCS\_deck
    + type : wall
  
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/kcs/wall.png"><br> wall
</p>

+ centerplane, far\_side, far\_bottom
    + type : symmetry

### Reference Values

Set the Reference Value for the hydrodynamic force coefficient calculation as follows.

+ Area : 4.75605(half of wetted area, because we use symmetry condition)
+ Density : 1000
+ Length : 7.2786
+ Velocity : 2.196

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/kcs/ref.png"><br> Reference Values
</p>

### Numerical Conditions

In this example, we'll change the settings as shown below.

+ Pressure-Velocity Coupling Scheme : SIMPLE
+ Use Momentum Predictor : Off
+ Discretization Schemes : Pressure는 Momentum Weighted Reconstruct, Second Order Upwind for the rest
+ Under-Relaxation Factors : 1 for all
+ Improve Stability : Off
+ Max Iteration per Time Step : 1
+ Number of Correctors : 2
+ Multiphase and Convergence Criteria : default values for all
  
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/kcs/numerical.png"><br> Numerical Conditions
</p>

### Monitor

Monitor the force coefficient.

Click [Monitor]-[Add]-[Forces] and set values as follows

+ Lift Direction : (0 0 1)
+ Drag Direction : (-1 0 0)
+ Center of Rotation : (0 0 0)
+ Boundaries : KCS\_dummy\_hub, KCS\_hub\_aft, KCS\_hub\_cap, KCS\_hull, KCS\_transom, KCS\_deck

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/kcs/monitor.png"><br> Monitors
</p>


### Initialization

Set values as follows

+ velocity : (-2.196 0 0)
+ Pressure : 0
+ Scale of Velocity : 2.196
+ Turbulent Intensity : 1
+ Turbulent Viscosity Ratio : 10
+ Volume Fraction - water : 0

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/kcs/initial.png"><br> initial conditions
</p>

To initialize the water, click [Initialization]-[Advanced]-[Section]-[Create] and select [Section Type] as Hex. Enter the range and values as shown below.

+ Min.point : (-999 -999 -999)
+ Max.point : (999 999 0)
+ Volume Fraction - water : 1

Activate [Override Boundary Value] option to change boundary value also.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/kcs/setFields.png"><br> Section initialize
</p>

### Run

Set [Number of Iterations] as 2000.

Selct [Parallel]-[Environment] in menu. Set Number of Cores as you want and select [Local Machine] for [Parallel Type].

Click [Start Calculation] button.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/kcs/parallel.png"><br> Parallel environment
</p>

When the calculation is started, you can see the graphs of Residuals and Force monitor as shown below.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/kcs/residual.png"><br> 
</p>

### Post-processing

Click the parview button in [External tools] to open the paraview.

Change the [Case Type] to [Decomposed Case].

Select boundaries of ship and change [Coloring] to alpha.water.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/kcs/post.png"><br> 
</p>
<!--------------------------------------------------------------------------------------------------->
## Cavitation - NACA66 hydrofoil

[Download mesh](https://drive.google.com/file/d/1a6uUi1CKbzZEYa9e4R3-ABR8ZY6LOtKq/view?usp=sharing)

[Download simulation](https://drive.google.com/file/d/1iaVyUwowI6nrSYlnIkcniAcM3aiDFBXu/view?usp=sharing)

### Introduction 

[![intro](https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavitation/cav-contour.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavitation/cav-contour.png)

[![intro](https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavitation/cav-Cp.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavitation/cav-Cp.png)

This is a validation problem of cavitation around a NACA66 hydrofoil.

The geometry and conditions from the following papers, which are often used for validation of cavitation problems, are used.

_Viscous and Nuclei Effects on Hydrodynamic Loadings and Cavitation of NACA66(MOD) Foil section, Y.T.Shen, P.E. Dimotakis, J. Fluids Eng. sep. 1989_

The velocity is 2.01 m/s and the cavitation number is 0.84.

The cavitation model is Schnerr-Sauer and the coefficients are as follows.

* Evaporation Coefficient : 1
* Condensation Coefficient : 1
* Bubble Diameter : 2e-6
* Bubble Number Density : 1.6e+9
* Vapor pressure : 2420 Pa


### Start BaramFlow and load mesh

Run the program and select 'New Case' from the launcher. In the launcher, select [Pressure-based] for [Solver Type] and [Volume of Fluid] for [Multiphase Model].

Use the given polyMesh folder. In the top tab, click [File]-[Load Mesh]-[OpenFOAM] in that order and select the polyMesh folder. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavitation/launcher.png"><br> launcher
</p>
<br>

### General

Change Time to Transient.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavitation/general.png"><br> General
</p>

### Models

For this example, we'll use $Standard$ $k-\epsilon$ model for turbulence.

### Materials

Since this example is multi-phase flow, two fluids are required. You can add fluids by pressing the (+) in the top right corner of the Material Configuration section. Add waterLiquid and waterVapor. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavitation/material.png"><br> Materials
</p>

### Cell zone Conditions

In Cell Zone Conditions, there is region0 (multiple regions are displayed when it is multi-region). Set the fluid of the region. 

Double-click region0 to open the setting window. Specify the Primary Material as waterVapor and the Secondary material as waterLiquid. 

Use 0.07 for Surface Tension.

Turn on the Cavitation option. Select Schnerr-Sauer for Model and enter 2430 for Vaporization Pressure. Use the following values for Model Constants

* Evaporation Coefficient : 1
* Condensation Coefficient : 1
* Bubble Diameter : 2.0e-06
* Bubble Number Density : 1.6e+9

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavitation/region.png"><br> Cell Zone Conditions
</p>

### Boundary Conditions

Set the boundary type and values as shown below.

+ B\_INLET
    + type : Velocity Inlet
    + Velocity Magnitude : 2.01
    + Turbulent Intensity : 1
    + Turbulent Viscosity Ratio : 10
    + Volume Fraction(water) : 1
  
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavitation/inlet.png"><br> B\_INLET
</p>

+ B\_OUTLET
    + type : Pressure Outlet
    + Pressure : 4113.788
  
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavitation/outlet.png"><br> B\_OUTLET
</p>

+ B\_SYM
    + type : Symmetry
  
+ FOIL\_DOWN, FOIL\_UP
    + type : Wall
  
+ EMPTY
    + type : Empty


### Numerical Conditions

In this example, we'll change the settings as shown below.

+ Pressure-Velocity Coupling Scheme : SIMPLE

+ Use Momentum Predictor : On

+ Discretization Schemes
    + Time : First Order Implicit
    + Pressure : Momentum Weighted Reconstruct
    + Momentum : Second Order Upwind
    + Turbulence : First Order Upwind
    + Volume Fraction : Second Order Upwind

+ Under-Relaxation Factors : use default values

+ Improve Stability : Off

+ Max Iteration per Time Step : 10

+ Number of Correctors : 2

+ Multiphase와 Convergence Criteria : use default values
  
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavitation/numerical.png"><br> Numerical Conditions
</p>

### Initialization

Set values as follows

+ velocity : (2.01 0 0)
+ Pressure : 4113.788
+ Scale of Velocity : 2.01
+ Turbulent Intensity : 1
+ Turbulent Viscosity Ratio : 10
+ Volume Fraction(water) : 1

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavitation/initial.png"><br> initial values
</p>

Enter the value and click the Initialize button at the bottom. Then click the [File]-[Save] menu to save the case file.


### Run

Change the values as shown below, and click [Start Calculation] button.

+ Time Stepping Method : Fixed
+ Time Step Size : 0.01
+ End time : 10
+ Save Interval : 0.5
+ Selct [Parallel]-[Environment] in menu. Set Number of Cores as you want and select [Local Machine] for [Parallel Type]

### Post-processing

Click the parview button in [External tools] to open the paraview.

For parallel, change the [Case Type] to [Decomposed Case].

Change [Coloring] to p\_rgh or alpha.waterLiquid.

[![intro](https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavitation/cav-contour.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavitation/cav-contour.png)

To get the pressure data of the hydrofoil surface, select only the desired boundary (FOIL\-UP) and run File-Save Data from the menu to get the data file in csv format.







