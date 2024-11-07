# Materials

BARAM provides a database of properties for several materials. You can add materials to be used in the simulation and modify their properties if necessary. These materials can be combined to form a mixture, which is used to calculate chemical species mixtures.

BARAM provides the following material databases

* Gas : Air, Oxygen, Nitrogen, Carbon-dioxide, Hydrogen, Argon, Carbon-monoxide, Methane, Water-vapor

* Liquid : Water-liquid

* Solid : Steel, Concrete, Aluminum, Copper

Pressing the (+) button in the top right corner of figure below, will bring up the window shown in the middle, and allow you to add a material. The image on the right shows the result of adding oxygen.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/material.png"><br>Material setup</center>


To add a mixture, select multiple substances at once and use the 'Create Mixture' button, as shown in the figure below. This button only appears when species calculation is enabled. 

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/material-maxture.png"><br>Mixture material setup</center>

When you press the Edit Property Value button, the window shown in figure below appears.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/materialProperties.png" width="300" height="300"><br>Material properties setup</center>

The following items can be modified: material name, density, specific heat at constant pressure, viscosity, thermal conductivity, molecular weight, absorption coefficient, saturation vapor pressure, emissivity, etc. The items will vary slightly depending on the gas/liquid/solid and whether you are calculating the energy equation.

<!--------------------------------------------------------------------------------------------->
### Density

You can choose Constant, Perfect Gas, Polynomial, or Incompressible perfect gas. Only Constant can be used when not solving energy equations and for liquids or solids. 

Perfect gas is calculated as a function of temperature and pressure using the following equation.

<center>$\rho = \frac {p}{RT}$</center>

$R$ : gas constant

The polynomial is defined as a function of temperature, and the coefficients $a_0$, $a_1$, $a_2$, etc. of the equation. The coefficients are set in the window in figure below. 

<center>$S = a_0 \cdot T^0 + a_1 \cdot T^1 + a_2 \cdot T^2 + ... + a_n \cdot T^n$</center>

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/density-polynomial.png" width="300" height="300"><br>Polynomial setup</center>

Incompressible perfect gases determine their density as a function of temperature only, as shown in the equation~\ref{eq:rho}. **Currently only available when calculating species mixtures, but in the next version it will be available for all cases of solving energy equations**.

<center>$\rho = \frac {p_{ref}} {RT}$</center>

$p_{ref}$ : The pressure set in Reference Values is used.

### Specific Heat Capacity, $C_p$

You can choose between Constant and Polymomial.

### Viscosity

You can choose Constant, Sutherland, or Polynomial, and for liquids, you can choose a non-Newtonian fluid model such as Cross, Hershel-Bulkley, Bird-Carreau, or Non-Newtonian-power-law.

The Sutherland relation expresses the viscosity of an ideal gas as a function of temperature and can only be used when calculating the energy equation. It is expressed as Sutherland Coefficient and Sutherland Temperature.

<center>$\mu = \mu_0 \left ( \frac {T} {T_0} \right )^{2/3} \frac{T_{ref} + T_s}{T + T_s} = \frac{A_s T^{2/3}}{T+T_s}$</center>

For air, $\mu_0$=1.716e-5, $T_s$=110.4K, $A_s$=1.458e-6 when $T_0$=273.15K.

With Sutherland, thermal conductivity is disabled because it is calculated with the Chapman-Enskog approach.

<center>$\kappa = \mu C_v \left(1.32+1.77 \frac {R}{C_v} \right)$</center>

### Non-Newtonian viscosity

Cross, Hershel-Bulkley, Bird-Carreau, Non-Newtonian-power-law, etc. are non-Newtonian fluid models, which **can only be used when the material is a liquid and the turbulence model is laminar**. Each model uses the following equations

[Newtonian viscosity tutorial : blood flow of FDA Nozzle](https://baramcfd.org/tutorials/2024/08/29/blood-post)

#### Cross

Use a cross power law model.

<center>$\nu = \nu_\infty + \frac {\nu_0 - \nu_\infty}{1+(m \gamma)^n}$</center>

* $\nu_0$ : zero shear viscosity
* $\nu_\infty$ : infinite shear viscosity
* $m$ : natural time
* $n$ : power law index
* $\gamma$ : shear strain rate

#### Hershel-Bulkley

<center>$\nu = min (\nu_0 , \tau_0 / \gamma + k \gamma^{n-1})$</center>

* $\nu_0$ : zero shear viscosity
* $\tau_0$ : yield stress threshold
* $k$ : consistency index
* $n$ : power law index
* $\gamma$ : shear strain rate


#### Bird-Carreau

<center>$\nu = \nu_\infty + (\nu_0 - \nu_\infty )[1+(k \gamma)^a]^{\frac {n-1}{a}}$</center>

* $\nu_0$ : zero shear viscosity
* $\nu_\infty$ : infinite shear viscosity
* $k$ : relaxation time
* $n$ : power law index
* $a$ : linearity deviation
* $\gamma$ : shear strain rate


#### Non-Newtonian-power-law

<center>$\nu = k \gamma ^{n-1}$</center>

* $k$ : consistency index
* $n$ : power law index
* $\gamma$ : shear strain rate

This model can constrain values using the maximum and minimum values $\nu_0$, $\nu_\infty$.

### Thermal Conductivity

Constant and Polymomial can be used, but **it depends on how the viscosity is set**. If the viscosity is a constant or a non-Newtonian fluid model, the thermal conductivity is automatically set to constant. If the viscosity coefficient is polynomial, the thermal conductivity is also polynomial. When the viscosity coefficient is Sutherland, the input part is disabled because the thermal conductivity is calculated by the Chapman-Enskog approach.

### Molecular Weight

Only appears for liquids and gases.

### Absorption Coefficient

Only appears for gases. Only used when calculating radiative heat transfer (currently unnecessary)

### Saturation Pressure

Only appears in liquids. Used for phase change calculations (currently unnecessary)

### Emissivity

Appears only in solids. Only used for radiative heat transfer calculations (currently unnecessary)

## Setting Properties for a Mixture

When you click the Modify Property Values for Mixture button, the window of figure~\ref{fig:mixture-properties} appears.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mixture-properties.png" width="300" height="300"><br>Setting Properties for a Mixture</center>

The following fields can be modified: Name, Density Spec., Specific Heat Spec., Transport Spec., Mass Diffusivity, and Primary Specie.

### Density Spec.

A method for calculating the density of each material that makes up a mixture. You can choose constant, perfect gas, incompressible perfect gas, or polynomial. You can only use a constant when you are not solving an energy equation and when the material is liquid or solid. The density of a mixture is determined by the mass fraction of each substance.

### Specific Heat Spec.

You can select constant and polynomial. Set the value of each material that makes up the mixture. The value of the mixture is determined by the mass fraction of each material.

### Transport Spec.

You can select Constant, Polynomial, or Sutherland as the method for determining the viscosity and thermal conductivity.

If Constant or Polynomial is selected, it sets the viscosity and thermal conductivity for each material in the mixture. If Sutherland, set the Sutherland Coefficient (As) and Sutherland Temperature (Ts) for each material. The value of the mixture is determined by the mass fraction of each material.

### Mass Diffusivity

Currently, it can only be entered as a constant.

### Primary Specie

The transfer equation for this species is determined as 1 minus the sum of the mass fractions of the remaining species without calculation.







