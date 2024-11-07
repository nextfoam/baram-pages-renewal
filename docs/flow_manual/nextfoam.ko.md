# BaramFlow의 설정이 NextFOAM에서 사용되는 방법

BaramFlow는 넥스트폼이 개발한 OpenFOAM의 포크인 NextFOAM을 사용한다. BaramFlow의 각종 설정들이 NextFOAM에 어떻게 적용되는지를 설명한다. NextFOAM에서 사용되는 용어들은 이탤릭체로 표현하였다.

## 기본조건(General)

### 시간 전진 기법 설정(Time)

시간에 대한 설정은 **system/fvSchemes** 파일의 _ddtSchemes_ 딕셔너리에서 한다.

정상상태 압력기반 솔버의 단상유동은 _steadyState_, 2상유동은 _localEuler_ 이다. 밀도기반 솔버도 _localEuler_ 이다.

```
ddtSchemes
{
    default     <steadyState or localEuler>;
}
```

비정상상태는 시간의 이산화 기법이 1차 음해법(First Order Implicit)일 때는 _Euler_, 2차 음해법(Second Order Implicit)일 때는 _backward_ 이다.


```
ddtSchemes
{
    default     <Euler or backward>;
}
```
<!---------------------------------------------------------------------------------->
### 중력(Gravity)

중력 벡터의 입력값을 **constant/g** 파일에 다음과 같이 설정한다.

```
dimensions  [0 1 -2 0 0 0 0];
value       (<x-value> <y-value> <z-value>);
```
<!---------------------------------------------------------------------------------->
### 작동조건(Operating Conditions)

작동 조건(Operating Conditions) 입력값을 **constant/operatingConditions** 파일에 다음과 같이 설정한다.

```
operatingPressure operatingPressure [1 -1 -2 0 0 0 0]   <value>;
```
<!---------------------------------------------------------------------------------->
## 모델(Model)

### 난류(Turbulence) 

난류는 **constant/turbulenceProperties** 파일에 다음과 같이 설정한다.

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

_simulationType_ 설정과 그에 따른 딕셔너리가 필요하다. _simulationType_ 은 _laminar_, _RAS_, _LES_ 등이 있다.

_turbulence_ 와 _printCoeffs_ 는 항상 on 이다. 

_viscosityRatioMax_ 는 [수치해석기법] - [고급설정]에서 입력한 최대 점도 비율(Maximum Viscosity Ratio) 값이다.

#### 층류(Laminar)

```
simulationType  laminar;
```

뉴턴 유체의 경우 아무런 추가 설정이 없다. 비뉴턴체의 경우 _laminar_ 딕셔너리에서 모델을 설정한다. 비뉴턴유체 모델은 Cross, Herschel-Bulkley, Bird-Carreau, Non-Newtonian power law를 선택할 수 있다. 각 모델의 계수는 다음과 같이 설정한다.

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

항상 다음과 같다.

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

_RASModel_ 은 Standard일 때는 _kEpsilon_, RNG일 때는 _RNGkEpsilon_, Realizable일 때는 _realizableKE_ 를 사용한다.

_Prt_ 는 난류 프란틀 수(Turbulent Prandtl Number)의 내부 유동변수에 사용(for internal Field)에 입력된 값이다.

_Sct_ 는 난류 슈미트 수(Turbulent Schmidt Number)의 값이다.

_ReyStar_ 와 _deltaRey_ 는 realizable 모델에서 벽함수가 Enhanced Wall Treatment(two layer)일 때만 사용되며 항상 60과 10이다.

#### k-omega SST

항상 다음과 같다.

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

LES 모델의 _simulationType_은 _LES_ 이다. Subgrid-Scale 모델과 Length-Scale 모델을 설정하고 그에 따른 딕셔너리가 필요하다.

Subgrid-Scale 모델은 _LESModel_ 에서 설정하고 Length-Scale 모델은 _delta_ 에서 설정한다.

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

##### Subgrid-Scale 모델

Smagorinsky-Lilly(_Smagorinsky_), WALE(_WALE_), Kinetic-Energy Transport(_dynamicKEqn_), One Equation Eddy Viscosity(_kEqn_) 4가지를 선택할 수 있다. Smagorinsky-Lilly일 때는 위와 같으며, 나머지들의 딕셔너리는 다음과 같다.

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

_Sct_ 는 난류 슈미트 수이다. 화학종을 계산할 때만 입력창이 나타난다. 화학종 계산이 없을 때는 항상 0.7을 사용한다.

##### Length-Scale 모델

Cube-root Volume(_cubeRootVol_), Van Driest(_dynamicKEqn_), smooth(_smooth_) 3가지를 선택할 수 있다. Cube-root Volume일 때는 위와 같으며, 나머지들의 딕셔너리는 다음과 같다.

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

DES 모델의 _simulationType_은 _LES_ 이다. 

RAS 모델, DES 옵션, Length-Scale 모델을 설정하고 그에 따른 딕셔너리가 필요하다.

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
RAS 모델은 _LESModel_ 에서 설정하고 Length-Scale 모델은 _delta_ 에서 설정한다.

##### RAS 모델

Spalart-Allmaras(_SpalartAllmarasDES_), k-omega SST(_kOmegaSSTDES_) 2가지를 선택할 수 있다. 

Spalart-Allmaras일 때 Low-Re Damping 옵션이 있다. 이 옵션을 사용할 때는 _SpalartAllmarasDESCoeffs_ 딕셔너리의 _lowReCorrection_ 이 _yes_가 되고 아닐 때는 _no_가 된다.

##### DES 옵션

DES 옵션의 Delayed DES를 켜면 Shielding Function 옵션이 나타나고 DDES와 IDDES를 선택할 수 있다. IDDES를 선택하면 Lengh-Scale 모델은 없어진다.

