# Cell Zone Conditions

The cell zone condition displays the regions and cell zones of the mesh, as shown in the figure below. For a single-region problem, only one region is displayed; for a multi-region problem, as many regions appear. The cell zones belonging to each region appear below it.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/cellZoneUI.png" width="300" height="300"><br>Cell zone setup</center>

## Region

Double-click on a Region to bring up the window of figure below. 

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/region.png"><br>region setup</center>

Select one of the materials in [Setup-Materials] as the material for the region. If you are calculating a mixture of chemical species, select the appropriate mixture. If you want to give the entire region a source term or a constant value, set the appropriate item.

In the case of multiphase flow, set one as primary and the other as secondary, and set the surface tension and cavitation between the two substances. Click the Select button in the figure~\ref{fig:regionVOF} to open the window shown in the figure on the right to select the substances.

**If you are using a cavitation model, you should set primary to gas and secondary to liquid**.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/regionVOF.png"><br>Select fluid in multiphase</center>

### Cavitation Models

The cavitation model sets the vapor pressure, the model, and the model coefficients. Cavitation occurs when the pressure of the liquid is lower than the vapor pressure.

[Cavitation tutorial : NACA66 hydrofoil](https://baramcfd.org/tutorials/2024/10/07/cavitation-post/)

#### Schnerr-Sauer

This is the model presented in the following paper. Enter the Evaporation Coefficient ($C_v$), Condensation Coefficient ($C_c$), and the diameter (dNuc) and number density (n) of the bubble.

*Schnerr, G. H., And Sauer, J., "Physical and Numerical Modeling of Unsteady Cavitation Dynamics", Proc. 4th International Conference on Multiphase Flow,         New Orleans, U.S.A., 2001.*

#### Kunz

This is the model presented in the following paper. Enter th Evaporation Coefficient($C_v$), Condensation Coefficient($C_c$), Mean flow time scale($t_{Inf}$) and free stream velocity($U_{Inf}$).

*Kunz, R.F., Boger, D.A., Stinebring, D.R., Chyczewski, Lindau. J.W.,         Gibeling, H.J., Venkateswaran, S., Govindan, T.R., "A Preconditioned Implicit Method for Two-Phase Flows with Application to Cavitation Prediction," Computers and Fluids, 29(8):849-875, 2000.*

#### Merkle

This is the model presented in the following paper. Enter Evaporation Coefficient($C_v$), Condensation Coefficient($C_c$), Mean flow time scale($t_{Inf}$) and free stream velocity($U_{Inf}$).

*C. L. Merkle, J. Feng, and P. E. O. Buelow, "Computational modeling of the dynamics of sheet cavitation", in Proceedings Third International Symposium on Cavitation Grenoble, France 1998.*

#### Zwart-Gerber-Belamri

This is the model presented in the following paper. Enter Evaporation Coefficient($C_v$), Condensation Coefficient($C_c$), bubble diameter(dNuc)and nucleate site volume fraction(aNuc). **This is not supported in Baram-v24.4 and will be supported in the next version of Baram**.

*P. J. Zwart, A. G. Gerber, and T Belamri. A two-phase flow model for predicting cavitation dynamics. In Proceedings of the International Conference on Multiphase Flow (ICMF 04), Yokohama, Japan, 2004*
<!---------------------------------------------------------------------------------------------------->
## Cell Zone Model

Double-click the cell zone to bring up the window of figure below. You can set the Zone Type, Source Terms, and Fixed Values.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/cellZone.png"><br>cell zone setup</center>

### Multiple Reference Frame, MRF

When simulating a rotating object, multiple reference frames are a way to compute the flow around the rotating body in a rotating reference coordinate system without rotating the object. When you select Multiple Reference Frame, the settings section shown in the figure below appears. The detailed settings are as follows

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mrf.png" width="300" height="300"><br>MRF setup</center>

* Rotating Speed : RPM, Revolution Per Minute
* Rotating Axis Origin
* Rotating Axis Direction : **Set the rotation direction to be counterclockwise based on the right-hand rule**.
* Static Boundary : **You must also select an interface face. If you don't know which of the two interface faces is in the cell zone, you can select both. Including faces that are not in the cell zone does not affect the calculation**.

[MRF tutorial : fan](https://baramcfd.org/tutorials/2024/03/21/simpleFan-post/)

### Sliding Mesh

A sliding mesh is a method of organizing cellzones around a moving (currently only rotationally supported in the current version, translationally supported in the future) object and moving the mesh around it, where each boundary between a moving cellzone and a stationary cellzone must be configured as an interface. **Moving boundaries inside a cellzone should use a moving wall boundary condition**.

The settings are rotation speed (RPM), rotation axis center coordinates, and rotation axis direction, with the rotation direction being the same as in a MRF.

[Sliding Mesh tutorial : propeller](https://baramcfd.org/tutorials/2024/06/17/propeller-post/)

### Porous Zone

When there is porous media or very complex geometry in a small area, it is a way to model the pressure loss as a function of velocity without directly implementing the geometry. The pressure loss can be modeled using the Darcy-Forchheimer model and the power law model.

OpenFOAM's porous media model suffers from a discontinuous velocity distribution in the porous region, and the pressure loss is slightly different from the input conditions. NextFOAM, which Baram uses, solves this problem by improving the interpolation of pressure in porous regions.([For more information on this, see the documentation at this link](https://nextfoam.co.kr/proc/DownloadProc.php?fName=231101140051_yvpJhMF0nY.pdf&realfName=10thOKUCC_OpenFOAM%EC%82%AC%EC%86%8C%ED%95%9C%EB%AC%B8%EC%A0%9C%EB%93%A4.pdf))

[Porous Zone tutorial : duct flow](https://baramcfd.org/tutorials/2023/09/05/porousMedia-post/)

#### Darcy-Forchheimer model

You can set the pressure loss for each of the three directions. The direction is determined by two vectors. The two vectors are determined by the inputs(Direction-1 Vector, Direction-2 Vector) and the third direction is the direction perpendicular to the two vectors.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/darcy.png" width="300" height="300"><br>Darcy-Forchheimer model setup</center>

Enter the Inertial Resistance Coefficient(f, Forchheimer coefficient) and Viscous Resistance Coefficient(d, Darcy coefficient) as vectors. The pressure loss is calculated by the following equation.

<center>$S_i = \left( \mu d + \rho |U| \frac{f}{2} \right) U$</center>

#### Power law model

Using two values of $C_0$ and $C_1$, calculate with the following equation

<center>$S_i = - \rho C_0 |U|^{C_1 -1} U$</center>

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/porous-powerlaw.png" width="300" height="300"><br>Power Law model setup</center>

### Actuator Disk

The Actuator Disk model models a rotating body, such as a propeller, as a disk and treats it as a source term for velocity. It has the following settings

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/actuatorDisk.png" width="300" height="300"><br>Actuator disk setup</center>

* Disk Direction
* Power Coefficient, $C_p$
* Thrust Coefficient, $C_t$
* Disk Area, $A$
* Upstream Point : Coordinates of any point upstream of the disk
* Force Computation method : Froude's method or Variable-Scaling method

Among the methods for calculating thrust, Froud's method uses the following equation

<center>$T = 2 \rho A |u_m \cdot n|^2 a(1-a)$</center>

<center>$a = 1 - \frac{C_p}{C_t}$</center>

To calculate thrust, the Variable-Scaling method uses the following expression

<center>$T = 0.5 \rho A |u_0 \cdot n|^2 C_T ^*$</center>

<center>${C_T}^* = C_T \left( \frac{|u_m|}{|u_0|}\right)^2$</center>

* T : Thrust magnitude
* $\rho$ : Monitored incoming fluid density
* A : Actuator disk planar surface area
* $U_m$ : Incoming velocity spatial-averaged on monitored region
* $U_0$ : Incoming velocity spatial-averaged on actuator disk
* n : Surface-normal vector of the actuator disk pointing upstream
* a : Axial induction factor
* $C_p$ : Power coefficient
* $C_T$ : Thrust coefficient
* $C_T*$ : Calibrated thrust coefficient

## Source Terms

You can set source terms for energy, mass, turbulence-related fields, and more. 

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/source.png" width="300" height="300"><br>Source term setup</center>

There are two ways to give the source term: a value to be applied to the entire cell zone or a value to be applied per unit volume.

For transient problems, source terms can be given by piecewise linear and polynomial according to time.

[Time dependent source tutorial : Fire in Room](https://baramcfd.org/tutorials/2024/06/16/fireInRoom-post/)

## Fixed Values

You can fix the mean value of the velocity vector, temperature, and turbulence field in the cellzone. For velocity, a relaxation factor is used because it can cause instability in the calculation.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/fixedValue.png" width="300" height="300"><br>Fixed values setup</center>


