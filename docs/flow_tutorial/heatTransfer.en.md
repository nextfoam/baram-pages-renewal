# Heat Transfer

## Conjugate Heat Transfer

[Download mesh](https://drive.google.com/file/d/1f5GOixMllPA3UXtCD2sPAV8vxpfWI6rI/view?usp=sharing)

[Download simulation](https://drive.google.com/file/d/1yyFUzNlA3fyoXMdc9MrLn5vs7zykL4GM/view?usp=sharing)

### Introduction 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.1.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.1.png)

This is an example of heat transfer analysis including heat conduction in a solid. It is computed with a multi-region method consisting of two regions: solid and fluid. The solver uses chtMultiRegionSimpleNFoam developed by NextFoam.

A circular fluid region is surrounded by a rectangular solid region. The fluid region has a rotating rod and uses MRF. The temperature of the rod is 373 K, and gravity acts in the -y direction.

The calculation conditions are as follows 

+ solver : chtMultiRegionSimpleNFoam
+ turbulence model : $Standard$ $k-\epsilon$
+ material properties of fluid
    + density : Perfect Gas
    + viscosity : 1.79e-5 $kg/ms$
    + thermal conductivity : 0.024 $5W/mK$
    + heat capacity(Cp) : 1,006 $J/kg$
+ material properties of solid
    + density : 1,900 $kg/m^3$
    + thermal conductivity : 0.016 $W/mK$
    + heat capacity(Cp) : 1,004 $J/kg$

### Start BaramFlow and load mesh

Run the program and select [New Case] from the launcher. In the launcher, select [Pressure-based] for [Solver Type] and [None] for [Multiphase Model].

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/launcher.png"><br>
</p>

Use the given polyMesh folder. In the top tab, click [File]-[Load Mesh]-[OpenFOAM] in that order and select the polyMesh folder. 

### General

Set gravity of y direction as -9.81.

### Model

For this example, we'll use $Standard$ $k-\epsilon$ model for turbulence.

Energy is automatically set to include when the Multi-region lattice is read.

### Materials

For this example, we will use the materials Air and Solid. 

Materials defaults to Air, and click the (+) button to select and add a random Solid. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.2.1.png"><br>
</p>

Set material Properties of Air and Solid as follows

#### Air

+ Density : Perfect Gas
+ Specific Heat capacity $C_p$ : 1,006
+ Viscosity : 1.79e-05
+ Thermal Conductivity : 0.0245
+ Molecular Weight : 28.966
+ Absorption Coefficient : 0.0

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.2.2.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.2.png"><br>
</p>

#### Solid

+ Density : 1,900
+ Specific Heat capacity $C_p$ : 1004
+ Thermal Conductivity : 0.016
+ Emissivity : 0.039

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.3.png"><br>
</p>

Absorption Coefficient and Emissivity are not used because radiation is not considered.

### Cell zone Conditions

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.20.png"><br>
</p>

Double click fluid region and set select [Materials] as air. 

Double click solid region and select [Materials] as solid.

Double click rotor of fluid region and select MRF and set values as follows

+ Rotating Speed (RPM) : 100
+ Rotation-Axis Origin : (0, 0, 0)
+ Rotation-Axis Direction : (0, 0, 1)

### Boundary Conditions

Set the boundary type and values as shown below.

+ fluid\_to\_solid : Thermo-Coupled Wall(because fluid\_to\_solid and solid\_to\_fluid is at the same position, they mush be [Coupled Boundary])
    + Coupled Boundary : Region1:solid\_to\_fluid

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.6.png"><br>
</p>

+ inner-wall : wall
    + Velocity Condition : No Slip
    + Temperature : Constant Temperature
    + Temperature : 373 (K)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.7.png"><br>
</p>

+ external-wall : wall
    + Velocity Condition : No Slip
    + Temperature : Convection
    + Heat Transfer Coefficient ($W/m^2 K$) : 4
    + Free Stream Temperature : 280 (K)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.8.png"><br>
</p>

+ frontAndBackPlanes : Empty

### Numerical Conditions

In this example, we'll change the settings as shown below.

+ Pressure-Velocity Coupling Scheme : SIMPLE

+ Discretization Schemes
    + Pressure : Momentum Weighted Reconstruct
    + Momentum : Second Order Upwind
    + Energy : Second Order Upwind
    + Turbulence : First Order Upwind

+ Under-Relaxation Factors : use default values

+ Convergence Criteria : use 1e-6 for Energy and fefault value for the rest

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.9.1.png"><br>
</p>


### Monito

Monitor the Area-Weighted Average Temperature of the fluid\_to\_solid surface.

On the Monitoring tab, click Add - Surfaces and set it up as shown below.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.10.png"><br>
</p>

### Initialization

Initialization is set for fluids and solids separately. You can set the initialization for fluid and solid separately by selecting the tabs at the bottom.

#### fluid

+ Velocity : (0 0 0) (m/s)
+ Pressure : 0 (Pa)
+ Temperature : 350 (K)
+ Turbulence
    + Scale of Velocity : 1 (m/s)
    + Turbulent Intensity : 1 (%)
    + Turbulent Viscosity Ratio : 10

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.11.png"><br>
</p>

#### solid

+ Temperature : 350 (K)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.11.png"><br>
</p>

Enter the value and click the Initialize button at the bottom. Then click the [File]-[Save] menu to save the case file.

### Run

Change the values as shown below, and click [Start Calculation] button.

+ Number of Iterations : 1000
+ Save Interval : 100
+ Data Write Format : Binary

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.12.png"><br>
</p>


<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.13.png"><br>residual plot
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.14.png"><br>monitoring
</p>

### Post-processing

Click the parview button in [External tools] to open the paraview.

At the [Regions] activate [fluid/internalMesh] and [solid/internalMesh]. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.15.png"><br>
</p>

On the tab at the top, change [vtkBlockColors] to T.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.16.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.17.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.18.png"><br>
</p>


<!---------------------------------------------------------------------------------------------------------->
