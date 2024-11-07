# 비정상상태 유동

## 와류 진동(Vortex shedding)

[격자 파일 다운로드](https://drive.google.com/file/d/1f-eW_euVw3nNLarKX_FCF7tVywuiHDnY/view?usp=sharing)

[계산 파일 다운로드](https://drive.google.com/file/d/1ez8LCp7MGVcOpT7peHTAzt4G-wROafQa/view?usp=sharing)

### 개요 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.1.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.1.png)

본 예제는 2차원 비정상상태 층류 유동해석 예제로, 레이놀즈 수 100인 2차원 실린더 주변의 와류 진동(vortex shedding) 문제이다.

계산 조건은 다음과 같다. 

+ 솔버 : buoyantPimpleNFoam
+ 난류 모델 : laminar
+ 레이놀즈 수 : 100

### 프로그램의 구동 및 격자

프로그램 실행 후 [새 작업(New Case)]를 선택한다. 시작 창에서 [솔버 유형(Solver Type)]은 [압력기반(Pressure-based)]를, [다상유동 모델(Multiphase Model)]은 [None]을 선택한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/launcher.png"><br>
</p>

격자는 주어진 polyMesh 폴더를 활용한다. 상단 탭에서 [파일(File)]-[격자 불러오기(Load Mesh)]-[OpenFOAM]을 순서대로 클릭하고 polyMesh 폴더를 선택한다. 

### 기본조건(General)

시간(Time)을 비정상상태(Transient)로 변경한다. 나머지는 디폴트 조건을 사용한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.2.png"><br>
</p>

### 모델(Models)

난류 모델은 층류(Laminar)를 사용한다. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.3.png"><br>
</p>

### 물질(Materials)

본 예제에서는 $U$ = 1 $m/s$ 조건을 사용한다. 이 때 레이놀즈 수가 100이 되는 조건으로 물성치를 설정한다.

+ 밀도 : 1 $kg/m^3$
+ 점성 계수 : 0.01 $kg/ms$ 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.4.png"><br>
</p>

### 경계조건(Boundary Conditions)

아래와 같이 경계면 타입과 경계값을 설정한다.

+ cylinder : 벽면(Wall)
    + 속도조건(Velocity Condition) : 정지(No Slip)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.5.png"><br>
</p>

+ sym : 대칭(Symmetry)

+ out : Pressure Outlet
    + Total Pressure : 0 (Pa)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.6.png"><br>
</p>

+ in : 입구 속도(Velocity Inlet)
    + 속도 크기(Velocity Magnitude) : 1 (m/s)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.7.png"><br>
</p>

+ frontAndBackPlanes : 2차원 경계(Empty)

### 기준값(Reference Values)

본 예제에서는 실린더의 항력계수(Drag Coefficient)와 양력계수(Lift Coefficient)를 확인한다. 이를 위해 기준값을 아래와 같이 설정한다.

+ 면적(Area) : 1
+ 밀도(Density) : 1
+ 길이(Length) : 1
+ 압력(Pressure) : 0
+ 속도(Velocity) : 1 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.8.png"><br>
</p>

### 수치해석 기법(Numerical Conditions)

본 예제에서는 아래와 같이 설정을 변경한다.

+ 운동량 방정식 계산(Use Momentum Predictor) : 활성화

+ 이산화 기법(Discretization)
    + 시간 : 2차 음해법(Second Order Implicit)
    + 압력 : Momentum Weighted Reconstruct
    + 운동량 : 2차 상류기법(Second Order Upwind)

+ 시간당 반복계산 회수(Max iterations per Time Step) : 10
+ 압력보정 회수(Number of Correctors) : 2

나머지는 디폴트 조건을 사용한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.9.png"><br>
</p>

### 모니터(Monitor)

본 예제에서는 실린더에 작용하는 항력/양력계수(Drag/Lift Coefficient)와 실린더 중심에서 1m 떨어진 지점의 속도/압력을 모니터링 한다.

실린더에 작용하는 항력/양력계수

+ [추가(Add)]-[힘(Forces)] 버튼을 선택한다.
+ 자동저장간격(Write Interval) : 1
+ Lift Direction : 0 1 0
+ Drag Direction : 1 0 0
+ Center of Rotation : 0 0 0
+ Boundaries : cylinder

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.10.1.png"><br>
</p>

실린더 중심에서 1m 떨어진 지점의 속도, 압력

+ Add - Points 버튼을 선택한다.
+ Point Monitoring은 아래와 같이 설정한다.
    + Write Interval : 1
    + Field : Pressure
    + Coordinate : 1 0 0

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.10.2.png"><br>
</p>

같은 방식으로 동일한 지점의 속도 크기(Velocity Magnitude) 모니터링도 설정한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.10.3.png"><br>
</p>

### 초기화(Initialization)

X-속도에 1을 입력하고 나머지 값들은 디폴트값을 사용한다. 

하단의 [초기화(Initialize)] 버튼을 클릭한다. 그 후, 메뉴의 [파일(File)]-[저장(Save)] 버튼을 클릭하여 저장한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.11.png"><br>
</p>

### 계산

메뉴의 [병렬연산(Parallel)]-[환경설정(Environment)]를 클릭하고 원하는 코어수를 입력한다. 

[계산 조건(Run Conditions)]은 다음과 같이 설정하고 [계산시작(Start Calculation)] 버튼을 누르면 계산이 시작된다.

+ 시간전진기법(Time Stepping Method) : 적응시간기법(Adaptive)
+ Courant Number : 1
+ 종료시간(End Time) : 150
+ 자동 저장 간격(Save Interval) : 0.5

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.12.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.18.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.19.png"><br>
</p>


계산이 시작되면 아래와 같이 잔차(residual)과 모니터링 그래프가 그려진다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.13.1.png"><br>
</p>


<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.13.2.png"><br>
</p>

### 후처리

실린더 주변의 속도, 압력 분포를 확인한다.

메뉴에서 [외부 프로그램(External tools)]-[ParaView] 버튼을 클릭해서 paraview를 실행한다.

병렬연산이면 [Case Type]을 [Decomposed Case]로 변경한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.14.png"><br>
</p>

상단 툴바의 [Solid Color]를 [U] 혹은 [p\_rgh]로 변경하고 [play] 아이콘을 클릭한다.<br>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.15.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cylinder/5.16.png"><br>
</p>

<!-------------------------------------------------------------------------------------------->
## 시간에 따른 경계조건 변화

[격자 파일 다운로드](https://drive.google.com/file/d/1pzp6DXomC0cyaxn0xzlpcneShrEkRW68/view?usp=sharing)

[계산 파일 다운로드](https://drive.google.com/file/d/1dXzAUlGhnKvABA1bGgk-Us3JHDh8OyZB/view?usp=sharing)

### 개요

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/profileBC/10.1.png"><br>
</p>

본 예제는 시간에 따라 입구의 속도와 온도가 변하는 경계조건을 설정하는 예제이다.

OpenFOAM 튜토리얼에 있는 pitzDaily 격자를 사용한다.

계산 조건은 다음과 같다. 

+ 솔버 : buoyantPimpleNFoam 
+ 난류 모델 : $Standard$ $k-\epsilon$
+ 밀도 : Perfect Gas (Ideal Gas)
+ 점성 계수 : 1.79e-5 $kg/ms$

### 프로그램의 구동 및 격자

프로그램 실행 후 [새 작업(New Case)]를 선택한다. 시작 창에서 [솔버 유형(Solver Type)]은 [압력기반(Pressure-based)]를, [다상유동 모델(Multiphase Model)]은 [None]을 선택한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/launcher.png"><br>
</p>

격자는 주어진 polyMesh 폴더를 활용한다. 상단 탭에서 [파일(File)]-[격자 불러오기(Load Mesh)]-[OpenFOAM]을 순서대로 클릭하고 polyMesh 폴더를 선택한다. 

### 기본조건(General)

시간(Time)을 비정상상태(Transient)로 변경한다. 나머지는 디폴트 조건을 사용한다.

### 모델(Models)

난류 모델은 $Standard$ $k-\epsilon$ 모델을 사용하고 에너지(Energy)는 포함(Include)로 바꿔준다.

### 물질(Materials)

본 예제에서는 공기를 작동 유체로 사용한다. 유체의 이름과 물성치를 아래와 같이 변경한다. 

+ 밀도(Density) : 완전기체(Perfect Gas) (m/s)
+ 정압비열(Specific Heat, Cp) : 1004 $J/kgK$ (m/s)
+ 점성계수(Viscosity) : 1.79e-05 $kg/ms$
+ 열전도도(Thermal Conductivity) : 0.0245 $W/mK$

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/profileBC/Perfect Gas.png"><br>
</p>

### 경계조건(Boundary Conditions)

아래와 같이 경계면 타입과 경계값을 설정한다.

+ inlet : 입구 속도(Velocity Inlet)
    + 속도분포(Velocity Profile Type) : 시간에 따른 변화(Temporal Distribution)
        + 조각별 선형함수(piecewise linear) : (0, 1) (0.1 2) (0.2 1.5)
    + 난류 강도(Turbulent Intensity) : 1 (%)
    + 난류 점도 비율(Turbulent Viscosity Ratio) : 10
    + 온도분포(Temperature Profile Type) : 시간에 따른 변화(Temporal Distribution)
        + 조각별 선형함수(piecewise linear) : (0, 300) (0.1 400) (0.2 350)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/profileBC/10.3.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/profileBC/10.4.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/profileBC/10.5.png"><br>
</p>

+ outlet : 출구 압력(Pressure Outlet)
    + 압력(Pressure) : 0 (Pa)

+ upperWall, lowerWall : 벽면(Wall)
    + 속도조건(Velocity Condition) : 정지(No Slip)
    + 온도조건(Temperature Condition) : 단열(Adiabatic)

+ frontAndBack : 2차원 경계(empty)

### 수치해석 기법(Numerical Conditions)

본 예제에서는 디폴트 조건을 사용한다.

### 모니터(Monitor)

모니터를 클릭하면 세부설정상자가 나온다. 입구에서 유량과 온도 변화를 모니터링 한다. 

하단의 [추가(Add)]-[면(Surfaces)]를 누르면 창이 열린다. 아래 그림과 같이 설정을 변경한다.

+ 면 모니터 1(Surface Monitor 1)
    + 함수(Report Type) : 질량유량(Mass Flow Rate)
    + 면(Surface) : inlet

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/profileBC/10.6.png"><br>
</p>

+ 면 모니터 1(Surface Monitor 2)
    + 함수(Report Type) : 면적가중평균(Area-Weighted Average)
    + 유동변수(Field Variable) : 온도(Temperature)
    + 면(Surface) : inlet

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/profileBC/10.7.png"><br>
</p>

### 초기화(Initialization)

다음과 같이 초기값을 설정한다.

+ 속도(Velocity)
    + X-속도 : 0 (m/s)
    + Y-속도 : 0 (m/s)
    + Z-속도 : 0 (m/s)

+ 압력(Pressure)
    + 0 (Pa)

+ 온도(Temperature) : 300 (K)

+ 난류(Turbulence)
    + 속도 크기 : 1 (m/s)
    + 난류 강도 : 1 (%)
    + 난류 점도 비율 : 10

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/profileBC/10.8.png"><br>
</p>

값을 입력하고 하단의 [초기화(Initialize)] 버튼을 클릭한다. 그 후, 메뉴의 [파일(File)]-[저장(Save)] 버튼을 클릭하여 저장한다.

### 계산

[계산 조건(Run Conditions)]은 다음과 같이 설정하고 [계산시작(Start Calculation)] 버튼을 누르면 계산이 시작된다.

+ 시간 전진 간격(Time Step Size) : 0.001
+ 종료 시간(End Time) : 1
+ 자동 저장 간격(Save Interval) : 0.1

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/profileBC/10.9.png"><br>
    그림 11.9
</p>

계산이 시작되면 아래와 같이 잔차(residual)과 모니터링 그래프가 그려진다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/profileBC/10.10.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/profileBC/10.11.png"><br>
</p>

### 후처리

시간에 따른 온도 및 속도의 분포를 확인한다. 

메뉴에서 [외부 프로그램(External tools)]-[ParaView] 버튼을 클릭해서 paraview를 실행한다.

상단의 [Solid Color]를 [T]로 변경한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/profileBC/10.12.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/profileBC/10.13.png"><br>
</p>

[Set Range]를 눌러 온도 범위를 340 - 350K으로 조정한다.

그리고 상단의 [Play] 아이콘을 눌러 시간에 따른 온도 변화를 확인한다.

아래 그림은 최종 순간에서 온도분포를 나타내는 그림이다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/profileBC/10.14.png"><br>
</p>