DDES를 선택하면 _LESModel_ 이 RAS 모델에 따라 _SpalartAllmarasDDES_, _kOmegaSSTDDES_ 가 된다.
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

IDDES를 선택하면 _LESModel_ 이 RAS 모델에 따라 _SpalartAllmarasIDDES_, _kOmegaSSTIDDES_ 가 된다.
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
##### Length-Scale 모델

Lengh-Scale 모델은 LES에서와 같다.
<!---------------------------------------------------------------------------------->
### 에너지(Energy)

에너지 방정식 계산 여부는 **system/fvSolution** 파일의 _SIMPLE_ 과 _PIMPLE_ 딕셔너리의 _solveEnergy_ 에 설정된다.

```
SiMPLE //PIMPLE
{
    ...
    solveEnergy     <yes or no>;
    ...
}
```
<!---------------------------------------------------------------------------------->
### 화학종 혼합(Species)

화학종 혼합은 **constant/thermophysicalProperties** 파일에 설정된다.

#### thermoType 딕셔너리

_mixture_ 가 다음과 같이 _multiComponentMixture_ 가 된다.

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

#### species 딕셔너리

계산에 사용될 화학종들을 써 준다. 이와 함께 _inertSpecie_ 를 지정해 주어야 한다. 이것은 수송방정식을 계산하지 않으며 질량분율은 1에서 나머지 화학종 값의 합을 뺀 것을 사용한다. 

```
species
(
    <species1>
    <species2>
    ...
);

inertSpecie     <specie1>
```

#### 각 화학종의 딕셔너리

각 화확종의 물성값을 설정하는 딕셔너리가 있어야 한다. _mixture_ 딕셔너리와 같은 형태인데 단지 _transport_ 에 확산계수 _Dm_ 이 있는 것만 다르다.

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
### 사용자 정의 스칼라(User-defined Scalars)

사용자 정의 스칼라는 **system/controlDict** 파일의 functions 딕셔너리에 설정된다.

확산 계수가 상수인 경우 입력한 값은 _D_ 에 사용되고, 층류 및 난류 확산계수인 경우는 _alphaD_ 와 _alphaDt_ 에 사용된다.

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

_schemesField_ 는 항상 _scalar_ 이다. 이것은 수치해석 기법을 **system/fvSolution**, **system/fvSchemes** 파일에서 설정할 때 scalar라는 이름의 값을 사용하는 것이다.

_writeInterval_ 은 계산조건에서 설정한 자동 저장 간격(Save Interval) 값을 사용한다.
<!---------------------------------------------------------------------------------->
## 물질(Materials)

물성값은 단상유동은 **constant/thermophysicalProperties** 파일에서, 다상유동은 **constant/transportProperties** 파일에서 설정한다. 비뉴턴유체의 점성 모델은 **constant/turbulenceProperties** 파일에서 설정한다.

### thermophysicalProperties

#### thermoType 딕셔너리

_thermoType_ 은 _mixture_, _transport_, _thermo_, _equationOfState_, _specie_, _energy_ 등의 6가지를 설정한다.

유체일 때
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
고체일 때
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

* type : 유체일 때는 항상 _heRhoThermo_ 를 사용하고, 고체일 때는 _heSolidThermo_ 를 사용한다. 

* mixture : 화학종 혼합을 계산하지 않을 때는 항상 _pureMixture_ 를 사용한다. 화학종 혼합을 계산할 때는 _multiComponentMixture_ 를 사용한다. 고체일 때는 항상 _pureMixture_ 를 사용한다.

* transport : 유체인 경우 점성계수와 열전도도가 상수일 때는 _const_ 를 사용하고, 다항식일 때는 _polynomial_ 을 사용한다. 고체인 경우 열전도도가 상수일 때는 _constIso_ 를 사용한다. 

* thermo : 정압비열이 상수일 때 _hConst_ 를 사용하고, 다항식일 때 _hPolynomial_ 을 사용한다.

* equationOfState : 밀도가 상수일 때 _rhoConst_ 를 사용하고, 완전기체일 때는 _perfectGas_ 를, 비압축성 완전기체일 때는 _incompressiblePerfectGas_ 를 사용한다. 다항식일 때는 _icoPolynomial_ 를 사용한다.

* specie : 항상 _specie_ 를 사용한다.

* energy : 항상 _sensibleEnthalpy_ 를 사용한다.

#### mixture 딕셔너리

화학종 혼합을 계산하지 않을 때 유체의 물성값은 _mixture_ 딕셔너리에서 설정한다. _mixture_ 딕셔너리에는 _thermodynamics_, _transport_, _specie_, _equationOfState_ 등의 딕셔너리가 있다. 사용 예는 다음과 같다.

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

_Cp_ 와 _Hf_ 를 설정한다. _Cp_ 는 정압비열이며 _Hf_ 는 생성열이다. BaramFlow에는 아직 화학반응이 포함되어 있지 않기 때문에 _Hf_ 는 사용되지 않으며 항상 0으로 설정된다.

정압비열이 상수일 때 다음과 같다.

```
thermodynamics
{
    Cp  <value>;
    Hf  0;
}
```

정압비열이 다항식일 때는 다음과 같다.

```
thermodynamics
{
    Hf          0;
    Sf          0;
    CpCoeffs<8> (<a0> <a1> <a2> ... <a7>)
}
```
_Sf_ 는 표준 엔트로피(standard entropy)이며 BaramFlow에는 아직 화학반응이 포함되어 있지 않기 때문에 _Sf_ 는 사용되지 않으며 항상 0으로 설정된다.


다항식은 아래의 식과 같이 7차식으로 표현하기 때문에 8개의 계수가 필요하다. 

$Cp = a_0 + a_1 T + a_2 T^2 + a_3 T^3 + a_4 T^4 + a_5 T^5 + a_6 T^6 + a_7 T^7$

##### transport

