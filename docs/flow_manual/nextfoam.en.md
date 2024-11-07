# How settings from BaramFlow are used in NextFOAM

BaramFlow uses NextFOAM, a fork of OpenFOAM developed by NEXTfoam Co.,Ltd. This section describes how various settings in BaramFlow are applied to NextFOAM. Terms used in NextFOAM are italicized.

## General

### Time

Time is set in the _ddtSchemes_ dictionary in the **system/fvSchemes** file.

If Time is steady, the pressure-based solver uses _steadyState_ for single-phase flows and _localEuler_ for two-phase flows. Density-based solver uses _localEuler_.

```
ddtSchemes
{
    default     <steadyState or localEuler>;
}
```
If Time is transient, use _Euler_ for First Order Implicit and _backward_ for Second Order Implicit.

```
ddtSchemes
{
    default     <Euler or backward>;
}
```
<!---------------------------------------------------------------------------------->
### Gravity

Set the input vector of gravity in the **constant/g** file as follows

```
dimensions  [0 1 -2 0 0 0 0];
value       (<x-value> <y-value> <z-value>);
```
<!---------------------------------------------------------------------------------->
### Operating Conditions

Set the input value of Operating Conditions in **constant/operatingConditions** file as follows

```
operatingPressure operatingPressure [1 -1 -2 0 0 0 0]   <value>;
```
<!---------------------------------------------------------------------------------->
## Model

### Turbulence

Turbulence is set in the **constant/turbulenceProperties** file as follows

```
simulationType  RAS;
RAS
{
    RASModel            kOmegaSST;
    turbulence          on;
    printCoeffs         on;
    viscosityRatioMax   1e5;
}
```
Set _simulationType_ and corresponding dictionary. The _simulationType_ can be _laminar_, _RAS_, _LES_, etc.

_turbulence_ and _printCoeffs_ are always _on_. 

_viscosityRatioMax_ is the input value of Maximum Viscosity Ratio at [Numerical Conditions] - [Advanced].

#### Laminar

```
simulationType  laminar;
```

For Newtonian fluids, there are no additional settings. For non-Newtonian flow, set the model in the _laminar_ dictionary. Non-Newtonian models can be Cross, Herschel-Bulkley, Bird-Carreau, or Non-Newtonian power law. The coefficients for each model are set as follows

Cross
```
laminar
{
    model           generalizedNewtonian;
    viscosityModel  CrossPowerLaw;
    CrossPowerLawCoeffs
    {
        nu0     <value>;
        nuInf   <value>;
        m       <value>;
        n       <value>;
        tauStar <value>;
    }
}
```
Herschel-Bulkley
```
laminar
{
    model           generalizedNewtonian;
    viscosityModel  HerschelBulkley;
    HerschelBulkleyCoeffs
    {
        nu0     <value>;
        tau0    <value>;
        k       <value>;
        n       <value>;
    }
}
```
Bird-Carreau
```
laminar
{
    model           generalizedNewtonian;
    viscosityModel  BirdCarreau;
    BirdCarreauCoeffs
    {
        nu0     <value>;
        nuInf   <value>;
        k       <value>;
        n       <value>;
        a       <value>;
    }
}
```
Non-Newtonian power law
```
laminar
{
    model           generalizedNewtonian;
    viscosityModel  powerLaw;
    powerLawCoeffs
    {
        nuMax   <value>;
        nuMin   <value>;
        k       <value>;
        n       <value>;
    }
}
```

#### Spalart-Allmaras

Always as follows

```
simulationType  RAS;
RAS
{
    RASModel            SpalartAllmaras;
    turbulence          on;
    printCoeffs         on;
    viscosityRatioMax   <value>;
}
```

#### k-epsilon

```
simulationType  RAS;
RAS
{
    RASModel            kEpsilon; // RNGkEpsilon, realizableKE
    turbulence          on;
    printCoeffs         on;
    viscosityRatioMax   <value>;
    kEpsilonCoeffs
    {
        Prt     <value>;
        Sct     <value>;
    }
    ReyStar     60;
    deltaRey    10;
}
```

_RASModel_ is _kEpsilon_ for Standard일, _RNGkEpsilon_ for RNG, _realizableKE_ for Realizable.

_Prt_ is the input of 'for internal Field' of 'Turbulent Prandtl Number'.

_Sct_ is the input of 'Turbulent Schmidt Number'.

_ReyStar_ and _deltaRey_ is used for Enhanced Wall Treatment(two layer) and is fixed as 60 and 10.

#### k-omega SST

Always as follows

```
simulationType  RAS;
RAS
{
    RASModel            kOmegaSST;
    turbulence          on;
    printCoeffs         on;
    viscosityRatioMax   <value>;
}
```

#### LES

_simulationType_ of LES is _LES_. 'Subgrid-Scale model', 'Length-Scale model' and corresponding dictionaries are needed.

Subgrid-Scale model is set at _LESModel_ and Length-Scale model is at _delta_.

```
simulationType          LES;
LES
{
    LESModel            <Smagorinsky or WALE or dynamicKEqn or kEqn>;
    turbulence          on;
    printCoeffs         off;
    delta               <cubeRootVol or dynamicKEqn or kEqn>; 
    viscosityRatioMax   <value>;
    SmagorinskyCoeffs
    {
        Ck              <value>;
        Ce              <value>;
        Sct             <value>;
    }
    cubeRootVolCoeffs
    {
        deltaCoeff      1;
    }
}
```

##### Subgrid-Scale model

You can choose between Smagorinsky-Lilly(_Smagorinsky_), WALE(_WALE_), Kinetic-Energy Transport(_dynamicKEqn_), One Equation Eddy Viscosity(_kEqn_). For Smagorinsky-Lilly, the dictionary looks like the above, and for the rest, it looks like this

WALE
```
    WALECoeffs
    {
        Ck      <value>;
        Ce      <value>;
        Cw      <value>;
        Sct     <value>;
    }
```
Kinetic-Energy Transport
```
    dynamicKEqnCoeffs
    {
        filter      simple;
        Sct         <value>;
    }
```
One Equation Eddy Viscosity
```
    kEqnCoeffs
    {
        Ck      <value>;
        Ce      <value>;
        Sct     <value>;
    }
```

_Sct_ is turbulent Schmidt number and only use for species transport. If species is not included, always use 0.7.

##### Length-Scale model

You can choose between Cube-root Volume(_cubeRootVol_), Van Driest(_dynamicKEqn_), smooth(_smooth_). For Cube-root Volume, the dictionary looks like the above, and for the rest, it looks like this.

Van Driest
```
    vanDriestCoeffs
    {
        delta           cubeRootVol;
        kappa           0.41;
        Aplus           26;
        Cdelta          0.158;
        calcInterval    1;
        cubeRootVolCoeffs
        {
            deltaCoeff  2.0;
        }
    }
```
smooth
```
    smoothCoeffs
    {
        delta           cubeRootVol;
        maxDeltaRatio   1.1;
        cubeRootVolCoeffs
        {
            deltaCoeff  1;
        }
    }
```

#### DES

_simulationType_ of DES model is _LES_. 

RAS Model, DES Option, Length-Scale Model and corresponding dictionaries are needed.

```
simulationType          LES;
LES
{
    LESModel            SpalartAllmarasDES; //kOmegaSSTDES; 
    turbulence          on;
    delta               <cubeRootVol or dynamicKEqn or smooth>;
    viscosityRatioMax   <value>;
    SpalartAllmarasDESCoeffs
    {
        CDES            <value>;
        lowReCorrection <yes or no>;
        Sct             <value>;
    }
    kOmegaSSTDES
    {
        CDESkom         <value>;
        CDESkeps        <value>;
        Sct             <value>;
    }
    cubeRootVolCoeffs
    {
        deltaCoeff      1;
    }
}
```
RAS Model is set at _LESModel_ and Length-Scale Model is at _delta_.

##### RAS Model

You can choose between Spalart-Allmaras(_SpalartAllmarasDES_), k-omega SST(_kOmegaSSTDES_). 

For Spalart-Allmaras, there is Low-Re Damping option. When useing this option, _lowReCorrection_ of _SpalartAllmarasDESCoeffs_ is _yes_ and otherwise _no_.

##### DES Option

For Delayed DES, there is Shielding Function options of DDES and IDDES. For IDDES, Lengh-Scale Model disappears.

For DDES, _LESModel_ is SpalartAllmarasDDES or kOmegaSSTDDES according to RAS Model.
```
simulationType          LES;
LES
{
    LESModel            SpalartAllmarasDDES; //kOmegaSSTDDES;
    turbulence          on;
    delta               <cubeRootVol or dynamicKEqn or smooth>;
    viscosityRatioMax   <value>;
    SpalartAllmarasDDESCoeffs
    {
        CDES            <value>;
        lowReCorrection <yes or no>;
        Sct             <value>;
    }
    kOmegaSSTDDES
    {
        CDESkom         <value>;
        CDESkeps        <value>;
        Sct             <value>;
    }
    cubeRootVolCoeffs
    {
        deltaCoeff      1;
    }
}
```

