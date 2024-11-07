# 자동차 외부유동

## Ahmed body

[격자 파일 다운로드](https://drive.google.com/file/d/1qVQqF6oavui3NCoAQ2QCpSmo824CVbx_/view?usp=sharing)

[계산 파일 다운로드](https://drive.google.com/file/d/1bvY3-XtcuimHAnliKtSojHWPNmcYnmZr/view?usp=sharing)

### 개요 

[![intro](https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/intro.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/intro.png)


S.R. Ahmed는 단순화된 자동차 모형을 이용해 후방 경사각에 따른 유동 구조의 변화를 실험을 통해 관찰하였다. 이후 이 문제는 자동차 외부 공력해석의 검증용으로 많이 사용되고 있다. 이 예제는 후방 경사각도가 25°인 경우에 속도 40m/s 조건에 대한 예제로 정상상태 비압축성 유동 조건을 사용한다. 

ref : _S.R. Ahmed, G. Ramm, Some Salient Features of the Time-Averaged Ground Vehicle Wake, SAE-Paper 840300, 1984_

논문의 실험 결과 항력계수(Cd)는 0.285이며 계산 결과는 0.287로 0.7%의 차이를 보여준다.

계산 조건은 다음과 같다.

+ 솔버 : buoyantSimpleNFoam 
+ 난류 모델 : Realizable $k-\epsilon$ model
+ 밀도 : 1.2 $kg/m^3$
+ 점성 계수 : 1.8e-5 $kg/ms$
+ 유동 조건 : inlet에서 40 $m/s$

### 프로그램의 구동 및 격자

프로그램 실행 후 [새 작업(New Case)]를 선택한다. 시작 창에서 [솔버 유형(Solver Type)]은 [압력기반(Pressure-based)]를, [다상유동 모델(Multiphase Model)]은 [None]을 선택한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/launcher.png"><br>
</p>

격자는 주어진 polyMesh 폴더를 활용한다. 상단 탭에서 [파일(File)]-[격자 불러오기(Load Mesh)]-[OpenFOAM]을 순서대로 클릭하고 polyMesh 폴더를 선택한다. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.1.1.png"><br>
</p>


### 기본조건(General)

본 예제에서는 디폴트 조건을 사용한다.

### 모델(Models)

난류 모델은 Realizable $k-\epsilon$ 모델을 사용하고 나머지는 디폴트 조건을 사용한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.2.png"><br>
</p>

### 물질(Materials)

본 예제에서는 공기의 물성치를 다음과 같이 수정하여 사용한다. 

+ air
    + Density : 1.2 𝑘𝑔/㎥ 
    + Viscosity : 1.8e-5 𝑘𝑔/𝑚s

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.3.png"><br>
</p>

### 경계조건(Boundary Conditions)

아래와 같이 경계면 타입과 경계값을 설정한다.

+ minx : 입구 속도(velocity Inlet)
    + 속도 크기(Velocity Magnitude) : 40 (m/s)
    + 난류 강도(Turbulent Intensity) : 1 (%)
    + 난류 점도 비율(Turbulent Viscosity Ratio)  : 10

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.4.png"><br>
</p>

+ maxx : 출구 압력(Pressure Outlet)
    + 압력(Pressure) : 0 (Pa)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.5.png"><br>
</p>

+ miny : 벽면(Wall)
    + 속도 조건(Velocity Condition) : 직선 속도(Translation Moving Wall), 
    + 속도(Velocity) : (40, 0, 0) (m/s)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.6.png"><br>
</p>

+ bottom, leg, nose1, nose2, nose3, nose4, nose5, rear, side, slant, top : 벽면(Wall)
    + 속도 조건(Velocity Condition) : 정지(No Slip)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.7.png"><br>
</p>

+ minz, maxz, maxy : 대칭(symmetry)

### 기준값(Reference Values)

공력계수 계산을 위한 기준값을 다음과 같이 설정한다.

+ 면적(Area) : 0.056(kg/m<sup>2</sup>, 유동 방향에 수직한 단면적의 50%)
+ 밀도(Density) : 1.2 (kg/m<sup>3</sup>)
+ 길이(Length) : 1 (m)
+ 압력(Pressure) : 0 (Pa)
+ 속도(Velocity) : 40 (m/s)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.8.png"><br>
</p>

### 수치해석 기법(Numerical Conditions)

본 예제에서는 아래와 같이 설정을 변경한다.

+ [압력-속도 연성기법(Pressure-Velocity coupling)] : SIMPLE

+ [이산화 기법(Discretization)]
    + 압력 : Momentum Weighted Reconstruct
    + 운동량 : 2차 상류기법(Second Order Upwind)
    + 난류 : 2차 상류기법(Second Order Upwind)

+ [완화계수(Relaxation factors)]
    + 압력 : 0.3
    + 운동량 : 0.7
    + 난류 : 0.7

+ [수렴 판정 기준(Convergence criteria)]
    + 압력 : 0.001
    + 운동량 : 0.001
    + 난류 : 0.001

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.10.1.png"><br>
</p>


### 모니터(Monitor)

본 예제에서는 자동차에 걸리는 공력 계수를 모니터링한다.

[추가(Add)]-[힘(Forces)]를 선택하여 아래 그림과 같이 설정한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.9.png"><br>
</p>

### 초기화(Initialization)

다음과 같이 초기값을 설정한다.

+ 속도(Velocity)
    + X-속도 : 40 (m/s)
    + Y-속도 : 0 (m/s)
    + Z-속도 : 0 (m/s)

+ 압력(Pressure)
    + 0 (Pa)

+ 난류(Turbulence)
    + 속도 크기 : 40 (m/s)
    + 난류 강도 : 1 (%)
    + 난류 점도 비율 : 10

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.11.png"><br>
</p>

값을 입력하고 하단의 [초기화(Initialize)] 버튼을 클릭한다. 그 후, 메뉴의 [파일(File)]-[저장(Save)] 버튼을 클릭하여 저장한다.

### 계산

메뉴의 [병렬연산(Parallel)]-[환경설정(Environment)]를 클릭하고 원하는 코어수를 입력한다. 

[계산 조건(Run Conditions)]은 다음과 같이 설정하고 [계산시작(Start Calculation)] 버튼을 누르면 계산이 시작된다.

+ 계산회수(Number of Iterations) : 2000
+ 자동 저장 간격(Save Interval) : 300


<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.12.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.25.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.26.png"><br>
</p>

계산이 시작되면 아래와 같이 잔차(residual)과 유체력 계수의 그래프가 그려진다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/residual.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/force.png"><br>
</p>

### 후처리

#### 경계면 스칼라 분포

BARAM에서는 ParaView를 이용하여 후처리를 진행한다. 메뉴에서 [외부 프로그램(External tools)]-[ParaView] 버튼을 클릭한다. 본 예제에서는 유동장 내 압력 분포와 유선을 그려본다.

병렬연산이면 [Case Type]을 [Decomposed Case]로 변경한다.

+ [Mesh Regions]에서 아래 경계면들을 선택한다.
    + Bottom, internalMesh, leg, miny, nose1, nose2, nose3, nose5, rear, side, slant, top

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.20.png"><br>
</p>

[solid color]를 p\_rgh로 변경하고 단면에서 압력 분포를 확인한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.15.png"><br>
</p>

#### 유선(Streamline)

차량 주변 유동의 유선을 확인한다.

아래 그림과 같이 [extract block] 기능을 활용하여 차량 벽면과 바닥면의 형상을 추출한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/21.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.24.png"><br>
</p>

p를 [Solid Color]로 변경한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.21.png"><br>
</p>

변경 후 모습

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.22.png"><br>
</p>

왼쪽 Pipeline Browser에서 baram.foam을 한번 클릭하여 활성화 한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.23.png"><br>
</p>

이후, [Stream Tracer] 아이콘을 클릭한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.16.png"><br>
</p>

그 후, 설정을 아래와 같이 변경한다.

+ Seed Type : Point Cloud
+ Center : (0.7, 0.1, 0.1)
+ Radius :  0.2
+ Number of Points : 100
+ Coloring : Vorticity

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.17.png"><br>
</p>

아래 그림과 같은 유선이 만들어진다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.18.png"><br>
</p>


## DriveAer


[격자 파일 다운로드](https://drive.google.com/file/d/1uL_roJ1xnBsmG87HTwQ4fie4txDhjCjW/view?usp=sharing)

[계산 파일 다운로드](https://drive.google.com/file/d/1sug9iCm2bMCYez2AsrI164yn_xAm1IOX/view?usp=sharing)

### 개요 

[![intro](https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/intro.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/intro.png)

DrivAer는 자동차 공학 분야에서 사용되는 차량 외부 디자인 및 공기역학 테스트를 위한 실제 차량 모델로 차량의 외부 형태와 공기역학적 특성을 시뮬레이션하고 평가하기 위해 많이 사용된다. 단순화된 모델과 매우 복잡한 양산차 사이의 격차를 줄이기 위해 도입된 모델로 여러 종류의 형상에 대한 CAD 파일과 실험결과들이 공개되어 있다.

이 예제는 사이드 미러와 바퀴가 포함된 fastback 모델을 사용한다.

[https://www.epc.ed.tum.de/en/aer/research-groups/automotive/drivaer/](https://www.epc.ed.tum.de/en/aer/research-groups/automotive/drivaer/)

정상상태 비압축성 유동에서 moving ground, rotating wheel 조건을 사용한다.

논문의 실험 결과 저항계수(Cd)는 ASME는 0.247, SAE는 0.243이며 계산 결과는 Cd = 0.243으로 SAE 실험결과와 일치한다.

계산 조건은 다음과 같다. 

+ 솔버 : buoyantSimpleNFoam
+ 난류 모델 : $Realizable$ $k-\epsilon$ model
+ 밀도 : 1.205 $kg/m^3$
+ 점성 계수 : 1.82e-5 $kg/ms$
+ 유동 조건 : inlet에서 30 $m/s$ 

### 프로그램의 구동 및 격자

프로그램 실행 후 [새 작업(New Case)]를 선택한다. 시작 창에서 [솔버 유형(Solver Type)]은 [압력기반(Pressure-based)]를, [다상유동 모델(Multiphase Model)]은 [None]을 선택한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/launcher.png"><br>
</p>

격자는 주어진 polyMesh 폴더를 활용한다. 상단 탭에서 [파일(File)]-[격자 불러오기(Load Mesh)]-[OpenFOAM]을 순서대로 클릭하고 polyMesh 폴더를 선택한다. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/1.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/2.png"><br>
</p>

### 기본조건(General)

본 예제에서는 디폴트 조건을 사용한다.

### 모델(Models)

난류 모델은 $Realizable$ $k-\epsilon$ 모델을 사용하고 나머지는 디폴트 조건을 사용한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/3.png"><br>
</p>

### 물질(Materials)

본 예제에서는 공기의 물성치를 다음과 같이 수정하여 사용한다. 

+ air
    + Density : 1.205𝑘𝑔/㎥ (m/s)
    + Viscosity : 1.82e-5𝑘𝑔/𝑚s

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/4.png"><br>
</p>

### 경계조건(Boundary Conditions)

아래와 같이 경계면 타입과 경계값을 설정한다.

+ minX : 입구 속도(velocity Inlet)
    + 속도 크기(Velocity Magnitude) : 30 (m/s)
    + 난류 강도(Turbulent Intensity) : 1 (%)
    + 난류 점도 비율(Turbulent Viscosity Ratio) : 10

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/5.png"><br>
</p>

+ maxX : 출구 압력(Pressure Outlet)
    + 압력(Pressure) : 0 (Pa)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/6.png"><br>
</p>

+ minY, maxY, maxZ : Symemtry

+ minZ : 벽면(Wall)
    + 속도 조건(Velocity Condition) : 직선 속도(Translation Moving Wall), 
    + 속도(Velocity) : (30, 0, 0)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/7.png"><br>
</p>

+ body\_no\_wheel : 벽면(Wall)
    + 속도 조건(Velocity Condition) : 정지(No Slip)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/8.png"><br>
</p>

+ Wheels\_Front\_Smooth : 벽면(Wall)
    + 속도 조건(Velocity Condition) : 회전 속도(Rotational Moving Wall)
    + 회전속도(Speed (RPM)) : 898.8
    + 회전축 중심(Rotation-Axis Origin) : (0.007, 0, 0)
    + 회전축 방향(Rotation-Axis Direction) : (0, -1, 0)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/9.png"><br>
</p>

+ Wheels\_Rear\_Smooth : 벽면(Wall)
    + 속도 조건(Velocity Condition) : 회전 속도(Rotational Moving Wall)
    + 회전속도(Speed (RPM)) : 898.8
    + 회전축 중심(Rotation-Axis Origin) : (2.7932, 0, 0)
    + 회전축 방향(Rotation-Axis Direction) : (0, -1, 0)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/10.png"><br>
</p>

### 기준값(Reference Values)

공력계수 계산을 위한 기준값을 다음과 같이 설정한다.

+ Area : 1.08
+ Density : 1.205
+ Length : 4.6132
+ Pressure : 0
+ Velocity : 30

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/11.png"><br>
</p>

### 수치해석 기법(Numerical Conditions)

본 예제에서는 아래와 같이 설정을 변경한다.

+ [압력-속도 연성기법(Pressure-Velocity coupling)] : SIMPLEC

+ [이산화 기법(Discretization)]
    + 압력 : Momentum Weighted Reconstruct
    + 운동량 : 2차 상류기법(Second Order Upwind)
    + 난류 : 2차 상류기법(Second Order Upwind)

+ [완화계수(Relaxation factors)]
    + 압력 : 0.9
    + 운동량 : 0.9
    + 난류 : 0.9

+ [수렴 판정 기준(Convergence criteria)]
    + 압력 : 0.00001
    + 운동량 : 0.001
    + 난류 : 0.001

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/13.png"><br>
</p>


### 모니터(Monitor)

본 예제에서는 자동차에 걸리는 공력 계수를 모니터링한다.

[추가(Add)]-[힘(Forces)]를 선택하여 아래 그림과 같이 설정한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/12.png"><br>
</p>

### 초기화(Initialization)

다음 값을 초기값으로 사용한다.

+ 속도(Velocity)
    + X-속도(Velocity) : 30 (m/s)
    + Y-속도(Velocity) : 0 (m/s)
    + Z-속도(Velocity) : 0 (m/s)

+ 압력(Pressure)
    + 0 (Pa)

+ 난류(Turbulence)
    + 속도 크기 : 30 (m/s)
    + 난류 강도 : 1 (%)
    + 난류 강도 : 10

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/15.png"><br>
</p>

값을 입력하고 하단의 [초기화(Initialize)] 버튼을 클릭한다. 그 후, 메뉴의 [파일(File)]-[저장(Save)] 버튼을 클릭하여 저장한다.

### 계산

메뉴의 [병렬연산(Parallel)]-[환경설정(Environment)]를 클릭하고 원하는 코어수를 입력한다. 

[계산 조건(Run Conditions)]은 다음과 같이 설정하고 [계산시작(Start Calculation)] 버튼을 누르면 계산이 시작된다.

+ 계산회수(Number of Iterations) : 2,000
+ 자동 저장 간격(Save Interval) : 100

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/16.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/28.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/29.png"><br>
</p>

계산이 시작되면 아래와 같이 잔차(residual)과 유체력 계수의 그래프가 그려진다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/residual.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/force.png"><br>
</p>

### 후처리

#### 경계면 스칼라 분포

메뉴에서 [외부 프로그램(External tools)]-[ParaView] 버튼을 클릭한다. 본 예제에서는 차량 주변 압력 분포와 유선을 그려본다.

병렬연산이면 [Case Type]을 [Decomposed Case]로 변경한다.

+ [Mesh Regions]에서 아래 경계면들을 선택한다.
    + Wheels\_Front\_Smooth, Wheels\_Rear\_Smooth, body\_no\_wheels

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/19.png"><br>
</p>

아래 그림과 같이 extract block 기능을 활용하여 차량 벽면과 바닥면의 형상을 추출한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/21.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/22.png"><br>
</p>

solid color를 p_rgh로 변경하고 단면에서 압력 분포를 확인한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/20.png"><br>
</p>

#### 유선(Streamline)

차량 주변 유동의 유선을 확인한다.

[p]를 [Solid Color]로 변경한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/23.png"><br>
</p>

변경 후 모습

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/24.png"><br>
</p>

왼쪽 Pipeline Browser에서 baram.foam을 한번 클릭하여 활성화 한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/25.png"><br>
</p>

이후, [Stream Tracer] 아이콘을 클릭한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/2.16.png"><br>
</p>

그 후, 설정을 아래와 같이 변경한다.

+ Seed Type : Point Cloud
+ Center : (-1.5, 0.1, 0.1)
+ Radius :  0.2
+ Number of Points : 100
+ Coloring : U

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/26.png"><br>
</p>

아래 그림과 같은 유선이 만들어진다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/drivAer/27.png"><br>
</p>




