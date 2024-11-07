# Numerical Conditions

Set the numerical conditions used in the simulation.

## Pressure-Velocity Coupling Scheme

You can choose between SIMPLE and SIMPLEC, which applies to _consistent_ in the SIMPLE dictionary of the fvSolution file. When using SIMPLEC, you can speed up convergence by setting the relaxation factor to a value greater than or equal to 0.9.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/scheme00.png" width="300" height="300"><br>Pressure-Velocity Coupling</center>

## Momentum Predictor

It only appears during transients.

For low Reynolds number flows or multiphase flow problems, turning off this can sometimes improve stability.

## Formulation

Appears only in density-based compressible solvers(with Density-based selected as the solver type). Currently, only Implicit can be selected.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/scheme0.png" width="300" height="300"><br>Formulation, Flux Type</center>

## Flux Type

Appears only in density-based compressibility solvers(Solver Type selected as Density-based).

There are three options to choose from: Roe-FDS, AUSM, and AUSM-up.

If you select Roe-FDS, you can enter the Entropy Fix Coefficient, $\epsilon$. Enter a value between 0 and 1.

AUSM-up allows you to set the Cut-off Mach Number, $M_\infty$. Enter a value between 0 and 10.

If you select AUSM, there is no setting.

## Discretization Schemes

Set the discretization schemes for time, pressrue, momentum, energy, turbulence and volume fraction.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/scheme1.png" width="300" height="300"><br>Discretization Schemes</center>


### Time

You can choose between First Order Implicit and Second Order Implicit. These are used in the ddtSchemes dictionary of the fvSchemes file. _Euler_ is used for first order implicit and _backward_ is used for second order implicit.

### Pressure

You can select Linear, Momentum Weighted Reconstruct, or Momentum Weighted to select an extrapolation technique that calculates the pressure at the cell faces from the pressure at the center of the cell.

* Linear : A linear interpolation using the cell centroids on both sides of the face, which is more stable. The linear method is recommended when there are sources of momentum such as multiple reference frames(MRFs) or porous models.
* Momentum Weighted : This method uses the coefficients of the momentum equation ($a_p$) as weighting factors and has good convergence. There may be stability issues with momentum sources.
* Momentum Weighted Reconstruct : This is a second order accuracy method that calculates the values on the left and right sides of the cell face by extrapolation using the pressure gradient at the center of the cell and averages them to use as the value of the cell face. The average uses the coefficient of the momentum equation ($a_p$) as a weighting factor. It can give more accurate results, but can be less stable.

### Momentum, Energy, Turbulence, Volume Fraction

First Order Upwind and Second Order Upwind can be selected. These are used in the divSchemes dictionaries of the fvSchemes file.

Momentum, turbulence, and energy use '_Gauss upwind_' for the first-order upwind method and '_Gauss linearUpwind_' and Venkatakrishnan's limiter(_VKLimited Gauss linear 1.0_) for the second-order upwind method.

## Under-Relaxation Factors

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/relaxation.png" width="300" height="300"><br>Under-Relaxation</center>

Enter only the default value in the steady, or the default value and the final value in the transient. Corresponds to the _relaxationFactors_ in the fvSolution file. Larger values may result in faster convergence but may be unstable. A value of 0.9 or higher is recommended when using SIMPLEC.

When using density-based compressibility solvers, only the turbulence part is enabled.

## Improve Stability

Used for laplacian schemes to improve stability. When the option is off, fvSchemes.laplacianScheme uses '_Gauss linear corrected_', and when the option is on, it uses '_Gauss linear limited corrected [limiting factor]_'. Larger limiting factor values are more stable but can be less accurate.

## Max Iterations per Time Step, Number of Correctors

'Max Iterations per Time Step' is the maximum number of iterations to perform at each time step in the transient simulation, and 'Number of Correctors' is how many times the pressure equation is to be calculated for each iteration. It applies to _nOuterCorrectors_ and _nCorrectors_ in the fvSolution.PIMPLE dictionary.

OpenFOAM uses the PIMPLE algorithm, a combination of the SIMPLE and PISO algorithms, for transient simulation. If the 'Max Iterations per Time Step' is 1 and the 'Number of Correctors' is 2, it is the same as the PISO algorithm. If 'Max Iterations per Time Step' is greater than 1 and 'Number of Correctors' is 1, it is equivalent to the transient SIMPLE algorithm. 

## Multiphase

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/multiphase.png" width="300" height="300"><br>Multiphase</center>

'Max Iterations per Time Step' is the maximum number of iterations of the alpha, volume fraction equation to be performed at each time step in the transient simulation, and 'Number of Correctors' is how many times the alpha, volume fraction equation is to be calculated in each iteration. These are the _nAlphaSubCycles_ and _nAlphaCorr_ values of solvers-alpha(volume fraction) in the fvSolution file.

The MULES technique selects the method of the MUlti-dimensionsal Limiter for Explicit Solution, which is the _MULESCorr_ value of solvers-alpha (volume fraction) in the fvSolution file. The value is no for 'explicit' and yes for 'semi-implicit'.

The 'Phase Interface Compression Factor' is a factor that controls the compression of the phase interface. A value of 0 corresponds to no compression and a value of 1 corresponds to conservative compression. Larger values result in thinner boundaries, but can be unstable. The value of _cAlpha_ in solvers-alpha (volume fraction) in the fvSolution file.

'Number of MULES iterations over the limiter' is the number of MULES iterations over the limiter, which is the value of _nLimiterIter_ in solvers-alpha(volume fraction) of the fvSolution file.

## Convergence Criteria

Convergence Criteria indicates the convergence conditions for each field. It corresponds to _residualControl_ in the SIMPLE or PIMPLE dictionary of the fvSolution file. Absolute values are applied to _tolerance_ and relative, which is activated in transient conditions, is applied to _relTol_.

## Advanced

It consists of two parts: Limits and Equations.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/limit.png" width="300" height="300"><br>Advanced</center>

#### Limits

The value of Limits limits the minimum and maximum values for calculation stability. You can limit the temperature and Viscosity Ratio.

The temperature limit uses _limitTemperature_ from fvOptions.

The Maximum Viscosity Ratio is set in the turbulence model dictionary in the turbulenceProperties file. The default is 1e5, and while smaller values may provide better stability, they may also compromise the accuracy of the results.

#### Equations

Turn on/off equations for flow, energy, user-defined functions, chemical species, etc.

For energy equations, the Viscous dissipation, Kinetic energy, and Pressure Work terms can be toggled on/off. If viscous heating is included, the other two are included. If their influence is very small, omitting them can improve stability. For high velocity, high temperature, and high viscosity flows, leaving them off can cause accuracy issues.

The Brinkman Number($Br$) can be used to determine the importance of viscous dissipation. The Brinkman Number is a dimensionless number that represents the ratio of energy dissipated by viscosity to energy conducted, and if it is very small, less than 1, viscous heating is negligible.

<center>$Br = \frac{U^2 \mu}{k \Delta T}$</center>

Kinetic energy becomes important when a fluid has a high velocity. Pressure work refers to the work done when a fluid is compressed or expanded and is important in problems with pumps, turbines, compressors, etc.

If you are using a Sliding mesh, you can verify that the motion of the grid is properly implemented by calculating with everything off.