For IDDES, _LESModel_ is _SpalartAllmarasIDDES_ or _kOmegaSSTIDDES_ according to RAS Model.
```
simulationType          LES;
LES
{
    LESModel            SpalartAllmarasIDDES; //kOmegaSSTIDDES;
    turbulence          on;
    delta               IDDESDelta;
    viscosityRatioMax   1e5;
    SpalartAllmarasIDDESCoeffs
    {
        CDES            <value>;
        lowReCorrection <yes or no>;
        Sct             <value>;
    }
    kOmegaSSTIDDES
    {
        CDESkom         <value>2;
        CDESkeps        <value>;
        Sct             <value>;
    }
    IDDESDeltaCoeffs
    {
        hmax maxDeltaxyzCubeRoot;
        maxDeltaxyzCubeRootCoeffs
        {
        }
    }
}
```
##### Length-Scale Model

Lengh-Scale Model is same as LES.
<!---------------------------------------------------------------------------------->
### Energy

Energy optopn is set in the **system/fvSolution** file. _solveEnergy_ of _SIMPLE_ and _PIMPLE_ dictionary is _yes_ or _no_.

```
SiMPLE //PIMPLE
{
    ...
    solveEnergy     <yes or no>;
    ...
}
```
<!---------------------------------------------------------------------------------->
### Species

Species is set in the **constant/thermophysicalProperties** file.

#### thermoType dictionary

_mixture_ is set as _multiComponentMixture_.

```
thermoType
{
    type            heRhoThermo;
    mixture         multiComponentMixture;
    transport       const;
    thermo          hConst; 
    equationOfState rhoConst; 
    specie          specie;
    energy          sensibleEnthalpy;
}
```

#### species dictionary 

Write the chemical species to be used in the calculation. You must also specify an _inertSpecie_. It does not calculate the transport equations and uses a mass fraction of 1 minus the sum of the remaining chemical species values. 

```
species
(
    <species1>
    <species2>
    ...
);

inertSpecie     <specie1>
```

#### dictionaries of each species

There must be material properties dictionary for each species. The dictionary is same with _mixture_ dictionary except thers is _Dm_ at _transport_.

```
<specie1>
{
    thermodynamics
    {
        ...
    }
    transport
    {
        mu  <value>;
        Pr  <value>;
        Dm  <value>;
    }
    specie
    {
        ...
    }
    equationOfState
    {
        ...
    }
}

<specie2>
...
```
<!---------------------------------------------------------------------------------->
### User-defined Scalars

User-defined Scalars is set in the functions dictionary of **system/controlDict** file.

If diffussivity is Constant, input value is set at _D_. If diffussivity is 'Laminar and Turbulent Diffusivity' , input values are set at _alphaD_ and _alphaDt_.

```
functions
{
    ...

    <name>
    {
        type            scalarTransport;
        libs            ("/baram/solvers/openfoam/lib/libsolverFunctionObjects.so");
        field           <uds name>;
        schemesField    scalar;
        nCorr           2;
        writeControl    runTime;
        writeInterval   <value>;
        D               <value>; // Constant
        //alphaD          <value>; // Laminar and Turbulent Viscosity
        //alphaDt         <value>; // Laminar and Turbulent Viscosity
    }
}
```

_schemesField_ is fixed as _scalar_. So there must be settings for _scalar_ at **system/fvSolution** and **system/fvSchemes** file.

_writeInterval_ is the input value of 'Save Interval' at 'Run Conditions'.
<!---------------------------------------------------------------------------------->
## Materials

Material properties are set in the **constant/thermophysicalProperties** file for single-phase and in **constant/transportProperties** for multi-phase. For non-Newtonial flow properties are set in the **constant/turbulenceProperties** file.

### thermophysicalProperties

#### thermoType dictionary

There are _mixture_, _transport_, _thermo_, _equationOfState_, _specie_ and _energy_ in _thermoType_.

For fluid
```
thermoType
{
    type            heRhoThermo;
    mixture         pureMixture; 
    transport       const;
    thermo          hConst; 
    equationOfState rhoConst; 
    specie          specie;
    energy          sensibleEnthalpy;
}
```
for solid
```
thermoType
{
    type            heSolidThermo;
    mixture         pureMixture; 
    transport       constIso; 
    thermo          hConst; 
    equationOfState rhoConst;
    specie          specie;
    energy          sensibleEnthalpy;
}
```

* type : _heRhoThermo_ for fluid, _heSolidThermo_ for solid

* mixture : If species is not included, _pureMixture_, otherwise _multiComponentMixture_. For solid, _pureMixture_.

* transport : If viscosity and thermal conductivity is constant,  _const_, if polynimial, _polynomial_. For solid, if thermal conductivity is constant _constIso_. 

* thermo : _hConst_ for specific heat is constant, _hPolynomial_ for Polynomial.

* equationOfState : _rhoConst_ for density is constant, _perfectGas_ for Perfect gas, _incompressiblePerfectGas_ for Incompressible perfect gas. _icoPolynomial_ for Polynomial

* specie : always _specie_

* energy : always _sensibleEnthalpy_

#### mixture dictionary

If species is not included, material properties are set in _mixture_ dictionary. There are _thermodynamics_, _transport_, _specie_, _equationOfState_ in _mixture_. Example is as follows

```
...
mixture
{
    thermodynamics
    {
        Cp      1006.0;
        Hf      0;
    }
    transport
    {
        mu      1.79e-05;
        Pr      0.7349959183673469;
    }
    specie
    {
        nMoles      1;
        molWeight   28.966;
    }
    equationOfState
    {
        rho     1.225;
    }
}
...
```

##### thermodynamics

Set _Cp_ and _Hf_. _Cp_ is specific heat capacity and _Hf_ is heat of formation. BaramFlow has no capability of chemical reaction yet, so _Hf_ is not used and set as 0.

If _Cp_ is constant, dictionary is as follows

```
thermodynamics
{
    Cp  <value>;
    Hf  0;
}
```

If _Cp_ is Polynomial, dictionary is like this

```
thermodynamics
{
    Hf          0;
    Sf          0;
    CpCoeffs<8> (<a0> <a1> <a2> ... <a7>)
}
```
_Sf_ is standard entropy and it also not use and set as 0.


As polynomial equation is expressed as 7th equation as follows, 8 coefficients are required. The coefficients not given are set as 0.

$Cp = a_0 + a_1 T + a_2 T^2 + a_3 T^3 + a_4 T^4 + a_5 T^5 + a_6 T^6 + a_7 T^7$

##### transport

Set viscosity, _mu_ and Prandtl Number, _Pr_. 

If viscosity and  thermal conductivity is constannt,

```
transport
{
    mu  <value>;
    Pr  <value>;
}
```

If viscosity and  thermal conductivity is polynomial,

```
transport
{
    muCoeffs<8>     (<a0> <a1> <a2> ... <a7>)
    kappaCoeffs<8>  (<a0> <a1> <a2> ... <a7>)
}
```

If viscosity is Sutherland,
```
transport
{
    As  <value>;
    Ts  <value>;
}
```
_As_ is Sutherland coefficient, _Ts_ is Sutherland temperature

##### specie

Set molecular weight at _specie_
```
specie
{
    nMoles      1;
    molWeight   <value>;
}
```

##### equationOfState

If density is constant,
```
equationOfState
{
    rho <value>;
}
```

If density is polynomial

```
equationOfState
{
    rhoCoeffs<8>     (<a0> <a1> <a2> ... <a7>)
}
```

If density is perfect gas, equationOfState dictionary is not needed.

if density is incomprssible perect gas, 

```
equationOfState
{
    pRef    <value>;
}
```
Pressure at [Reference] is used at pRef.
<!---------------------------------------------------------------------------------->
### transportProperties

Define material properties for multi-phase flow. There must be _phases_ dictionary and additional dictionaries for each phase. 

Surface tension and Cavitation is set here.

#### phases

For 2 pahse

```
phases  (<phase1> <phase2>);
```

For more than 2 pahse
```
phases
(
    <phase1> 
    {
        transportModel  Newtonian;
        nu              <value>;
        rho             <value>;
    } 
    <phase2>
    {
        transportModel  Newtonian;
        nu              <value>;
        rho             <value>;
    }
    ... 
);

```
#### Properties for each phase 

For 2 phase
```
<phase1>
{
    transportModel  Newtonian;
    nu              <value>;
    rho             <value>;
}
<phase2>
{
    transportModel  Newtonian;
    nu              <value>;
    rho             <value>;
}
```

