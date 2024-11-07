# General

Set the Time, Gravity, and Operating Conditions.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/general.png" width="400" height="400"><br>General setup</center>

## Time

Select Steady or Transient.

Baram has separate steady and transient solvers for simulation flow, heat transfer, and species mixing. So different solvers are run depending on the time selection. Multiphase flows, on the other hand, use a single solver for transient simulation. If there are only two phases, the steady simulation is performed using LTS(Local Time Step) method, **but if there are three or more phases or cavitation is included, only the transient simulation can be performed**. 

## Gravity

Set the gravity vector component.

Gravity is disabled in density-based solver.

Since it is often necessary to include gravity in multiphase flows, a separate window for setting gravity is shown at program startup to reduce user errors. The settings here can be modified on the main window.

Gravity must be accounted for if heat transfer by natural convection is to be considered in a single-phase flow problem. The Richardson Number($Ri$) is used to determine if natural convection should be considered. The Richardson Number is expressed as a function of the Grashof Number($Gr$) and the Reynolds Number. The Grashof Number is the ratio of buoyancy to viscous force.

<center>$Gr = \frac {\beta \Delta T L^3 g } {\nu^2}$</center>

$\beta$ : thermal expansion coefficient

$L$ : distance

$\nu$ : kinematic viscosity

<center>$Ri = \frac {Gr} {Re^2}$</center>

In general, if the Richardson number is less than 0.1, natural convection can be ignored. If you do not need to account for natural convection, it is recommended to set the gravity to (0 0 0) for stability of the simulation. 

## Operating Conditions

Set Operating Pressure. **When entering pressures in the BaramFlow, use a relative pressure based on this value**. If you use 0 as this value, all pressures will be absolute value.

In OpenFOAM, standard solvers that include energy equation use absolute pressure without the concept of relative pressure. Since absolute pressure is large value of $O(10^5)$, it is not effective for calculating small pressure changes and is inconvenient in post-processing. Therefore, BaramFlow and NextFOAM use relative pressure, which is based on the operating pressure.

The pressure-based solvers in BaramFlow use two pressure variables, p\_rgh and p. p\_rgh is the pressure minus the hydrostatic pressure, relative to the operating pressure, and is used to solve the pressure equation. p is not used in the calculation, but is obtained from the calculated p\_rgh as a value that includes hydrostatic pressure and is represented as absolute pressure.  Density-based solvers only use p because they do not account for gravity.