_mu_와 _Pr_을 설정한다. _mu_ 는 점성계수이며 _Pr_ 은  프란트 수(Prandtl Number)이다. 


Viscosity와 Thermal Conductivity가 상수일 때 다음과 같다.

```
transport
{
    mu  <value>;
    Pr  <value>;
}
```

점성계수와 열전도도가 다항식일 때는 다음과 같다.

```
transport
{
    muCoeffs<8>     (<a0> <a1> <a2> ... <a7>)
    kappaCoeffs<8>  (<a0> <a1> <a2> ... <a7>)
}
```
점성계수가 Sutherland일 때는 다음과 같다.
```
transport
{
    As  <value>;
    Ts  <value>;
}
```
_As_ 는 Sutherland 계수, _Ts_ 는 Sutherland 온도

##### specie

_specie_에는 물질의 분자량을 설정한다.
```
specie
{
    nMoles      1;
    molWeight   <value>;
}
```

##### equationOfState

밀도가 상수일 때 다음과 같다.
```
equationOfState
{
    rho <value>;
}
```

밀도가 다항식일 때는 다음과 같다.


```
equationOfState
{
    rhoCoeffs<8>     (<a0> <a1> <a2> ... <a7>)
}
```

밀도가 이상기체일 때는 equationOfState 딕셔너리가 필요 없다.

밀도가 비압축성 이상기체일 때는 다음과 같다.

```
equationOfState
{
    pRef    <value>;
}
```
[기준값]의 압력을 사용한다.
<!---------------------------------------------------------------------------------->
### transportProperties

다상유동 문제에서 각 상의 물성을 설정한다. _phases_ 라는 딕셔너리와 각 상에 대한 딕셔너리가 필요하며, 표면장력, 캐비테이션 등을 설정한다.

#### phases

상이 2개일 때 다음과 같다.

```
phases  (<phase1> <phase2>);
```

상이 3개 이상일 때는 다음과 같다.
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
#### 각 상의 물성값 

상이 2개일 때는 다음과 같다.
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

상이 3개 이상일 때는 _phases_ 딕셔너리에 설정하기 때문에 필요없다. 


#### 표면장력

상이 2개일 때 다음과 같다.

```
sigma   <value>;
```

상이 3개 이상일 때는 각 상간의 값을 다음과 같이 설정한다.
```
sigmas
(
    (<phase1> <phase2>) <value>
    (<phase1> <phase3>) <value>
    (<phase2> <phase3>) <value>
    ...
);
```

#### 캐비테이션

캐비테이션 문제에서는 증기압을 _pSat_ 에 설정하고, 캐비테이션 모델을 _phaseChangeTwoPhaseMixture_ 에 설정한다. 

```
pSat                        <value>;
phaseChangeTwoPhaseMixture  SchnerrSauer; //Kunz, Merkle, Zwart
```

각 모델의 계수는 별도의 딕셔너리로 다음과 같이 설정한다.

Schnerr-Sauer 모델
```
SchnerrSauerCoeffs
{
    n       <value>;
    dNuc    <value>;
    Cc      <value>;
    Cv      <value>;
}
```
Kunz, Merkle 모델
```
KunzCoeffs // MerkleCoeffs
{
    UInf    <value>;
    tInf    <value>;
    Cc      <value>;
    Cv      <value>;
}
```
Zwart-Gerber-Belamri 모델
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
## 셀존 조건(Cell Zone Conditions)

### 다중기준좌표계(Multiple Reference Frame, MRF)

다중기준좌표계는 **constant/MRFProperties**에서 다음과 같이 설정한다.

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

_omega_ 는 입력한 분당 회전수(RPM) 값을 초당 라디안(radian)으로 변환해서 사용한다.

### 미끄럼 격자(Sliding Mesh)

미끄럼 격자는 **constant/dynamicMeshDict** 파일에서 다음과 같이 설정한다.

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

_omega_ 는 입력한 분당 회전수(RPM) 값을 초당 라디안(radian)으로 변환해서 사용한다.

### 다공성 매질(Porous Zone)

다공성 매질은 **system/fvOptions** 파일에서 설정한다.

#### Power Law 모델

입력 받은 C0, C1은 _powerLawCoeffs_ 에 사용된다.

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
#### Darcy Forchheimer 모델

입력 받은 방향벡터 1과 2는 _coordinateSystem_ 의 _e1_, _e2_ 에 사용된다. 관성저항계수와 점성저항계수는 _DarcyForchheimerCoeffs_ 의 _f_ 와 _d_ 에 사용된다.


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
### 액추에이터 디스크(Actuator Disk)

액추에이터 디스크 모델은 **system/fvOptions** 파일에서 설정한다.

입력 받은 디스크 축 방향은 _diskDir_ 에, 파워 계수는 _Cp_ 에, 추력 계수는 _Ct_ 에  디스크 면적은 _diskArea_ 에, 상류쪽 점의 좌표는 _monitorCoeffs에, 힘 계산 방법은 _variant_ 에 사용된다. 

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
### 생성항(Source Terms)

질량, 에너지, 난류, 화학종 관련 필드에 생성항을 줄 수 있다. 

생성항은 **system/fvOptions** 파일에서 설정한다.

무엇에 대한 생성항인지는 _sources_에서 설정한다. 질량은 _rho_, 에너지는 _h_, 난류 관련 필드는 _k_, _epsilon_, _omega_ 등이다. 

값은 _sources_ 에서 (\<explicit value> \<implicit value>)와 같이 주는데 입력한 값은 explicit value에 쓰고 implicit value에는 0.0을 쓴다.

값의 크기 설정 방법(Specification Method)는 전체 체적에 대한 값(Value for Entire Cell Zone)과 단위 체적당 값(Value per Unit Volume) 두 가지가 있으며 _volumeMode_ 에서 설정한다. 전체 체적에 대한 값은 _absolute_, 단위 체적당 값은 _specific_ 이다.