For more than 2 phase, noting is needed because properties are set in _phases_. 


#### Surface tension

For 2 phase

```
sigma   <value>;
```

For more than 2 phase, values for each other are needed as follows
```
sigmas
(
    (<phase1> <phase2>) <value>
    (<phase1> <phase3>) <value>
    (<phase2> <phase3>) <value>
    ...
);
```

#### Cavitation

Set vapor pressure at _pSat_ and cavitation model at _phaseChangeTwoPhaseMixture_ 

```
pSat                        <value>;
phaseChangeTwoPhaseMixture  SchnerrSauer; //Kunz, Merkle, Zwart
```

Coefficients for each model must be set as dictionary as follows

Schnerr-Sauer model
```
SchnerrSauerCoeffs
{
    n       <value>;
    dNuc    <value>;
    Cc      <value>;
    Cv      <value>;
}
```
Kunz, Merkle model
```
KunzCoeffs // MerkleCoeffs
{
    UInf    <value>;
    tInf    <value>;
    Cc      <value>;
    Cv      <value>;
}
```
Zwart-Gerber-Belamri model
```
ZwartCoeffs
{
    aNuc    <value>;
    dNuc    <value>;
    Cc      <value>;
    Cv      <value>;
}
```
<!---------------------------------------------------------------------------------->
## Cell Zone Conditions

### Multiple Reference Frame, MRF

MRF is set in the **constant/MRFProperties** file as follows

```
<name>
{
    cellZone    <cell zone name>;
    active      yes;
    nonRotatingPatches
    (
        <patch name 1>
        <patch name 2>
        ...
    );
    origin      (<x> <y> <z>);
    axis        (<x-axis> <y-axis> <z-axis>);
    omega       <radian per sec>;
}
```

_omega_ is calculate from the input of RPM to rps(radian per sec)

### Sliding Mesh

Sliding Mesh is set in the **constant/dynamicMeshDict** file as follows

```
dynamicFvMesh       dynamicMotionSolverListFvMesh;
motionSolverLibs    ("libfvMotionSolvers.so");
motionSolver        fvMotionSolvers;
solvers
{
    <name>
    {
        solver solidBody;
        solidBodyMotionFunction rotatingMotion;
        cellZone rotating;
        rotatingMotionCoeffs
        {
            origin      (<x> <y> <z>);
            axis        (<x-axis> <y-axis> <z-axis>);
            omega       <radian per sec>;
        }
    }
}
```

_omega_ is calculate from the input of RPM to rps(radian per sec)

### Porous Zone

Porous Zone is set in the **system/fvOptions** file.

#### Power Law model

Inputs of C0 and C1 is used at _powerLawCoeffs_.

```
<name>
{
    type    explicitPorositySource;
    explicitPorositySourceCoeffs
    {
        type    powerLaw;
        powerLawCoeffs
        {
            C0      <value>;
            C1      <value>;
            coordinateSystem
            {
                type        cartesian;
                origin      (<x> <y> <z>);
                rotation
                {
                    type    axesRotation;
                    e1      (1 0 0);
                    e2      (0 1 0);
                }
            }
        }
        selectionMode   cellZone;
        cellZone        <cell zone name>;
    }
}
```
#### Darcy Forchheimer model

Input values of direction vector 1 and 2 are used at _e1_ and _e2 of _coordinateSystem_. Inertial Resistance Coefficient(f, Forchheimer coefficient) and Viscous Resistance Coefficient(d, Darcy coefficient) is used at _f_ and _d_ of _DarcyForchheimerCoeffs_.


```
<name>
{
    type    explicitPorositySource;
    explicitPorositySourceCoeffs
    {
        type    DarcyForchheimer;
        DarcyForchheimerCoeffs
        {
            d d [ 0 -2 0 0 0 0 0 ]  (<x-d> <y-d> <z-d>);
            f f [ 0 -1 0 0 0 0 0 ]  (<x-f> <y-f> <z-f>);
            coordinateSystem
            {
                type        cartesian;
                origin      (<x> <y> <z>);
                rotation
                {
                    type    axes;
                    e1      (<x-dir1> <y-dir1> <z-dir1>);
                    e2      (<x-dir2> <y-dir2> <z-dir2>);
                }
            }
        }
        selectionMode   cellZone;
        cellZone        <cell zone name>;
    }
}
```
### Actuator Disk

Actuator Disk is set in the **system/fvOptions** file.

Input of Disk Direction is used at _diskDir_, Power Coefficient at _Cp_, Thrust Coefficient at _Ct_, Disk Area at _diskArea_, Upstream Point at _monitorCoeffs_ and Force Computation method at _variant_. 

```
actuationDiskSource_porousZone
{
    type            actuationDiskSource;
    fields          (U);
    diskDir         (<x-dir> <y-dir> <z-dir>);
    Cp              <value>;
    Ct              <value>;
    diskArea        <value>;
    monitorMethod   points;
    monitorCoeffs
    {
        points      ((<x> <y> <z>));
    }   
    variant         <Froude or variableScaling>;
    selectionMode   cellZone;
    cellZone        <cell zone name>;
}
```
### Source Terms

Mass, energy, turbulence and species source can be set in the **system/fvOptions** file.

What the source term is for is set at _sources_. _rho_ for mass, _h_ for energy, _k_, _epsilon_, _omega_, _nuTilda_ for turbulence. 

The value is given in _sources_ as (\<explicit value> \<implicit value>), where the value entered is written to the explicit value and 0.0 is written to the implicit value.

There are 2 options of 'Value for Entire Cell Zone' and 'Value per Unit Volume' for Specification Method. It is set at _volumeMode_ as  _absolute_ for 'Value for Entire Cell Zone' and _specific_ for 'Value per Unit Volume'.

When giving source term to an entire region rather than a cell zone, the _selectionMode_ will be _all_.

```
<name>
{
    type            scalarSemiImplicitSource;
    volumeMode      <absolute or specific>;
    sources
    {
        rho         (<value> 0.0); // h, k, epsilon, omega, nuTilda, <species>
    }
    selectionMode   cellZone; //all;
    cellZone        <cell zone name>;
}
```
For piecewise linear
```
<name>
{
    type            scalarSemiImplicitSource;
    volumeMode      <absolute or specific>;
    sources
    {
        h
        {
            explicit table  ( (<t0> <t1> ...) (<flowrate0> <flowrate1>) ... );
            implicit        none;
        }
    }
    selectionMode   cellZone;
    cellZone        <cell zone name>;
}
```
For polynomial
```
<name>
{
    type            scalarSemiImplicitSource;
    volumeMode      <absolute or specific>;
    sources
    {
        h   
        {
            explicit polynomial ( (<a0> 0) (<a1> 1) ... );
            implicit none;
        }
    }
    selectionMode   cellZone;
    cellZone        <cell zone name>;
}
```

#### Source term of user defined scalar

Source term of user defined scalar is not set in **system/fvOptions** file, but in the _functions_ dictionary of **system/controlDict** file.

In the user define scalar dictionary of _functions_, _fvOptions_ dictionary is set as follows

```
functions
{
    ...

    <name>
    {
        type            scalarTransport;
        ...
        fvOptions
        {
            <name>
            {
                type            scalarSemiImplicitSource;
                volumeMode      absolute;
                sources     
                {
                    <uds name>  (<value> 0.0);
                }
                selectionMode   cellZone;
                cellZone        <cell zone name>;
            }
        }
    }
}
```

### Fixed Values

You can fix velocity, temprature, turbulence, specie and user defined scalars in the file **system/fvOptions**. 

#### Velocity

_type_ is _meanVelocityForce_. 

Input velocity is set at _Ubar_, relaxation factors are at _relaxation_.

```
<name>
{
    type            meanVelocityForce;
    active          yes;
    fields          (U);
    Ubar            (<Ux> <Uy> <Uz>);
    relaxation      <value>;
    selectionMode   cellZone;
    cellZone        <cell zone name>;
}
```

#### Temperature 

_type_ is _fixedTemperatureConstraint_. 

Input temperature is set at _temperature_.
```
fixedTemperature_porousZone
{
    type            fixedTemperatureConstraint;
    active          yes;
    mode            uniform;
    temperature     constant <value>;
    selectionMode   cellZone;
    cellZone        <cell zone name>;
}
```

#### Turbulence and Species 

_type_ is _scalarFixedValueConstraint_. 

Field and input value is set at _fixedValues_.
```
fixedTemperature_porousZone
{
    type            scalarFixedValueConstraint;
    active          yes;
    fieldValues
    {
        k   <value>;  // epsilon, omega, nuTilda
    }
    selectionMode   cellZone;
    cellZone        <cell zone name>;
}
```

#### Fixed value of user defined scalar

Fixed value of user defined scalar is not set in **system/fvOptions**, but in the _functions_ dictionary of **system/controlDict** file.

