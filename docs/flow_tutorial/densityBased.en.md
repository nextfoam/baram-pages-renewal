# Density based compressible solver

## RAE2822 transonic airfoil

[Download mesh](https://drive.google.com/file/d/1XfaXhTFvdUD5P3-avf8ShqQpn-5D25iy/view?usp=sharing)

[Download simulation](https://drive.google.com/file/d/1VD8BLW-qfvr_Vkq9apW7v0GNKfQ6SAyB/view?usp=sharing)

### Introduction

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/RAE2822/rae-mesh.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/RAE2822/rae-mesh.png)

This is a steady-state compressible flow example using a density-based solver. The RAE2822 airfoil transonic flow validation problem uses the conditions from the website below.

[https://www.grc.nasa.gov/www/wind/valid/raetaf/raetaf01/raetaf01.html](https://www.grc.nasa.gov/www/wind/valid/raetaf/raetaf01/raetaf01.html)

A structured mesh of plot3d format is converted to OpenFOAM. The far boundary consists of farfield in and farfield out and the airfoil name is wing.

The calculation conditions are as follows

+ solver : TSLAeroFoam
+ turbulence model : $SST$ $k-\omega$
+ Mach number : 0.729
+ Angle of attack : 2.31 degree
+ farfield pressure : 108988 Pa
+ farfield temperature : 255.556 K
+ farfield turbulence : k = 0.08, omega = 7400 

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

Set [Operating Conditions] as 0. 

### Models

For this example, we'll use $SST$ $k-\omega$ model for turbulence.

### Materials

Set density as Perfect Gas, Viscosity as Sutherland.

### Boundary Conditions

Set the boundary type and values as shown below.

* wing
    + Wall - No slip, adiabatic 

* farfield\_in, farfield\_out
    + Far-Field Riemann 
    + Flow Direction
        + Specification Method : AOA and AOA
        + Direction at AOA=0, AOS=0 : Drag direction (1 0 0), Lift direction (0 1 0)
        + Angle of Atteck : 2.31
        + Angle of Sideslip : 0 
    + Mach Number : 0.729
    + Static Pressure : 108988
    + Static Temperature : 255.556 
    + Turbulence : k and omega (k=0.08, omega=7400)
  
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/RAE2822/rae-farfield.png"> 
    <br> farfield Riemann 
</p>

+ frontAndBackPlanes
    + Empty
  
### Reference Values

Set values as follows

+ Area, Length : 0.3048(chord of airfoil)
+ Density : 1.4858(farfield condition)
+ Pressure : 108988(farfield condition)
+ Velocity : 233.6177(farfield condition)

### Numerical Conditions

Set [Formulation] as [Implicit], [Flux Type] as [Roe-FDS] and [Entropy Fix Coefficient] as 0.5. 

Set [Discretization Schemes] as [Second Order Upwind] for Flow and Turbulence.

Set [Convergence Criteria] as 1e-6 for Density.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/RAE2822/rae-nume.png"> 
    <br> Numerical Conditions
</p>

### Monitor

Select [Add]-[Forces] and set values as follows

+ Flow Direction
    + Specification Method : AOA and AOA
    + Direction at AOA=0, AOS=0 : Drag direction (1 0 0), Lift direction (0 1 0)
    + Angle of Atteck : 2.31
    + Angle of Sideslip : 0 
+ Center of Rotation : (0 0 0)
+ Boundaries : wing


### Initialization

Enter the value and click the Initialize button at the bottom. Then click the [File]-[Save] menu to save the case file.

+ Velocity : (233.428, 9.416, 0)
+ Pressure : 108988
+ Temperature : 255.556
+ Turbulence
    + Scale of Velocity : 233.6177
    + Turbulent Intensity : 0.1
    + Turbulent Viscosity Ratio : 1 

### Run

Change the values as shown below, and click [Start Calculation] button.

+ Number of Iterations : 3000
+ Courant Number : 1000
+ Save Interval : 500


<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/RAE2822/rae-run.png"> 
    <br> Residual graph
</p>

### Post-processing

Click the parview button in [External tools] to open the paraview.

Change [Coloring] to p.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/RAE2822/rae-paraview.png"> 
    <br> pressure contour
</p>
<!------------------------------------------------------------------------------------------>
## ONERA M6 transonic wing

[Download mesh](https://drive.google.com/file/d/1JxCKWMaAFoi--1_VFXkVVIhtus1N0ntG/view?usp=sharing)

[Download simulation](https://drive.google.com/file/d/1mzvMV6pzZt09v1p06xbuY8sdcII3JMhJ/view?usp=sharing)

### Introduction

[![intro](https://github.com/nextfoam/baram-pages/raw/main/screenshots/onera/onera-mesh.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/onera/onera-mesh.png)

This example is a steady-state compressible flow analysis using a density-based solver. As a validation problem for the ONERA M6 wing, the calculation conditions from the following site are used.

[https://www.grc.nasa.gov/WWW/wind/valid/m6wing/m6wing.html](https://www.grc.nasa.gov/WWW/wind/valid/m6wing/m6wing.html)

[![intro](https://github.com/nextfoam/baram-pages/raw/main/screenshots/onera/cp0.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/onera/cp0.png)

[![intro](https://github.com/nextfoam/baram-pages/raw/main/screenshots/onera/cp1.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/onera/cp1.png)

<p align='center'>Pressure coefficient for each section of the wing in the span direction</p>

A structured mesh is converted to OpenFOAM. 

Simulation conditions are as follows

+ solver : TSLAeroFoam
+ turbulence model : $SST$ $k-\omega$
+ Mach number : 0.8395
+ Anglf of Attack : 3.06 degree
+ farfield pressure : 315979 Pa
+ farfield temperature : 255.56 K
+ farfield turbulence : k = 2.714, $\omega$ = 131360

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

Set [Operating Conditions] as 0. 

### Models

For this example, we'll use $SST$ $k-\omega$ model for turbulence.

### Materials

Set Density as Perfect Gas, Viscosity as Sutherland.

### Boundary Conditions

Set the boundary type and values as shown below.

* Wall-wing-1, Wall-wing-2
    + Wall - No slip, adiabatic 

+ sym-1, sym-2, sym-3, sym-4 
    + symmetry
  
* inlet-1, inlet-2, outer-1, outer-2, outlet-1, outlet-2
    + Far-Field Riemann 
    + Flow Direction : direction of AOA 3.06 (0.998574, 0.053382, 0) 
    + Mach Number : 0.8395
    + Static Pressure : 315980
    + Static Temperature : 255.56
    + Turbulence : k and omega(k = 2.714, omega = 131360)
  
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/onera/onera-farfield.png"> 
    <br> farfield Riemann condition
</p>

### Numerical Conditions

Set [Formulation] as [Implicit], [Flux Type] as [Roe-FDS] and [Entropy Fix Coefficient] as 0.5. 

Set [Discretization Schemes] as [Second Order Upwind] for Flow and Turbulence.

Set [Convergence Criteria]-[Density의] as 1e-5.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/RAE2822/rae-nume.png"> 
    <br> Numerical Conditions
</p>

### Monitor

Select [Add]-[Forces] and set values as follows

+ Lift Direction : (-0.053382, 0.998574, 0)
+ Drag Direction : (0.998574, 0.053382, 0)
+ Boundaries : wall-wing-1, wall-wing-2


### Initialization

Enter the value and click the Initialize button at the bottom. Then click the [File]-[Save] menu to save the case file.

+ Velocity : (268.647, 14.361, 0)
+ Pressure : 315980
+ Temperature : 255.56
+ Turbulence
    + Scale of Velocity : 269.031
    + Turbulent Intensity : 0.1
    + Turbulent Viscosity Ratio : 1 


### Run

Change the values as shown below, and click [Start Calculation] button.

+ Number of Iterations : 3000
+ Courant Number : 1000
+ Save Interval : 500

When the calculation is started, you can see the graphs of Residuals and Force monitor as shown below.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/onera/onera-run.png"> 
    <br> Residual graph
</p>

### Post-processing

Click the parview button in [External tools] to open the paraview.

Change [Coloring] to p.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/onera/onera-paraview.png"> 
    <br> pressure distribution
</p>

<!------------------------------------------------------------------------------------------>
## Supersonic space shuttle

[Download mesh](https://drive.google.com/file/d/12oc-gY76vct8fNCBbF4dNbAVqmuVCNVP/view?usp=sharing)

[Download simulation](https://drive.google.com/file/d/1Z4Agp5f_C1MaX_a7aFHvcMGrLrvWLU89/view?usp=sharing)

### Introduction

[![intro](https://github.com/nextfoam/baram-pages/raw/main/screenshots/ss/ss-mesh.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/ss/ss-mesh.png)

This is an example of a steady-state compressible supersonic flow using a density-based solver.

The computational conditions are as follows

+ solver : TSLAeroFoam
+ turbulence model : $SST$ $k-\omega$
+ Mach number : 3
+ Anglf of Attack : 15 degree
+ farfield pressure : 100000 Pa
+ farfield temperature : 288 K

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

Set [Operating Conditions] as 0. 

### Models

For this example, we'll use $SST$ $k-\omega$ model for turbulence.

### Materials

Set Density as Perfect Gas, Viscosity as Sutherland.


### Boundary Conditions

Set the boundary type and values as shown below.

* spaceShuttle
    + Wall - No slip, adiabatic 

+ maxy 
    + symmetry
  
* minx, maxx, miny, minz, maxz
    + Far-Field Riemann 
    + Flow Direction : direction for AOA 15, (0.965926, 0, 0.258819) 
    + Mach Number : 3
    + Static Pressure : 100000
    + Static Temperature : 288
    + Turbulence : intensity and viscosity ratio(0.1 and 1)
  
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ss/ss-farfield.png"> 
    <br> farfield Riemann condition
</p>

  
### Reference Values

Set values as follows

+ Area, Length : 1
+ Density : 1.2097(farfield condition)
+ Pressure : 100000(farfield condition)
+ Velocity : 1020.5933(farfield condition)


### Numerical Conditions

Set [Formulation] as [Implicit], [Flux Type] as [Roe-FDS] and [Entropy Fix Coefficient] as 0.5. 

Set [Discretization Schemes] as [Second Order Upwind] for Flow and Turbulence.

Set [Convergence Criteria]-[Density] as 1e-5.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/RAE2822/rae-nume.png"> 
    <br> Numerical Conditions
</p>

### Monitor

Select [Add]-[Forces] and set values as follows

+ Lift Direction : (-0.258819, 0, 0.965926)
+ Drag Direction : (0.965926, 0, 0.258819)
+ Boundaries : spaceShuttle


### Initialization

Enter the value and click the Initialize button at the bottom. Then click the [File]-[Save] menu to save the case file.

+ Velocity : (985.817, 0, 264.149)
+ Pressure : 100000
+ Temperature : 288
+ Turbulence
    + Scale of Velocity : 1020.5933
    + Turbulent Intensity : 0.1
    + Turbulent Viscosity Ratio : 1 


### Run

Change the values as shown below, and click [Start Calculation] button.

+ Number of Iterations : 3000
+ Courant Number : 0.1
+ Save Interval : 500

For supersonic flows, starting with a high Courant Number often results in divergence, so starting with a small value and increasing it gradually as the calculation stabilizes can speed up convergence. During the calculation, you can modify the value in [Run Condition] and press the [Update Configuration] button in [Run] to apply it. In this example, we started with 0.1 and increased the value to 1 at around 400 iterations and then to 100 at around 700 iterations.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ss/ss-run.png"> 
    <br> Residual graph
</p>

### Post-processing

Click the parview button in [External tools] to open the paraview.

Change [Coloring] to p.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ss/ss-paraview.png"> 
    <br> pressure distribution
</p>
<!------------------------------------------------------------------------------------------>
## Supersonic nozzle

[Download mesh](https://drive.google.com/file/d/1Z5d0Ic9GsMxF1fPr8rSCpv9juU223xuM/view?usp=sharing)

[Download simulation](https://drive.google.com/file/d/1Bs0zWE-aVx8dZIik4VDxQt4H3WVuBEYS/view?usp=sharing)

### Introduction

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/nozzle/nozzle-mesh.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/nozzle/nozzle-mesh.png)

This is an example of solving an axisymmetric supersonic nozzle flow using a density-based solver.

The computational conditions are as follows

+ solver : TSLAeroFoam
+ turbulence model : $SST$ $k-\omega$
+ nozzle inlet total pressure : 4e+5 Pa
+ nozzle inlet temperature : 3000 K
+ farfield pressure : 1e+4 Pa
+ farfield temperature : 300 K

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

Set [Operating Conditions] as 0. 

### Models

For this example, we'll use $SST$ $k-\omega$ model for turbulence.

### Materials

Set Density as Perfect Gas, Viscosity as Sutherland.

### Boundary Conditions

Set the boundary type and values as shown below.

* inlet
    + Pressure Inlet
    + Total Pressure : 400000
    + Temperature : 3000
    + Turbulence : intensity and viscosity ratio(0.1 and 1)
+ inletAir
    + Pressure Inlet
    + Total Pressure : 10000
    + Temperature : 300 K
    + Turbulence : intensity and viscosity ratio(0.1 and 1)
+ outlet
    + Pressure Outlet
    + Pressure : 10000
+ nozzle
    + Wall - no slip, adiabatic
+ top 
    + symmetry 
* bottomEmptyFaces, topEmptyFaces
    + Wedge 
  
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/nozzle/nozzle-inlet.png"> 
    <br> farfield Riemann condition
</p>

  
### Numerical Conditions

Set [Formulation] as [Implicit], [Flux Type] as [Roe-FDS] and [Entropy Fix Coefficient] as 0.5. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/nozzle/nozzle-nume.png"> 
    <br> Numerical Conditions
</p>

### Monitor

Monitor flowrate at nozzle inlet. Select [Add]-[Surface] and set values as follows

+ Report Type : Mas Flow Rate
+ Surface : inlet


### Initialization

Set values as follows

+ Velocity : (0, 0, 0)
+ Pressure : 10000
+ Temperature : 300
+ Turbulence
    + Scale of Velocity : 100
    + Turbulent Intensity : 0.1
    + Turbulent Viscosity Ratio : 1 

The solver is unstable at the beginning of the calculation because of the large difference between the flow variables in the region through which the nozzle flow passes and the outer region. To solve this problem, set the initialization values for the nozzle flow section separately.

Click [Initialization]-[Advanced]-[Section]-[Create] and set the following settings.

+ region1 - Hex
    + Min. point : (-0.15, -0.1, -0.1)
    + Max. point : (0.12, 0.065 0.1)
    + Select Velocity and set value (100, 0, 0)
    + Select Pressure and set value 400000


<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/nozzle/nozzle-setFields.png">
    <br> Section initialize
</p>

### Run

Change the values as shown below, and click [Start Calculation] button.

+ Number of Iterations : 100000
+ Courant Number : 1000
+ Save Interval : 500

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/nozzle/nozzle-run.png"> 
    <br> Residual graph
</p>

### Post-processing

Click the parview button in [External tools] to open the paraview.

Change [Coloring] to Mach.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/nozzle/nozzle-paraview.png"> 
    ><br> Mach number
</p>