셀존이 아닌 전체 영역에 생성항을 줄 때는 _selectionMode_ 가 _all_ 이 된다.


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
값의 변화 설정 방법이 조각별 선형 함수일 때
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
값의 변화 설정 방법이 다항식일 때
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

#### 사용자 정의 스칼라의 생성항

사용자 정의 스칼라의 생성항은 **system/fvOptions** 파일이 아닌 **system/controlDict** 파일의 _functions_ 딕셔너리에서 설정한다.

_functions_ 의 사용자 정의 스칼라 부분에 _fvOptions_  라는 딕셔너리로 다음과 같이 설정한다.

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

### 고정값(Fixed Values)

속도, 온도, 난류 관련 필드 등의 값을 **system/fvOptions** 파일에서 고정할 수 있다. 

#### 속도

_type_ 은 _meanVelocityForce_ 이다. 

입력한 속도는 _Ubar_ 에, 완화계수는 _relaxation_ 에 사용된다.

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

#### 온도 

_type_ 은 _fixedTemperatureConstraint_ 이다. 

입력한 온도는  _temperature_ 에 사용된다.
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

#### 난류, 화학종 

_type_ 은 _scalarFixedValueConstraint_ 이다. 

스칼라의 종류와 입력한 값은 _fixedValues_ 딕셔너리에 사용된다.
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

#### 사용자 정의 스칼라

사용자 정의 스칼라의 고정값은 **system/fvOptions** 파일이 아닌 **system/controlDict** 파일의 _functions_ 딕셔너리에서 설정한다.

_functions_ 의 사용자 정의 스칼라 부분에 _fvOptions_ 라는 딕셔너리로 다음과 같이 설정한다.

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
## 경계조건(Boundary Condition)

경계조건은 **0** 폴더 아래의 파일들에 설정된다.

### 입구속도(Velocity Inlet)

#### U

속도지정방법(Velocity Specification Method)이 경계면에 수직한 속도(Magnitude, Normal to Boundary)일 때
```
<boundayr name> 
{
    type     surfaceNormalFixedValue;
    refValue uniform -<value>;
}
```

속도지정방법(Velocity Specification Method)이 x, y, z 성분(Component)일 때
```
<boundayr name>
{
    type    fixedValue;
    value   uniform (<Ux> <Uy> <Uz>);
}
```
시간에 따른 속도분포를 사용할 때, 속도지정방법이 경계면에 수직한 속도이면
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
시간에 따른 속도분포를 사용할 때, 속도지정방법이 x, y, z 성분이면
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
속도의 공간분포를 사용할 때, x, y, z 좌표 파일은 **constant/boundaryData/\<patch name>/points\_U** 이고, Ux, Uy, Uz 파일은 **constant/boundaryData/\<patch name>/0/U** 이다.

```
<boundayr name> 
{
    type    timeVaryingMappedFixedValue;
    points  points_U;
}
```
constant/boundaryData/\<patch name>/points_U
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

상수일 때
```
<boundayr name>
{
    type    fixedValue;
    value   uniform <value>;
}
```
시간에 따른 변화에 조각별 선형함수(piecewise linear)를 사용할 때
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
시간에 따른 변화에 다항식(polynomial)을 사용할 때
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
지정방법이 K와 Epsilon일 때
```
<boundayr name>
{
    type        inletOutlet;
    inletValue  uniform <inlet value>;
    value       uniform <value>;
}
```
지정방법이 난류강도와 점도비율(Intensity and Viscosity Ratio)일 때
```
<boundayr name>
{
    type            turbulentIntensityInletOutletTKE;
    turbIntensity   uniform <value>;
    value           uniform <value>;
}
```
#### epsilon, omega
지정방법이 K와 Epsilon일 때
```
<boundayr name>
{
    type        inletOutlet;
    inletValue  uniform <inlet value>;
    value       uniform <value>;
}
```
지정방법이 난류강도와 점도비율(Intensity and Viscosity Ratio)일 때
```
<boundayr name>
{
    type            viscosityRatioInletOutletTDR;
    viscosityRatio  uniform <value>;
    value           uniform <value>;
}
```
#### nuTilda
지정방법이 보정 난류 점상계수(Modifed Turbulent Viscosity)일 때 
```
<boundayr name>
{
    type    fixedValue;
    value   uniform <value>;
}
```
지정방법이 난류 점도비율(Turbulent Viscosity Ratio)일 때
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
#### 체적분율, 화학종, 사용자 정의 스칼라
```
<boundayr name>
{
    type    fixedValue;
    value   uniform <value>;
}
```
<!---------------------------------------------------------------------------------->
### 입구 유량(Flow Rate Inlet)

#### U
유량 정의 방법이 체적 유량(Volume Flow Rate)일 때
```
<boundayr name>
{
    type                flowRateInletVelocity;
    volumetricFlowRate  <value>;
}
```
유량 정의 방법이 질량 유량(Mass Flow Rate)일 때
```
<boundayr name>
{
    type            flowRateInletVelocity;
    massFlowRate    <value>;
    rhoInlet        <value>;
}
```

압력, 온도, 난류의 설정은 입구 속도 조건일 때와 같다.
<!---------------------------------------------------------------------------------->
### 입구 전압력(Pressure Inlet)

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