In the user define scalar dictionary of _functions_, _fvOptions_ dictionary is set as follows

```
functions
{
    ...

    <name>
    {
        type            scalarTransport;
        ...
        fvOptions
        {
            <name>
            {
                type            scalarFixedValueConstraint;
                active          yes;
                fieldValues
                {
                    <uds name>  <value>;
                }
                selectionMode   cellZone;
                cellZone        <cell zone name>;
            }
        }
    }
}
```

<!---------------------------------------------------------------------------------->
## Boundary Conditions

Boundary Conditions are set in files in **0** folder. 

### Velocity Inlet

#### U

Velocity Specification Method : Magnitude, Normal to Boundary
```
<boundayr name> 
{
    type     surfaceNormalFixedValue;
    refValue uniform -<value>;
}
```

Velocity Specification Method : Component
```
<boundayr name>
{
    type    fixedValue;
    value   uniform (<Ux> <Uy> <Uz>);
}
```
time dependent profile, magnitude, normal to boundary
```
<boundayr name> 
{
    type            uniformNormalFixedValue;
    uniformValue    table;
    uniformValueCoeffs
    {
        values
        (
            (<time0>    -<velocityMagnitude0>)
            (<time1>    -<velocityMagnitude1>)
            (<time2>    -<velocityMagnitude2>)
            ...
        );
    }
}
```
time dependent profile, component
```
<boundayr name>
{
    type            uniformFixedValue;
    uniformValue    table;
    uniformValueCoeffs
    {
        values
        (
            (<time0>  (<Ux0> <Uy0> <Uz0>))
            (<time1>  (<Ux1> <Uy1> <Uz1>))
            (<time2>  (<Ux2> <Uy2> <Uz2>))
            ...
        );
    }
}
```
Using spatial profile, file of x, y, z coordinate is **constant/boundaryData/\<patch name>/points\_U** and file of Ux, Uy, Uz valueis **constant/boundaryData/\<patch name>/0/U**
```
<boundayr name> 
{
    type    timeVaryingMappedFixedValue;
    points  points_U;
}
```
constant/boundaryData/\<patch name>/points\_U
```
<number of points>
(
    (<x-coordinate0> <y-coordinate0> <z-coordinate0>)
    (<x-coordinate1> <y-coordinate1> <z-coordinate1>)
    ...    
)
```
constant/boundaryData/\<patch name>/0/U
```
<number of points>
(
    (<Ux0> <Uy0> <Uz0>)
    (<Ux1> <Uy1> <Uz1>)
    ...    
)
```

#### p\_rgh

```
<boundayr name>
{
    type     zeroGradient;
}
```
#### T

Constant
```
<boundayr name>
{
    type    fixedValue;
    value   uniform <value>;
}
```
time dependent, piecewise linear
```
<boundayr name> 
{
    type            uniformFixedValue;
    uniformValue    table;
    uniformValueCoeffs
    {
        values
        (
            (<time0>    <T0>)
            (<time1>    <T1>)
            (<time2>    <T2>)
            ...
        );
    }
}
```
time dependent, polynomial
```
<boundayr name> 
{
    type            uniformFixedValue;
    uniformValue    polynomial
    (
        (a0  0)
        (a1  1)
        (a2  2)
        ...
    );
}
```

#### k
Specification Method : K and Epsilon
```
<boundayr name>
{
    type        inletOutlet;
    inletValue  uniform <inlet value>;
    value       uniform <value>;
}
```
Specification Method : Intensity and Viscosity Ratio
```
<boundayr name>
{
    type            turbulentIntensityInletOutletTKE;
    turbIntensity   uniform <value>;
    value           uniform <value>;
}
```
#### epsilon, omega
Specification Method : K and Epsilon
```
<boundayr name>
{
    type        inletOutlet;
    inletValue  uniform <inlet value>;
    value       uniform <value>;
}
```
Specification Method : Intensity and Viscosity Ratio
```
<boundayr name>
{
    type            viscosityRatioInletOutletTDR;
    viscosityRatio  uniform <value>;
    value           uniform <value>;
}
```
#### nuTilda
Specification Method : Modifed Turbulent Viscosity
```
<boundayr name>
{
    type    fixedValue;
    value   uniform <value>;
}
```
Specification Method : Turbulent Viscosity Ratio
```
<boundayr name>
{
    type            viscosityRatioInletOutletNuTilda;
    viscosityRatio  <value>;
    value           uniform <value>;
}
```
#### nut, alphat
```
<boundayr name>
{
    type    calculated;
    value   uniform <value>;
}
```
#### volume fraction, species, user defined scalar
```
<boundayr name>
{
    type    fixedValue;
    value   uniform <value>;
}
```
<!---------------------------------------------------------------------------------->
### Flow Rate Inlet

#### U
Flow Rate Specification Method : Volume Flow Rate
```
<boundayr name>
{
    type                flowRateInletVelocity;
    volumetricFlowRate  <value>;
}
```
Flow Rate Specification Method : Mass Flow Rate
```
<boundayr name>
{
    type            flowRateInletVelocity;
    massFlowRate    <value>;
    rhoInlet        <value>;
}
```

Dictionary of p\_rgh, T, turbulence is the same with Velocity Inlet
<!---------------------------------------------------------------------------------->
### Pressure Inlet

#### U
```
<boundayr name>
{
    type    pressureInletOutletVelocity;
    value   uniform <value>;
}
```
#### p\_rgh
```
<boundayr name>
{
    type    totalPressure;
    p0      uniform <value>;
}
```

Dictionary of T, turbulence is the same with Velocity Inlet
<!---------------------------------------------------------------------------------->
### ABL Inlet

#### U
```
<boundayr name>
{
    type    atmBoundaryLayerInletVelocity;
    flowDir (<x-dir> <y-dir> <z-dir>);
    zDir    (<x-dir> <y-dir> <z-dir>);
    Uref    <value>;
    Zref    <value>;
    z0      <value>;
    d       <value>;
}
```
#### p\_rgh
```
<boundayr name>
{
    type    zeroGradient;
}
```
#### k
```
<boundayr name>
{
    type    atmBoundaryLayerInletK;
    flowDir (<x-dir> <y-dir> <z-dir>);
    zDir    (<x-dir> <y-dir> <z-dir>);
    Uref    <value>;
    Zref    <value>;
    z0      <value>;
    d       <value>;
}
```
#### epsilon
```
<boundayr name>
{
    type    atmBoundaryLayerInletEpsilon;
    flowDir (<x-dir> <y-dir> <z-dir>);
    zDir    (<x-dir> <y-dir> <z-dir>);
    Uref    <value>;
    Zref    <value>;
    z0      <value>;
    d       <value>;
}
```
#### omega
```
<boundayr name>
{
    type    atmBoundaryLayerInletOmega;
    flowDir (<x-dir> <y-dir> <z-dir>);
    zDir    (<x-dir> <y-dir> <z-dir>);
    Uref    <value>;
    Zref    <value>;
    z0      <value>;
    d       <value>;
}
```
#### nut
```
<boundayr name>
{
    type    calculated;
    value   uniform <value>;
}
```
<!---------------------------------------------------------------------------------->
### Free Stream

#### U
```
<boundayr name>
{
    type            freestreamVelocity;
    freestreamValue uniform <value>;
}
```
#### p\_rgh
```
<boundayr name>
{
    type            freestreamPressure;
    freestreamValue uniform <value>;
}
```
#### T
```
<boundayr name>
{
    type            freestream;
    freestreamValue uniform <value>;
}
```
#### k
Specification Method : K and Epsilon
```
<boundayr name>
{
    type            freestream;
    freestreamValue uniform <value>;
}
```
Specification Method : Intensity and Viscosity Ratio
```
<boundayr name>
{
    type            turbulentIntensityInletOutletTKE;
    turbIntensity   uniform <value>;
    value           uniform <value>;
}
```
#### epsilon, omega
Specification Method : K and Epsilon
```
<boundayr name>
{
    type            freestream;
    freestreamValue uniform <inlet value>;
}
```
Specification Method : Intensity and Viscosity Ratio
```
<boundayr name>
{
    type            viscosityRatioInletOutletTDR;
    viscosityRatio  uniform <value>;
    value uniform   <value>;
}
```
#### nuTilda
Specification Method : Modifed Turbulent Viscosity
```
<boundayr name>
{
    type            freestream;
    freestreamValue uniform <inlet value>;
}
```
Specification Method : Turbulent Viscosity Ratio
```
<boundayr name>
{
    type            viscosityRatioInletOutletNuTilda;
    viscosityRatio  <value>;
    value           uniform <value>;
}
```
#### nut, alphat
```
<boundayr name>
{
    type    calculated;
    value   uniform <value>;
}
```
#### species, user defined scalar
```
<boundayr name>
{
    type    fixedValue;
    value   uniform <value>;
}
```
<!---------------------------------------------------------------------------------->
### Open Channel Inlet

