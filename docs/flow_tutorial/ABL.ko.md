# 대기경계층
 
## 경계조건 검증

[격자 파일 다운로드](https://drive.google.com/file/d/19kMYRiWaB84kaUzCoobMRZBKCc_uKxVU/view?usp=drive_link)

[계산 파일 다운로드](https://drive.google.com/file/d/1wMjgdSaHfAD8GzPNRVkUayhCyOFwK_9C/view?usp=sharing)

### 개요 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/8.1.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/8.1.png)


본 예제는 대기 경계층 유동해석 예제이다. 해석 영역은 600m X 50m X 600m이며 대기경계층 조건으로 주어진 입구의 유동 분포가 출구까지 유지되는지를 확인한다.

격자는 주어진 OpenFOAM 격자를 사용한다

계산 조건은 다음과 같다. 

+ 솔버 : buoyantSimpleNFoam
+ 난류 모델 : standard 𝑘 − ε model
+ 밀도 : 1.225 𝑘𝑔/㎥
+ 점성 계수 : 1.79e-5 𝑘𝑔/𝑚s
+ 유동 조건 : 대기경계층 속도 및 난류 조건

대기 경계층 조건은 OpenFOAM이 제공하는 조건으로 D.M. Hargreaves and N.G. Wright의 다음 논문의 식을 이용한다.

*"On the use of the k-epsilon model in commercial CFD software to model the neutral atmospheric boundary layer"*

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/8.2.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/8.3.png"><br>
</p>

### 프로그램의 구동 및 격자

프로그램 실행 후 [새 작업(New Case)]를 선택한다. 시작 창에서 [솔버 유형(Solver Type)]은 [압력기반(Pressure-based)]를, [다상유동 모델(Multiphase Model)]은 [None]을 선택한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/launcher.png"><br>
</p>

격자는 주어진 polyMesh 폴더를 활용한다. 상단 탭에서 [파일(File)]-[격자 불러오기(Load Mesh)]-[OpenFOAM]을 순서대로 클릭하고 polyMesh 폴더를 선택한다. 

### 기본조건(General)

본 예제는 디폴트 조건을 사용한다.

### 모델(Models)

난류 모델은 $Standard$ $k-\epsilon$ 모델을 사용한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/8.4.png"><br>
</p>

### 물질(Materials)

본 예제에서는 공기를 작동 유체로 사용한다. 물성치는 디폴트 값을 사용한다.

### 경계조건(Boundary Conditions)

아래와 같이 경계면 타입과 경계값을 설정한다.

+ inlet : 대기경계층 입구(ABL Inlet)
    + 바람 방향(Flow Direction) : 1 0 0
    + 지면에 수직한 방향(Ground-Normal Direction) : 0 0 1
    + 기준 풍속(Reference Flow Speed) : 7 (m/s)
    + 기준 고도(Reference Height) : 9 (m)
    + 지표면 조도(Surface Roughness Length) : 0.0002 (m)
    + 지표면 최소 Z 좌표(Minimum z-coordinate) : 0.0 (m)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/8.5.png"><br>
</p>

+ Sea : 벽면(Wall)
    + 속도 조건(Velocity Condition) : 대기경계층 지표면(Atmospheric Wall)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/8.6.png"><br>
</p>

+ outlet : 출구 압력(Pressure Outlet)
    + 압력(Pressure) : 0 (Pa)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/8.7.png"><br>
</p>

+ minY, maxY, sky : 대칭(Symmetry)

### 수치해석 기법(Numerical Conditions)

수치해석 기법은 다음과 같이 설정한다.

+ 압력-속도 연성기법(Pressure-Velocity coupling) : SIMPLE

+ 이산화 기법(Discretization)
    + 압력 : Momentum Weighted Reconstruct
    + 운동량 : Second Order Upwind
    + 난류 : First Order Upwind

+ 수렴 판정 기준(Convergence Criteria) : 1e-6 (모든 값)

+ 고급설정(Advanced)
    + 최대 점도비율(Maximum Viscosity Ratio)를 1e7으로 설정한다 - _계산 영역이 매우 크기 때문에 이 값을 디폴트 값으로 사용하면  난류 운동에너지(k)의 분포가 유지되지 않는다._

나머지는 디폴트 조건을 사용한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/8.8.1.png"><br>
</p>


### 초기화(Initialization)

다음의 값을 입력하고 하단의 [초기화(Initialize)] 버튼을 클릭한다. 그 후, 메뉴의 [파일(File)]-[저장(Save)] 버튼을 클릭하여 저장한다.

+ X-속도 : 7 (m/s)
+ 압력(Pressure) : 0 (Pa)
+ 난류(Turbulence)
    + 속도 크기(Scale of Velocity) : 7 (m/s)
    + 난류 강도(Turbulent Intensity) : 1 (%)
    + 난류 점도 비율(Turbulent Viscosity Ratio) : 10

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/8.9.png"><br>
</p>


### 계산

[계산 조건(Run Conditions)]은 다음과 같이 설정하고 [계산시작(Start Calculation)] 버튼을 누르면 계산이 시작된다.

+ [계산회수(Number of Iterations)] : 1000
+ [자동 저장 간격(Save Interval)] : 1000

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/8.10.png"><br>
</p>

계산이 시작되면 아래와 같이 잔차(residual)과 유체력 계수의 그래프가 그려진다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/residual.png"><br>
</p>

### 후처리

대기경계층 속도 분포를 확인한다. 메뉴에서 [외부 프로그램(External tools)]-[ParaView] 버튼을 클릭해서 Paraview를 실행한다.

병렬연산이면 [Case Type]을 [Decomposed Case]로 변경한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/8.12.png"><br>
</p>

상단 툴바의 [Solid Color]를 [U]로 변경한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/8.13.png"><br>
</p>

상단 툴바에서 [Slice] 아이콘을 클릭하고 아래와 같이 설정한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/8.16.png"><br>
</p>


상단 툴바의 [Plot Over Line] 아이콘을 클릭하고 아래와 같이 입구와 출구에 각각 Line을 1개 생성한다.

입구, 출구의 라인을 이용하여 속도 프로파일이 그대로 유지되는지 정량적으로 확인한다.

1번 라인 (입구)

+ Point1 : 0 25 0
+ Point2 : 0 25 600
+ X Array Name : U_X
+ Series Parameters : Points_Z

2번 라인 (출구)

+ Point1 : 600 25 0
+ Point2 : 600 25 600
+ X Array Name : U_X
+ Series Parameters : Points_Z

[Series Parameters]에서 해당 Parameter의 색을 변경할 수 있다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/8.14.1.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/8.14.2.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/8.14.3.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/8.14.4.png"><br>
</p>

아래 그림과 같이 높이에 따른 속도 크기 분포를 확인할 수 있다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ABL/8.15.png"><br>
</p>

위 그림에서 빨간선이 입구영역, 초록선이 출구영역에서 속도 프로파일이다.
