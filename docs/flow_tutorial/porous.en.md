# Porous Media

## Porous cell zone

[Download mesh](https://drive.google.com/file/d/1V0pE28Q-8MEHR-VXaE6upP5mgt_cImGF/view?usp=sharing)

[Download simulation](https://drive.google.com/file/d/1WPrNMbi0e91vl34ZO3O_HFbzCyE2QQJS/view?usp=sharing)

### Introduction 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousMedia/intro.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousMedia/intro.png)

This is an example of flow analysis using porous media conditions. The problem involves flow entering from the top left and flowing down through a porous region (the blue area is the porous cell zone on the left side of the image above).

OpenFOAM's porous media model suffers from a discontinuous velocity distribution in the porous region, and the pressure loss is slightly different from the input conditions. The left side of the figure below shows the results from Baram-v24, while the right side shows the results using the standard solver in OpenFOAM 2306. 

NextFOAM, which Baram uses, solves this problem by improving the interpolation of pressure in porous regions (see the documentation linked below for more information). You can see that the convergence is much improved along with the accuracy of the results.

### * [Porous Media Reference](https://nextfoam.co.kr/proc/DownloadProc.php?fName=231101140051_yvpJhMF0nY.pdf&realfName=10thOKUCC_OpenFOAM%EC%82%AC%EC%86%8C%ED%95%9C%EB%AC%B8%EC%A0%9C%EB%93%A4.pdf)

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/porousMedia/res.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/porousMedia/res.png)

<p align='center'>결과 (left)Baram v24, (right)openfoam 2306 standard solver</p>

[![residual](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/porousMedia/residual-1.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/porousMedia/residual-1.png)

<p align='center'>Residual (left)Baram v24, (right)openfoam 2306 standard solver</p>

### Start BaramFlow and load mesh

Run the program and select [New Case] from the launcher. In the launcher, select [Pressure-based] for [Solver Type] and [None] for [Multiphase Model].

Use the given polyMesh folder. In the top tab, click [File]-[Load Mesh]-[OpenFOAM] in that order and select the polyMesh folder. 

### General

For this example, we'll use default conditions.

### Models

For this example, we'll use $Standard$ $k-\epsilon$ model for turbulence.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousMedia/turbulence.png"><br> Turbulence Model setup
</p>
<br/>

### Materials

Use material properties of air.

### Cell zone Conditions

In Cell Zone Conditions, there is a porousZone in region0. Double-click it to open the settings window. Select the Zone Type as 'Porous Zone', and set values as follows

+ Model : Power Law
+ C0 : 5000
+ C1 : 1.9

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousMedia/cellZone.png"><br> Cell Zone Conditions setup
</p>

### Boundary Conditions

Set the boundary type and values as shown below.

+ inlet
    + type : Velocity Inlet
    + Velocity magnitude : 1
    + Turbulent Intensity : 1
    + Turbulent Viscosity Ratio : 10 
 
 <p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousMedia/inlet.png"><br> inlet boundary condition
 </p>

+ outlet
    + type : Pressure Outlet
    + Total Pressure : 0 
  
   <p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousMedia/outlet.png"><br> outlet boundary condition
   </p>

Set as wall for the rest.

### Numerical Conditions

Set [Discretization Schemes]-[Pressure] as Linear. If you have a momentum source like Porous, use Linear because Momentum Weighted or Momentum Weighted Reconstruct techniques can have stability issues. 
([user guide](https://baramcfd.org/userguidelist/2023/09/05/numericalCondition-post/))

Set [Convergence Criteria]-[Pressure] as 0.0001 and use default values for the rest.

### Initialization

Use default conditions.

Click the Initialize button at the bottom. Then click the [File]-[Save] menu to save the case file.

### Run

Change the values as shown below, and click [Start Calculation] button.

+ Number of Iterations : 1000
+ Save Interval : 1000
+ Selct [Parallel]-[Environment] in menu. Set Number of Cores as you want and select [Local Machine] for [Parallel Type].

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousMedia/parallel.png"><br> parallel setup
</p>

When the calculation is started, you can see the graphs of Residuals as shown below.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousMedia/residual.png"><br> residual plot
</p>

### Post-processing

Click the parview button in [External tools] to open the paraview.

For parallel case, change the [Case Type] to [Decomposed Case].

<p style="text-align: left">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousMedia/slice.png"> Click [Slice] icon.
</p>

In the Pipeline Browser, select Y Normal and choose Coloring as U. You should see something like this

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousMedia/post.png"> <br> velocity at the center plane
</p>
<!---------------------------------------------------------------------------------------------------------->
## Porous Jump 

[Download mesh](https://drive.google.com/file/d/1c7RgueGF8kfG_pqA0tGbU_TbPpZ99tNG/view?usp=sharing)

[Download simulation](https://drive.google.com/file/d/1OnXwBiE6TIIh11fSAyhqJlfJ6y9jZ72_/view?usp=sharing)

### Introduction 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousJump/9.1.png"><br>
</p>

This is an example of the Porous Jump boundary condition, which can simulate a porous plate or fan inside the computational domain as a thicknessless boundary. The problem is to create a square face inside a hexahedral duct and vary the pressure and velocity using the Porous Jump boundary condition.

The computational conditions are as follows

+ solver : buoyantsimpleNFoam
+ turbulence model : $Standard$ $k-\epsilon$ model
+ density : 1.225 $kg/m^3$
+ viscosity : 1.79e-5 $kg/ms$
+ boundary conditions for porous jump
    + Darcy Coefficient : -100
    + Inertial Coefficient : -5
    + porous media in the flow direction : 0.05 m

The porousBafflePressure boundary condition that computes the Porous Jump uses the expression below.

<p align='left'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousJump/9.2.png"><br>
</p>

### Start BaramFlow and load mesh

Run the program and select 'New Case' from the launcher. In the launcher, select Pressure-based for 'Solver Type' and None for 'Multiphase Model'.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/launcher.png"><br>
</p>

Use the given polyMesh folder. In the top tab, click [File]-[Load Mesh]-[OpenFOAM] in that order and select the polyMesh folder. 

### General

For this example, we'll use default conditions.

### Models

For this example, we'll use $Standard$ $k-\epsilon$ model for turbulence.

### Materials

Use material properties of air. 

### Boundary Conditions

Set the boundary type and values as shown below.

+ duct : Wall
    + Velocity Condition : No Slip

+ inlet : Velocity Inlet
    + Velocity Specification Method : Magnitude, Normal to Boundary
    + Profile Type : Constant
    + Velocity Magnitude : 10 (m/s)
    + Turbulent Intensity : 1 (%)
    + Turbulent Viscosity Ratio : 10 (m)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousJump/9.3.png"><br>
</p>

+ outlet : Pressure Outlet
    + Total Pressure : 0 (Pa)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousJump/9.4.png"><br>
</p>

+ plane_master : Porous Jump
    + Coupled Boundary : plane_slave
    + Darcy Coefficient : -100
    + Inertial Coefficient : -5
    + Porous Media Thickness : 0.05

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousJump/9.5.png"><br>
</p>

### Numerical Conditions

In this example, we'll change the settings as shown below.

+ Pressure-Velocity Coupling Scheme : SIMPLE

+ Discretization Schemes
    + Momentum : Second Order Upwind
    + Turbulence : First Order Upwind

+ Convergence Criteria
    + Pressure : 1e-6
    + Momentum : 0.001
    + Turbulence : 0.001

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousJump/9.6.1.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousJump/9.6.2.png"><br>
</p>

### Initialization

Enter the values and click the Initialize button at the bottom. Then click the [File]-[Save] menu to save the case file.

+ X-Velocity : 10 (m/s)
+ Pressure : 0 (Pa)
+ Turbulence
    + Scale of Velocity : 10 (m/s)
    + Turbulent Intensity : 1 (%)
    + Turbulent Viscosity Ratio : 10

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousJump/9.7.png"><br>
</p>

### Run

Change the values as shown below, and click [Start Calculation] button.

+ Number of Iterations : 1000
+ Save Interval : 1000
+ Data Write Format : Binary
+ Number of Cores : 1 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousJump/9.8.png"><br>
</p>

When the calculation is started, you can see the graphs of Residuals as shown below.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousJump/9.9.png"><br>Residual plot
</p>

### Post-processing

Draw the pressure distribution inside the duct.

Click the parview button in [External tools] to open the paraview.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousJump/9.10.png"><br>
</p>

Select the [Slice] icon in the top toolbar and click the [Z Normal] button.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousJump/9.11.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousJump/9.12.png"><br>
</p>

Change [Solid Color] to p\_rgh.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousJump/9.13.png"><br>
</p>

