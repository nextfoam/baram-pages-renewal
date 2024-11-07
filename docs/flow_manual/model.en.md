# Models

Set the turbulence model, whether energy is accounted for, multiphase flow, chemical species mixtures, custom scalars, etc.

The Solver Type and Multiphase are set at program startup and cannot be changed.

When the mesh is read, if it is a multiregion mesh, the energy is set to include and cannot be changed. The same is true when the solver type is density-based.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/models.png" width="400" height="400"><br>Models setup</center>

## Turbulence

Double-click Turbulence to bring up the settings window of figure below.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/turbulence.png" width="400" height="400"><br>Turbulence setup</center>

Once you've selected your model, you'll see additional settings that are required accordingly.

#### Inviscid

There are no settings for models that do not account for viscosity, often referred to as Euler in compressible flows. When this option is selected, laminar flow is selected internally and the viscosity coefficient is set to zero. It is not necessary to set the viscosity coefficient to zero directly in the property values.

#### Laminar

There are no settings for this.

The Reynolds number($Re$), the ratio of inertial and viscous forces, is used to determine whether a flow is laminar or turbulent. The criteria for laminar or turbulent flow using Reynolds number depends on the type of flow. In general, for pipe flows, Re less than 2000 is laminar and greater than 4000 is turbulent. For open channel flows, 500 and for flat plate flows, $10^5$ $\sim$ $10^6$ is the threshold.

<center>$Re = \frac {\rho U L} {\mu^2}$</center>

* $L$ : characteristic length
* $\mu$ : viscosity

In natural convection problems, we use the Rayleigh number($Ra$), which is the ratio of heat transfer by diffusion and convection. It is generally considered laminar below $10^7$ and turbulent above $10^9$.

<center>$Ra = \frac {\rho \beta \Delta T L^3 g} {\mu \alpha}$</center>

* $\beta$ : thermal expansion coefficient
* $L$ : distance

$\alpha$ : thermal diffusivity

#### Spalart-Allmaras

It is a one-equation turbulence model that computes the transport equation of modified kinematic viscosity ($\tilde{\nu}$) and is often used in aerospace to compute turbulent flows in boundary layers. The calculated $\tilde{\nu}$ value and a correction function for viscous effects near the wall are used to calculate the turbulent viscosity. The turbulent Prandtl number ($Pr_t$) can be set.

#### k-epsilon

It is a two-equation turbulence model that calculates the transport equations for the turbulent kinetic energy ($k$) and the turbulent dissipation rate ($\epsilon$). There are three models to choose from: Standard, RNG, and Realizable, and you can set the turbulent Prandtl number. If you select Realizable model, you can select standard wall function and two layer model as wall function options.

#### k-omega

It is a two-equation turbulence model that computes transport equations for turbulent kinetic energy ($k$) and specific dissipation rate ($\omega$). The k-omega model currently only supports Menter's Shear Stress Transport (SST) model. Turbulent Prandtl number can be set.

#### LES, Large Eddy Simulation

A model that converts energy into turbulent viscosity by directly computing large vortices and using a Subgrid Scale (SGS) model for small vortices. They are only active when set to transient. You can select the Length-Scale model, which is a filtering method that distinguishes the size of vortices from the Subgrid Scale model.

#### DES, Detached Eddy Simulation

A hybrid turbulence model that combines LES and RANS, using LES for the free flow domain and RANS for boundary layer flow. It is only active when set to transient. You can select the RANS model of the wall, the DES option, and the Length-Scale Model. For the RANS model, you can select Spalart-Allmaras and k-omega SST. For the Spalart-Allmaras option, you can select Low Reynolds Damping. For the DES option, you can select Delayed DES, and when the option is enabled, you can select DDES and IDDES.


#### Two layer Model

This model was developed by NEXTFoam and uses the blending function. The standard wall function may have accuracy problems when y+ is in the buffer layer, but this model has the advantage of being used regardless of y+. **Currently, we only support realizable k-epsilon models**. The blending function uses the following expressions.

<center>$\lambda = {\frac 1 2} \left[1 + tanh \left( \frac{Re_y - {Re_y}^* } {A} \right) \right ]$</center>

