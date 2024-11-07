# 압축성 아음속 유동

## 고속열차 공력해석 

[격자 파일 다운로드](https://drive.google.com/file/d/1eL9zqfmXct3zMtJIJUuoap27afeMkdq2/view?usp=sharing)

[계산 파일 다운로드](https://drive.google.com/file/d/1FnEN0tfTE5Pg2Co61oMx8VfsrBI1Oz7q/view?usp=sharing)

### 개요 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/train/intro.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/train/intro.png)

고속열차는 마하수가 0.3~0.4 범위의 아음속 압축성 유동 영역에서 주행한다. CFD에서 저속 유동에서는 SIMPLE 게열의 압력기반 솔버를, 고속유동에서는 밀도기반의 솔버를 많이 사용한다. Baram의 비압축성 솔버인 buoyantSimpleNFoam의 아음속 압축성 유동영역에서 솔버의 안정성을 검증하기 위한 에제이다.

차량 연결부, 대차(바퀴), 판토그라프를 단순화한 고속철도 모델을 사용하였으며, 열차의 속도는 400 km/h이다. 

약 200만개 정도의 격자로 300번 정도의 반복계산만에(8 코어 CPU에서 20분 정도에) 수렴된 결과를 얻을 수 있었다. OpenFOAM의 표준솔버인 buoyantSimpleFoam과 rhoSimpleFoam을 사용하여 해석한 결과와 비교했을 때 훨씬 뛰어난 수렴성을 보여준다.

계산 조건은 다음과 같다. 

+ 솔버 : buoyantSimpleNFoam 
+ 난류 모델 : $SST$ $k-\omega$ 
+ 밀도 : Perfect Gas
+ 점성 계수 : 1.79e-5 $kg/ms$
+ 유동 조건 : 400 $km/h$(111.11 $m/s$)

### 프로그램의 구동 및 격자

프로그램 실행 후 [새 작업(New Case)]를 선택한다. 시작 창에서 [솔버 유형(Solver Type)]은 [압력기반(Pressure-based)]를, [다상유동 모델(Multiphase Model)]은 [None]을 선택한다.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/train/mesh.png"><br>
</p>

격자는 주어진 polyMesh 폴더를 활용한다. 상단 탭에서 [파일(File)]-[격자 불러오기(Load Mesh)]-[OpenFOAM]을 순서대로 클릭하고 polyMesh 폴더를 선택한다. 

### 기본조건(General)

본 예제에서는 디폴트 조건을 사용한다.

### 모델(Models)

난류 모델은 $SST$ $k-\omega$ 모델을 사용한다.

[에너지(Energy)]를 [포함(include)]한다.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/train/tur.png"><br>
</p>

### 물질(Materials)

물성치는 다음과 같이 설정한다.

+ 밀도(Density) : 완전기체(Perfect Gas)
+ 정압비열(Specific heat) : 1006
+ 점성계수(Viscosity) : 1.79e-05
+ 열전도도(Thermal Conductivity) : 0.0245
+ 분자량(Molecular Weight) : 28.966

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/train/mat.png"><br>
</p>

### 경계조건(Boundary Conditions)

아래와 같이 경계면 타입과 경계값을 설정한다.

+ Hex6\_1\_xMin : 입구 속도(Velocity Inlet)
    + 속도 크기(Velocity Magnitude) : 111.11
    + 난류 강도(Turbulent Intensity) : 1
    + 난류 점도 비율(Turbulent Viscosity Ratio) : 100
    + 온도(Temperature) : 300

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/train/inletbc.png">
</p>

+ Hex6\_1\_xMax : 출구 압력(Pressure Outlet)
    + 압력(Pressure)  : 0

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/train/outletbc.png">
</p>

+ Hex6\_1\_zMin : 벽면(Wall)
    + 속도 조건(Velocity Condition) : 직선 속도(Translational Moving Wall), (111.11 0 0)

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/train/groundbc.png">
</p>

+ train\_surface\_0 : 벽면(Wall)

+ Hex6\_1\_yMin, Hex6\_1\_yMax, Hex6\_1\_zMax : 대칭(Symmetry)

### 기준값(Reference Values)

기준값은 공력계수를 계산할 때 사용된다. 이 문제는 실험 데이터가 있는 문제가 아니기 때문에 정확한 값을 입력할 필요는 없으며 속도를 111.11 로 설정한다.

### 수치해석 기법(Numerical Conditions)

다음과 같이 설정한다.

+ 압력-속도 연성기법(Pressure-Velocity coupling) : SIMPLEC

+ 이산화 기법(Discretization)
    + 압력 : Momentum Weighted Reconstruct
    + 운동량, 에너지, 난류 : 2차 상류기법(Second Order Wpwind)

+ 완화계수(Relaxation factors)
    + 압력 : 0.8
    + 운동량, 에너지, 난류 : 0.9
    + 밀도 : 1.0

+ 수렴 판정 기준(Convergence criteria)
    + 압력, 운동량, 난류 : 1e-4
    + 에너지 : 1e-6

### 모니터(Monitor)

본 예제에서는 차량의 공력 계수를 모니터링한다. [추가(Add)]-[힘(Forces)]를 선택한다. 모두 디폴트 설정을 사용하고 [경계면(Boundaries)]에 train\_surface\_0를 선택한다.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/train/monitor.png"><br>
</p>

### 초기화(Initialization)

초기조건은 다음과 같이 설정한다.

+ 속도 : (111.11 0 0)
+ 압력 : 0
+ 온도 : 300
+ 속도 크기 : 111.11 
+ 난류 강도 : 1
+ 난류 점도 비율 : 10

값을 입력하고 하단의 [초기화(Initialize)] 버튼을 클릭한다. 그 후, 메뉴의 [파일(File)]-[저장(Save)] 버튼을 클릭하여 저장한다.

### 계산

메뉴의 [병렬연산(Parallel)]-[환경설정(Environment)]를 클릭하고 원하는 코어수를 입력한다. 

[계산 조건(Run Conditions)]은 다음과 같이 설정하고 [계산시작(Start Calculation)] 버튼을 누르면 계산이 시작된다.

+ 계산 회수(Number of Iterations) : 1000
+ 자동 저장 간격(Save Interval) : 1000

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/train/run.png"><br>
</p>

계산이 시작되면 아래와 같이 잔차(residual)과 유체력 계수의 그래프가 그려진다.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/train/residual.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/train/residual.png)


[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/train/force.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/train/force.png)


### 후처리

메뉴에서 [외부 프로그램(External tools)]-[ParaView] 버튼을 눌러 ParaView를 실행한다.

병렬연산이면 [Case Type]을 [Decomposed Case]로 변경한다.

차량 주변 압력 분포를 그리기 위해 [Mesh Regions]에서 train\_surface\_0, ground, symmetry를 선택하고 [Coloring]을 [p\_rgh]로 선택한다.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/train/contour.png"><br>
</p>
<!------------------------------------------------------------------------------------------------------------->
## 축대칭 아음속 제트 

[격자 파일 다운로드](https://drive.google.com/file/d/1fq6KLFt2mpidQchHwQ9j9mRgty5rlr3G/view?usp=sharing)

[계산 파일 다운로드](https://drive.google.com/file/d/1I2zMwTUwXwLbYIoBFZ4UDt8m86tWS9Hr/view?usp=sharing)

### 개요 

|[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/jet/nozzle3d.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/jet/nozzle3d.png){:target="_blank"}|

|[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/jet/y0.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/jet/y0.png){:target="_blank"}|

노즐 입구의 온도는 260$^o C$, 노즐 목에서의 마하수는 0.376 정도인 고온 아음속 노즐 유동해석 예제이다.

NASA Langley Research Center에서 제공하는 형상 및 실험조건을 사용한다.

[https://turbmodels.larc.nasa.gov/jetsubsonichot_val.html](https://turbmodels.larc.nasa.gov/jetsubsonichot_val.html)

NASA ARN2(Acoustic Research Nozzle 2)의 실험 결과이며 형상과 조건은 다음과 같다.

* 노즐목의 반지름 : 1 inch
* 압력비, $p/p{ref}$ = 1.10203, $p{ref}$ = 14.3 psi
* 온도비, $T/T{ref}$ = 1.81388, $T{ref}$ = 530 R
* 노즐 출구 마하수 : 0.376

실험결과와 함께 NASA WIND 코드의 $SST$ k-$omega$ 모델과 Spalart-Allmaras 모델 계산 결과를 제공하고 있다.

+ 솔버 : buoyantSimpleNFoam 
+ 난류 모델 : $standard$ $k-\epsilon$ 
+ 밀도 : Perfect Gas
+ 점성 계수와 열전도도 : Sutherland law 
+ 노즐 입구 조건 : 10059.65 Pa, 534.086 K
 

### 프로그램의 구동 및 격자

프로그램 실행 후 [새 작업(New Case)]를 선택한다. 시작 창에서 [솔버 유형(Solver Type)]은 [압력기반(Pressure-based)]를, [다상유동 모델(Multiphase Model)]은 [None]을 선택한다.

격자는 주어진 polyMesh 폴더를 활용한다. 상단 탭에서 [파일(File)]-[격자 불러오기(Load Mesh)]-[OpenFOAM]을 순서대로 클릭하고 polyMesh 폴더를 선택한다. 

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/jet/nozzleMesh.png"><br>
</p>

### 기본조건(General)

[시간(Time)]은 [정상상태(Steady)]를, [중력(Gravity)]는 (0 0 0)을 사용한다.

[작동압력(Operating Pressure)]는 98595.03을 입력한다.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/jet/general.png"><br>
</p>

### 모델(Models)

난류 모델은 $standard$ $k-\epsilon$ 모델을 사용한다.

[에너지(Energy)]를 [포함(include)]한다.


### 물질(Materials)

물성치는 다음과 같이 설정한다.

+ 밀도(Density) : 완전기체(Perfect Gas)
+ 정압비열(Specific heat) : 1006
+ 점성계수(Viscosity) : Sutherland, As = 1.46e-6, Ts = 110.4
+ 분자량(Molecular Weight) : 28.966

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/jet/mat.png"><br>
</p>

### 경계조건(Boundary Conditions)

아래와 같이 경계면 타입과 경계값을 설정한다.

+ inlet : 입구 압력(Pressure Inlet)
    + 전압력(Total Pressure) : 10059.65
    + 난류 강도(Turbulent Intensity) : 1
    + 난류 점도 비율(Turbulent Viscosity Ratio) : 10
    + 온도(Temperature) : 534.086

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/jet/bc-inlet.png">
</p>

+ outlet : 출구 압력(Pressure Outlet)
    + 전압력(Total Pressure) : 0
    + 유입류 조건(Specify Backflow Properties) : on
        + 유입류의 전온도(Backflow Total Temperature) : 294.4444
        + 난류 강도(Turbulent Intensity) : 1
        + 난류 점도 비율(Turbulent Viscosity ratio) : 10

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/jet/bc-outlet.png">
</p>

+ farfield : 입구 속도(Velocity Inlet)
    + 속도 지정 방법(Velocity Specification Method) : x y z 성분(Component), (3.44 0 0)
    + 난류 강도(Turbulent Intensity) : 1
    + 난류 점도 비율(Turbulent Viscosity ratio) : 10
    + 온도(Temperature) : 294.4444

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/jet/bc-far.png">
</p>

+ nozzle : 벽면(Wall)
    + 속도 조건 : 정지(No Slip)
    + 온도 조건 : 단열(Adiabatic)

+ frontAndBackPlanes\_pos, frontAndBackPlanes\_neg : 축대칭 경계(Wedge)


### 수치해석 기법(Numerical Conditions)

다음과 같이 설정한다.

+ 압력-속도 연성기법(Pressure-Velocity coupling) : SIMPLEC

+ 이산화 기법(Discretization)
    + 압력 : Momentum Weighted Reconstruct
    + 운동량, 에너지, 난류 : 2차 상류기법(Second Order Wpwind)

+ 완화계수(Relaxation factors)
    + 압력 : 0.1
    + 운동량 : 0.3
    + 에너지 : 0.9
    + 난류 : 0.2

+ 수렴 판정 기준(Convergence criteria)
    + 압력 : 0
    + 운동량, 에너지, 난류 : 0.001

+ 고급설정(Advanced)
    + 최소 온도(Minimum Static Temperature) : 100
    + 최대 온도(Maximum Static Temperature) : 1000

### 모니터(Monitor)

본 예제에서는 축에서 x/D가 20인 점의 축방향 속도를 모니터링한다. [추가(Add)]-[점(Points)]를 선택한다. 

[유동 변수(Field)]는 [X-Velocity]를 [좌표(Coordinate)]는 (1.016 0 0)을 입력한다.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/jet/monitor.png"><br>
</p>

### 초기화(Initialization)

초기조건은 다음과 같이 설정한다.

+ 속도 : (3.44 0 0)
+ 압력 : 0
+ 온도 : 294.4444
+ 속도 크기 : 100 
+ 난류 강도 : 1
+ 난류 점도 비율 : 200

값을 입력하고 하단의 [초기화(Initialize)] 버튼을 클릭한다. 그 후, 메뉴의 [파일(File)]-[저장(Save)] 버튼을 클릭하여 저장한다.

### 계산

메뉴의 [병렬연산(Parallel)]-[환경설정(Environment)]를 클릭하고 원하는 코어수를 입력한다. 

[계산 조건(Run Conditions)]은 다음과 같이 설정하고 [계산시작(Start Calculation)] 버튼을 누르면 계산이 시작된다.

+ 계산 회수(Number of Iterations) : 20000
+ 자동 저장 간격(Save Interval) : 1000


<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/jet/run.png"><br>
</p>

계산이 시작되면 아래와 같이 잔차(residual)과 모니터링 그래프가 그려진다.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/jet/residual.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/jet/residual.png)


[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/jet/point.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/jet/point.png)


### 후처리

메뉴에서 [외부 프로그램(External tools)]-[ParaView] 버튼을 눌러 ParaView를 실행한다.

병렬연산이면 [Case Type]을 [Decomposed Case]로 변경한다.

[Coloring]을 [U]로 선택한다.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/jet/contour.png"><br>
</p>

<!------------------------------------------------------------------------------------------------------------->
## 아음속 공동(Cavity) 유동  

[격자 파일 다운로드](https://drive.google.com/file/d/1u__XyUIi_xiL5-LmuT9OaBNda7s7qpDk/view?usp=sharing)

[계산 파일 다운로드](https://drive.google.com/file/d/1rfwtg18SiB2Bh50VE4vnXEYeOg0U0qMs/view?usp=sharing)

### 개요 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavity/2d-pressureContour.png)](https://youtu.be/NI_MVr7M7dI){:target="_blank"}


아음속 2차원 공동(cavity) 유동을 계산하는 예제이다. 

형상과 유동 조건은 다음과 같다.

+ L/D = 2 (Cavity의 길이와 높이의 비율)
+ 마하수 = 0.6
+ 레이놀즈 수 = 2.75e+5
+ 프란틀 수 = 0.7

격자는 matlab으로 만든 plot3d 형식의 격자를 변환하였다. 

솔버는 넥스트폼이 개발한 압력기반 열유동 해석 솔버인 buoyantPimpleNFoam을 사용한다.

출구와 상면의 경계조건은 압력파의 반사가 없는 waveTransmissive(non-reflecting) 조건을 사용한다.

계산 조건은 다음과 같다. 

+ 솔버 : buoyantPimpleNFoam 
+ 난류 모델 : $SST$ $k-\omega$ 모델
+ 밀도 : Perfect Gas
+ 입구 온도 : 300 K
+ 입구 압력 : 101325 Pa

### 프로그램의 구동 및 격자

프로그램 실행 후 [새 작업(New Case)]를 선택한다. 시작 창에서 [솔버 유형(Solver Type)]은 [압력기반(Pressure-based)]를, [다상유동 모델(Multiphase Model)]은 [None]을 선택한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/launcher.png"><br>
</p>

격자는 주어진 polyMesh 폴더를 활용한다. 상단 탭에서 [파일(File)]-[격자 불러오기(Load Mesh)]-[OpenFOAM]을 순서대로 클릭하고 polyMesh 폴더를 선택한다. 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavity/2d-mesh.png "include surface's own gap")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavity/2d-mesh.png)

### 기본조건(General)

본 예제에서는 비정상상태(Transient) 조건을 사용한다. 중력은 고려하지 않고 작동압력은 101325를 사용한다. 

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavity/2d-general.png"><br>
</p>

### 모델(Models)

난류 모델은 $SST$ $k-\omega$ 모델을 사용한다.

[에너지(Energy)]를 [포함(include)]한다.

### 물질(Materials)

물성치는 다음과 같이 설정한다.

+ 밀도(Density) : 완전기체(Perfect Gas)
+ 정압비열(Specific heat) : 1006
+ 점성계수(Viscosity) : 0.00178 (레이놀즈 수를 맞추기 위한 값)
+ 열전도도(Thermal Conductivity) : 2.562 (프란틀 수를 맞추기 위한 값)
+ 분자량(Molecular Weight) : 28.966

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavity/2d-material.png"><br>
</p>

### 경계조건(Boundary Conditions)

아래와 같이 경계면 타입과 경계값을 설정한다.

+ inlet : 입구 속도(Velocity Inlet)
    + 속도 크기(Velocity Magnitude) : 208.31
    + 난류 강도(Turbulent Intensity) : 1
    + 난류 점도 비율(Turbulent Viscosity ratio) : 10
    + 온도(Temperature) : 300

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavity/2d-inletbc.png">
</p>

+ outlet, top : 출구 압력(Pressure Outlet)
    + 압력(Pressure)  : 0
    + Non-Reflecting Boundary 옵션 사용

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavity/2d-outletbc.png">
</p>

+ cavityFront, cavityBottom, cavityRear, frontBottom, rearBottom : 벽면(Wall)
    + 속도 조건(Velocity Condition) : 정지(No Slip)

+ frontPlane, backPlane : 2차원 경계(Empty)


### 수치해석 기법(Numerical Conditions)

다음과 같이 설정한다.

+ [압력-속도 연성기법(Pressure-Velocity coupling)] : SIMPLE

+ [이산화 기법(Discretization)]
    + 시간 : 2차 음해법(Second Order Implicit)
    + 압력 : Momentum Weighted Reconstruct
    + 운동량, 에너지, 난류 : 2차 상류기법(Second Order Wpwind)

+ 시간당 반복계산 회수(Max iterations per Time Step) : 20

+ 압력보정 회수(Number of Correctors) : 2

+ [완화계수(Relaxation factors)]
    + 압력 : 0.3 / 1
    + 운동량, 난류 : 0.7 / 1
    + 에너지, 밀도 : 1 / 1


### 모니터(Monitor)

본 예제에서는 공동(cavity) 중앙의 점에서 압력을 모니터링한다. [추가(Add)]-[점(Point)]를 선택한다. 

좌표는 (0 -0.5 0.25)를 입력한다.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavity/2d-monitor.png"><br>
</p>

### 초기화(Initialization)

초기조건은 다음과 같이 설정한다.

+ 속도 : (208.31 0 0)
+ 압력 : 0
+ 온도 : 300
+ 속도 크기(Scale of velocity) : 208.31
+ 난류 강도(Turbulent Intensity) : 1
+ 난류 점도 비율(Turbulent Viscosity ratio) : 10

공동 내부의 속도를 0으로 초기화한다. [추가설정(Advanced)]-[영역(Sections)]에서 [만들기(Create)]-[육면체(Hex)]를 선택하고 최소/최대 좌표에 (-1 -1 -1), (1 0 1)을 입력한다. [속도(Velcity)]를 선택하고 값은 (0 0 0)을 준다.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavity/2d-setField.png"><br>
</p>

하단의 [초기화(Initialize)] 버튼을 클릭한다. 그 후, 메뉴의 [파일(File)]-[저장(Save)] 버튼을 클릭하여 저장한다.

### 계산

메뉴의 [병렬연산(Parallel)]-[환경설정(Environment)]를 클릭하고 원하는 코어수를 입력한다. 

[계산 조건(Run Conditions)]은 다음과 같이 설정하고 [계산시작(Start Calculation)] 버튼을 누르면 계산이 시작된다.

+ 시간 전진 기법(Time Stepping Method) : 일정(Fixed)
+ 시간 전진 간격(Time Step Size) : 1e-5
+ 종료 시간(End Time) : 0.5
+ 자동 저장 간격(Save Interval) : 0.002 sec

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavity/2d-run.png"><br>
</p>


### 후처리

메뉴에서 [외부 프로그램(External tools)]-[ParaView] 버튼을 눌러 ParaView를 실행한다.

병렬연산이면 [Case Type]을 [Decomposed Case]로 변경한다.

[Coloring]을 [U]로 선택하면 다음과 같은 그림을 확인할 수 있다.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavity/2d-contour.png"><br>
</p>