온도, 난류의 설정은 입구 속도 조건일 때와 같다.
<!---------------------------------------------------------------------------------->
### 대기경계층 입구(ABL Inlet)

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
### 비압축성 자유류(Free Stream)

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
지정방법이 K와 Epsilon일 때
```
<boundayr name>
{
    type            freestream;
    freestreamValue uniform <value>;
}
```
지정방법이 난류강도와 점도비율(Intensity and Viscosity Ratio)일 때
```
<boundayr name>
{
    type            turbulentIntensityInletOutletTKE;
    turbIntensity   uniform <value>;
    value           uniform <value>;
}
```
#### epsilon, omega
지정방법이 K와 Epsilon일 때
```
<boundayr name>
{
    type            freestream;
    freestreamValue uniform <inlet value>;
}
```
지정방법이 난류강도와 점도비율(Intensity and Viscosity Ratio)일 때
```
<boundayr name>
{
    type            viscosityRatioInletOutletTDR;
    viscosityRatio  uniform <value>;
    value uniform   <value>;
}
```
#### nuTilda
지정방법이 보정 난류 점상계수(Modifed Turbulent Viscosity)일 때 
```
<boundayr name>
{
    type            freestream;
    freestreamValue uniform <inlet value>;
}
```
지정방법이 난류 점도비율(Turbulent Viscosity Ratio)일 때
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
### 개수로 입구(Open Channel Inlet)

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

난류의 설정은 입구 속도 조건일 때와 같다.
<!---------------------------------------------------------------------------------->
### 압축성 리만(Far-field Riemann)

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

난류의 설정은 입구 속도 조건일 때와 같다.
<!---------------------------------------------------------------------------------->
### 아음속 입구(Subsonic Inlet)

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

난류의 설정은 입구 속도 조건일 때와 같다.
<!---------------------------------------------------------------------------------->
### 초음속 입구(Supersonic Inflow)

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

난류의 설정은 입구 속도 조건일 때와 같다.
<!---------------------------------------------------------------------------------->
### 출구 압력(Pressure Outlet)

#### U
Non-reflecging Boundary 옵션을 사용하지 않을 때
```
<boundayr name>
{
    type    pressureInletOutletVelocity;
    value   uniform (<Ux> <Ux> <Ux>);
}
```
Non-reflecging Boundary 옵션을 사용할 때
```
<boundayr name>
{
    type    waveTransmissive;
    gamma   <value>;
}
```
#### p\_rgh
Non-reflecging Boundary 옵션을 사용하지 않을 때
```
<boundayr name>
{
    type    totalPressure;
    p0      uniform <value>; # value is static pressure
}
```
Non-reflecging Boundary 옵션을 사용할 때
```
<boundayr name>
{
    type    waveTransmissive;
    gamma   <value>;
}
```
#### T
유입류 조건(Specify Backflow Properties) 옵션을 사용하지 않을 때
```
<boundayr name>
{
    type    zeroGradient;
}
```
유입류 조건(Specify Backflow Properties) 옵션을 사용할 때
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
유입류 조건(Specify Backflow Properties) 옵션을 사용하지 않을 때
```
<boundayr name>
{
    type    zeroGradient;
}
```
유입류 조건(Specify Backflow Properties) 옵션을 사용하고, 지정방법이 K와 Epsilon일 때
```
<boundayr name>
{
    type        inletOutlet;
    inletValue  uniform <value>;
    value       uniform <value>;
}
```
유입류 조건(Specify Backflow Properties) 옵션을 사용하고, 지정방법이 난류강도와 점도비율(Intensity and Viscosity Ratio)일 때
```
<boundayr name>
{
    type            turbulentIntensityInletOutletTKE;
    turbIntensity   uniform <value>;
    value           uniform <value>;
}
```
#### epsilon, omega
유입류 조건(Specify Backflow Properties) 옵션을 사용하지 않을 때
```
<boundayr name>
{
    type    zeroGradient;
}
```
유입류 조건(Specify Backflow Properties) 옵션을 사용하고, 지정방법이 K와 Epsilon일 때
```
<boundayr name>
{
    type        inletOutlet;
    inletValue  uniform <value>;
    value       uniform <value>;
}
```
유입류 조건(Specify Backflow Properties) 옵션을 사용하고, 지정방법이 난류강도와 점도비율(Intensity and Viscosity Ratio)일 때
```
<boundayr name>
{
    type        viscosityRatioInletOutletTDR;
    inletValue  uniform <value>;
    value       uniform <value>;
}
```
#### nuTilda
유입류 조건(Specify Backflow Properties) 옵션을 사용하지 않을 때
```
<boundayr name>
{
    type    zeroGradient;
}
```
유입류 조건(Specify Backflow Properties) 옵션을 사용하고, 지정방법이 보정 난류 점상계수(Modifed Turbulent Viscosity)일 때 
```
<boundayr name>
{
    type        inletOutlet;
    inletValue  uniform <value>;
    value       uniform <value>;
}
```
유입류 조건(Specify Backflow Properties) 옵션을 사용하고, 지정방법이 난류 점도비율(Turbulent Viscosity Ratio)일 때
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
### 개수로 출구(Open Channel Outlet)

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
지정방법이 K와 Epsilon일 때
```
<boundayr name>
{
    type        inletOutlet;
    inletValue  uniform <value>;
    value       uniform <value>;
}
```
지정방법이 난류강도와 점도비율(Intensity and Viscosity Ratio)일 때
```
<boundayr name>
{
    type            turbulentIntensityInletOutletTKE;
    turbIntensity   uniform <value>;
    value           uniform <value>;
}
```
#### epsilon, omega
지정방법이 K와 Epsilon일 때
```
<boundayr name>
{
    type        inletOutlet;
    inletValue  uniform <value>;
    value       uniform <value>;
}
```
지정방법이 난류강도와 점도비율(Intensity and Viscosity Ratio)일 때
```
<boundayr name>
{
    type            viscosityRatioInletOutletTDR;
    viscosityRatio  uniform <value>;
    value           uniform <value>;
}
```
#### nuTilda
지정방법이 보정 난류 점상계수(Modifed Turbulent Viscosity)일 때 
```
<boundayr name>
{
    type        inletOutlet;
    inletValue  uniform <value>;
    value       uniform <value>;
}
```
지정방법이 난류 점도비율(Turbulent Viscosity Ratio)일 때
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
### 유출(Outflow)

