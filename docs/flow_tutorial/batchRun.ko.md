# 일괄계산

## 받음각 변화에 대한 계산(RAE2822 에어포일)

[격자 파일 다운로드](https://drive.google.com/file/d/1XfaXhTFvdUD5P3-avf8ShqQpn-5D25iy/view?usp=sharing)

[계산 파일 다운로드](https://drive.google.com/file/d/1NVP39oK6pboQgZoitJndGW-YSfV85eDD/view?usp=sharing)

### 개요

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/RAE2822/rae-mesh.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/RAE2822/rae-mesh.png)

본 예제는 RAE2822 천음속 에어포일의 받음각 변화에 따른 유동해석을 일괄계산(batch run)으로 진행한다. 격자는 RAE2822 transonic airfoil 튜토리얼의 격자를 사용한다.

계산조건은 다음과 같다.

+ 난류 : 비점성(Inviscid)
+ 마하수 : 1.5
+ 원방경계 압력 : 100000 Pa
+ 원방경계 온도 : 288 K
+ 받음각 : -20~20도, 2도 간격으로 계산

### 프로그램의 구동 및 격자

프로그램 실행 후 [새 작업(New Case)]를 선택한다. 시작 창에서 [솔버 유형(Solver Type)]은 [밀도기반(Density-based)]를 선택한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/RAE2822/launcher-densityBased.png"> 
    <br> 
</p>

격자는 주어진 polyMesh 폴더를 활용한다. 상단 탭에서 [파일(File)]-[격자 불러오기(Load Mesh)]-[OpenFOAM]을 순서대로 클릭하고 polyMesh 폴더를 선택한다. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/1.2.png"><br>
</p>

### 기본조건(General)

본 예제에서는 디폴트 조건을 사용한다.

### 모델(Models)

난류 모델에서 비점성(Inviscid)를 선택한다.

### 물질(Materials)

밀도는 완전기체(Perfect Gas), 점성계수는 Sutherland를 선택한다. 나머지는 디폴트 조건을 사용한다.

### 사용자 변수 선언

일괄계산을 위해 필요한 사용자 변수는 받음각, 속도, 항력방향, 양력방향 등이 있으며 다음과 같이 정의한다.

+ AOA : 받음각, 변수로 사용되지는 않지만 다른 변수를 계산할 때 사용된다.
+ UX, UY : x,y 방향 속도, 초기조건 설정에 사용된다.

[솔루션(Solution)]-[실행(Run)]으로 가서 변수를 선언한다.

[사용자 파라미터(User Parameters)]의 편집 버튼을 눌러 창이 열리면 [파라미터 값(Parameter Values)] 옆의 (+)를 눌러 아래 그림과 같이 하나씩 추가한다. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/batchRun-RAE2822/batchRAE-editParameter.png"> 
    <br> 사용자 변수 선언
</p>


### 경계조건(Boundary Conditions)

경계조건은 다음과 같이 설정한다.

* wing : 벽면(Wall)
    + 속도 조건(Velocity Condition) : 정지(No slip)
    + 온도 조건(Temperature Condition) : 단열(adiabatic)
* farfield\_in, farfield\_out : 압축성 원방 리만(Far-Field Riemann) 
    + 유동 방향(Flow Direction)
        + 설정 방법(Specification Method) : AOA and AOA
        + AOA=0, AOS=0일 때의 방향 : 항력 방향(Drag Direction) (1 0 0), 양력 방향(Lift Direction) (0 1 0)
        + 받음각(AOA) : $AOA
        + 옆미끄럼각(AOS) : 0 
    + 마하수 : 1.5
    + 게이지 압력(Static Pressure) : 100000
    + 온도(Static Temperature) : 288  
  
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/batchRun-RAE2822/batchRAE-farfield.png" > 
    <br>
</p>

+ frontAndBackPlanes : 2차원 경계(Empty)
  
### 기준값(Reference Values)

+ 면적, 길이 : 0.3048(에어포일의 길이)
+ 밀도 : 1.21(원방 조건)
+ 압력 : 100000(원방 조건)
+ 속도 : 510.2611(원방 조건)

### 수치해석 기법(Numerical Conditions)

[Formulation]은 [Implicit], [Flux Type]은 [Roe-FDS]를 사용한다. [Entropy Fix Coefficient]는 0.5를 사용한다. 

[이산화 기법(Discretization Schemes)]에서 [유동(Flow)]은 [2차 상류기법(Second Order Upwind)]를 사용한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/batchRun-RAE2822/rae-nume.png" > 
    <br> 
</p>

### 모니터(Monitor)

[추가(Add)]-[힘(Forces)]를 선택하고 다음과 같이 설정한다.

+ 유동 방향(Flow Direction)
    + 설정 방법(Specification Method) : AOA and AOA
    + AOA=0, AOS=0일 때의 방향 : 항력 방향(Drag Direction) (1 0 0), 양력 방향(Lift Direction) (0 1 0)
    + 받음각 : 2.31
    + 옆미끄럼각 : 0 
+ 회전 중심(Center of Rotation) : (0 0 0)
+ 경계면(Boundaries) : wing

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/batchRun-RAE2822/batchRAE-monitor.png" > 
    <br>
</p>

### 초기화(Initialization)

초기조건은 다음과 같이 설정한다.

+ 속도 : ($UX, $UY, 0)
+ 압력 : 100000
+ 온도 : 288

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/batchRun-RAE2822/batchRAE-init.png" > 
    <br> 
</p>

### 계산

[계산 조건(Run Conditions)]은 다음과 같이 설정한다.

+ 계산 회수(Number of Iterations) : 3000
+ Courant Number : 1000
+ 자동 저장 간격(Save Interval)  : 500

[일괄계산모드로 전환(Switch To Batch Running Mode)] 버튼을 누르면 아래 그림과 같이 [일괄계산 케이스(Batch Cases)] 설정 부분이 나타난다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/batchRun-RAE2822/batchRAE-batchCases.png" > 
    <br> 일괄계산 조건
</p>

각 변수의 조건을 설정한 파일을 [불러오기(import)] 버튼을 눌러 선택하면 조건들이 표시된다. 조건 설정 파일은 csv(comma separated values) 혹은 xlsx 파일 형식을 사용할 수 있다.

이 예제에서는 아래 그림과 같은 엑셀 파일이다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/batchRun-RAE2822/batchRAE-excel.png" > 
    <br> 
</p>

[엑셀파일 다운로드](https://drive.google.com/file/d/1KOb8dQ3D1b2gYoWnwmkhgfGxySfArUBP/view?usp=sharing)


[불러오기(Import)] 버튼을 누르고 위 파일을 선택하면 [일괄계산 케이스(Batch Cases)] 부분이 다음 그림과 같이 바뀐다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/batchRun-RAE2822/batchRAE-batchCases1.png" > 
<br> 
</p>

마우스의 드래그를 이용해서 전체를 선택하고 오른쪽 마우스 버튼을 눌러 [계산목록(Schedule Calculation)]을 선택하면 [계산(Calc.)] 열에 체크 표시가 나타난다. 체크 표시가 된 것들만 계산된다. 

[계산 시작(Start Calculation)]을 누르면 순차적으로 계산이 시작된다. 

제일 왼쪽 열에 현재 계산중인 케이스에 화살표가 나타난다. 계산이 완료된 케이스는 [결과(Result)] 열에 초록색으로 표시되며, 계산 중 발산한 경우는 빨간색으로 표시된다.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/batchRun-RAE2822/batchRAE-run.png "residual")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/batchRun-RAE2822/batchRAE-run.png)

### 후처리

계산이 끝난후 [일괄계산 케이스(Batch Cases)]에서 케이스를 선택하고 마우스 오른쪽 버튼으로 [불러오기(Load)]를 선택하면 해당 케이스의 결과가 활성화 되고 잔차(residual)과 모니터 그래프를 확인할 수 있다. 

메뉴에서 [외부 프로그램(External tools)]-[ParaView] 버튼을 눌러 ParaView를 실행하고 압력을 선택하면 다음과 같은 분포를 확인할 수 있다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/batchRun-RAE2822/batchRAE-paraview.png" > 
    <br> 받음각 -20도 경우의 압력 분포
</p>