#### U
```
<boundayr name>
{
    type        variableHeightFlowRateInletVelocity;
    alpha       alpha.<secondary material name>;
    flowRate    <value>;
    value       uniform <value>;
}
```
#### p\_rgh
```
<boundayr name>
{
    type    zeroGradient;
}
```
#### alpha.\<secondary material name>
```
<boundayr name>
{
    type        variableHeightFlowRate;
    lowerBound  0.0;
    upperBound  1.0;
    value       uniform <value>;
}
```

Dictionary of turbulence is the same with Velocity Inlet
<!---------------------------------------------------------------------------------->
### Far-field Riemann

#### U
```
<boundayr name>
{
    type    farfieldRiemann;
    flowDir (<x-dir> <y-dir> <z-dir>);
    MInf    <value>;
    pInf    <value>;
    TInf    <value>;
    value   uniform (<Ux> <Ux> <Ux>);
}
```
#### p, T
```
<boundayr name>
{
    type    farfieldRiemann;
    flowDir (<x-dir> <y-dir> <z-dir>);
    MInf    <value>;
    pInf    <value>;
    TInf    <value>;
    value   uniform <value>;
}
```

Dictionary of turbulence is the same with Velocity Inlet
<!---------------------------------------------------------------------------------->
### Subsonic Inlet

#### U
```
<boundayr name>
{
    type    subsonicInlet;
    flowDir (<x-dir> <y-dir> <z-dir>);
    p0      <value>;
    T0      <value>;
    value   uniform (<Ux> <Ux> <Ux>);
}
```
#### p, T
```
<boundayr name>
{
    type    subsonicInlet;
    flowDir (<x-dir> <y-dir> <z-dir>);
    p0      <value>;
    T0      <value>;
    value   uniform <value>;
}
```

Dictionary of turbulence is the same with Velocity Inlet
<!---------------------------------------------------------------------------------->
### Supersonic Inflow

#### U
```
<boundayr name>
{
    type    fixedValue;
    value   uniform (<Ux> <Ux> <Ux>);
}
```
#### p, T
```
<boundayr name>
{
    type    fixedValue;
    value   uniform <value>;
}
```

Dictionary of turbulence is the same with Velocity Inlet
<!---------------------------------------------------------------------------------->
### Pressure Outlet

#### U
Without Non-reflecging Boundary option
```
<boundayr name>
{
    type    pressureInletOutletVelocity;
    value   uniform (<Ux> <Ux> <Ux>);
}
```
With Non-reflecging Boundary option
```
<boundayr name>
{
    type    waveTransmissive;
    gamma   <value>;
}
```
#### p\_rgh
Without Non-reflecging Boundary option
```
<boundayr name>
{
    type    totalPressure;
    p0      uniform <value>; # value is static pressure
}
```
With Non-reflecging Boundary option
```
<boundayr name>
{
    type    waveTransmissive;
    gamma   <value>;
}
```
#### T
Without Specify Backflow Properties
```
<boundayr name>
{
    type    zeroGradient;
}
```
With Specify Backflow Properties
```
<boundayr name>
{
    type        inletOutletTotalTemperature;
    gamma       <value>;
    inletValue  uniform <value>;
    T0          uniform <value>;
}
```
#### k
Without Specify Backflow Properties
```
<boundayr name>
{
    type    zeroGradient;
}
```
With Specify Backflow Properties, k and Epsilon
```
<boundayr name>
{
    type        inletOutlet;
    inletValue  uniform <value>;
    value       uniform <value>;
}
```
With Specify Backflow Properties, Intensity and Viscosity Ratio
```
<boundayr name>
{
    type            turbulentIntensityInletOutletTKE;
    turbIntensity   uniform <value>;
    value           uniform <value>;
}
```
#### epsilon, omega
Without Specify Backflow Properties
```
<boundayr name>
{
    type    zeroGradient;
}
```
With Specify Backflow Properties, k and Epsilon
```
<boundayr name>
{
    type        inletOutlet;
    inletValue  uniform <value>;
    value       uniform <value>;
}
```
With Specify Backflow Properties, Intensity and Viscosity Ratio
```
<boundayr name>
{
    type        viscosityRatioInletOutletTDR;
    inletValue  uniform <value>;
    value       uniform <value>;
}
```
#### nuTilda
Without Specify Backflow Properties
```
<boundayr name>
{
    type    zeroGradient;
}
```
With Specify Backflow Properties, Modifed Turbulent Viscosity
```
<boundayr name>
{
    type        inletOutlet;
    inletValue  uniform <value>;
    value       uniform <value>;
}
```
With Specify Backflow Properties, Turbulent Viscosity Ratio
```
<boundayr name>
{
    type        viscosityRatioInletOutletNuTilda;
    inletValue  uniform <value>;
    value       uniform <value>;
}
```
#### nut, alphat
Without Specify Backflow Properties
```
<boundayr name>
{
    type    zeroGradient;
}
```
With Specify Backflow Properties, Intensity and Viscosity Ratio
```
<boundayr name>
{
    type    calculated;
    value   uniform <value>;
}
```
<!---------------------------------------------------------------------------------->
### Open Channel Outlet

#### U
```
<boundayr name>
{
    type    outletPhaseMeanVelocity;
    Umean   <value>;
    alpha   alpha.<sescondary phase name>;
    value   uniform <value>;
}
```
#### p\_rgh
```
<boundayr name>
{
    type    zeroGradient;
}
```
#### k
Specification Method : K and Epsilon
```
<boundayr name>
{
    type        inletOutlet;
    inletValue  uniform <value>;
    value       uniform <value>;
}
```
Specification Method : Intensity and Viscosity Ratio
```
<boundayr name>
{
    type            turbulentIntensityInletOutletTKE;
    turbIntensity   uniform <value>;
    value           uniform <value>;
}
```
#### epsilon, omega
Specification Method : K and Epsilon
```
<boundayr name>
{
    type        inletOutlet;
    inletValue  uniform <value>;
    value       uniform <value>;
}
```
Specification Method : Intensity and Viscosity Ratio
```
<boundayr name>
{
    type            viscosityRatioInletOutletTDR;
    viscosityRatio  uniform <value>;
    value           uniform <value>;
}
```
#### nuTilda
Specification Method : Modified Turbulent Viscosity
```
<boundayr name>
{
    type        inletOutlet;
    inletValue  uniform <value>;
    value       uniform <value>;
}
```
Specification Method : Turbulent Viscosity Ratio
```
<boundayr name>
{
    type        viscosityRatioInletOutletNuTilda;
    inletValue  uniform <inlet value>;
    value       uniform <value>;
}
```
#### nut
```
<boundayr name>
{
    type    calculated;
    value   uniform <value>;
}
```
<!---------------------------------------------------------------------------------->
### Outflow

#### U, p\_rgh, T, k, epsilon, omega, nuTilda
```
<boundayr name>
{
    type    zeroGradient;
}
```
<!---------------------------------------------------------------------------------->
### Subsonic Outflow

#### U, p, T
```
<boundayr name>
{
    type    subsonicOutflow;
    pExit   <value>;
    value   uniform <value>;
}
```
#### k, epsilon, omega, nuTilda
```
<boundayr name>
{
    type    zeroGradient;
}
```
#### nut, alphat
```
<boundayr name>
{
    type    calculated;
    value   uniform <value>;
}
```
<!---------------------------------------------------------------------------------->
### Supersonic Outflow

#### U, p\_rgh, T, k, epsilon, omega, nuTilda
```
<boundayr name>
{
    type    zeroGradient;
}
```
<!---------------------------------------------------------------------------------->
### Wall

