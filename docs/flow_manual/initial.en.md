# Initialization

Initialization serves to set the initial values used in the calculation and to clear and initialize all data saved by the calculation. It sets initial values for the velocity components in the x, y, and z directions, as well as pressure, temperature, and volume fraction.

For turbulence, $\tilde{\nu}$ is used when using the Spalart-Allmaras model, and $k-\epsilon$ series and SST $k-\omega$  turbulence models, velocity scale, turbulence intensity, and turbulence viscosity ratio are used. These three values are used to calculate the values of $\nu_t$, $k$, $\epsilon$, and $\omega$, which are used as initial conditions.

<center>$\nu_t = viscosityRatio * \nu$</center>

<center>$k = 1.5 \cdot (velocityScale \cdot turbulentIntensity)^2$</center>

<center>$\epsilon = 0.09 \cdot k^2 / \nu_t$</center>

<center>$\omega = k / \nu_t$</center>

<center>$\tilde{\nu} = viscosityRatio * \nu$</center>

## Advanced

In the Advanced settings, you can set field values for a certain volume. OpenFOAM's _setFields_ utility is used for this function.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/setfield.png"><br>Advanced</center>

Volumes can be hexahedra, cylinders, spheres, and cellzones. A hexahedron is defined by the coordinates of two points. A cylinder is defined by the coordinates and radius of two points, and a sphere is defined by the center point and radius. The flow fields to initialize are velocity, pressure, temperature, and volume fraction. If Override Boundary Value is checked, the value of the boundary included in the region is also set to the initial value.

[Advanced Initialization tutorial : Weir flow](https://baramcfd.org/tutorials/2023/09/05/weir-post/)