#### U, p\_rgh, T, k, epsilon, omega, nuTilda
```
<boundayr name>
{
    type    zeroGradient;
}
```
<!---------------------------------------------------------------------------------->
### 아음속 출구(Subsonic Outflow)

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
### 초음속 출구(Supersonic Outflow)

#### U, p\_rgh, T, k, epsilon, omega, nuTilda
```
<boundayr name>
{
    type    zeroGradient;
}
```
<!---------------------------------------------------------------------------------->
### 벽면(Wall)

#### U
정지(No Slip)
```
<boundayr name>
{
    type    fixedValue;
    value   uniform (0 0 0)
}
```
미끄럼 벽(Slip)
```
<boundayr name>
{
    type    slip;
}
```
움직이는 벽(Moving Wall)
```
<boundayr name>
{
    type    movingWallVelocity;
    value   uniform (0 0 0)
}
```
대기경계층 지표면(Atmospheric Wall)
```
<boundayr name>
{
    type    fixedValue;
    value   uniform (0 0 0)
}
```
직선 속도(Translational Moving Wall)
```
<boundayr name>
{
    type    fixedValue;
    value   uniform (<Ux> <Uy> <Uz>)
}
```
회전 속도(Rotational Moving Wall)
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
단열(Adiabatic)
```
<boundayr name>
{
    type    zeroGradient;
}
```
일정 온도(Constant Temperature)
```
<boundayr name>
{
    type    fixedValue;
    value   uniform <value>;
}
```
일정 열유속(Constant Heat Flux)
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
외부로 대류열전달(Convection)
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
표준벽함수(Standard Wall Function)
```
<boundayr name>
{
    type    epsilonWallFunction;
    value   uniform <value>;
}
```
Two layer 벽함수
```
<boundayr name>
{
    type    epsilonBlendedWallFunction;
    value   uniform <value>;
}
```
대기경계층 지표면(Atmospheric Wall)
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
표준벽함수(Standard Wall Function)
```
<boundayr name>
{
    type    nutkWallFunction;
    value   uniform <value>;
}
```
Two layer 벽함수, SST k-omega 모델, Spalart-Allmaras 모델
```
<boundayr name>
{
    type    nutSpaldingWallFunction;
    value   uniform <value>;
}
```
대기경계층 지표면(Atmospheric Wall)
```
<boundayr name>
{
    type    atmNutkWallFunction;
    z0      <value>;
    value   uniform <value>;
}
```
#### alphat
단열(Adiabatic) 조건일 때
```
<boundayr name>
{
    type    compressible::alphatWallFunction;
    Prt     <value>;
    value   uniform <value>;
}
```
일정 온도, 일정 열유속, 외부로 대류열전달 조건일 때
```
<boundayr name>
{
    type    compressible::alphatJayatillekeWallFunction;
    Prt     <value>;
    value   uniform <value>;
}
```
<!---------------------------------------------------------------------------------->
### 연결 벽면(Thermo-Coupled Wall)

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
표준벽함수(Standard Wall Function)
```
<boundayr name>
{
    type    epsilonWallFunction;
    value   uniform <value>;
}
```
Two layer 벽함수
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
k-epsilon, 표준벽함수(Standard Wall Function)
```
<boundayr name>
{
    type    nutkWallFunction;
    value   uniform <value>;
}
```
k-omega, Two-layer 벽함수, Spalart-Allmaras
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
### 영역간 인터페이스(Region Interface)

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
### 2차원 경계(Empty)
```
<boundayr name>
{
    type    empty;
}
```
### 축대칭 경계(Wedge)
```
<boundayr name>
{
    type    wedge;
}
```
### 대칭(Symmetry)
```
<boundayr name>
{
    type    symmetry;
}
```
### 인터페이스(Interface)
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
### 다공성 압력 점프(Porous Jump)
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
### 팬(Fan)
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
## 수치해석 기법(Numerical Conditions)

수치해석 기법은 **system/fvSolution** 파일과 **system/fvSchemes** 파일에서 설정한다.

### 압력-속도 연성기법(Pressure-Velocity Coupling Scheme)

압력-속도 연성기법은 **system/fvSolution** 파일의 _SIMPLE_, _PIMPLE_ 딕셔너리의 _consistent_ 에 설정한다. 값이 SIMPLE 이면 _no_, SIMPLEC 이면 _yes_ 이다.

```
SIMPLE
{
    consistent      <yes or no>;
    ...
}
```

### 운동량방정식 계산(Momentum Predictor)

운동량방정식 계산은 **system/fvSolution** 파일 _PIMPLE_ 딕셔너리의 _momentumPredictor_ 에서 _on_ 혹은 _off_ 로 설정한다.

```
PIMPLE
{
    ...
    momentumPredictor   <on or off>;
    ...
}
```

### 플럭스 계산 기법(Flux Type)

플럭스 계산 기법(Flux Type)은 **system/fvSolution** 파일의 _Riemann_ 딕셔너리에 설정한다. _Riemann_ 은 밀도기반 솔버에서 사용되는 것으로 다음과 같다.

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
 _fluxScheme_ 은 Roe-FDS일 때는 _roeFlux_ 로, AUSM일 때는 _AUSMplusFlux_ 로, AUSM-up일 때는 _AUSMplusUpFlux_ 가 된다.

Entropy Fix Coefficient 값은 _roeFluxCoeffs_ 의 _epsilon_ 에 설정되고, Cut-off Mach Number 값은 _AUSMplusUpFluxCoeffs_ 의 _MInf_ 에 설정된다.

### 이산화 기법(Discretization Schemes)

#### 시간(Time)

**system/fvSchemes** 파일의 _ddtSchemes_ 딕셔너리에 설정된다.