#### U
No Slip
```
<boundayr name>
{
    type    fixedValue;
    value   uniform (0 0 0)
}
```
Slip
```
<boundayr name>
{
    type    slip;
}
```
Moving Wall
```
<boundayr name>
{
    type    movingWallVelocity;
    value   uniform (0 0 0)
}
```
Atmospheric Wall
```
<boundayr name>
{
    type    fixedValue;
    value   uniform (0 0 0)
}
```
Translational Moving Wall
```
<boundayr name>
{
    type    fixedValue;
    value   uniform (<Ux> <Uy> <Uz>)
}
```
Rotational Moving Wall
```
<boundayr name>
{
    type    rotatingWallVelocity;
    origin  (<x> <y> <z>);
    axis    (<x> <y> <z>);
    omega   <value>;
}
```
#### p\_rgh
```
<boundayr name>
{
    type    fixedFluxPressure;
}
```
#### p of density based solver
```
<boundayr name>
{
    type    zeroGradient;
}
```
#### T
Adiabatic
```
<boundayr name>
{
    type    zeroGradient;
}
```
Constant Temperature
```
<boundayr name>
{
    type    fixedValue;
    value   uniform <value>;
}
```
Constant Heat Flux
```
<boundayr name>
{
    type        externalWallHeatFluxTemperature;
    mode        flux;
    q           uniform <value>;
    kappaMethod fluidThermo;
    value       uniform <value>;
}
```
Convection
```
<boundayr name>
{
    type            externalWallHeatFluxTemperature;
    mode            coefficient;
    h               uniform <value>;
    Ta              uniform <value>;
    kappaMethod     fluidThermo;
    value           uniform <value>;
    thicknessLayers (<value0> <value1> <...>);
    kappaLayers     (<value0> <value1> <...>);
}
```
#### k
```
<boundayr name>
{
    type    kqRWallFunction;
    value   uniform <value>;
}
```
#### epsilon
Standard Wall Function
```
<boundayr name>
{
    type    epsilonWallFunction;
    value   uniform <value>;
}
```
Two layer Wall Function
```
<boundayr name>
{
    type    epsilonBlendedWallFunction;
    value   uniform <value>;
}
```
Atmospheric Wall
```
<boundayr name>
{
    type    atmEpsilonWallFunction;
    z0      <value>;
    d       <value>;
    value   uniform <value>;
}
```
#### omega
```
<boundayr name>
{
    type        omegaBlendedWallFunction;
    blending    tanh;
    value       uniform <value>;
}
```
#### nuTilda
```
<boundayr name>
{
    type    zeroGradient;
}
```
#### nut
Standard Wall Function
```
<boundayr name>
{
    type    nutkWallFunction;
    value   uniform <value>;
}
```
Two layer Wall Function, SST k-omega model, Spalart-Allmaras model
```
<boundayr name>
{
    type    nutSpaldingWallFunction;
    value   uniform <value>;
}
```
Atmospheric Wall
```
<boundayr name>
{
    type    atmNutkWallFunction;
    z0      <value>;
    value   uniform <value>;
}
```
#### alphat
Adiabatic boundary condition
```
<boundayr name>
{
    type    compressible::alphatWallFunction;
    Prt     <value>;
    value   uniform <value>;
}
```
Constant temperature, Constant heat flux, Convection boundary condition
```
<boundayr name>
{
    type    compressible::alphatJayatillekeWallFunction;
    Prt     <value>;
    value   uniform <value>;
}
```
<!---------------------------------------------------------------------------------->
### Thermo-Coupled Wall

#### U
```
<boundayr name>
{
    type    fixedValue;
    value   uniform (0 0 0);
}
```
#### p\_rgh
```
<boundayr name>
{
    type    fixedFluxPressure;
}
```
#### T
```
<boundayr name>
{
    type        compressible::turbulentTemperatureRadCoupledMixed;
    Tnbr        T;
    kappaMethod fluidThermo;
    value       uniform <value>;
}
```
#### k
```
<boundayr name>
{
    type    kqRWallFunction;
    value   uniform <value>;
}
```
#### epsilon
Standard Wall Function
```
<boundayr name>
{
    type    epsilonWallFunction;
    value   uniform <value>;
}
```
Two layer Wall Function
```
<boundayr name>
{
    type    epsilonBlendedWallFunction;
    value   uniform <value>;
}
```
#### omega
```
<boundayr name>
{
    type        omegaBlendedWallFunction;
    blending    tanh;
    value       uniform <value>;
}
```
#### nuTilda
```
<boundayr name>
{
    type        zeroGradient;
}
```
#### nut
k-epsilon, Standard Wall Function
```
<boundayr name>
{
    type    nutkWallFunction;
    value   uniform <value>;
}
```
k-omega, Two-layer Wall Function, Spalart-Allmaras
```
<boundayr name>
{
    type    nutSpaldingWallFunction;
    value   uniform <value>;
}
```
#### alphat
```
<boundayr name>
{
    type    compressible::alphatJayatillekeWallFunction;
    value   uniform <value>;
}
```
<!---------------------------------------------------------------------------------->
### Region Interface

#### U 
```
<boundayr name>
{
    type    fixedValue;
    value   uniform (0 0 0);
}
```
#### p\_rgh
```
<boundayr name>
{
    type    fixedFluxPressure;    
}
```
#### T
```
<boundayr name>
{
    type    compressible::turbulentTemperatureCoupledBaffleMixed;    
}
```
#### k
```
<boundayr name>
{
    type    kqRWallFunction;    
}
```
#### epsilon
standard wall function
```
<boundayr name>
{
    type    epsilonWallFunction;    
}
```
two layer wall function
```
<boundayr name>
{
    type    epsilonBlendedWallFunction;    
}
```
#### omega
```
<boundayr name>
{
    type    omegaBlendedWallFunction;    
}
```
#### nuTilda
```
<boundayr name>
{
    type    zeroGradient;    
}
```
#### nut
k-epsilon, standard wall function
```
<boundayr name>
{
    type    nutkWallFunction;    
}
```
two layer wall function, SST k-omega, Spalart-Allmaras
```
<boundayr name>
{
    type    nutSpaldingWallFunction;    
}
```
#### alphat
```
<boundayr name>
{
    type    compressible::alphatJayatillekeWallFunction;    
}
```
<!---------------------------------------------------------------------------------->
### Empty
```
<boundayr name>
{
    type    empty;
}
```
### Wedge
```
<boundayr name>
{
    type    wedge;
}
```
### Symmetry
```
<boundayr name>
{
    type    symmetry;
}
```
### Interface
```
<boundayr name>
{
    type    cyclicAMI;    
}
```
### Cyclic
```
<boundayr name>
{
    type    cyclic;
}
```
### Porous Jump
All fields except p\_rgh
```
<boundayr name>
{
    type    cyclic;
}
```
p\_rgh
```
<boundayr name>
{
    type        porousBafflePressure;
    patchType   cyclic;
    D           <value>;
    I           <value>;
    length      <value>;
    value       uniform <value>;
}
```
### Fan
All fields except p\_rgh
```
<boundayr name>
{
    type    cyclic;
}
```
p\_rgh
```
<boundayr name>
{
    type        fan;
    patchType   cyclic;
    jumpTable   csvFile;
    jumpTableCoeffs
    {
      nHeaderLine       0;
      refColumn         0;
      componentColumns  (1);
      separator         ",";
      mergeSeparators   no;
      file              "<path and file name>";
    }
}
```
<!---------------------------------------------------------------------------------->
## Numerical Conditions

Numerical Conditions are set in **system/fvSolution** and **system/fvSchemes** file.

### Pressure-Velocity Coupling Scheme

Pressure-Velocity Coupling Scheme is set at _consistent_ of _SIMPLE_ and _PIMPLE_ dictionary of **system/fvSolution file**. 

_no_ for SIMPLE, _yes_ for SIMPLEC

```
SIMPLE
{
    consistent      <yes or no>;
    ...
}
```

### Momentum Predictor

Momentum Predictor is set at the _momentumPredictor_ of _PIMPLE_ dictionary in the **system/fvSolution** file.

```
PIMPLE
{
    ...
    momentumPredictor   <on or off>;
    ...
}
```

### Flux Type

Flux Type is set at _Riemann_ dictionary in the **system/fvSolution** file. _Riemann_ dictionary is used only for density based solver and as follows

```
Riemann
{
    fluxScheme      <roeFlux or AUSMplusFlux or AUSMplusUpFlux>;
    secondOrder     <yes or no>;
    reconGradScheme VKLimited Gauss linear 1;
    roeFluxCoeffs
    {
        epsilon     <value>;
    }
    AUSMplusUpFluxCoeffs
    {
        MInf        <value>;
    }
}
```
 _fluxScheme_ is _roeFlux_ for Roe-FDS, _AUSMplusFlux_ for AUSM, _AUSMplusUpFlux_ for AUSM-up.

Entropy Fix Coefficient is set at _epsilon_ of _roeFluxCoeffs_, Cut-off Mach Number is at _MInf_ of _AUSMplusUpFluxCoeffs_.

### Discretization Schemes

#### Time

Time is set at _ddtSchemes_ dictionary in the **system/fvSchemes** file.

_Euler_ for First Order Implicit , _backward_ for Second Order Implicit

```
ddtSchemes
{
    default     <Euler or backward>;
}
```
#### Pressure

Pressure is set at _interpolationSchemes_ dictionar in the **system/fvSchemes** file.

For Linear
```
interpolationSchemes
{
    default             linear;
    interpolate(p)      linear;
    interpolate(p_rgh)  linear;
}
```
For Momentum Weighted
```
interpolationSchemes
{
  default               linear;
  interpolate(p)        momentumWeighted;
  interpolate(p_rgh)    momentumWeighted;
}
```
For Momentum Weighted Reconstruct
```
interpolationSchemes
{
    default             linear;
    interpolate(p)      momentumWeightedReconstruct;
    interpolate(p_rgh)  momentumWeightedReconstruct;
}
```

