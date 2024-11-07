# 화학종 혼합

## 덕트 내부 혼합 

[격자 파일 다운로드](https://drive.google.com/file/d/19BS3wUfZZeh8A8Mqx2B1gSMwbylE-xXS/view?usp=sharing)

[계산 파일 다운로드](https://drive.google.com/file/d/1VihUsXB47k3qHfJOZ6Lgm4THC3v6jsRw/view?usp=sharing)

### 개요 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/species/intro1.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/species/intro1.png)

본 예제는 비정상상태 화학종 혼합 해석 예제이다. 두 개의 입구가 있는 2차원 덕트의 위쪽 입구에는 공기, 아래쪽 입구에는 수증기가 유입된다.

계산 조건은 다음과 같다. 

+ 솔버 : buoyantSimpleNFoam 
+ 난류 모델 : $Standard$ $k-\epsilon$
+ 입구 유동 속도 : 0.5 $m/s$

### 프로그램의 구동 및 격자

프로그램 실행 후 [새 작업(New Case)]를 선택한다. 시작 창에서 [솔버 유형(Solver Type)]은 [압력기반(Pressure-based)]를, [다상유동 모델(Multiphase Model)]은 [None]을 선택한다.

격자는 주어진 polyMesh 폴더를 활용한다. 상단 탭에서 [파일(File)]-[격자 불러오기(Load Mesh)]-[OpenFOAM]을 순서대로 클릭하고 polyMesh 폴더를 선택한다. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/species/mesh1.png"><br>
</p>

### 기본조건(General)

[시간(Time)]은 [비정상상태(Transient)]로  설정한다.

[중력(Gravity)]는 (0 -9.81 0)으로 설정한다.


### 모델(Models)

[화학종 혼합(Species)]를 더블 클릭하고 [포함(Include)]를 선택한다.

### 물질(Materials)

[물질(Materials)] 패널 오른쪽 상단의 (+)를 클릭하여 혼합물(mixture)를 추가한다. 아래 그림의 왼쪽과 같이 air와 waterVapor를 선택하고 [혼합물 생성(Create Mixture)]를 클릭하면 오른쪽과 같이 혼합물이 만들어진다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/species/material.png"><br>
</p>


### 셀존 조건(Cell zone Conditions) 

셀존 조건의 region0를 더블 클릭해서 [물질(Material)]을 mixture로 바꾼다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/species/cellzone.png"><br>
</p>

### 경계조건(Boundary Conditions)

아래와 같이 경계면 타입과 경계값을 설정한다.

+ in-up : 입구 속도(Velocity Inlet)
    + 속도 크기(Velocity Magnitude) : 0.5 (m/s)
    + 난류 강도(Turbulent Intensity) : 0.1 (%)
    + 난류 점도 비율(Turbulent Viscosity Ratio) : 1
    + mixture - air : 1
    + mixture - waterVapor : 0
    + 온도(Temperature) : 300
    
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/species/inletBC1.png"><br>
</p>

+ in-down : 입구 속도(Velocity Inlet)
    + 속도 크기(Velocity Magnitude) : 0.5 (m/s)
    + 난류 강도(Turbulent Intensity) : 0.1 (%)
    + 난류 점도 비율(Turbulent Viscosity Ratio) : 1
    + mixture - air : 0
    + mixture - waterVapor : 1
    + 온도(Temperature) : 300
    
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/species/inletBC2.png"><br>
</p>


+ out : 출구 압력(Pressure Outlet)
    + 압력(Pressure) : 0 (Pa)

+ down_surface, up : 벽면(wall) - 정지(no slip)

+ zMin, zMax : 2차원 경계(Empty)


### 수치해석 기법(Numerical Conditions)

본 예제에서는 아래와 같이 설정을 변경한다.

+ 압력-속도 연성 기법(Pressure-Velocity Coupling Scheme) : SIMPLE

+ 이산화 기법(Discretization Scheme)
    + 시간 : 2차 음해법(Second Order Implicit)
    + 압력 : Linear
    + 운동량 : 2차 상류기법(Second Order Upwind)
    + 난류 : 2차 상류기법(Second Order Upwind)
    + 화학종 : 2차 상류기법(Second Order Upwind)

+ 완화계수(Under-Relaxation Factors)
    + 압력 : 0.3
    + 운동량 : 0.7
    + 난류 : 0.7
    + 화학종 : 1

+ 수렴 판정 기준(Convergence Criteria) : 디폴트 값 사용

+ 시간당 반복계산 회수(Max Iterations per Time Step) : 100

+ 압력보정 회수(Number of Correctors) : 2

### 초기화(Initialization)

다음과 같이 설정한다.

+ Velocity : (0.5 0 0)
+ Pressure : 0
+ Scale of Velocity : 0.5
+ Turbulent intensity : 0.1
+ Turbulent viscosity ratio : 1
+ mixture - air : 1
+ mixture - waterVapor : 0

값을 입력하고 하단의 [초기화(Initialize)] 버튼을 클릭한다. 그 후, 메뉴의 [파일(File)]-[저장(Save)] 버튼을 클릭하여 저장한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/species/init1.png"><br>
</p>

### 계산

[계산 조건(Run Conditions)]은 다음과 같이 설정하고 [계산시작(Start Calculation)] 버튼을 누르면 계산이 시작된다.

+ 시간 전진 기법(Time Stepping Method) : 일정(Fixed)
+ 시간 전진 간격(Time Step Size) : 0.001
+ 종료 시간(End Time) : 20
+ 자동 저장 간격(Save Interval) : 0.1

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/species/runCondition1.png"><br>
</p>


### 후처리

메뉴에서 [외부 프로그램(External tools)]-[ParaView] 버튼을 눌러 ParaView를 실행한다.

[Coloring]을 waterVapor로 바꾸면 다음과 같이 결과를 확인할 수 있다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/species/pv2.png"><br>
</p>
