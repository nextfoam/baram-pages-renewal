# 사용자 정의 스칼라

## 실내 화재 해석 

[격자 파일 다운로드](https://drive.google.com/file/d/1ySpMPSdtioU4DSJrWAKJEsCT0wihKo44/view?usp=sharing)

[계산 파일 다운로드](https://drive.google.com/file/d/1NH75is3AIN0Kl1nSYRWy58OgKT8UTRCJ/view?usp=sharing)

### 개요 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/fireInRoom/intro.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/fireInRoom/intro.png)

실내의 화재를 시뮬레이션하는 문제이다. 화학종과 연소 반응은 계산하지 않고, 화재성장곡선을 이용해서 시간에 따른 발열량과 연소가스 생성량을 에너지방정식과 사용자 정의 스칼라 방정식의 소스로 사용해서 계산한다. 테이블 아래쪽의 육면체 셀존에 소스항을 준다.

화재성장곡선은 다음과 같은 2차 함수를 사용하고 25초 동안 계산한다. 

$S_h = 1000 \cdot t^2$

$S_{gas} = 5 \cdot 10^{-6} \cdot t^2$

계산 조건은 다음과 같다. 

+ 솔버 : buoyantSimpleNFoam 
+ 난류 모델 : $standard$ $k-\epsilon$
+ 밀도 : 완전기체
+ 점성 계수 : 1.79e-5 $kg/ms$

### 프로그램의 구동 및 격자

프로그램 실행 후 [새 작업(New Case)]를 선택한다. 시작 창에서 [솔버 유형(Solver Type)]은 [압력기반(Pressure-based)]를, [다상유동 모델(Multiphase Model)]은 [None]을 선택한다.

격자는 주어진 polyMesh 폴더를 활용한다. 상단 탭에서 [파일(File)]-[격자 불러오기(Load Mesh)]-[OpenFOAM]을 순서대로 클릭하고 polyMesh 폴더를 선택한다. 

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/fireInRoom/mesh.png"><br>
</p>

### 기본조건(General)

[시간(Time)]은 [비정상상태(Transient)]로 설정한다.

[중력(Gravity)]는 (0 0 -9.81)로 설정한다.


### 모델(Models)

난류 모델은 $Standard$ $k-\epsilon$ 모델을 사용한다.

[에너지(Energy)]를 [포함(include)]한다.

[사용자 정의 스칼라(User-defined Scalar)]를 추가한다. [필드 이름(Field Name)]은 gas로 주고 [확산계수(diffusivity)]는 [Laminar and Turbulent Viscosity]를 선택한다. [Laminar/Turbulent Viscosity Coefficient]는 모두 1로 준다.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/fireInRoom/uds.png"><br>
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

### 셀존 조건(Cell zone Conditions) 

Hex_1에 에너지와 gas의 생성항(source term)을 설정한다. 

+ 값의 크기 설정 방법(Specification Method) : 전체 체적에 대한 값(Value for Entire Cell Zone)
+ 값의 변화 설정 방법(Temporal Profile Type) : 다항식(Polynomial)
    + 편집(Edit) 버튼을 눌러 (0 1 2)에 에너지는 (0 0 1000)을 gas는 (0 0 5e-6)을 입력한다.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/fireInRoom/cellZone.png"><br>
</p>

### 경계조건(Boundary Conditions)

아래와 같이 경계면 타입과 경계값을 설정한다.

+ room, desk\_surface : 벽면(wall)
    + 속도 조건(Velocity Condition) : 정지(No Slip)
    + 온도(Temperature) : 단열(Adiabatic)

+ door_surface, outlet : 출구 압력(Pressure Outlet)
    + 압력(Pressure)  : 0

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/train/outletbc.png">
</p>

### 수치해석 기법(Numerical Conditions)

[시간당 반복계산 회수(Max Iteration per Time Step)]은 20, [압력보정 회수(Number of Correctors)]는 2를 입력하고 나머지는 디폴트를 사용한다.


### 초기화(Initialization)

초기조건은 다음과 같이 설정한다.

+ Velocity : (0 0 0)
+ Pressure : 0
+ Temperature : 300
+ Scale of velocity : 1 
+ Turbulent Intensity : 1
+ Turbulent Viscosity Ratio : 10
+ User-defined Scalars - gas : 0

값을 입력하고 하단의 [초기화(Initialize)] 버튼을 클릭한다. 그 후, 메뉴의 [파일(File)]-[저장(Save)] 버튼을 클릭하여 저장한다.

### 계산

[계산 조건(Run Conditions)]은 다음과 같이 설정하고 [계산시작(Start Calculation)] 버튼을 누르면 계산이 시작된다.

+ 시간 전진 기법(Time Stepping Method) : 적응시간기법(Adaptive)
+ Courant Number : 4
+ 종료 시간(End Time) : 25
+ 자동 저장 간격(Save Interval) : 0.2

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/fireInRoom/run.png"><br>
</p>


### 후처리

메뉴에서 [외부 프로그램(External tools)]-[ParaView] 버튼을 눌러 ParaView를 실행한다.

[Coloring]은 gas를 선택한다. [Play]를 누르면 시간에 따른 gas의 분포를 확인할 수 있다.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/fireInRoom/contour.png"><br>
</p>