#### Momentum, Energy, Turbulence, Volume Fraction

These are set in divSchemes dictionary in the **system/fvSchemes** file.

For First Order Upwind

```
divSchemes
{
    default             Gauss linear;
    div(phi,U)          Gauss upwind;
    div(rhoPhi,U)       Gauss upwind;
    div(phiNeg,U)       Gauss upwind;
    div(phiPos,U)       Gauss upwind;
    div(phi,k)          Gauss upwind;
    div(phi,epsilon)    Gauss upwind;
    div(phi,omega)      Gauss upwind;
    div(phi,nuTilda)    Gauss upwind;
    div(phi,alpha)      Gauss upwind;
    div(phirb,alpha)    Gauss upwind;
    div(phi,scalar)     Gauss upwind;
}
```

For Second Order Upwind
```
divSchemes
{
    default             Gauss linear;
    div(phi,U)          Gauss linearUpwind momentumReconGrad;
    div(rhoPhi,U)       Gauss linearUpwind momentumReconGrad;
    div(phiNeg,U)       Gauss MinmodV;
    div(phiPos,U)       Gauss MinmodV;
    div(phi,k)          Gauss linearUpwind turbulenceReconGrad;
    div(phi,epsilon)    Gauss linearUpwind turbulenceReconGrad;
    div(phi,omega)      Gauss linearUpwind turbulenceReconGrad;
    div(phi,nuTilda)    Gauss linearUpwind turbulenceReconGrad;
    div(phi,alpha)      Gauss vanLeer;
    div(phirb,alpha)    Gauss linear;
    div(phi,scalar)     Gauss linearUpwind momentumReconGrad;
}
```
#### For density based solver

Discretization scheme of Flow is set at Riemann dictionary in the **system/fvSolution** file. For 'First Order Upwind' _secondOrder_ is _no_, otherwise _yes_

```
Riemann
{
    ...
    secondOrder     <yes or no>;
    ...
}
```

For turbulence same as pressure based solver.

### Under-Relaxation Factors

Relaxation Factors is set at _relaxationFactors_ dictionary in the **system/fvSolution** file.

Input values are set as follows

```
relaxationFactors
{
    fields
    {
        p           <value>;
        pFinal      <value>;
        p_rgh       <value>;
        p_rghFinal  <value>;
        rho         <value>;
        rhoFinal    <value>;
    }
    equations
    {
        U           <value>;
        UFinal      <value>;
        h           <value>;
        hFinal      <value>;
        "(k|epsilon|omega|nuTilda)"         <value>;
        "(k|epsilon|omega|nuTilda)Final"    <value>;
        alpha.waterLiquid       <value>;
        alpha.waterLiquidFinal  <value>;
        scalar                  <value>;
        scalarFinal             <value>;
    }
}
```

### Improve Stability

Improve Stability option is set at _laplacianSchemes_ dictionary in the **system/fvSchemes** file.

If option is Off

```
laplacianSchemes
{
    default     Gauss linear corrected;
}
```
If option is On

```
laplacianSchemes
{
    default     Gauss linear limited corrected 0.5;
}
```
If solver is density based, this option is not used and laplacianSchemes dictionary is same as option is off.

### Max Iterations per Time Step, Number of Correctors

These are set at _PIMPLE_ dictionary in the **system/fvSolution** file.

_nOuterCorrectors_ for Max Iterations per Time Step, _nCorrectors_ for Number of Correctors

```
PIMPLE
{
    ...
    nCorrectors                 <value>;
    nOuterCorrectors            <value>;
    ...
}
```

### Multiphase

Multiphase related setups are set at _solvers_ dictionary in the **system/fvSolution** file.
```
solvers
{
    ...
    "alpha.*"
    {
        ...
        nAlphaSubCycles     <value>;
        nAlphaCorr          <value>;
        MULESCorr           <yes or no>;
        cAlpha              <value>;
        nLimiterIter        <value>;
        ...
  }
    ...
}
```
_nAlphaSubCycles_ for Max. Iteration per Time Step, _nAlphaCorr_ for Number of Correctors

If MULES Variant is Explicit, _MULESCorr_ is _no_, otherwise _yes_

Phase Interface Compression Factor is set at _cAlpha_.

Number of MULES iterations over the limiter is set at _nLimiterIter_.

### Convergence Criteria

For pressure based solver convergence criteria is set at _residualControl_ dictionary of _SIMPLE_ and _PIMPLE_ in the **system/fvSolution** file. 

```
SIMPLE
{
    ...
    residualControl
    {
        p           <value>;
        p_rgh       <value>;
        U           <value>;
        h           <value>;
        "(k|epsilon|omega|nuTilda)" <value>;
        "alpha.*"   <value>;
    }
}
```
For transient, there are _tolerance_ and _relTol_ in _residualControl_ dictionary. Inputs of Absolute are set for _tolerance_ and inputs of Relative are set for _relTol_.
```
PIMPLE
{
    ...
    residualControl
    {
        ...
        p_rgh
        {
            tolerance       <value>;
            relTol          <value>;
        }
        U
        {
            tolerance       <value>;
            relTol          <value>;
        }
        ...
    }
}
```
For density based solver convergence criteria is set at _LU-SGS_ dictionay in the **system/fvSolution** file.

```
LU-SGS
{
    residualControl
    {
        rho     0.001;
        rhoU    0.001;
        rhoE    0.001;
        "(k|epsilon|omega|nuTilda)" 0.001;
    }
}
```

### Limits

_limitT_ dictionary in the **system/fvOptions** file is used for limit temperature.
```
limitT
{
    type            limitTemperature;
    active          yes;
    selectionMode   all;
    min             1;
    max             5000;
}
```

### Equations

In _SIMPLE_ and _PIMPLE_ dictionary in the **system/fvSolution** file, you can on/off equations of flow, energy and species. 

```
SIMPLE // PIMPLE
{
    ...
    solveFlow                   <yes or no>;
    solveEnergy                 <yes or no>;
    solveSpecies                <yes or no>;
    ...
}
```

3 terms of energy equation as Viscous dissipation, Kinetic energy, Pressure Work can be of/off in the **constant/thermophysicalProperties** file.
```
includeViscousDissipation   <true of false>;
includeKineticEnergy        <true of false>;
includePressureWork         <true of false>;
```


### Default numerical conditions

#### SIMPLE in fvSolution file
_nNonOrthogonalCorrectors_, _pRefCell_, _pRefValue_ are fixed as 0.

```
SIMPLE
{
    ...
    nNonOrthogonalCorrectors    0;
    pRefCell                    0;
    pRefValue                   0;
    ...
}
```
#### PIMPLE in fvSolution file
_turbOnFinalIterOnly_ is _false_. _nAlphaSpreadIter_ and _nAlphaSweepIter_ is 0, _rDeltaTSmoothingCoeff_ and _rDeltaTDampingCoeff_ is 0.5.

```
PIMPLE
{
    ...
    turbOnFinalIterOnly         false;
    nAlphaSpreadIter            0;
    nAlphaSweepIter             0;
    rDeltaTSmoothingCoeff       0.5;
    rDeltaTDampingCoeff         0.5;
    ...
}
```
#### solvers in fvSolution file

solvers dictionary is for matrix solver for each field. 

_solver_ and _preconditioner_ are depend on field. 

_tolerance_ is fixed as 1e-16, _relative tolerance(relTol)_ is 0.1, _minimum iteration(minIter)_ is 1 and _maximum iteration(maxIter)_ is 5 except volume fraction(alpha). 

For alpha, _tolerance_ is fixed as 1e-8, _relTol_ is 0, _minIter_ is 1 and _maxIter_ is 10.

##### Pressure

p\_rgh uses _PCG_ solver and _GAMG_ preconditioner.
```
solvers
{
    ...
    p_rgh
    {
        solver          PCG;
        preconditioner
        {
            preconditioner  GAMG;
            smoother        DIC;
            tolerance       1e-5;
            relTol          0.1;
        }
        tolerance       1e-16;
        relTol          0.1;
        minIter         1;
        maxIter         5;
    }
    ...
}
```
##### Velocity, turbulence, species, user defined scalar

U, k, epsilon, omega, nuTilda, scalar and Yi use _PBiCGStab_ solver and _DILU_ preconditioner.
```
solvers
{
    ...
    "(U|k|epsilon|omega|nuTilda|scalar|Yi)"
    {
        solver          PBiCGStab;
        preconditioner  DILU;
        tolerance       1e-16;
        relTol          0.1;
        minIter         1;
        maxIter         5;
  }
    ...
}
```
##### Energy