<center>$A = \frac {\Delta Re_y } {tanh(0.98)}$</center>

*Ref) Shih, T. H., Liou, W. W., Shabbir, A., Yang, Z., & Zhu, J. (1995). A New k-epsilon eddy Viscosity Model for High Reynolds Number Turbulent Flows. Computers and Fluids, 24(3), 227-238.*


#### $Pr_t$, Turbulent Prandtl Number

Turbulent Prantle number is a dimensionless number that represents the ratio between momentum diffusion and heat diffusion in a turbulent flow. Two settings can be set: Use for Internal Field and Use for Wall Function. The value used for Internal Field is applied to the turbulence model, and the value used for Wall Function is used for the wall function of the turbulent thermal diffusivity($\alpha_t$). Not used in DES/LES models.

<center>$Pr_t = \frac {\nu_t } {\alpha_t}$</center>

* $\nu_t$ : turbulent viscosity
* $\alpha_t$ : turbulent thermal diffusivity

#### $Sc_t$, Turbulent Schmidt Number

Turbulent Schmidt number is a dimensionless number that describes the ratio between mass diffusion and momentum diffusion in a turbulent flow. It is used for turbulent diffusion of chemical species and is not used when not calculating chemical species.

<center>$Sc_t = \frac {\nu_t } {D_t}$</center>

* $\nu_t$ : turbulent viscosity
* $D_t$ : turbulent diffusivity)

The diffusion flux of a chemical species is calculated by the following equation, where the smaller the turbulent Schmidt number, the better the mixing. 

<center>$\vec{J_i} = - \left(  \rho D_i + \frac{\mu_t}{Sc_t}  \right) \nabla Y_i = - \rho (D_i + K) \nabla Y_i$</center>

<!--------------------------------------------------------------------------------------------------------->
## Energy

Double-click Energy to bring up the settings window of figure~\ref{fig:energy}. Select whether to include or not.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/energy.png" width="200" height="200"><br>Energy setup</center>

OpenFOAM's standard solvers provide _simpleFoam_, which does not compute the energy equation, and _buoyantSimpleFoam_, which does. Instead of pressure, _simpleFoam_ uses the pressure divided by density, while _buoyantSimpleFoam_ uses the p\_rgh, which is pressure minus hydrostatic pressure ($\rho g h$). However, Baram uses one solver (_buoyantSimpleNFoam_) developed by NEXTFoam, which uses p\_rgh for pressure and allows you to control whether each governing equation is calculated or not.

## Species

Double-click the species to bring up the settings window of figure below. Select whether to include or not. In both cases, we use the same solver(_buoyantSimpleNFoam_) and control whether the equations are computed based on your choice.

[Species tutorial : duct flow](https://baramcfd.org/tutorials/2024/07/24/species-post/)

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/species.png" width="200" height="200"><br>Species setup</center>

## User-defined Scalars

A user-defined scalar is a variable that can be arbitrarily defined by the user and is also called a passive scalar because its distribution is determined by the flow field, but it does not affect the flow.

[User-defined Scalars tutorial : Fire in Room](https://baramcfd.org/tutorials/2024/06/16/fireInRoom-post/)

Double-click this to bring up the settings window of figure below. Click the (+) to the right of 'User-defined Scalar' to add a new scalar.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/uds0.png" width="300" height="300"><br>User-defined Scalar setup</center>

Each scalar can have a Field Name and a Diffusivity. There are two ways to set the diffusivity: Constant, Laminar and Turbulent Viscosity. 

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/uds1.png" width="300" height="300"><br>Diffusivity of user-defined scalar</center>

The Constant method inputs a constant, and the Laminar and Turbulent Viscosity method inputs two coefficients, Laminar and Turbulent, and uses the diffusion coefficient as follows.

<center>$D = D_{laminar} \cdot \nu + D_{turbulent} \cdot \nu_t$</center>

* $\nu$ : kinematic viscosity
* $\nu_t$ : turbulent kinematic viscosity

User-defined scalars are calculated using OpenFOAM's function object functionality. By defining scalars here, you can enter values in the boundary condition setting part just like any other flow variable and use them for monitoring, data extraction, etc.


