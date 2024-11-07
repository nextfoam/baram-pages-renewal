# 회전체 유동

## 팬(MRF) 

[격자 파일 다운로드](https://drive.google.com/file/d/1GL268zuyYtKNtp2sKrgOujRIUMROU_Ij/view?usp=sharing)

[계산 파일 다운로드](https://drive.google.com/file/d/1KkySXAnSB0DRb_2hwBreKQEd5xkKnKYG/view?usp=sharing)

### 개요 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/fan/intro.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/fan/intro.png)

본 예제는 정상상태 비압축성 유동해석 예제이다. 단순한 형상의 팬 내부에서 입펠러가 회전할 때 다중기준좌표계(Multiple Reference Frame, MRF)를 사용하여 유동을 예측하는 문제이다.

계산 조건은 다음과 같다. 

+ 솔버 : buoyantSimpleNFoam 
+ 난류 모델 : $Standard$ $k-\epsilon$ model
+ 밀도 : 1.225 $kg/m^3$
+ 점성 계수 : 1.79e-5 $kg/ms$
+ 임펠러 회전 수 : 1,000 RPM
+ 입구 유동 속도 : 10 $m/s$

### 프로그램의 구동 및 격자

프로그램 실행 후 [새 작업(New Case)]를 선택한다. 시작 창에서 [솔버 유형(Solver Type)]은 [압력기반(Pressure-based)]를, [다상유동 모델(Multiphase Model)]은 [None]을 선택한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/launcher.png"><br>
</p>

격자는 주어진 polyMesh 폴더를 활용한다. 상단 탭에서 [파일(File)]-[격자 불러오기(Load Mesh)]-[OpenFOAM]을 순서대로 클릭하고 polyMesh 폴더를 선택한다. 

### 기본조건(General)

모두 디폴트 조건을 사용한다.

### 모델(Models)

모두 디폴트 조건을 사용한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/4.3.png"><br>
</p>

### 물질(Materials)

본 예제에서 작동 유체는 공기이다. 유체의 물성치는 디폴트 조건을 사용한다.

### 셀존 조건(Cell zone Conditions) 

셀존 조건에서는 [다중기준좌표계], [미끄럼 격자], [생성항] 등을 설정할 수 있다. 본 예제에서는 rotating이라는 셀존에 다중기준좌표계(MRF) 조건을 사용한다.

[다중기준좌표계(Multiple Reference Frame, MRF)]를 선택하고 아래 값들을 입력한다.

+ 회전속도(Rotating Speed) : 1000 (RPM)
+ 회전축 중심(Rotation-Axis Origin) : (0 0 0)
+ 회전축 방향(Rotation-Axis Direction) : (0 0 1)
+ 셀존 안의 움직이지 않는 경계면(Static Boundary) : casing

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/fan/mrf.png"><br>
</p>

### 경계조건(Boundary Conditions)

아래와 같이 경계면 타입과 경계값을 설정한다.

+ blade, casing
    + 속도 조건(Velocity Condition) : No Slip

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/4.8.png"><br>
</p>

+ inlet : Velocity Inlet
    + Velocity Specification Method : Magnitudde, Normal to Boundary
    + Profile Type : Constant
    + Velocity Magnitude : 10 (m/s)
    + Turbulent Intensity : 1 (%)
    + Turbulent Viscosity Ratio : 10

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/fan/inletBC.png"><br>
</p>

+ outlet : Pressure Outlet
    + Total Pressure : 0 (Pa)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/4.10.png"><br>
</p>

### 수치해석 기법(Numerical Conditions)

본 예제에서는 아래와 같이 설정을 변경한다.

+ Pressure-Velocity Coupling Scheme : SIMPLE

+ Discretization Scheme
    + Pressure : Linear
    + Momentum : Second Order Upwind
    + Turbulence : Second Order Upwind

+ Under-Relaxation Factors
    + Pressure : 0.3
    + Momentum : 0.5
    + Turbulence : 0.7

+ Convergence Criteria
    + Pressure : 0.001
    + Momentum : 0.001
    + Turbulence : 0.001

### 초기화(Initialization)

모두 디폴트 값을 사용한다.

하단의 Initialize 버튼을 클릭한다. 그 후, File - Save 버튼을 클릭하여 case 파일을 저장한다. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/4.11.png"><br>
</p>

### 계산

메뉴의 [병렬연산(Parallel)]-[환경설정(Environment)]를 클릭하고 원하는 코어수를 입력한다. 

[계산 조건(Run Conditions)]에서 다음과 같이 설정 후 계산을 진행한다.

+ 계산 회수(Number of Iterations) : 1000
+ 자동 저장 간격(Save Interval) : 100

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/fan/runCondition.png"><br>
</p>

아래 그림은 계산이 종료된 상태의 Residuals 그래프이다.

|[![residual](https://github.com/nextfoam/baram-pages/raw/main/screenshots/fan/run.png "residual")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/fan/run.png){:target="_blank"}|

### 후처리

Fan 내부의 압력 분포를 확인해본다. External tools의 paraivew 버튼을 클릭하여 paraview를 실행한다.

Case Type을 Decomposed Case로 변경한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/fan/pv1.png"><br>
</p>

Slice 기능을 활용하여 용기 내부의 단면을 자른다.

Z-normal 버튼을 클릭 후, Origin의 z 값에 0.01을 입력한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/fan/pv2.png"><br>
</p>

아래 그림과 같이 fan 내부 속도 분포가 나오게 된다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/fan/pv3.png"><br>
</p>

<!-------------------------------------------------------------------------------------------------------->
## Mixer(MRF)

[격자 파일 다운로드](https://drive.google.com/file/d/1hDPj81oeEhsQD86Uu9fUZmnIaglgth7c/view?usp=sharing)

[계산 파일 다운로드](https://drive.google.com/file/d/1UBi_opGRIYnGxDFeFihTukVEDOI52h2m/view?usp=sharing)

### 개요 

[![속도/압력 분포](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixer/intro.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixer/intro.png)

본 예제는 다순한 형상의 믹서를 MRF를 이용하여 계산하는 예제이다. MRF 모델을 사용하면 정상상태 계산을 할 수 있으며, 임펠러 날개 하나를 기준으로 회전 주기조건을 사용할 수 있어 계산 비용을 크게 줄일 수 있다. 

계산 조건은 다음과 같다. 

+ solver : buoyantPimpleNFoam (넥스트폼이 개발한 비압축성 유동 해석 솔버)
+ 난류 모델 : $Standard$ $k-\epsilon$ model
+ 밀도 : 1000 $kg/m^3$
+ 점성 계수 : 0.001 $kg/ms$
+ 프로펠러 회전 수 : 100 RPM

### 프로그램의 구동 및 격자

프로그램 실행 후 launcher에서 ‘New’를 선택한다. Launcher에서 ‘Solver Type’은 Pressure-based를, ‘Multiphase Model’은 None, ‘Species’는 Not Include를 선택한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/launcher.png"><br>
</p>

격자는 주어진 polyMesh 폴더를 활용한다. 상단 탭에서 File - Load Mesh - OpenFOAM을 클릭하고 polyMesh 폴더를 선택한다. 

### 기본조건(General)

모두 Default를 사용한다.

### 모델(Models)

모두 Default를 사용한다.

### 물질(Materials)

Name을 water로 변경하고 Density는 1000, viscosity는 0.001을 입력한다.

### 셀존 조건(Cell zone Conditions) 

'cellZone'에 Multiple Reference Frame, MRF를 선택하고 아래 값들을 입력한다.

+ Multiple Reference Frame
    + Rotating Speed : 100(RPM)
    + Rotation-Axis Origin : (0 0 0)
    + Rotation-Axis Direction : (0 0 1)
    + Static Boundary : periodic1, periodic2

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixer/cellZone.png"><br>
</p>

### 경계조건(Boundary Conditions)

아래와 같이 경계면 타입과 경계값을 설정한다.

+ periodic1, periodic2 : Interface - Rotational Periodic
    + periodic1 : Rotational Periodic으로 변경 후, Coupled Boundary는 periodic2 선택. Rotation-Axis Origin은 (0 0 0), Rotation-Axis Direction은 (0 0 1)을 입력

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixer/interface.png"><br>
</p>

+ impeller, impeller_slave : Thermo-Coupled Wall
    + impeller : Thermo-Coupled Wall로 변경 후, Coupled Boundary는 impeller_slave를 선택

+ wall, hub, hub_rot : Wall
    + 속도 조건(속도 조건(Velocity Condition)) : noSlip

+ top : symmetry


### 수치해석 기법(Numerical Conditions)

모두 Default를 사용한다.


### 초기화(Initialization)

모두 Default를 사용한다.


### 계산

Run Conditions에서 Number of Iteration을 3000으로 설정하고 계산을 진행한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixer/residual.png"><br>
</p>

<!-------------------------------------------------------------------------------------------------------->
## 시로코팬(Sliding mesh) 

[격자 파일 다운로드](https://drive.google.com/file/d/1479KBQebn3GnVG_X5KlY9JBejJn4dX3Q/view?usp=sharing)

[계산 파일 다운로드](https://drive.google.com/file/d/1n7kY5ZSO4I9PsH1TVDLI0nTRLrl8lRhh/view?usp=sharing)

### 개요 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/intro.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/intro.png)

본 예제는 비정상상태 비압축성 유동해석 예제이다. 시로코팬 내부에서 입펠러가 회전할 때 내부의 유동을 예측하는 문제이다.

다중기준좌표계(MRF) 방법을 사용해서 정상상태 해석을 하고, 이 결과를 초기조건으로 미끄럼 격자(sliding mesh) 방법으로 비정상상태 계산을 한다.

계산 조건은 다음과 같다. 

+ 솔버 : buoyantPimpleNFoam
+ 난류 모델 : $Standard$ $k-\epsilon$ model
+ 밀도 : 1.225 $kg/m^3$
+ 점성 계수 : 1.79e-5 $kg/ms$
+ 임펠러 회전 수 : 2,000 RPM

### 프로그램의 구동 및 격자

프로그램 실행 후 [새 작업(New Case)]를 선택한다. 시작 창에서 [솔버 유형(Solver Type)]은 [압력기반(Pressure-based)]를, [다상유동 모델(Multiphase Model)]은 [None]을 선택한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/launcher.png"><br>
</p>

### 격자

격자는 fluent 격자를 변환하여 사용한다. 

메뉴에서 [File]-[Load Mesh]-[Fluent(ASCII)]를 선택하면 파일 선택창이 열린다. 다운로드한 siroccofan.msh 파일을 선택하면 아래와 같은 창이 열린다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/fluentToFoam.png"><br>
</p>

이 격자는 fluid와 rotating이라는 두 개의 셀존을 갖고 있다. 이 두개의 셀존과 region1, region2라는 두 개의 영역(region)이 표시되어 있다. region2 오른쪽의 (+) 아이콘을 누르면 영역이 추가된다. 다중영역 격자로 변환하기 위한 옵션이다. 이 문제는 다중영역 문제가 아니기 때문에 두 셀존 모두 region1으로 두면 된다. region2 아래의 휴지통 아이콘을 눌러 region2를 삭제해도 된다. 

### 정상상태 계산

#### 기본조건(General)

[시간(Time)]은 [정상상태(Steady)], [중력(Gravity)]는 (0 0 0), [작동 조건(Operating Conditions)]는 101325를 사용한다. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/general-steady.png"><br>
</p>

#### 모델(Models)

난류 모델은 $Standard$ $k-\epsilon$ 모델을 사용한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/4.3.png"><br>
</p>

#### 물질(Materials)

본 예제에서 작동 유체는 공기이다. 유체의 물성치는 Default 조건을 사용한다.

#### 셀존 조건(Cell zone Conditions) 

셀존 조건에서 rotating을 더블 클릭하면 새로운 창이 열린다. [다중기준좌표계(MUltiple Reference Frame, MRF)]를 선택하고 아래 값들을 입력한다.

+ 회전속도(Rotating Speed) : 2,000 (RPM)
+ 회전축 중심(Rotation-Axis Origin) : (0, 0, 0)
+ 회전축 방향(Rotation-Axis Direction) : (0, 0, 1)
+ 셀존 안의 움직이지 않는 경계면(Static Boundary) : interface-rotating, interface-stat
    + MRF를 사용할 셀존에 있는 경계면들 중 회전하지 않는 면들을 선택해 준다. 두 개의 인터페이스 면들 중 하나는 해당 셀존에 있고 나머지는 바깥에 있다. 보통은 어느 것이 해당되는지 알기 어렵기 때문에 두 개를 모두 선택하면 된다. 해당 셀존 밖에 있는 경계면이 포함되어도 문제되지 않는다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/mrf.png"><br>
</p>

#### 경계조건(Boundary Conditions)

아래와 같이 경계면 타입과 경계값을 설정한다.

+ interface-stat, interface-rotating : 내부 인터페이스(Internal Interface)
    + interface-stat : 내부 인터페이스(Internal Interface)로 변경 후, 연결된 경계면(Coupled Boundary)는 interface-rotating 선택

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/4.5.png"><br>
</p>

+ axis : Wall
    + 속도 조건(Velocity Condition) : 회전속도(Rotational Moving Wall)
    + 회전속도(Speed) : 2000 (RPM)
    + 회전축 중심(Rotation-Axis Origin) : 0 0 0
    + 회전축 방향(Rotation-Axis Direction) : 0 0 1

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/4.6.png"><br>
</p>

+ axis-r, blades, externalwalls, walls : 벽면(Wall)
    + 속도 조건(Velocity Condition) : 정지(No Slip)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/4.8.png"><br>
</p>

+ inlet : 입구 속도(Velocity Inlet)
    + 속도 크기(Velocity Magnitude) : 1 (m/s)
    + 난류 강도(Turbulent Intensity) : 1 (%)
    + 난류 점도 비율(Turbulent Viscosity Ratio) : 10

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/4.9.png"><br>
</p>

+ outlet : 출구 압력(Pressure Outlet)
    + 압력(Pressure) : 0 (Pa)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/4.10.png"><br>
</p>

#### 수치해석 기법(Numerical Conditions)

디폴트 조건을 사용한다.

#### 초기화(Initialization)

모두 디폴트 값을 사용한다.

값을 입력하고 하단의 [초기화(Initialize)] 버튼을 클릭한다. 그 후, 메뉴의 [파일(File)]-[저장(Save)] 버튼을 클릭하여 저장한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/initial.png"><br>
</p>

#### 계산

메뉴의 [병렬연산(Parallel)]-[환경설정(Environment)]를 클릭하고 원하는 코어수를 입력한다. 

[계산 조건(Run Conditions)]은 다음과 같이 설정하고 [계산시작(Start Calculation)] 버튼을 누르면 계산이 시작된다.

+ [계산회수(Number of Iterations)] : 1000
+ [자동 저장 간격(Save Interval)] : 100


<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/run.png"><br>
</p>

유동이 비정상상태이기 때문에 잔차(residual)이 수렴하지는 않지만, 비정상상태 계산을 위한 초기조건으로는 사용하기에 문제는 없다.

### 비정상상태 계산

#### 기본조건(General)

[시간(Time)]을 [비정상상태(Transient)]로 바꾼다. 다음과 같은 창이 나타난다. 정상상태 계산 결과를 비정상상태 계산의 초기조건으로 사용할 것인지를 묻는다. [Yes]를 선택하면 마지막으로 저장된 데이터가 초기조건으로 설정되고, 시간 0부터 계산이 시작된다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/toTransient.png"><br>
</p>

#### 셀존 조건(Cell zone Conditions) 

'rotating' 셀존의 조건을 [미끄럼 격자(Sliding Mesh)]로 바꾼다. [셀존 안의 움직이지 않는 경계면(Static Boundary)] 설정 부분은 사라진다.

#### 경계조건(Boundary Conditions)

움직이는 벽면에 대한 경계조건을 다음과 같이 바꾸어 준다.

+ axis-r, blades : 벽면(Wall)
    + 속도 조건(Velocity Condition) : 움직이는 벽(Moving Wall)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/4.7.png"><br>
</p>

#### 수치해석 기법(Numerical Conditions)

디폴트 조건을 사용한다.

#### 계산

[계산 조건(Run Conditions)]은 다음과 같이 설정하고 [계산시작(Start Calculation)] 버튼을 누르면 계산이 시작된다.

+ 시간 전진 기법(Time Stepping Method) : 일정(Fixed)
+ 시간 전진 간격(Time Step Size) : 0.0001
+ 종료 시간(End Time) : 0.3

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/4.12.png"><br>
</p>

계산이 시작되면 아래와 같이 잔차(residual)과 유체력 계수의 그래프가 그려진다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/4.13.png"><br>
</p>

### 후처리

팬 내부의 압력 분포를 확인해본다.  메뉴에서 [외부 프로그램(External tools)]-[ParaView] 버튼을 클릭해서 ParaView를 실행한다. 

병렬연산이면 [Case Type]을 [Decomposed Case]로 변경한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/4.14.png"><br>
</p>

[Slice] 기능을 활용하여 용기 내부의 단면을 자른다. [Z-normal] 버튼을 클릭 후, [Origin]을 다음과 같이 변경한다.

+ Origin : (0.06 -0.017 0.05)
+ Normal : (0 0 1)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/4.15.png"><br>
</p>

아래 그림과 같이 팬 내부 속도 분포가 그려진다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/4.16.png"><br>
</p>

<!-------------------------------------------------------------------------------------------------------->
## 프로펠러 

[계산 파일 다운로드](https://drive.google.com/file/d/1DD4Ba9iBaVpgORhmzyh1ppTtHYXIYX1t/view?usp=sharing)

### 개요 

[![속도분포](https://github.com/nextfoam/baram-pages/raw/main/screenshots/propeller/intro.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/propeller/intro.png)

본 예제는 OpenFOAM의 pimpleFoam tutorial에 있는 propeller 문제를 BaramFlow로 계산하는 예제이다. [미끄럼 격자(Sliding mesh)] 기법을 사용하여 프로펠러의 회전에 따른 유동을 예측하는 문제이다.

계산 조건은 다음과 같다. 

+ 솔버 : buoyantPimpleNFoam 
+ 난류 모델 : $Realizable$ $k-\epsilon$ model
+ 밀도 : 1000 $kg/m^3$
+ 점성 계수 : 0.001 $kg/ms$
+ 프로펠러 회전 수 : 1432 $RPM$
+ 입구 속도 : 5 $m/s$

### 프로그램의 구동 및 격자

프로그램 실행 후 [새 작업(New Case)]를 선택한다. 시작 창에서 [솔버 유형(Solver Type)]은 [압력기반(Pressure-based)]를, [다상유동 모델(Multiphase Model)]은 [None]을 선택한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/launcher.png"><br>
</p>

격자는 주어진 polyMesh 폴더를 활용한다. 상단 탭에서 [파일(File)]-[격자 불러오기(Load Mesh)]-[OpenFOAM]을 순서대로 클릭하고 polyMesh 폴더를 선택한다. 

### 기본조건(General)

[시간(Time)]을 [비정상상태(Transient)]로 변경한다. 나머지는 디폴트를 사용한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/slidingMesh/4.2.png"><br>
</p>

### 모델(Models)

난류 모델은 $Realizable$ $k-\epsilon$ 모델을 사용하고 [벽함수 옵션(Near-Wall Treatment)]는 [two layer 모델(Enhanced Wall Treatment)]를 선택한다. 나머지는 디폴트 조건을 사용한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/propeller/tur.png"><br>
</p>

### 물질(Materials)

이름을 water로 변경하고 밀도(Density)는 1000, 점성계수(viscosity)는 0.001을 입력한다.

### 셀존 조건(Cell zone Conditions) 

셀존 조건에서는 [다중기준좌표계], [미끄럼 격자], [생성항] 등을 설정할 수 있다. 

본 예제에서는 cellZone에 [미끄럼 격자(Sliding Mesh)] 조건을 설정한다.

+ 회전속도(Rotating Speed) : 1432 (RPM)
+ 회전축 중심(Rotation-Axis Origin) : (0 0 0)
+ 회전축 방향(Rotation-Axis Direction) : (-1 0 0)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/propeller/cellZone.png"><br>
</p>

### 경계조건(Boundary Conditions)

아래와 같이 경계면 타입과 경계값을 설정한다.

+ cellZone\_surface, cellZone\_surface\_slave : 내부 인터페이스(Internal Interface)
    + cellZone\_surface : 내부 인터페이스로 변경 후, 연결된 벽면(Coupled Boundary)는 cellZone\_surface\_slave 선택

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/propeller/interface.png"><br>
</p>

+ propeller, propellerStem : 벽면(Wall)
    + 속도 조건(Velocity Condition) : 움직이는 벽(Moving Wall)

+ far\_surface : 벽면(Wall)
    + 속도 조건(Velocity Condition) : 미끄럼 벽(Slip)

+ inlet : 입구 속도(Velocity Inlet)
    + 속도 크기(Velocity Magnitude) : 5 (m/s)
    + 난류 강도(Turbulent Intensity) : 1 (%)
    + 난류 점도 비율(Turbulent Viscosity Ratio) : 10

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/propeller/inlet.png"><br>
</p>

+ outlet : 출구 압력(Pressure Outlet)
    + 압력(Pressure) : 0 (Pa)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/propeller/outlet.png"><br>
</p>

### 수치해석 기법(Numerical Conditions)

[시간당 반복계산 회수(Max iterations per Time Step)]을 20, [압력보정 회수(Number of Correctors)]를 2로 설정한다. 나머지는 디폴트 조건을 사용한다.

### 초기화(Initialization)

X-속도는 5, [속도 크기(Scale of Velocity)]는 5, 나머지는 디폴트 값을 사용한다.

하단의 [초기화(Initialize)] 버튼을 클릭한다. 그 후, 메뉴의 [파일(File)]-[저장(Save)] 버튼을 클릭하여 저장한다.

### 계산

메뉴의 [병렬연산(Parallel)]-[환경설정(Environment)]를 클릭하고 원하는 코어수를 입력한다. 

[계산 조건(Run Conditions)]은 다음과 같이 설정하고 [계산시작(Start Calculation)] 버튼을 누르면 계산이 시작된다.

+ 시간 전진 기법(Time Stepping Method) : 일정(Fixed)
+ 시간 전진 간격(Time Step Size) : 1e-5
+ 종료 시간(End Time) : 0.1
+ 자동 저장 간격(Save Interval) : 0.001

<!-------------------------------------------------------------------------------------------------------->
## 팬이 있는 실내 공간

### 개요 

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/room/room-intro.png" ><br>
</p>

본 예제는 미끄럼 격자(sliding mesh)를 이용한 계산 예제이다.

실내에 100 RPM으로 회전하는 팬이 창을 통해 유동이 흐르는 간단한 문제이다.

계산 조건은 다음과 같다.

+ 솔버 : buoyantSimpleNFoam
+ 난류 모델 : $Standard$ $k-\epsilon$
+ 팬 회전 속도 : 100 RPM

### 프로그램의 구동 및 격자

BaramFlow 실행 후 [기존 계산 열기(Open)]을 선택하고 [격자 생성 튜토리얼](https://baramcfd.org/mesh/2024/03/21/fanInRoomMesh-post/)에서 만든 폴더를 선택한다(혹은 [새 계산(New Case)]를 선택하고 메뉴의 [파일(File)]-[격자불러오기(Load Mesh)]-[OpenFOAM]에서 [caseName]/case/constant 폴더를 선택한다).

시작 창에서 [솔버 유형(Solver Type)]은 [압력기반(Pressure-based)]를, [다상유동 모델(Multiphase Model)]은 [None]을 선택한다.

### 기본조건(General)

비정상상태 등온 단상유동 계산이다. [시간(Time)]은 [비정상상태(Transient)]로 설정한다.

### 모델(Models)

난류 모델은 Standard $k-\epsilon$ 모델을 사용하고 나머지는 디폴트를 사용한다.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixer/3.3.png"><br> 난류 모델 설정
</p>

### 물질(Materials)

본 예제의 작동유체는 공기이다. 디폴트 값을 사용한다.

### 셀존 조건(Cell zone Conditions) 

AMI라는 셀존을 더블 클릭하면 설정 창이 열린다. [셀존 종류(Zone Type)]을 [미끄럼 격자(Sliding Mesh)]로 주고 다음과 같이 설정한다.

+ 회전속도(Rotating Speed) : 100 RPM
+ 회전축 중심(Rotation Axis Origin) : (-3 2 2.6)
+ 회전축 방향(Rotation Axis Direction) : (0 0 1)

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/room/room-cellZone.png" >
    <br> 셀존 설정
</p>

### 경계조건(Boundary Conditions)

아래와 같이 경계면 타입과 경계값을 설정한다.

+ desk\_surface_0, door, room : 벽면(Wall)
    + 속도조건 : 정지(No Slip)

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/room/room-noSlip.png" >
</p> 

+ fan\_surface_0 : 벽면(Wall)
    + 속도조건 : 움직이는 벽(Moving Wall)

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/room/room-movingWall.png" >
</p>

+ outlet : 출구 압력(Pressure Outlet)
    + 압력(Pressure) = 0

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/room/room-outlet.png" >
</p>

+ AMI\_surface\_0, AMI\_surface\_0\_slave : 내부 인터페이스(Internal Interface)

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/room/room-interface.png" >
</p>

전체 설정을 완료하면 아래 그림과 같이 된다.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/room/room-bc.png" >
    <br> 경계조건 설정
</p> 

### 수치해석 기법(Numerical Conditions)

본 예제에서는 아래와 같이 설정을 변경한다. 

+ 압력-속도 연성기법(Pressure-Velocity coupling) : SIMPLE

+ 운동량방정식 계산(Use Momentum Predictor) : On

+ 이산화 기법(Discretization)
    + 시간 : 1차 음해법(First Order Implicit)
    + 압력 : Linear
    + 운동량 : 2차 상류기법(Second Order Upwind)
    + 난류 : 1차 상류기법(First Order Upwind)

+ 완화계수(Relaxation factors) : 모두 1로 설정

* 시간당 반복계산 회수(Max iterations per Time Step) : 3

* 압력보정 회수(Number of Correctors) : 1

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/room/room-nume.png" >
    <br> 수치해석 조건 설정
</p>

### 초기화(Initialization)

초기값은 디폴트 값을 그대로 사용한다.

하단의 [초기화(Initialize)] 버튼을 클릭한다. 그 후, 메뉴의 [파일(File)]-[저장(Save)] 버튼을 클릭하여 저장한다.

### 계산

[계산 조건(Run Conditions)]은 다음과 같이 설정하고 [계산시작(Start Calculation)] 버튼을 누르면 계산이 시작된다.

+ 시간 전진 기법(Time Stepping Method) : 적응시간기법(Adaptive)
+ Courant Number : 1
+ 종료 시간(EndTime) : 1
+ 자동 저장 간격(Save Interval) : 0.02

계산이 시작되면 아래와 같이 잔차(residual) 그래프가 그려진다.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/room/room-residual.png">
    <br> 
</p> 