h uses _PBiCGStab_ solver and _GAMG_ preconditioner.
```
solvers
{
    ...
    h
    {
        solver          PBiCGStab;
        preconditioner
        {
            preconditioner  GAMG;
            smoother        DILU;
            tolerance       1e-5;
            relTol          0.1;
        }
        tolerance       1e-16;
        relTol          0.1;
        minIter         1;
        maxIter         5;
    }
    ...
}
```
##### Density

rho uses _PCG_ solver and _DIC_ preconditioner.
```
solvers
{
    ...
    rho
    {
        solver          PCG;
        preconditioner  DIC;
        tolerance       1e-16;
        relTol          0.1;
        minIter         1;
        maxIter         5;
    }
    ...
}
```
##### alpha

alpha uses _smoothSolver_ solver and _symGaussSeidel_ smoother.

_icAlpha_ is fixed as 0 and _alphaApplyPrevCorr_ is fixed as _yes_.
```
solvers
{
    ...
    "alpha.*"
    {
        solver              smoothSolver;
        smoother            symGaussSeidel;
        icAlpha             0;
        alphaApplyPrevCorr  yes;
        tolerance           1e-8;
        relTol              0;
        minIter             1;
        maxIter             10;
  }
    ...
}
```
#### fieldBounds in fvSolution file

_fieldBounds_ is used only in density based solver for limit values as follows
```
fieldBounds
{
    p       1e-06   1e+10;
    rho     1e-06   1e+10;
    h       1e-06   1e+10;
    e       1e-06   1e+10;
    rhoE    1e-06   1e+10;
    T       1e-06   3e+4;
    U       3e+4;
}
```
#### gradSchemes in fvSchemes file

In pressure based solver always use _Gauss linear_. _momentumReconGrad_, _energyReconGrad_ and _turbulenceReconGrad_ are definition to use in _divSchemes_.
```
gradSchemes
{
    default             Gauss linear;
    momentumReconGrad   VKLimited Gauss linear 1.0;
    energyReconGrad     VKLimited Gauss linear 1.0;
    turbulenceReconGrad VKLimited Gauss linear 1.0;
}
```
In density based solver, always use conditions below
```
gradSchemes
{
    default         Gauss linear;
    grad(k)         VKLimited Gauss linear 0.5;
    grad(epsilon)   VKLimited Gauss linear 0.5;
    grad(omega)     VKLimited Gauss linear 0.5;
    grad(nuTilda)   VKLimited Gauss linear 0.5;
    reconGrad       VKLimited Gauss linear 0.5;
}
```
#### interpolationSchemes in fvSchemes file

In density based solver setup is fixed as follows
```
interpolationSchemes
{
    default             linear;
    interpolate(rho)    linearUpwind phi grad(rho);
}
```
#### snGradSchemes int fvSchemes file

Always use setup below
```
snGradSchemes
{
    default     corrected;
}
```
<!---------------------------------------------------------------------------------->
## Monitor

Monitor is set in _functions_ dictionary in the **system/controlDict** file.

### Force

```
functions
{
    ...
    <name of forces>
    {
        type            forces;
        libs    ("/baram/solvers/openfoam/lib/libforces.so");
        patches
        (
            <patch name 1>
            ...
        );
        CofR            (<x> <x> <x>);
        updateHeader    false;
        log             false;
        executeControl  timeStep;
        executeInterval 1;
        writeControl    timeStep;
        writeInterval   <value>;
    }
    <name of force coefficients>
    {
        type            forceCoeffs;
        libs    ("/baram/solvers/openfoam/lib/libforces.so");
        patches
        (
            <patch name 1>
            ...
        );
        coefficients    (Cd Cl CmPitch);
        rho             rho;
        Aref            <value>;
        lRef            <value>;
        magUInf         <value>;
        rhoInf          <value>;
        dragDir         (<x-dir> <y-dir> <z-dir>);
        liftDir         (<x-dir> <y-dir> <z-dir>);
        CofR            (<x> <y> <z>);
        updateHeader    false;
        log             false;
        pRef            <value>;
        executeControl  timeStep;
        executeInterval 1;
        writeControl    timeStep;
        writeInterval   <value>;
    }
}
```
Write Interval is set at _writeInterval_. Drag Direction is set at _dragDir_, Lift Direction is at _liftDir_, Center of Rotation is at _CofR_. Selected Boundaries are set at _patches_.

### Point

```
functions
{
    ...
    <name>
    {
        type            probes;
        libs    ("/baram/solvers/openfoam/lib/libsampling.so");
        fields          (<field>);
        probeLocations  ( (<x> <y> <z>) );
        updateHeader    false;
        log             false;
        executeControl  timeStep;
        executeInterval 1;
        writeControl    timeStep;
        writeInterval   <value>;
    }
}
```
_writeInterval_ for Write Interval, _fields_ for Field, _probeLocations_ for Coordidate

### Surface

```
functions
{
    ...
    <name>
    {
        type            surfaceFieldValue;
        libs    ("/baram/solvers/openfoam/lib/libfieldFunctionObjects.so");
        regionType      patch;
        name            <patch name>;
        surfaceFormat   none;
        fields          (<field>);
        operation       <value>;
        writeFields     false;
        updateHeader    false;
        log             false;
        executeControl  timeStep;
        executeInterval 1;
        writeControl    timeStep;
        writeInterval   <value>;
    }
}
```
_writeInterval_ for Write Interval, _fields_ for Field Variable, _operation_ for Report Type, _name_ for Surface

### Volume

```
functions
{
    ...
    <name>
    {
        type            volFieldValue;
        libs    ("/baram/solvers/openfoam/lib/libfieldFunctionObjects.so");
        regionType      cellZone;
        name            <cel zone name>;
        fields          (<field>);
        operation       <value>;
        writeFields     false;
        updateHeader    false;
        log             false;
        executeControl  timeStep;
        executeInterval 1;
        writeControl    timeStep;
        writeInterval   <value>;
    }
}
```
_writeInterval_ for Write Interval, _fields_ for Field Variable, _operation_ for Report Type, _name_ for Volume


### Residual

To create and save residual data, use _functions_ dictionary in the **system/controlDict** file. 

_fields_ are depend on solver.

```
functions
{
    ...
    <name>
    {
        type                solverInfo;
        libs    ("/baram/solvers/openfoam/lib/libutilityFunctionObjects.so");
        executeControl      timeStep;
        executeInterval     1;
        writeResidualFields no;
        fields
        (
            U
            p_rgh
            k
            epsilon
            ...
        );
    }
}
```

<!---------------------------------------------------------------------------------->
## Initialization

To initialize volume value, **system/setFields** file is used.

Values of _defaultFieldValues_ is the input values for all domain and values of _regions_ are for certain volume.
```
defaultFieldValues
(
    volScalarFieldValue <field> <value> 
    ...
);
regions
(
    boxToCell
    {
        box  (<x1> <y1> <z1>) (<x2> <y2> <z2>);
        fieldValues
        (
            volScalarFieldValue <field> <value>
            ... 
        );
    }
    ... 
);
```
For volume fraction initialize, values of all phases are set as follows
```
defaultFieldValues
(
    volScalarFieldValue alpha.<secondary phase1>  <value> 
    volScalarFieldValue alpha.<primaty phase>     <1-value> 
);
regions
(
    boxToCell
    {
        box  (<x1> <y1> <z1>) (<x2> <y2> <z2>);
        fieldValues
        (
            volScalarFieldValue alpha.<secondary phase1>    <value> 
            volScalarFieldValue alpha.<primaty phase1>      <1-value> 
        );
    } 
);
```
The value of Primary phase must be set as sum of all phase is one.

<!---------------------------------------------------------------------------------->
## Run Conditions

Run Conditions are set in the **system/controlDict** file as follows

```
application         <solver>;
startFrom           latestTime;
startTime           0;
stopAt              endTime;
endTime             <value>;
deltaT              <value>;
writeControl        adjustableRunTime;
writeInterval       <value>;
purgeWrite          <value>;
writeFormat         <binary or ascii>;
writePrecision      <value>;
writeCompression    off;
writeAtEnd          true;
timeFormat          general;
timePrecision       <value>;
runTimeModifiable   yes;
adjustTimeStep      <yes or no>;
maxCo               <value>;
maxDi               <value>;
maxAlphaCo          <value>;
```

* _endTime_ : value of Number of Iteration or End Time
* _deltaT_ : value of Time Step Size
* _writeInterval_ : value of Save Interval
* _purgeWrite_ : value of Retain Only the Most Recent Files
* _writeFormat_ : value of Data Write Format
* _writePrecision_ : value of Data Write Precision
* _timePrecision_ : value of Time Precision
* _adjustTimeStep_ : value of Time Stepping Method
* _maxCo_ : value of Courant Number
* _maxDi_ : value of Maximum Diffusion Number
* _maxAlphaCo_ : value of Max Courant Number for VoF