1차 음해법(First Order Implicit)일 때 _Euler_, 2차 음해법(Second Order Implicit)일 때 _backward_ 가 된다.

```
ddtSchemes
{
    default     <Euler or backward>;
}
```
#### 압력(Pressure)

**system/fvSchemes** 파일의 _interpolationSchemes_ 에 설정된다.

Linear 이면 다음과 같다.
```
interpolationSchemes
{
    default             linear;
    interpolate(p)      linear;
    interpolate(p_rgh)  linear;
}
```
Momentum Weighted 면 다음과 같다.
```
interpolationSchemes
{
  default               linear;
  interpolate(p)        momentumWeighted;
  interpolate(p_rgh)    momentumWeighted;
}
```
Momentum Weighted Reconstruct 이면 다음과 같다.
```
interpolationSchemes
{
    default             linear;
    interpolate(p)      momentumWeightedReconstruct;
    interpolate(p_rgh)  momentumWeightedReconstruct;
}
```

#### 운동량(Momentum), 에너지(Energy), 난류(Turbulence), 체적분율(Volume Fraction)

**system/fvSchemes** 파일의 divSchemes 딕셔너리에 설정된다.

1차 상류기법(First Order Upwind)일 때는 다음과 같다.

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

2차 상류기법(Second Order Upwind)일 때는 다음과 같다.
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
#### 밀도기반 솔버

밀도기반 솔버에서는 유동(Flow)의 이산화 기법은 **system/fvSolution** 파일의 Riemann 딕셔너리에서 설정한다. 1차 상류기법(First Order Upwind)일 때는 _secondOrder_ 가 _no_, 2차 상류기법(Second Order Upwind)일 때는 _yes_ 가 된다.

```
Riemann
{
    ...
    secondOrder     <yes or no>;
    ...
}
```

난류에 대해서는 압력기반 솔버와 같다.

### 완화계수(Under-Relaxation Factors)

완화계수는 **system/fvSolution** 파일의 _relaxationFactors_ 딕셔너리에 설정된다.

완화계수에 입력한 값들이 다음과 같이 설정된다.

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

### 안정성 향상(Improve Stability)

안정성 향상 옵션은 **system/fvSchemes** 파일의 _laplacianSchemes_ 딕셔너리에 설정된다.

안정성 향상 옵션이 꺼져 있을 때 다음과 같다.

```
laplacianSchemes
{
    default     Gauss linear corrected;
}
```
안정성 향상 옵션이 켜져 있을 때 다음과 같다.

```
laplacianSchemes
{
    default     Gauss linear limited corrected 0.5;
}
```
밀도기반 솔버에서는 이 옵션이 적용되지 않고, 옵션이 꺼져 있을 때와 같은 설정을 사용한다.

### 시간당 반복계산 회수(Max Iterations per Time Step), 압력보정 회수(Number of Correctors)

**system/fvSolution** 파일의 _PIMPLE_ 딕셔너리에 설정된다.

시간당 반복계산 회수는 _nOuterCorrectors_, 압력보정 회수는 _nCorrectors_ 에 사용된다.

```
PIMPLE
{
    ...
    nCorrectors                 <value>;
    nOuterCorrectors            <value>;
    ...
}
```

### 다상유동(Multiphase)

다상유동은 **system/fvSolution** 파일의 _solvers_ 딕셔너리에 설정된다.
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
시간당 반복계산 회수(Max. Iteration per Time Step)는 _nAlphaSubCycles_ 값으로 설정되며, 보정 회수(Number of Correctors)는 _nAlphaCorr_ 값으로 설정된다.

MULES 기법(MULES Variant)이 Explicit이면 _MULESCorr_ 은 _no_ 가 되고 Semi-implicit이면 _yes_가 된다. 

상 경계면 압축계수(Phase Interface Compression Factor)는 _cAlpha_ 값으로 설정된다.

제한자에 대한 MULES 반복 회수(Number of MULES iterations over the limiter)는 _nLimiterIter_ 값으로 설정된다.

### 수렴 판정 기준(Convergence Criteria)

압력기반 솔버는 **system/fvSolution** 파일에 설정된다. _SIMPLE_ 과 _PIMPLE_ 딕셔너리의 _residualControl_ 을 사용한다.

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
비정상상태 문제에서는 _residualControl_ 에 _tolerance_ 와 _relTol_ 2가지가 있다. 설정한 Absolute 값은 _tolerance_ 에, Relative 값은 _relTol_ 에 사용된다.
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
밀도기반 솔버는 **system/fvSolution** 파일의 _LU-SGS_ 딕셔너리에 설정된다.

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

### 값의 제한(Limits)

**system/fvOptions** 파일에 _limitT_ 딕셔너리를 사용해서 온도의 최소/최대값을 제한한다.
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

### 방정식 선택(Equations)

**system/fvSolution** 파일에서 유동, 에너지, 화학종 방정식 각각에 대해 계산할 것인지를 선택한다. 

_SIMPLE_ 과 _PIMPLE_ 딕셔너리의 _solveFlow_, _solveEnergy_, _solveSpecies_ 를 사용한다.

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

에너지방정식은 점성가열, 운동에너지, 압력에 의한 일 등의 3가지를 계산할 것인지 선택할 수 있다. **constant/thermophysicalProperties** 파일에 다음과 같이 설정된다.
```
includeViscousDissipation   <true of false>;
includeKineticEnergy        <true of false>;
includePressureWork         <true of false>;
```


### 기타 디폴트 설정

#### fvSolution 파일의 SIMPLE
_nNonOrthogonalCorrectors_, _pRefCell_, _pRefValue_ 는 항상 0을 사용한다.

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
#### fvSolution 파일의 PIMPLE
_turbOnFinalIterOnly_ 는 항상 _false_ 이다. _nAlphaSpreadIter_ 와 _nAlphaSweepIter_ 은 항상 0이고, _rDeltaTSmoothingCoeff_ 와 _rDeltaTDampingCoeff_ 는 항상 0.5이다.

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
#### fvSolution 파일의 solvers

