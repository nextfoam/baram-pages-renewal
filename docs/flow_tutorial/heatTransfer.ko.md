# 열전달

## 복합열전달(Conjugate Heat Transfer)

[격자 파일 다운로드](https://drive.google.com/file/d/1f5GOixMllPA3UXtCD2sPAV8vxpfWI6rI/view?usp=sharing)

[계산 파일 다운로드](https://drive.google.com/file/d/1yyFUzNlA3fyoXMdc9MrLn5vs7zykL4GM/view?usp=sharing)

### 개요 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.1.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.1.png)

본 예제는 고체의 열전도를 포함한 열전달 해석 예제이다. 고체와 유체 2개의 영역(region)으로 구성된 다중영역 방식으로 계산된다. 솔버는 넥스트폼에서 개발한 chtMultiRegionSimpleNFoam를 사용한다.

원형의 유체 영역을 사각형의 고체 영역이 둘러싸고 있다. 유체 영역에 회전하는 막대가 있으며 다중기준좌표계(MRF)를 사용한다. 막대의 온도는 373K이며, 중력은 -y 방향으로 작용한다.

계산 조건은 다음과 같다. 

+ 솔버 : chtMultiRegionSimpleNFoam
+ 난류 모델 : $Standard$ $k-\epsilon$
+ 유체의 물성값
    + 밀도 : Perfect Gas
    + 점성 계수 : 1.79e-5 $kg/ms$
    + 열전도도 : 0.024 $5W/mK$
    + 비열(Cp) : 1,006 $J/kg$
+ 고체의 물성값
    + 밀도 : 1,900 $kg/m^3$
    + 열전도도 : 0.016 $W/mK$
    + 비열(Cp) : 1,004 $J/kg$

### 프로그램의 구동 및 격자

프로그램 실행 후 [새 작업(New Case)]를 선택한다. 시작 창에서 [솔버 유형(Solver Type)]은 [압력기반(Pressure-based)]를, [다상유동 모델(Multiphase Model)]은 [None]을 선택한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/launcher.png"><br>
</p>

격자는 주어진 polyMesh 폴더를 활용한다. 상단 탭에서 [파일(File)]-[격자 불러오기(Load Mesh)]-[OpenFOAM]을 순서대로 클릭하고 polyMesh 폴더를 선택한다. 

### 기본조건(General)

중력 방향은 -y 방향이므로 [중력(Gravity)]는 y축에 -9.81을 입력한다.

### 모델(Models)

난류 모델은 $Standard$ $k-\epsilon$ 모델을 사용한다. 다중영력 격자를 읽으면 에너지(Energy)는 자동으로 포함된다.

### 물질(Materials)

본 예제에서는 Air와 Solid라는 물질을 사용한다. 

물질(Materials)에 디폴트로 Air가 있고, (+) 버튼을 클릭하여 임의의 고체를 선택하여 추가한다. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.2.1.png"><br>
</p>

Air와 Solid의 물성은 다음과 같이 설정한다.

#### Air

+ 밀도 : 완전기체(Perfect Gas)
+ 정압비열($C_p$) : 1,006
+ 점성계수 : 1.79e-05
+ 열전도도 : 0.0245
+ 분자량 : 28.966
+ 흡수계수(Absorption Coefficient) : 0.0

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.2.2.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.2.png"><br>
</p>

#### Solid

+ 밀도 : 1,900
+ 정압비열($C_p$) : 1004
+ 열전도도 : 0.016
+ 방사율(Emissivity) : 0.039

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.3.png"><br>
</p>

흡수계수와 방사율은는 복사열전달을 고려하지 않기 때문에 사용되지 않는다.

### 셀존 조건(Cell zone Conditions) 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.20.png"><br>
</p>

fluid라는 영역(region)을 더블 클릭하고 물질을 Air로 설정한다. 

solid라는 영역(region)을 더블 클릭하고 물질을 Solid로 설정한다.

fluid 영역의 rotor 셀존을 더블 클릭하고 MRF 조건을 다음과 같이 설정한다.

+ 회전속도(Rotating Speed) (RPM) : 100
+ 회전축 중심(Rotation-Axis Origin) : (0, 0, 0)
+ 회전축 방향(Rotation-Axis Direction) : (0, 0, 1)

### 경계조건(Boundary Conditions)

아래와 같이 경계면 타입과 경계값을 설정한다.

+ fluid\_to\_solid : Thermo-Coupled Wall(fluid\_to\_solid와 solid\_to\_fluid는 같은 위치에 있는 면이므로 [연결 벽면(Coupled Boundary)]로 설정)
    + 연결된 경계면(Coupled Boundary) : Region1:solid\_to\_fluid

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.6.png"><br>
</p>

+ inner-wall : 벽면(wall)
    + 속도 조건(Velocity Condition) : 정지(No Slip)
    + 온도(Temperature) : Constant Temperature, 373 (K)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.7.png"><br>
</p>

+ external-wall : 벽면(wall)
    + 속도 조건(Velocity Condition) : 정지(No Slip)
    + 온도 : 외부로 대류열전달(Convection)
    + 열전달계수(Heat Transfer Coefficient) : 4 ($W/m^2 K$)
    + 외부 온도(Free Stream Temperature) : 280 (K)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.8.png"><br>
</p>

+ frontAndBackPlanes : 2차원 경계(Empty)

### 수치해석 기법(Numerical Conditions)

수치해석 기법은 다음과 같이 설정한다.

+ 압력-속도 연성기법(Pressure-Velocity Coupling Scheme) : SIMPLE

+ 이산화 기법(Discretization Schemes)
    + 압력 : Momentum Weighted Reconstruct
    + 운동량 : 2차 상류기법(Second Order Upwind)
    + 에너지 : 2차 상류기법(Second Order Upwind)
    + 난류 : 1차 상류기법(First Order Upwind)

+ 완화 계수(Under-Relaxation Factors) : 디폴트 값 사용

+ 수렴 판정 기준(Convergence Criteria) : 에너지(Energy)는 1e-6을 사용, 나머지는 디폴트 값 사용

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.9.1.png"><br>
</p>


### 모니터(Monitor)

fluid\_to\_solid 면의 [면적가중평균(Area-Weighted Average)]-[온도(Temperature)]를 모니터링한다.

모니터링 탭에서 [추가(Add)]-[면(Surfaces)]를 클릭하여  아래 그림과 같이 설정한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.10.png"><br>
</p>

### 초기화(Initialization)

초기화는 fluid와 solid에 대해 각각 설정한다. 하단의 탭 선택으로 fluid와 solid에 대해 각각 초기화를 설정할 수 있다.

#### fluid

+ 속도 : (0 0 0) (m/s)
+ 압력 : 0 (Pa)
+ 온도 : 350 (K)
+ 난류
    + 속도 크기(Scale of Velocity) : 1 (m/s)
    + 난류 강도(Turbulent Intensity) : 1 (%)
    + 난류 점도 비율(Turbulent Viscosity Ratio) : 10

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.11.png"><br>
</p>

#### solid

+ 온도 : 350 (K)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.11.png"><br>
</p>

값을 입력하고 하단의 [초기화(Initialize)] 버튼을 클릭한다. 그 후, 메뉴의 [파일(File)]-[저장(Save)] 버튼을 클릭하여 저장한다.

### 계산

[계산 조건(Run Conditions)]은 다음과 같이 설정하고 [계산시작(Start Calculation)] 버튼을 누르면 계산이 시작된다.

+ 계산회수(Number of Iterations) : 1000
+ 자동 저장 간격(Save Interval) : 100

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.12.png"><br>
</p>

계산이 시작되면 아래와 같이 잔차(residual)과 모니터링 그래프가 그려진다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.13.png"><br>residual plot
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.14.png"><br>monitoring
</p>

### 후처리

메뉴에서 [외부 프로그램(External tools)]-[ParaView] 버튼을 눌러 ParaView를 실행한다. 

[Mesh Regions]에서 [fluid/internalMesh]와 [solid/internalMesh]가 활성화된 것을 확인한다. 유체와 고체의 격자 모두 활성화한다는 의미이다.

병렬연산이면 [Case Type]을 [Decomposed Case]로 변경한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.15.png"><br>
</p>

상단의 탭에서 [vtkBlockColors]라고 되어 있는 설정을 [T]로 변경한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.16.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.17.png"><br>
</p>

아래 그림과 같이 해석 영역 내부의 온도 분포를 확인할 수 있다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/CHT/10.18.png"><br>
</p>


<!---------------------------------------------------------------------------------------------------------->
