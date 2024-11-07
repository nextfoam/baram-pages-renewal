# 비뉴턴유체(non-Newtonian flow)

## 혈액 유동 - FDA Nozzle 

[격자 파일 다운로드](https://drive.google.com/file/d/1fq6KLFt2mpidQchHwQ9j9mRgty5rlr3G/view?usp=sharing)

[계산 파일 다운로드](https://drive.google.com/file/d/1GT4riUP0E9Niwrw6LWrh0ZV0l49YlZJG/view?usp=sharing)

### 개요 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/blood/intro.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/blood/intro.png)

‘FDA’s Nozzle Challenge’는 미국 식품의약청이 주관한 시뮬레이션 검증을 위한 벤치마크 테스트 프로그램이다. 이 프로그램에서 혈액 운반 의료기기의 특성을 반영하는 소형 노즐에 대한 실험 및 시뮬레이션 연구가 진행되었고, 관련 논문 중 Trias 등의 다음 논문을 참고했다.

_Trias, Miquel, Antonio Arbona, Joan Massó, Borja Miñano, and Carles Bona. “FDA’s nozzle numerical simulation challenge: non-Newtonian fluid effects and blood damage.” PloS one 9, no. 3 (2014): e92638._

노즐 목 직경을 기준으로 Reynolds number는 500이며, 비뉴턴유체 점성은 Bird-Carreau 모델을 사용한다.

### 프로그램의 구동 및 격자

프로그램 실행 후 [새 작업(New Case)]를 선택한다. 시작 창에서 [솔버 유형(Solver Type)]은 [압력기반(Pressure-based)]를, [다상유동 모델(Multiphase Model)]은 [None]을 선택한다.

격자는 주어진 polyMesh 폴더를 활용한다. 상단 탭에서 [파일(File)]-[격자 불러오기(Load Mesh)]-[OpenFOAM]을 순서대로 클릭하고 polyMesh 폴더를 선택한다. 

형상과 격자는 다음과 같다.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/blood/fda-diagram.png"><br>
</p>

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/blood/fda-mesh.png"><br>
</p>


### 기본조건(General)

본 예제에서는 디폴트 조건을 사용한다.

### 모델(Models)

난류 모델은 층류(Laminar)를 사용하고 나머지는 디폴트를 사용한다.

### 물질(Materials)

[물질(Materials)]의 (+)를 클릭하여 waterLiquid를 추가한다. 

waterLiquid 편집창을 열고 다음과 같이 설정한다.

+ 이름 : blood
+ 밀도 : 1056
+ 점도(Viscosity) : Bird-Carreau

Bird-Carreau 옆의 편집(Edit) 버튼을 눌러 계수를 다음과 같이 입력한다.

+ Zero shear viscosity : 0.0001515
+ Infinite shear viscosity : 3.3144e-6
+ Relaxation time = 8,2
+ Power-law index = 0.2128
+ Linearity deviation, a = 0.64

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/blood/material.png"><br>
</p>

### 셀존 조건(Cell zone Conditions) 

region0를 더블 클릭하면 설정창이 나타난다. 물질(Material)을 blood로 선택한다.


### 경계조건(Boundary Conditions)

아래와 같이 경계면 타입과 경계값을 설정한다.

+ in : 입구 속도(Velocity Inlet)
    + 속도 크기(Velocity magnitude) : 0.04607

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/blood/bc-in.png">
</p>

+ outlet : 출구 압력(Pressure Outlet)
    + 압력(Pressure) : 0

+ wall, wall1, wall2 : 벽면(Wall)
    + 속도 조건(Velocity Condition) : 정지(No Slip)

+ back, front : 축대칭 경계(Wedge)


### 수치해석 기법(Numerical Conditions)

수렴 판정 기준(Convergence Criteria)에서 압력(Pressure)와 운동량(Momentum)의 값을 1e-6으로 입력한다.

나머지는 모두 디폴트를 사용한다.

### 초기화(Initialization)

초기조건은 다음과 같이 설정한다.

+ 속도 : (0.04607 0 0)
+ 압력 : 0

값을 입력하고 하단의 [초기화(Initialize)] 버튼을 클릭한다. 그 후, 메뉴의 [파일(File)]-[저장(Save)] 버튼을 클릭하여 저장한다.

### 계산

[계산 조건(Run Conditions)]은 다음과 같이 설정하고 [계산시작(Start Calculation)] 버튼을 누르면 계산이 시작된다.

+ 계산 회수(Number of Iterations) : 2000
+ 자동 저장 간격(Save Interval) : 500

계산이 시작되면 아래와 같이 잔차(residual) 그래프가 그려진다.

[![intro](https://github.com/nextfoam/baram-pages/raw/main/screenshots/blood/residual.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/blood/residual.png)


### 후처리

메뉴에서 [외부 프로그램(External tools)]-[ParaView] 버튼을 눌러 ParaView를 실행한다.

[Coloring]을 [U]로 선택한다.

축에서 속도 그래프를 확인하기 위해 [Fileters] 메뉴에서 [Plot Over Line]을 선택한다.

[Line Parameters]에서 Point1은 (-0.1 0 0.001), Point2는 (0.1 0 0.001)을 입력한다. 

[X Axis Parameters]에서 [Use Index for XAxis]를 비활성화하고, [X Array Name]은 [Point_X]를 선택한다.

[Series Parameters]에서 [U_X]를 선택하고 [Apply] 버튼을 누르면 아래와 같은 그래프를 확인할 수 있다.

[![intro](https://github.com/nextfoam/baram-pages/raw/main/screenshots/blood/paraview.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/blood/paraview.png)

[File] 메뉴의 [Save Data]를 실행하면, 축에서 속도를 데이터 파일로 얻을 수 있다. 아래 그림은 이 데이터를 실험결과와 점성을 상수로 준 경우의 결과와 비교하여 그린 것이다.

[![intro](https://github.com/nextfoam/baram-pages/raw/main/screenshots/blood/result.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/blood/result.png)