계산할 유동 변수에 대한 행렬 연산 방법을 설정한다. _solver_ 와 _preconditioner_ 는 변수에 따라 다르다. _tolerance_ 는 1e-16, _relative tolerance(relTol)_ 는 0.1, _minimum iteration(minIter)_ 는 1을, _maximum iteration(maxIter)_ 는 5를 사용한다. 다만 체적분율(alpha)의 경우 _tolerance_ 는 1e-8, _relTol_ 은 0, _minIter_ 는 1, _maxIter_ 는 10을 사용한다.

##### 압력

p\_rgh은 _PCG_ solver와 _GAMG_ preconditioner를 사용한다.
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
##### 속도, 난류, 화학종, 사용자 정의 스칼라

U, k, epsilon, omega, nuTilda, scalar, Yi 등은 _PBiCGStab_ solver와 _DILU_ preconditioner를 사용한다.
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
##### 에너지

h는 _PBiCGStab_ solver와 _GAMG_ preconditioner를 사용한다.
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
##### 밀도

rho는 _PCG_ solver와 _DIC_ preconditioner를 사용한다.
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
##### 체적 분율(alpha)

alpha는 _smoothSolver_ solver와 _symGaussSeidel_ smoother를 사용한다.

_icAlpha_ 는 0, _alphaApplyPrevCorr_ 는 _yes_ 로 설정된다.
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
#### fvSolution 파일의 fieldBounds

_fieldBounds_ 는 밀도기반 솔버에서 사용되는 값의 제한으로 항상 다음과 같다.
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
#### fvSchemes 파일의 gradSchemes

압력기반 솔버는 항상 _Gauss linear_ 를 사용한다. _momentumReconGrad_, _energyReconGrad_, _turbulenceReconGrad_ 등은 _divSchemes_에 사용하기 위한 정의이다.
```
gradSchemes
{
    default             Gauss linear;
    momentumReconGrad   VKLimited Gauss linear 1.0;
    energyReconGrad     VKLimited Gauss linear 1.0;
    turbulenceReconGrad VKLimited Gauss linear 1.0;
}
```
밀도기반 솔버는 다음과 같은 조건을 항상 사용한다.
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
#### fvSchemes 파일의 interpolationSchemes

밀도기반 솔버에서는 다음의 설정을 사용한다.
```
interpolationSchemes
{
    default             linear;
    interpolate(rho)    linearUpwind phi grad(rho);
}
```
#### fvSchemes 파일의 snGradSchemes

항상 다음의 설정을 사용한다.
```
snGradSchemes
{
    default     corrected;
}
```
<!---------------------------------------------------------------------------------->
## 모니터(Monitor)

모니터링은 **system/controlDict** 파일의 _functions_ 딕셔너리를 사용한다.

### 힘(Force)

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
저장 간격(Write Interval)은 _writeInterval_ 에 설정된다. 유동 방향(Flow Direction)의 항력 방향(Drag Direction)은 _dragDir_ 에, 양력 방향(Lift Direction)은 _liftDir_ 에, 회전중심(Center of Rotation)은 _CofR_ 에 설정된다. 경계면(Boundaries)에 선택된 것들은 _patches_ 에 설정된다.

### 점(Point)

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
저장 간격(Write Interval)은 _writeInterval_ 에, 유동 변수(Field)는 _fields_ 에, 좌표(Coordidate)는 _probeLocations_ 에 설정된다.

### 면(Surface)

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
저장 간격(Write Interval)은 _writeInterval_ 에, 유동 변수(Field Variable)은 _fields_ 에, 함수(Report Type)은 _operation_ 에, 면(Surface)는 _name_ 에 설정된다.

### 체적(Volume)

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
저장 간격(Write Interval)은 _writeInterval_ 에, 유동 변수(Field Variable)은 _fields_ 에, 함수(Report Type)은 _operation_ 에, 체적(Volume)은 _name_ 에 설정된다.


### 잔차(Residual)

잔차 데이터를 생성하고 저장하기 위해서 **system/controlDict** 파일의 _functions_ 딕셔너리를 사용한다. 

_fields_ 는 솔버에 따라 다르게 설정된다.

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
## 초기화(Initialization)

특정 부분을 초기화할 때 **system/setFields** 파일을 사용한다.

_defaultFieldValues_ 의 값은 전체 영역에 대해 설정한 값이며, _regions_ 의 값은 특정 영역에 대해 설정한 값이다.
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
다상유동의 체적분율(alpha)을 초기화할 때는 다음과 같이 모든 상을 값을 설정한다.
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
첫번째 상(Primary phase)의 값은 전체 값의 합이 1이 되도록 결정된다.

<!---------------------------------------------------------------------------------->
## 계산 조건(Run Conditions)

계산 조건은 **system/controlDict** 파일에 다음과 같이 설정된다.

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

* _endTime_ : 계산 회수(Number of Iteration) 혹은 종료 시간(End Time)의 값
* _deltaT_ : 시간 전진 간격(Time Step Size)의 값
* _writeInterval_ : 자동 저장 간격(Save Interval)의 값
* _purgeWrite_ : 가장 최근 파일만 저장(Retain Only the Most Recent Files)의 값
* _writeFormat_ : 데이터 저장 포맷(Data Write Format)의 값
* _writePrecision_ : 데이터 저장 유효숫자(Data Write Precision)의 값
* _timePrecision_ : 시간 저장 유효숫자(Time Precision)의 값
* _adjustTimeStep_ : 시간 전진 방법(Time Stepping Method)의 값
* _maxCo_ : Courant Number의 값
* _maxDi_ : Maximum Diffusion Number의 값
* _maxAlphaCo_ : Max Courant Number for VoF의 값














