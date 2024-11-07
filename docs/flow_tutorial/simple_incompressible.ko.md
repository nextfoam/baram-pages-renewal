# 간단한 비압축성 유동 예제

## 관내 혼합(Mixing Pipe) 

[격자 파일 다운로드](https://drive.google.com/file/d/12CyYZO_FSl7Baz-MmbtXnBst02EGg5Tg/view?usp=sharing)

[계산 파일 다운로드](https://drive.google.com/file/d/19ge2_LwDOPlN2tYopBoOD5OGSadkzo65/view?usp=sharing)

### 개요 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/1.1.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/1.1.png)


본 예제는 정상상태 비압축성 유동해석 예제이다. 2개의 입구와 1개의 출구로 이루어진 원형 파이프 내부 유동의 혼합을 예측한다.

계산 조건은 다음과 같다. 

+ 솔버 : buoyantSimpleNFoam 
+ 난류 모델 : Standard $k-\epsilon$ model
+ 밀도 : 1.225 $kg/m^3$
+ 점성 계수 : 1.79e-5 $kg/ms$
+ 유동 조건 : 면적이 넓은 입구(in-1)의 속도는 5m/s, 면적이 좁은 입구(in-2)의 속도는 10m/s, outlet은 대기압 조건

### 프로그램의 구동

프로그램 실행 후 [새 작업(New Case)]를 선택한다. 시작 창에서 [솔버 유형(Solver Type)]은 [압력기반(Pressure-based)]를, [다상유동 모델(Multiphase Model)]은 [None]을 선택한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/launcher.png"><br>
</p>

### 격자

격자는 주어진 polyMesh 폴더를 활용한다. 상단 탭에서 [파일(File)]-[격자 불러오기(Load Mesh)]-[OpenFOAM]을 순서대로 클릭하고 polyMesh 폴더를 선택한다. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/1.2.png"><br>
</p>

_※ 주의 사항 : OpenFOAM 격자를 읽을 때는 "polyMesh" 혹은 "constant" 폴더를 선택한다. OpenFOAM의 격자는 영역(region)이 하나인 경우 constant 폴더 아래의 polyMesh라는 폴더이며, 영역이 여러 개인 다중영역(multi-region)일 때는 여러 개의 polyMesh 폴더가 constant 폴더 아래의 영역 이름 폴더에 있다._


### 기본조건(General)

기본조건에서는 [시간(Time)], [중력(Gravity)], [작동압력(Operating Pressure)] 등을 설정할 수 있다. 본 예제에서는 디폴트 조건을 사용한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/1.3.png"><br>
</p>

### 모델(Models)

모델에서는 [난류(turbulence)], [에너지(Energy)], [화학종(Species)], [사용자 정의 스칼라(User-defined Scalar)]를 설정할 수 있다. 다상유동 모델과 솔버 유형은 시작 창에서 설정한다.

본 예제에서는 Standard $k-\epsilon$ 모델을 사용한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/1.4.png"><br>
</p>

### 물질(Materials)

물질에서는 작동 유체의 물성치를 설정할 수 있다. 지금 예제에서는 공기의 물성치를 그대로 사용한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/1.5.png"><br>
</p>

### 셀존 조건(Cell Zone Conditions)

셀존 조건에서는 [생성항(Source)], [다중기준좌표계(MRF)], [미끄럼 격자(Sliding Mesh)] 등을 설정할 수 있다. 본 예제에서는 디폴트 조건을 사용한다. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/1.6.png"><br>
</p>

### 경계조건(Boundary Conditions)

여러 경계면의 조건을 설정할 수 있다. 각 경계면을 선택하면 해당 경계면이 붉은색으로 변한다. 

경계면을 마우스 오른쪽 버튼으로 클릭하면 경계면 타입을 변경할 수 있고, 더블 클릭하거나 아래의 [편집(Edit)] 버튼을 누르면 값을 설정할 수 있는 창이 열린다.

각 경계조건은 다음과 같이 설정한다.

+ in-1 : 입구 속도(Velocity Inlet)
    + 속도 크기(Velocity Magnitude) : 5 (m/s)
    + 난류 강도(Turbulent Intensity) : 1 (%)
    + 난류 점도 비율(Turbulent Viscosity Ratio) : 10
      
+ in-2 : 입구 속도(Velocity Inlet)
    + 속도 크기(Velocity Magnitude) : 10 (m/s)
    + 난류 강도(Turbulent Intensity) : 1 (%)
    + 난류 점도 비율(Turbulent Viscosity Ratio) : 10
      
+ out : 출구 압력(Pressure Outlet)
    + 압력(Pressure) : 0 (Pa)
      
+ wall
    + 속도 조건(Velocity Condition) : 정지(No Slip)

### 수치해석 기법(Numerical Conditions)

[이산화 기법(Discretization)], [완화계수(Relaxation factors)], [수렴 판정 기준(Convergence criteria)], [압력-속도 연성기법(Pressure-Velocity coupling)] 등의 항목을 설정할 수 있다.

본 예제에서는 아래와 같이 설정을 변경한다.

+ 압력-속도 연성기법(Pressure-Velocity Coupling Scheme) : SIMPLEC

+ 이산화 기법(Discretization Scheme)
    + 압력 : Momentum Weighted Reconstruct
    + 운동량 : 2차 상류기법(Second Order Upwind)
    + 난류 : 2차 상류기법(Second Order Upwind)

+ 완화계수(Under-Relaxation Factors)
    + 압력, 운동량, 난류 : 0.9

+ 수렴 판정 기준(Convergence Criteria)
    + 압력 : 0.0001
    + 운동량 : 0.001
    + 난류 : 0.001

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/1.7.1.png"><br>
</p>


### 모니터(Monitor)

본 예제에서는 (0, 0, 1) 위치에서 압력을 모니터링 한다.

창 하단의 [추가(Add)]-[점(Points)]를 클릭해서 아래 그림과 같이 설정한다.

+ 저장 간격(Write Interval) : 1
+ 유동 변수(Field) : Pressure
+ 좌표(Coordinate) : (0, 0, 1)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/1.8.png"><br>
</p>

### 초기화(Initialization)

초기값으로 x, y, z 방향의 속도와 압력을 입력할 수 있다.

난류 모델을 사용할 경우 [속도 크기(Velocity Scale)], [난류 강도(Turbulent Intensity)], [난류 점도 비율(Viscosity Ratio)] 값을 입력하면 $k$와 $\epsilon$ 값이 계산되어 사용된다. 

+ 속도(Velocity)
    + X-속도 : 0 (m/s)
    + Y-속도 : 0 (m/s)
    + Z-속도 : 0 (m/s)

+ 압력(Pressure)
    + 0 (Pa)

+ 난류(Turbulence)
    + 속도 크기 : 5 (m/s)
    + 난류 강도 : 1 (%)
    + 난류 점도 비율 : 10

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/1.9.png"><br>
</p>

값을 입력하고 하단의 [초기화(Initialize)] 버튼을 클릭한다. 그 후, 메뉴의 [파일(File)]-[저장(Save)] 버튼을 클릭하여 저장한다.

### 계산

[계산 조건(Run Conditions)]에서는 [계산회수(Number of Iterations)], [자동 저장 간격(Save Interval)], [병렬연산(Parallel)] 등을 설정한다.

본 예제에서는 모두 디폴트 조건을 사용한다. 

이후, [실행(Run)]-[계산시작(Start Calculation)] 버튼을 누르면 계산이 시작된다.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/residual.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/residual.png)
<p align='center'>잔차(residual)와 모니터링 그래프</p>

### 후처리

BARAM은 ParaView를 이용한다. [외부 프로그램(External tools)]-[ParaView] 버튼을 누르면 ParaView가 구동된다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/1.10.png"><br>
</p>

ParaView를 초기 실행 시, 필요한 기능에 대한 설명은 다음과 같다.

+ Skip Zero Time : 초기값을 제외한 결과를 보여준다.

+ Case Type : cpu 개수에 따른 설정이다.
    + Reconstructed Case : 1core 계산 혹은 Parallel로 계산을 진행했지만 reconstructPar를 진행한 OpenFOAM case
    + Decomposed Case : Parallel 계산을 진행한 case

+ Mesh Regions : 보고 싶은 Internal mesh, 경계면 등을 설정할 수 있다.

+ Cell Arrays : 보고 싶은 물리량을 설정할 수 있다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/1.11.png"><br>
</p>

#### 경계면 스칼라 분포

벽면에 걸리는 압력 분포를 그려본다. 초기 설정을 아래와 같이 해준다.

+ Skip Zero Time : 비활성화
+ Mesh Regions : internalMesh - 활성화
+ 나머지 : Default

$p_rgh$는 압력에서 중력에 의한 항($\rho gh$)을 뺀 값으로 이 문제와 같이 중력을 고려하지 않은 경우는 압력과 같은 값이다. $p_rgh$는 operating pressure 기준의 상대압이고 $p$는 절대압력이다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/1.12.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/1.13.png"><br>
</p>


#### 축단면 스칼라 분포

파이프 내부의 압력 분포를 확인해본다.

[slice] 버튼을 클릭하고 방향을 Y-normal로 바꿔서 파이프 내부 압력을 확인한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/1.14.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/1.15.png"><br>
</p>

<!------------------------------------------------------------------------------------------------------------->
## 외팔보(Cantilever Beam)

[격자 파일 다운로드](https://drive.google.com/file/d/1bk5svPtGY4So9_1C18lgWcAy6lOsb524/view?usp=sharing)

### 개요 

본 예제는 정상상태 비압축성 유동해석 예제이다. 입구 영역에서 유입되는 공기에 의해 외팔보(Cantilever Beam)가 변형되는 정도를 확인하는 1-way FSI(Fluid Structure Interaction) 예제이다.

외팔보는 두께 5mm, 길이 50mm, 높이 150mm이다. 절반만 모델링하여 대칭(Symmetry) 경계 조건을 부여한다.

아래 그림에서 형상과 격자를 나타내었다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/1.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/2.png"><br>
</p>

계산 조건은 다음과 같다. 

+ 솔버 : buoyantSimpleNFoam 
+ 난류 모델 : $Standard$ $k-\epsilon$
+ 밀도 : 1.225 $kg/m^3$
+ 점성 계수 : 1.79e-5 $kg/ms$
+ 유동 조건 : inlet에서 80 $m/s$

### 프로그램의 구동 및 격자

프로그램 실행 후 [새 계산(New Case)]를 선택한다. 시작 창에서 [솔버 유형(Solver Type)]은 [압력기반(Pressure-based)]를, [다상유동 모델(Multiphase Model)]은 [None]을 선택한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/launcher.png"><br>
</p>

격자는 주어진 polyMesh 폴더를 활용한다. 상단 탭에서 [파일(File)]-[격자 불러오기(Load Mesh)]-[OpenFOAM]을 순서대로 클릭하고 polyMesh 폴더를 선택한다. 

### 기본조건(General)

본 예제에서는 디폴트 조건을 사용한다.

### 모델(Models)

본 예제에서는 $Standard$ $k-\epsilon$ 난류 모델을 사용한다. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/3.png"><br>
</p>

### 물질(Materials)

본 예제에서는 공기의 물성값을 사용한다. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/4.png"><br>
</p>

### 경계조건(Boundary Conditions)

아래와 같이 경계면 타입과 경계값을 설정한다.

+ Hex6_1_xMin : velocity Inlet
    + Velocity Specfication Method : Magnitude, Normal to Boundary
    + Profile Type : Constant
    + Velocity Magnitude : 80 (m/s)
    + Turbulent Intensity : 1 (%)
    + Turbulent Viscosity Ratio : 10

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/5.png"><br>
</p>

+ Hex6_1_zMax : velocity Inlet
    + Velocity Specfication Method : Component
    + Profile Type : Constant
    + X-Velocity : 80 (m/s)
    + Y-Velocity : 0 (m/s)
    + Z-Velocity : 0 (m/s)
    + Turbulent Intensity : 1 (%)
    + Turbulent Viscosity Ratio : 10

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/Hex6_1_zMax.png"><br>
</p>

+ Hex6_1_xMax : Pressure Outlet
    + Total Pressure : 0 (Pa)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/6.png"><br>
</p>

+ Hex6_1_yMin, Hex6_1_yMax : Symemtry

+ cantileverBeam_surface_0, Hex6_1_zMin : Wall
    + Velocity Condition : No Slip


### 수치해석 기법(Numerical Conditions)

본 예제에서는 아래와 같이 설정을 변경한다. 

+ 압력-속도 연성기법(Pressure-Velocity Coupling Scheme) : SIMPLEC

+ 이산화 기법(Discretization Scheme)
    + 운동량 : 2차 상류기법(Second Order Upwind)
    + 난류 : 1차 상류기법(Second Order Upwind)

+ 완화계수(Under-Relaxation Factors)
    + 압력 : 0.9
    + 운동량 : 0.9
    + 난류 : 0.9

+ 수렴 판정 기준(Convergence Criteria)
    + 압력 : 0.001
    + 운동량 : 0.001
    + 난류 : 0.001

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/9.png"><br>
</p>


### 모니터(Monitor)

본 예제에서는 외팔보에 걸리는 힘을 모니터링한다. [추가(Add)]-[힘(Forces)] 창의 경계면에서 cantileverBeam\_surface\_0을 선택한다.

이후, [추가(Add)]-[면(Surface)]를 선액하고 아래 그림과 같이 설정한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/7.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/8.png"><br>
</p>

### 초기화(Initialization)

다음과 같이 초기값을 설정한다.

+ 속도(Velocity)
    + X-속도 : 80 (m/s)
    + Y-속도 : 0 (m/s)
    + Z-속도 : 0 (m/s)

+ 압력(Pressure)
    + 0 (Pa)

+ 난류(Turbulence)
    + 속도 크기 : 80 (m/s)
    + 난류 강도 : 0.1 (%)
    + 난류 점도 비율 : 10

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/11.png"><br>
</p>

값을 입력하고 하단의 [초기화(Initialize)] 버튼을 클릭한다.그 후, 메뉴의 [파일(File)]-[저장(Save)] 버튼을 클릭하여 저장한다.

### 계산

메뉴의 [병렬연산(Parallel)]-[환경설정(Environment)]를 클릭하고 원하는 코어수를 입력한다. 

[계산 조건(Run Conditions)]에서 다음과 같이 설정 후 계산을 진행한다.

+ [계산회수(Number of Iterations)] : 1,000
+ [자동 저장 간격(Save Interval)] : 100


<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/12.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/13.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/14.png"><br>
</p>

계산이 시작되면 아래와 같이 잔차(residual) 그래프가 그려진다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/15.png"><br>
</p>

<!--
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/16.png"><br>
</p>
-->

### 후처리

[외부 프로그램(External tools)]-[ParaView] 버튼을 클릭하여 실행한다. 본 예제에서는 외팔보 주변 압력장을 그려본다.

병렬연산이면 [Case Type]을 [Decomposed Case]로 변경한다.

[Slice] 아이콘을 눌러 단면을 자른다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/17.png"><br>
</p>

축 방향은 [Y Normal]로 변경, [Origin]은 (200, 115, 125)로 변경한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/18.png"><br>
</p>

이후 상단의 p를 p\_rgh로 변경한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/19.png"><br>
</p>

아래 그림과 같은 외팔보 주변 압력 분포가 나오게 된다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cantilever/20.png"><br>
</p>
