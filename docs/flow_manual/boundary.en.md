# Boundary Conditions

When you select Boundary Conditions, the settings will display the boundaries for each region as shown below. You can set a boundary condition by selecting a boundary and pressing the right mouse button. When a boundary is selected, it is colored red in the graphic window. 

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/boundaryCondition.png" width="300" height="300"><br>Boundary Conditions setup</center>

You can use the filter string feature to filter by entering a string to ensure that only boundary surfaces containing a specific string are displayed.

The boundary conditions are divided into four categories: Inlet, Outlet, Wall, and Misc, and each category contains the following types of boundary conditions. The display differs depending on the condition, such as multiphase flow, compressible flow, etc. Select a boundary surface and double-click it or click the Edit button to display the detailed settings of the boundary condition.

* Inlet
      + Velocity Inlet
      + Flow Rate Inlet
      + Pressure Inlet
      + ABL Inlet, Atmospheric Boundary Layer Inlet
      + Free Stream
      + Open Channel Inlet) : Only for multiphase
      + Far-field Riemann) : Only for compressible flow
      + Subsonic Inlet) : Only for compressible flow
      + Supersonic Inflow) : Only for compressible flow
* Outlet
      + Pressure Outlet
      + Outflow
      + Open Channel Outlet) : Only for multiphase
      + Subsonic Outflow) : Only for compressible flow
      + Supersonic Outflow) : Only for compressible flow
* Wall
      + Wall
      + Thermo-Coupled Wall
* Misc.
      + Symmetry
      + Interface
      + Empty
      + Wedge
      + Cyclic
      + Porous Jump
      + Fan

## Velocity Inlet

Velocity Inlet condition is condition that give velocity, turbulence, temperature, chemical species mass fraction values, etc. at the inlet of the flow.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/velocityInlet.png" width="300" height="300"><br>Velocity Inlet setup</center>

Velocity can be given by the x, y, and z components(Component) or by the normal velocity boundary (Magnitude, Normal to Boundary).

Turbulence can be represented by the value of the turbulence field (k, epsilon, omega, nuTilda) and the turbulence intensity and viscosity ratio (intensity/viscosity ratio). The Spalart-Allmaras model only supports the method of giving the Modified Turbulent Viscosity $\tilde{\nu}$.

The temperature is given a constant value.

The species mass fraction should be given so that the sum of all chemical species is 1.

The openfoam boundary conditions used by each field are as follows

* Velocity : _fixedValue_ for Component, _surfaceNoramlVelocity_ for Magnitude
* Pressure : _zeroGradient_
* Temperature : _fixedValue_
* Turbulent kinetic energy(k) : _turbulentIntensityInletOutletTKE_
* Turbulent dissipation rate($\epsilon$), specific dissipation rate($\omega$) : _viscosityRatioInletOutletTDR_
* Modified kinematic viscosity($\tilde{\nu}$) : _fixedValue_
* Species : _fixedValue_

### Boundary profile

In the Velocity Inlet condition, the velocity and temperature can be specified as a temporal variation or a spatial distribution.

#### Temporal Distribution

If the velocity distribution (Profile Type) is selected as 'Temporal Distribution' for the Velocity Inelt condition, it can be specified using a piecewise linear function in the window of firgure below.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/profile.png" width="300" height="300"><br>Temporal Distribution - piecewise linear</center>

If the temperature distribution under the Velocity Inlet condition is selected as 'Temporal Distribution', it can be specified as a piecewise linear function and a polynomial. The polynomial specifies the coefficients $a_n$ of the expression in the window of figure below.

<center>$S = a_0 \cdot t^0 + a_1 \cdot t^1 + a_2 \cdot t^2 + ... + a_n \cdot t^n$</center>

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/polynomial.png" width="300" height="300"><br>Temporal Distribution - polynomial</center>

