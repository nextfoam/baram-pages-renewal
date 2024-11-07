# 밀도 기반 압축성 솔버

## RAE2822 천음속 에어포일

[격자 파일 다운로드](https://drive.google.com/file/d/1XfaXhTFvdUD5P3-avf8ShqQpn-5D25iy/view?usp=sharing)

[계산 파일 다운로드](https://drive.google.com/file/d/1VD8BLW-qfvr_Vkq9apW7v0GNKfQ6SAyB/view?usp=sharing)

### 개요

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/RAE2822/rae-mesh.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/RAE2822/rae-mesh.png)

본 예제는 밀도기반 솔버를 사용하는 정상상태 압축성 유동해석 예제이다. RAE2822 에어포일 천음속 유동 검증 문제로 아래 웹사이트의 조건을 사용한다.

[https://www.grc.nasa.gov/www/wind/valid/raetaf/raetaf01/raetaf01.html](https://www.grc.nasa.gov/www/wind/valid/raetaf/raetaf01/raetaf01.html)

격자는 정렬격자인 plot3d 격자를 OpenFOAM으로 변환한 것을 사용한다. 원방경계는 farfield\_in, farfield\_out으로 구성되어 있으며 에어포일은 wing이다.

계산 조건은 다음과 같다.

+ 솔버 : TSLAeroFoam
+ 난류모델 : $SST$ $k-\omega$
+ 마하수 : 0.729
+ 받음각 : 2.31 degree
+ 원방경계 압력 : 108988 Pa
+ 원방경계 온도 : 255.556 K
+ 원방경계 난류조건 : k=0.08, omega=7400 

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

난류 모델은 $SST$ $k-\omega$ 모델을 선택한다.

### 물질(Materials)

밀도는 완전기체(Perfect Gas), 점성계수는 Sutherland를 선택한다. 나머지는 디폴트 조건을 사용한다.

### 경계조건(Boundary Conditions)

경계조건은 다음과 같이 설정한다.

* wing : 벽면(Wall)
    + 속도 조건(Velocity Condition) : 정지(No slip)
    + 온도 조건(Temperature Condition) : 단열(adiabatic)

* farfield\_in, farfield\_out : 압축성 원방 리만(Far-Field Riemann) 
    + 유동 방향(Flow Direction)
        + 설정 방법(Specification Method) : AOA and AOA
        + AOA=0, AOS=0일 때의 방향 : 항력 방향(Drag Direction) (1 0 0), 양력 방향(Lift Direction) (0 1 0)
        + 받음각(AOA) : 2.31
        + 옆미끄럼각(AOS) : 0 
    + 마하수 : 0.729
    + 게이지 압력(Static Pressure) : 108988
    + 온도(Static Temperature) : 255.556 
    + 난류(Turbulence) : k and omega (k=0.08, omega=7400)
  
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/RAE2822/rae-farfield.png"> 
    <br> 
</p>

+ frontAndBackPlanes : 2차원 경계(Empty)
  
### 기준값(Reference Values)

+ 면적, 길이 : 0.3048(에어포일의 길이)
+ 밀도 : 1.4858(원방 조건)
+ 압력 : 108988(원방 조건)
+ 속도 : 233.6177(원방 조건)

### 수치해석 기법(Numerical Conditions)

[Formulation]은 [Implicit], [Flux Type]은 [Roe-FDS]를 사용한다. [Entropy Fix Coefficient]는 0.5를 사용한다. 

[이산화 기법(Discretization Schemes)]에서 [유동(Flow)]와 [난류(Turbulence)] 모두 [2차 상류기법(Second Order Upwind)]를 사용한다.

[수렴 판정 기준(Convergence Criteria)]에서 밀도를 1e-6으로 설정한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/RAE2822/rae-nume.png"> 
    <br> 수치해석 조건
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


### 초기화(Initialization)

초기조건은 다음과 같이 설정한다.

+ 속도 : (233.428, 9.416, 0)
+ 압력 : 108988
+ 온도 : 255.556
+ 난류
    + 속도 크기 : 233.6177
    + 난류 강도 : 0.1
    + 난류 점도 비율 : 1 

값을 입력하고 하단의 [초기화(Initialize)] 버튼을 클릭한다. 그 후, 메뉴의 [파일(File)]-[저장(Save)] 버튼을 클릭하여 저장한다.

### 계산

[계산 조건(Run Conditions)]은 다음과 같이 설정하고 [계산시작(Start Calculation)] 버튼을 누르면 계산이 시작된다.

+ 계산 회수(Number of Iterations) : 3000
+ Courant Number : 1000
+ 자동 저장 간격(Save Interval) : 500

계산이 시작되면 아래와 같이 잔차(residual) 그래프가 그려진다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/RAE2822/rae-run.png"> 
    <br> Residual 그래프
</p>

### 후처리

메뉴에서 [외부 프로그램(External tools)]-[ParaView] 버튼을 눌러 ParaView를 실행한다. 

압력을 선택하면 다음과 같은 분포를 확인할 수 있다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/RAE2822/rae-paraview.png"> 
    <br> 
</p>
<!------------------------------------------------------------------------------------------>
## ONERA M6 천음속 날개

[격자 파일 다운로드](https://drive.google.com/file/d/1JxCKWMaAFoi--1_VFXkVVIhtus1N0ntG/view?usp=sharing)

[계산 파일 다운로드](https://drive.google.com/file/d/1mzvMV6pzZt09v1p06xbuY8sdcII3JMhJ/view?usp=sharing)

### 개요

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/onera/onera-mesh.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/onera/onera-mesh.png)


본 예제는 밀도기반 솔버를 사용하는 정상상태 압축성 유동해석 예제이다. ONERA M6 wing의 검증 문제로 아래 사이트의 계산 조건을 사용한다.

[https://www.grc.nasa.gov/WWW/wind/valid/m6wing/m6wing.html](https://www.grc.nasa.gov/WWW/wind/valid/m6wing/m6wing.html)


[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/onera/cp0.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/onera/cp0.png)

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/onera/cp1.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/onera/cp1.png)

<p align='center'>날개의 span 방향 각 섹션의 압력계수</p>


격자는 정렬격자로 만들어진 격자를 OpenFOAM으로 변환한 것을 사용한다. 

계산 조건은 다음과 같다.

+ 솔버 : TSLAeroFoam
+ 난류모델 : $SST$ $k-\omega$
+ 마하수 : 0.8395
+ 받음각 : 3.06 degree
+ 원방경계 압력 : 315979 Pa
+ 원방경계 온도 : 255.56 K
+ 난류 조건 : k=2.714, $\omega$=131360

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

난류 모델은 $SST k - \omega$ 모델을 선택한다.

### 물질(Materials)

밀도는 완전기체(Perfect Gas), 점성계수는 Sutherland를 선택한다. 나머지는 디폴트 조건을 사용한다.

### 경계조건(Boundary Conditions)

경계조건은 다음과 같이 설정한다.

* Wall-wing-1, Wall-wing-2 : 벽면(Wall)
    + 속도 조건(Velocity Condition) : 정지(No slip)
    + 온도 조건(Temperature Condition) : 단열(adiabatic)
+ sym-1, sym-2, sym-3, sym-4 : 대칭(symmetry)
  
* inlet-1, inlet-2, outer-1, outer-2, outlet-1, outlet-2 : 압축성 원방 리만(Far-Field Riemann) 
    + 유동 방향(Flow Direction) : 받음각 3.06에 해당하는 방향, (0.998574, 0.053382, 0) 
    + 마하수 : 0.8395
    + 게이지 압력(Static Pressure) : 315980
    + 온도(Static Temperature) : 255.56 
    + 난류(Turbulence) : k and omega(k = 2.714, omega = 131360)
  
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/onera/onera-farfield.png"> 
    <br> 
</p>

### 수치해석 기법(Numerical Conditions)

[Formulation]은 [Implicit], [Flux Type]은 [Roe-FDS]를 사용한다. [Entropy Fix Coefficient]는 0.5를 사용한다. 

[이산화 기법(Discretization Schemes)]에서 [유동(Flow)]와 [난류(Turbulence)] 모두 [2차 상류기법(Second Order Upwind)]를 사용한다.

[수렴 판정 기준(Convergence Criteria)]에서 밀도의 값을 1e-5으로 설정한다

나머지는 모두 디폴트를 사용한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/RAE2822/rae-nume.png"> 
    <br> 
</p>

### 모니터(Monitor)

[추가(Add)]-[힘(Forces)]를 선택하고 다음과 같이 설정한다.

+ 양력 방향(Lift Direction) : (-0.053382, 0.998574, 0)
+ 항력 방향(Drag Direction) : (0.998574, 0.053382, 0)
+ 경계면(Boundaries) : wall-wing-1, wall-wing-2


### 초기화(Initialization)

초기조건은 다음과 같이 설정한다.

+ 속도 : (268.647, 14.361, 0)
+ 압력 : 315980
+ 온도 : 255.56
+ 난류
    + 속도 크기 : 269.031
    + 난류 강도 : 0.1
    + 난류 점도 비율 : 1 


### 계산

메뉴의 [병렬연산(Parallel)]-[환경설정(Environment)]를 클릭하고 원하는 코어수를 입력한다. 

[계산 조건(Run Conditions)]은 다음과 같이 설정하고 [계산시작(Start Calculation)] 버튼을 누르면 계산이 시작된다.

+ 계산 회수(Number of Iterations) : 3000
+ Courant Number : 1000
+ 자동 저장 간격(Save Interval) : 500

계산이 시작되면 아래와 같이 잔차(residual) 그래프가 그려진다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/onera/onera-run.png"> 
    <br> Residual 그래프
</p>

### 후처리

메뉴에서 [외부 프로그램(External tools)]-[ParaView] 버튼을 눌러 ParaView를 실행한다. 

압력을 선택하면 다음과 같은 분포를 확인할 수 있다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/onera/onera-paraview.png"> 
    <br> 
</p>

<!------------------------------------------------------------------------------------------>
## 초음속 우주왕복선

[격자 파일 다운로드](https://drive.google.com/file/d/12oc-gY76vct8fNCBbF4dNbAVqmuVCNVP/view?usp=sharing)

[계산 파일 다운로드](https://drive.google.com/file/d/1Z4Agp5f_C1MaX_a7aFHvcMGrLrvWLU89/view?usp=sharing)

### 개요

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/ss/ss-mesh.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/ss/ss-mesh.png)


본 예제는 밀도기반 솔버를 사용하는 정상상태 압축성 초음속 유동해석 예제이다.

계산 조건은 다음과 같다.

+ 솔버 : TSLAeroFoam
+ 난류모델 : kOmegaSST
+ 마하수 : 3
+ 받음각 : 15 degree
+ 원방경계 압력 : 100000 Pa
+ 원방경계 온도 : 288 K

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

난류 모델은 $SST k - \omega$ 모델을 선택한다.

### 물질(Materials)

밀도는 완전기체(Perfect Gas), 점성계수는 Sutherland를 선택한다. 나머지는 디폴트 조건을 사용한다.


### 경계조건(Boundary Conditions)

경계조건은 다음과 같이 설정한다.

* spaceShuttle : 벽면(Wall)
    + 속도 조건(Velocity Condition) : 정지(No slip)
    + 온도 조건(Temperature Condition) : 단열(adiabatic)

+ maxy : 대칭(symmetry)
  
* minx, maxx, miny, minz, maxz : 압축성 원방 리만(Far-Field Riemann) 
    + 유동 방향(Flow Direction) : 받음각 15에 해당하는 방향, (0.965926, 0, 0.258819) 
    + 마하수 : 3
    + 게이지 압력(Static Pressure) : 100000
    + 온도(Static Temperature) : 288 
    + 난류 강도(Turbulent Intensity) : 0.1
    + 난류 점도 비율(Turbulent Viscosity ratio) : 1
  
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ss/ss-farfield.png"> 
    <br> 
</p>

  
### 기준값(Reference Values)

+ 면적, 길이 : 1
+ 밀도 : 1.2097(원방 조건)
+ 압력 : 100000(원방 조건)
+ 속도 : 1020.5933(원방 조건)


### 수치해석 기법(Numerical Conditions)

[Formulation]은 [Implicit], [Flux Type]은 [Roe-FDS]를 사용한다. [Entropy Fix Coefficient]는 0.5를 사용한다. 

[이산화 기법(Discretization Schemes)]에서 [유동(Flow)]와 [난류(Turbulence)] 모두 [2차 상류기법(Second Order Upwind)]를 사용한다.

[수렴 판정 기준(Convergence Criteria)]에서 밀도를 1e-5로 설정한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/RAE2822/rae-nume.png"> 
    <br> 
</p>

### 모니터(Monitor)

[추가(Add)]-[힘(Forces)]를 선택하고 다음과 같이 설정한다.

+ 양력 방향(Lift Direction) : (-0.258819, 0, 0.965926)
+ 항력 방향(Drag Direction) : (0.965926, 0, 0.258819)
+ 경계면(Boundaries) : spaceShuttle


### 초기화(Initialization)

초기조건은 다음과 같이 설정한다.

+ 속도 : (985.817, 0, 264.149)
+ 압력 : 100000
+ 온도 : 288
+ 난류
    + 속도 크기 : 1020.5933
    + 난류 강도 : 0.1
    + 난류 점도 비율 : 1 

값을 입력하고 하단의 [초기화(Initialize)] 버튼을 클릭한다. 그 후, 메뉴의 [파일(File)]-[저장(Save)] 버튼을 클릭하여 저장한다.

### 계산

메뉴의 [병렬연산(Parallel)]-[환경설정(Environment)]를 클릭하고 원하는 코어수를 입력한다. 

[계산 조건(Run Conditions)]은 다음과 같이 설정하고 [계산시작(Start Calculation)] 버튼을 누르면 계산이 시작된다.

+ 계산 회수(Number of Iterations) : 3000
+ Courant Number : 0.1
+ 자동 저장 간격(Save Interval) : 500

초음속 유동의 경우 Courant Number를 높게 시작하면 초기에 발산하는 경우가 많아 작은 값으로 시작한 후 계산이 어느 정도 안정되면 조금씩 높여주면 수렴 속도를 높일 수 있다. 계산 중 [계산 조건(Run Conditions)]에서 값을 수정하고 [계산(Run)]에서 [설정 변경 바로 적용(Update Configuration)] 버튼을 누르면 적용된다. 이 예제에서는 0.1로 시작해서 400번 정도에서 값을 1로 높여주고 700번 정도에서 100으로 높여주었다.

계산이 시작되면 아래와 같이 잔차(residual) 그래프가 그려진다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ss/ss-run.png"> 
    <br> 
</p>

### 후처리

메뉴에서 [외부 프로그램(External tools)]-[ParaView] 버튼을 눌러 ParaView를 실행한다.

병렬연산이면 [Case Type]을 [Decomposed Case]로 변경한다.

압력을 선택하면 다음과 같은 분포를 확인할 수 있다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/ss/ss-paraview.png"> 
    <br> 
</p>
<!------------------------------------------------------------------------------------------>
## 초음속 노즐

[격자 파일 다운로드](https://drive.google.com/file/d/1Z5d0Ic9GsMxF1fPr8rSCpv9juU223xuM/view?usp=sharing)

[계산 파일 다운로드](https://drive.google.com/file/d/1Bs0zWE-aVx8dZIik4VDxQt4H3WVuBEYS/view?usp=sharing)

### 개요

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/nozzle/nozzle-mesh.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/nozzle/nozzle-mesh.png)


본 예제는 밀도기반 솔버를 사용하는 축대칭 초음속 노즐 유동해석 예제이다.

계산 조건은 다음과 같다.

+ 솔버 : TSLAeroFoam
+ 난류모델 : kOmegaSST
+ 노즐 입구 전압력 : 4e+5 Pa
+ 노즐 입구 온도 : 3000 K
+ 원방경계 압력 : 1e+4 Pa
+ 원방경계 온도 : 300 K

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

난류 모델은 $SST k - \omega$ 모델을 선택한다.

### 물질(Materials)

밀도는 완전기체(Perfect Gas), 점성계수는 Sutherland를 선택한다. 나머지는 디폴트 조건을 사용한다.

### 경계조건(Boundary Conditions)

경계조건은 다음과 같이 설정한다.

* inlet : 입구 압력(Pressure Inlet)
    + 전압력(Total Pressure) : 400000
    + 온도(Temperature) : 3000
    + 난류 강도(Turbulent Intensity) : 0.1
    + 난류 강도(Turbulent Viscosity Ratio) : 1
+ inletAir : 입구 압력(Pressure Inlet)
    + 전압력(Total Pressure) : 10000
    + 온도(Temperature) : 300 K
    + 난류 강도(Turbulent Intensity) : 0.1
    + 난류 강도(Turbulent Viscosity Ratio) : 1
+ outlet : 출구 압력(Pressure Outlet)
    + 압력(Pressure) : 10000
+ nozzle : 벽면(Wall)
    + 속도 조건(Velocity Condition) : 정지(No slip)
    + 온도 조건(Temperature Condition) : 단열(adiabatic)
+ top : 대칭(symmetry)
  
* bottomEmptyFaces, topEmptyFaces : 축대칭 경계(Wedge)
  
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/nozzle/nozzle-inlet.png"> 
    <br> 
</p>

  
### 수치해석 기법(Numerical Conditions)

[Formulation]은 [Implicit], [Flux Type]은 [Roe-FDS]를 사용한다. [Entropy Fix Coefficient]는 0.5를 사용한다. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/nozzle/nozzle-nume.png"> 
    <br> 
</p>

### 모니터(Monitor)

노즐 입구에서의 유량을 모니터링 한다. [추가(Add)]-[면(Surface)]를 선택하고 다음과 같이 설정한다.

+ 함수(Report Type) : 질량유량(Mas Flow Rate)
+ 면(Surface) : inlet


### 초기화(Initialization)

초기조건은 다음과 같이 설정한다.

+ 속도 : (0, 0, 0)
+ 압력 : 10000
+ 온도 : 300
+ 난류
    + 속도 크기 : 100
    + 난류 강도 : 0.1
    + 난류 점도 비율 : 1 

노즐 유동이 지나는 영역과 외부 영역의 유동 변수들의 차이가 크기 때문에 계산 초기에 솔버가 불안정하다. 이 문제를 해결하기 위해 노즐 유동 부분의 초기값을 별도로 설정한다.

[추가설정(Advanced)]-[영역(Section)]-[만들기(Create)]를 클릭한 후 다음과 같이 설정한다.

+ region1 - 육면체(Hex)
    + 최소(Min. point) : (-0.15, -0.1, -0.1)
    + 최대(Max. point) : (0.12, 0.065 0.1)
    + 속도를 선택하고 (100, 0, 0)을 입력
    + 압력을 선택하고 400000를 입력


<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/nozzle/nozzle-setFields.png">
    <br> Section 설정 및 압력 초기조건
</p>

### 계산

메뉴의 [병렬연산(Parallel)]-[환경설정(Environment)]를 클릭하고 원하는 코어수를 입력한다. 

[계산 조건(Run Conditions)]은 다음과 같이 설정하고 [계산시작(Start Calculation)] 버튼을 누르면 계산이 시작된다.

+ 계산 회수(Number of Iterations) : 100000
+ Courant Number : 1000
+ 자동 저장 간격(Save Interval) : 500

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/nozzle/nozzle-run.png"> 
    <br> 
</p>

### 후처리

메뉴에서 [외부 프로그램(External tools)]-[ParaView] 버튼을 눌러 ParaView를 실행한다.

병렬연산이면 [Case Type]을 [Decomposed Case]로 변경한다.

[Mach]를 선택하면 다음과 같은 분포를 확인할 수 있다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/nozzle/nozzle-paraview.png"> 
    ><br> 
</p>