[Time dependent boundary condition tutorial](https://baramcfd.org/tutorials/2023/09/05/ProfileBC-post/)

#### Spatial Distribution

In the Velocity Inlet condition, you can select a CSV file when you select the distribution of velocity or temperature as a spatial distribution. If Velocity specification method is 'Magnitude, Normal to Boundary', 'Spatial Distribution' is not applicable for velocity.

The csv file should contain the values of (x, y, z, value) separated by comma(,). In the Velocity Inlet condition, the velocity would be in the following format: (x, y, z, Ux, Uy, Uz). The data must be in a single plane and not on a single straight line. If you have a distribution in only one direction, such as in a two-dimensional problem, at least one point must be outside the straight line. 

```
0.0, 0.0, 0.0, 0.1, 0.2, 0.0
0.0, 0.7, 0.0, 0.1, 0.3, 0.0
0.0, 1.2, 0.0, 0.1, 0.4, 0.0
...
```

## Flow Rate Inlet

Flow Rate Inlet is condition that give flow rate, turbulence, and temperature values at the inlet of the flow.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/flowrate.png" width="300" height="300"><br>Flow Rate Inlet setup</center>

The flow rate can be given a mass flow rate and a volume flow rate, and the turbulence and temperature are the same as for the 'Velocity Inlet' condition.

The openfoam boundary condition used by the velocity(U) is _flowRateIneltVelocity_, with the same pressure, turbulence, and temperature as the 'Velocity Inlet' condition. 

## Pressure Inlet

Pressure Inlet is a condition that gives the total pressure, turbulence, and temperature values at the inlet of the flow.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/pressureInlet.png" width="300" height="300"><br>Pressure Inlet setup</center>

Total pressure can be given as a constant, and the turbulence and temperature are the same as for the 'Velocity Inlet' condition.

The boundary conditions for openfoam use _totalPressure_ for the pressure and _pressureInletOutletVelocity_ for the velocity.

## ABL Inlet

ABL Inlet is the condition that gives the velocity and turbulence distribution of the atmospheric boundary layer at the inlet of the flow.

[Atmospheric Boundary Layer tutorial](https://baramcfd.org/tutorials/2023/09/05/Atomospheric-post/)

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/ablInlet.png" width="300" height="300"><br>ABL Inlet setup</center>

The inputs are as follows

* Flow Direction
* Ground-Normal Direction
* Reference Height, $z_{ref}$
* Reference Flow Speed, $U_{ref}$) : velocity at reference height
* Surface Roughness Length, $z_0$
* Minimum z-coordinate, d) : minimum z-coordinate of ground(height from the ground is calculated as z-d)

The velocity and turbulence distributions use the following equations

<center>$u = \frac{u^*}{\kappa} ln\left(\frac{z - d + z_0}{z_0}\right)$</center>

<center>$k = \frac{(u^* )^2}{\sqrt{C_\mu}} \sqrt{C_1 ln \left( \frac{z - d + z_0}{z_0} \right) + C_2}$</center>

<center>$\epsilon = \frac{(u^* )^3}{\kappa (z - d + z_0)} \sqrt{C_1 ln \left( \frac{z - d + z_0}{z_0} \right) + C_2}$</center>

<center>$\omega = \frac{u^*}{\kappa \sqrt{C_\mu}} \frac{1}{z - d + z_0}$</center>

<center>$u^* = \frac{u_{ref} \kappa} {ln \left( \frac{z_{ref} + z_0}{z_0} \right)}$</center>

* $z$ : z-coordinate
* $d$ : minimum z-coordinate of ground
* $\kappa$ : Von Karman's constant, 0.41
* $C_\mu$ : constant, 0.09
* $C_1$ : constant, 0
* $C_2$ : constant, 1

The boundary conditions in openfoam used by each field are as follows

* Velocity : _atmBoundaryLayerInletVelocity
* Tressure : _zeroGradient
* Turbulent kinetic energy(k) : _atmBoundaryLayerInletK
* Turbulent dissipation rate($\epsilon$), specific dissipation rate($\omega$) : _atmBoundaryLayerInletEpsilon_, _atmBoundaryLayerInletOmega_
* Species : _fixedValue_

## Free Stream

Free Stream is a condition where the flow has a constant velocity entering the domain and a zero velocity gradient leaving the domain. It is often used as a farfield boundary condition for incompressible external flows.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/freestream.png" width="300" height="300"><br>Free Stream</center>

For Free Stream Velocity, enter a velocity vector, pressure is a constant, and turbulence and temperature are the same as for the 'Velocity Inlet' condition.

The openfoam boundary conditions used by each field are as follows

* Velocity : _freestreamVelocity_
* Pressure : _freestreamPressure_
* Temperature : _freestream_
* Turbulence : _freestream_
* Species : _fixedValue_

## Open Channel Inlet

Open Channel Inlet is a condition where flow rate is constant and the water surface height can change accordingly when calculating the free surface.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/openChannelInlet.png" width="300" height="300"><br>Open Channel Inlet setup</center>

Enter the volume flow rate as a constant, and give the turbulence the same as the 'Velocity Inlet' condition.

The openfoam boundary conditions used by each field are as follows

* Velocity : _variableHeightFlowRateInletVelocity_
* Pressure : _zeroGradient_
* Volume fraction : _variableHeightFlowRate_
* Turbulent kinetic energy(k) : _turbulentIntensityInletOutletTKE_
* Turbulent dissipation rate($\epsilon$), specific dissipation rate($\omega$) : _viscosityRatioInletOutletTDR_
* Modified kinematic viscosity($\tilde{\nu}$) : _fixedValue_

## Far-field Riemann

Riemann boundary condition used for farfield of compressible flow.

[Far-field Riemann tutorial : RAE2822 airfoil](https://baramcfd.org/tutorials/2024/03/21/rae2822-post/)

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/riemann.png"><br>Farfield Riemann setup</center>

Enter the direction vector of the flow, Mach number, static pressure and static temperature. 

There are two ways to determine the direction of the flow: Direct and “AOA and AOS". For Direct, enter the direction vector. For “AOA and AOS", enter the direction of drag and lift when the AOA and AOS are zero, as well as the value of AOA and AOS.

The openfoam boundary conditions for velocity, pressure, and temperature are _farfieldRiemann_ and the turbulence is the same as the 'Velocity Inlet' condition.

## Subsonic Inlet

Subsonic Inlet is an inlet subsonic boundary condition for internal flows such as turbo-machinery in compressible flows. Enter the flow direction vector, total pressure, and total temperature.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/subsonicInflow.png" width="300" height="300"><br>Subsonic Inlet setup</center>

The openfoam boundary condition for velocity, pressure, and temperature is _subsonicInflow_ and the turbulence is the same as the 'Velocity Inlet' condition.

## Supersonic Inflow

Supersonic Inflow is a boundary condition used when the inlet of the flow is supersonic. Enter a velocity vector, static pressure, and static temperature.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/supersonicInflow.png" width="300" height="300"><br>Supersonic Inflow setup</center>

The openfoam boundary conditions for velocity, pressure, and temperature are _fixedValue_ and the turbulence is the same as the 'Velocity Inlet' condition.

## Pressure Outlet

Pressure Outlet is a condition that applies a constant pressure to the outlet boundary. The pressure is used as static pressure for outflow while it is used as total pressure for backflow.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/pressureOutlet.png" width="300" height="300"><br>Pressure Outlet setup</center>

Enter a pressure and select two options - 'Non-Reflecting Boundary' and 'Specify Backflow Properties'

When no options are used, the boundary conditions in openfoam used by each field are as follows

* Velocity : _pressureInletOutletVelocity_
* Pressure : _totalPressure_ (This condition use total pressure for inflow condition, and use static pressure for outflow condition.)
* Temperature : _zeroGradient_
* Turbulence : _zeroGradient_
* Species : _zeroGradient_

#### Specify Backflow Properties option

The Specify Backflow Properties option allows you to specify the total temperature and turbulence conditions of the flow entering the computational domain.

With this option, the boundary conditions used by openfoam for the temperature, turbulence field, and chemical species are as follows

* Temperature : _inletOutletTotalTemperature_
* Turbulent kinetic energy(k) : _turbulentIntensityInletOutletTKE_
* Turbulent dissipation rate($\epsilon$), specific dissipation rate($\omega$) : _viscosityRatioInletOutletTDR_
* Modified kinematic viscosity($\tilde{\nu}$) : _inletOutlet_
* Species : _inletOutlet_

#### Non-Reflecting Boundary option

This is a condition where pressure waves are not reflected at the boundary. It can be used when energy equation is On, the density is a perfect gas and the specific heat capacity is constant. 

We use openfoam's _waveTransmissive_ boundary condition for velocity and pressure, and the rest of the fields are the same as without the option. 

[Non-Reflecting Boundary tutorial : subsonic cavity flow](https://baramcfd.org/tutorials/2024/09/19/2d_cavity-post/)

## Open Channel Outlet

Open Channel Outlet is a condition that gives the outlet a constant average velocity when calculating the free surface and allows the height of the water surface to vary accordingly.

[Open Channel Outlet tutorial : ship resistance](https://baramcfd.org/tutorials/2023/09/05/KCS-post/)

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/openChannelOutlet.png" width="300" height="300"><br>Open Channel Outlet setup</center>

Enter the average velocity as a constant.

The openfoam boundary conditions used by each field are as follows

* Velocity : _outletPhaseMeanVelocity_
* Pressure : _zeroGradient_
* Volume fraction : _variableHeightFlowRate_
* Turbulent kinetic energy(k) : turbulentIntensityInletOutletTKE_
* Turbulent dissipation rate($\epsilon$), specific dissipation rate($\omega$) : _viscosityRatioInletOutletTDR_
* Modified kinematic viscosity($\tilde{\nu}$) : _inletOutlet_ 

## Outflow

Outflow uses zero gradient conditions for all fields on the exit.

## Subsonic Outflow

Supersonic Outflow is a supersonic outlet boundary condition for a compressible flow.

There is nothing to enter, and the openfoam boundary condition for all fields is _zeroGradient_.

## Supersonic Outflow

Supersonic Outflow is a supersonic outlet boundary condition for a compressible flow.

There is nothing to enter, and the openfoam boundary condition for all fields is _zeroGradient_.

<!------------------------------------------------------------------------------------------------>
## Wall

Wall is a boundary through which the flow cannot pass, and you can specify temperature conditions. Velocity conditions can be applied to the motion of the wall. 

### Velocity Condition

The velocity conditions can be No Slip, Slip, Moving Wall, Atmospheric Wall, Translational Moving Wall, or Rotational Moving Wall.

No-Slip condition is a wall sticking condition with velocity set to (0, 0, 0).

Slip condition is a frictionless wall condition where the wall has no velocity component normal to the wall.

**Moving Wall is a condition on a moving object that is applied to a moving object when using a dynamic mesh such as a sliding mesh**. 

Atmospheric Wall is used for the ground in atmospheric boundary layer problems.

Translational/Rotational Moving Wall sets constant/rotatingal velocity at the wall, while mesh is not moving. 

### Temperature Condition

The temperature condition can be Adiabatic, Constant Temperature, Constant Heat Flux, or Convection heat transfer to the outside.

#### Adiabatic

Nothing to set as an adiabatic condition.

Use _zeroGradient_ if there is no radiative heat transfer, and use the condition that the heat flux including radiation is zero(_externalWallHeatFluxTemperature_) if there is radiative heat transfer.

#### Constant Temperature

Use _fixedValue_ condition.

#### Constant Heat Flux

Use _externalHextFluxTemperature_ condition.

#### Convection

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/convectionWallLayer.png" width="300" height="300"><br>Convection</center>

This condition uses a constant reference temperature and heat transfer coefficient. The heat flux through the wall is given by equation~\ref{eq:convection}. Use the _externalHextFluxTemperature_ condition. 

<center>$q_{external} = h(T_{wall}-T_a)$</center>

* $h$ : Heat Transfer Coefficient
* $T_a$ : Free Stream Temperature

The thickness and thermal conductivity of a wall can be used to set the thermal resistance of a solid. The thickness and thermal conductivity of multiple layers of a solid can be set for each layer. This is a way to consider the heat conduction in a solid without considering multi-region modeling. However, it has limitations in that it only considers heat conduction perpendicular to the wall and not lateral heat conduction, and it cannot consider temperature changes over time in transient calculations.

In this case, the thermal resistance is calculated by the equation

<center>$h = \frac {1} {\frac{1}{h_{convection}} + \Sigma \frac{l_{layer}}{\kappa_{layer}}}$</center>

### Turbulence Condition

The turbulence condition at the wall uses the wall function as the turbulence model, with the following conditions.

#### standard $k-\epsilon$, realizable $k-\epsilon$, RNG $k-\epsilon$ 모델

* $k$ : _kqRWallFunction_
* $\epsilon$ : _epsilonWallFunction_ for standard, _epsilonBlendedWallFunction_ for two layer
* $\nu_t$ : _nutkWallFunction_ for standard, _nutSpaldingWallFunction_ for two layer
* $\alpha_t$ : _compressible::alphatJayatillekeWallFunction_

#### SST $k-\omega$ model

* $k$ : _kqRWallFunction_
* $\omega$ : _omegaBlendedWallFunction_
* $\nu_t$ : _nutSpaldingWallFunction_
* $\alpha_t$ : _compressible::alphatJayatillekeWallFunction_

#### Spalart-Allmaras model

* $\tilde{\nu}$ : _zeroGradient_
* $\nu_t$ : _nutSpaldingWallFunction_
* $\alpha_t$ : _compressible::alphatJayatillekeWallFunction_

#### Atmospheric Wall

* $k$ : _kqRWallFunction_
* $\epsilon$ : _atmEpsilonWallFunction_
* $\nu_t$ : _atmNutkWallFunction_

#### $\alpha_t$

$\alpha_t$ depends on heat transfer boundary condition.

* Adiabatic : _compressible::alphatWallFunction_
* Constant temperature, Constant heat flux, Convection : _compressible::alphatJayatillekeWallFunction_

## Thermo-Coupled Wall

Thermo-Coupled Wall is a condition for a zero-thickness wall(baffle) inside the computational domain. The baffle is paired with two boundary surfaces, master and slave, both of which use the Thermo-Coupled Wall condition. It is also used for boundaries between regions(fluid-solid, solid-solid) in multi-region problems.

The boundary conditions in openfoam used by each field are as follows

* Velocity : _noSlip_
* Pressure : _fixedFluxPressure_
* Temperature : _turbulentTemperatureCoupledBaffleMixed_
* Turbulence : same with Wall
* Species : _zeroGradient_

## Misc.

### Empty, Wedge

OpenFOAM does not have two-dimensional or axisymmetric mesh. A three-dimensional mesh with one layer in the height direction and Empty boundary conditions on the top and bottom faces becomes a two-dimensional problem. A wedge-oriented three-dimensional mesh  with one layer in the rotation direction and Wedge boundary conditions on both sides becomes an axisymmetric problem.

### Symmetry

Use for boundary surfaces with symmetry conditions. 

### Interface

A condition that describes the relationship between two boundary surfaces. It is used when there are two boundary surfaces at the same location. It is used on boundary surfaces that have no thickness inside the computational domain and through which flow passes, or on cell zone boundaries. The mesh of the two paired boundary surfaces do not need to match.

There are three types of interfaces: Internal Interface, Rotational Periodic, and Translational Periodic.

Use openfoam's _cyclicAMI_ condition.

[Rotational Periodic tutorial](https://baramcfd.org/tutorials/2024/06/28/mixer-post/)

### Cyclic

interface, but the two paired mesh must match completely.

### Porous Jump

Porous Jump is a condition that causes a pressure change in a cyclic plane inside the computational domain. 

[Porous Jump tutorial](https://baramcfd.org/tutorials/2023/09/05/porousJump-post/)

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/porousJump.png" width="300" height="300"><br>Porous Jump setup</center>

The inputs are as follows

* Darcy Coefficient, $D$
* Inertia Coefficient, $I$
* Porous media thickness, $L$
* Coupled boundary

**Positive values of the Darcy Coefficient and Inertial Coefficient result in a pressure drop and negative values result in a pressure increase**.

The pressure change is calculated by the following equation where $\mu$ is the viscosity, $\rho$ is the density, and $U$ is the velocity.

<center>$\Delta p = -\left(D \mu U + \frac{1}{2} I \rho U^2 \right)L$</center> 

* $\mu$ : viscosity
* $\rho$ : density
* $U$ : velocity

The boundary conditions used by openfoam are _porousBafflePressure_ for pressure and _cyclic_ for everything else.

### Fan

Fan is a condition that velocity-pressure curve is used for pressure and velocity calculation. It can be used for the cyclic plane inside the computational domain. 

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/fan.png" width="300" height="300"><br>Fan setup</center>

**The velocity-pressure curve file can be a text file in csv format with the velocity in the first column and the pressure in the second column**.

The boundary conditions used by openfoam are _fan_ for pressure and _cyclic_ for everything else.


