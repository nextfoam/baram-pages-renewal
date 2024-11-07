# 다상유동

## 위어(Weir) 유동

[격자 파일 다운로드](https://drive.google.com/file/d/1ODSzqSH9TfIjkmnVNm5p5XLBPX-gjplN/view?usp=sharing)

[계산 파일 다운로드](https://drive.google.com/file/d/1mZsJla3w9X_95I51-dUixL-jbFXyVT4Z/view?usp=sharing)

### 개요 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/main.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/main.png)

위어(Weir)는 수공학에서 수로에 설지하는 구조물로 물이 넘치게 만들어 특정 수위를 유지하거나 유량을 측정하는데 사용한다. CFD에서는 이론식으로 구할 수 있는 유량과 해석 결과를 비교하여 코드 검증용으로 사용하기도 한다.

본 예제는 사각 위어에서 수위가 일정한 경우에 유량 해석을 위한 예제이다.

계산 조건은 다음과 같다.

+ 물의 입구 수위 : 1.6 m, 15696 Pa
+ 솔버 : interFoam
+ 난류모델 : standard 𝑘 − ε 

### 프로그램의 구동 및 격자

프로그램 실행 후 [새 작업(New Case)]를 선택한다. 시작 창에서 [솔버 유형(Solver Type)]은 [압력기반(Pressure-based)]를, [다상유동 모델(Multiphase Model)]은 [Volume of Fluid]를 선택한다. 중력은 (0 0 -9.81)로 설정한다.

격자는 주어진 polyMesh 폴더를 활용한다. 상단 탭에서 [파일(File)]-[격자 불러오기(Load Mesh)]-[OpenFOAM]을 순서대로 클릭하고 polyMesh 폴더를 선택한다. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/launcher.png"><br> 
</p>
<br>

### 기본조건(General)

[시간(Time)]은 [비정상상태(Transient)]로 설정한다.

[중력(Gravity)]는 (0 0 -9.81)을 사용한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/general.png"><br> 
</p>

### 모델(Models)

난류 모델은 $standard$ $k-\epsilon$ 모델을 사용하고 나머지는 디폴트를 사용한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/turbulence.png"><br> 
</p>


### 물질(Materials)

본 예제는 이상유동이므로 두 개의 유체가 필요하다. [물질(Material)]의 상단 오른쪽의 (+)를 누르면 유체를 추가할 수 있다. water-liquid를 추가하고 이름을 water로 바꾸어 준다.

+ water
    + 밀도 : 1000
    + 점성계수 : 0.001
  
+ air
    + 밀도 : 1.225
    + 점성계수 : 1.79e-5

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/material.png"><br> Materials 설정
</p>

### 셀존 조건(Cell zone Conditions) 

[셀존 조건(Cell zone Conditions)]에는 region0가 있다.(다중영역일 때는 여러개의 영역이 표시된다.) 영역의 유체를 설정한다. region0를 더블 클릭하면 설정창이 열린다. [첫번째 유체(Primary Material)]은 air, [두번째 유체(Secondary material)]은 water로 지정한다. 표면장력(Surface Tension)은 0을 사용한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/region.png"><br> Cell Zone Conditions 설정
</p>

### 경계조건(Boundary Conditions)

경계조건은 다음과 같이 설정한다.

+ water-in : 입구 압력(Pressure Inlet)
    + 전압력(Total Pressure) : 15696
    + 난류 강도(Turbulent Intensity) : 1
    + 난류 점도 비율(Turbulent Viscosity Ratio) : 10
    + 체적분율(Volume Fraction, water) : 1
  
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/inlet.png"><br> 
</p>

+ air-in : 입구 압력(Pressure Inlet)
    + 전압력(Total Pressure) : 0
    + 난류 강도(Turbulent Intensity) : 1
    + 난류 점도 비율(Turbulent Viscosity Ratio) : 10
    + 체적분율(Volume Fraction, water) : 0
  
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/inlet-air.png"><br> 
</p>

+ top : 출구 압력(Pressure Outlet), 유입류 조건(Specify Backflow Properties) 
    + 전압력(Total Pressure) : 0
    + 유입류 난류 강도(Backflow Turbulent Intensity) : 1
    + 유입류 난류 점도 비율(Backflow Turbulent Viscosity Ratio) : 10
    + 유입류 체적분율(Backflow Volume Fraction, water) : 0

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/top.png"><br> 
</p>

+ out, out-1 : 유출(Outflow)
  
+ weir, bottom : 벽면(wall)
  
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/wall.png"><br> 
</p>

+ front, front-1, back, back-1 : 대칭(symmetry)


### 수치해석 기법(Numerical Conditions)

수치해석 조건은 다음과 같이 설정한다.

+ 압력-속도 연성 기법(Pressure-Velocity Coupling Scheme) : SIMPLE

+ 운동량방정식 계산(Use Momentum Predictor) : On

+ 이산화 기법(Discretization Scheme)
    + 시간 : 1차 음해법(First Order Implicit)
    + 압력 : Momentum Weighted Reconstruct
    + 운동량 : 2차 상류기법(Second Order Upwind)
    + 난류 : 1차 상류기법(First Order Upwind)
    + 체적분율 : 2차 상류기법(Second Order Upwind)

+ 완화계수(Under-Relaxation Factors) : 모두 1로 설정

+ 안정성 향상(Improve Stability) : Off

+ 시간당 반복계산 회수(Max Iterations per Time Step) : 1

+ 압력보정 회수(Number of Correctors) : 2

+ 다상유동(Multiphase)와 수렴 판정 기준(Convergence Criteria) : 디폴트 사용
  
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/numerical.png"><br> 
</p>


### 모니터(Monitor)

[추가(Add)]-[면(Surface)]를 선택하여 아래 그림과 같이 설정한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/monitor.png"><br> 
</p>


### 초기화(Initialization)

초기조건은 다음과 같이 입력한다.

+ 속도 : (0 0 0)
+ 압력 : 0
+ 속도 크기(Scale of Velocity) : 1
+ 난류 강도(Turbulent Intensity) : 1
+ 난류 점도 비율(Turbulent Viscosity Ratio) : 10
+ 체적분율(Volume Fraction, water) : 0

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/initial.png"><br> 
</p>

water 영역의 초기조건을 주기 위해 두 개의 섹션을 만든다.

[초기화(Initialization)]-[추가설정(Advanced)]-[영역(Section)]-[만들기(Create)]를 클릭한 후 다음과 같이 설정한다.

+ region1
    + 영영 형태(Section Type) : 육면체(Hex)
    + 최소(Min.point) : (0.05 -1 0)
    + 최대(Max.point) : (2 1 0.2)
    + 체적분율(Volume Fraction, water) : 1

+ region2
    + 영영 형태(Section Type) : 육면체(Hex)
    + 최소(Min.point) : (-2 -1 0)
    + 최대(Max.point) : (-0.05 1 1.6)
    + 체적분율(Volume Fraction, water) : 1
  
[경계면도 포함(Override Boundary Value)] 옵션은 사용하지 않는다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/setFields.png"><br> 
</p>

[추가설정(Advanced)]-[영역(Section)]에 두 개의 섹션이 만들어졌고 각 항목의 눈 모양 표시를 클릭하면 영역을 디스플레이 할 수 있다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/setFieldsDisplay.png"><br> 초기화 영역 디스플레이
</p>

하단의 [초기화(Initialize)] 버튼을 클릭한다. 그 후, 메뉴의 [파일(File)]-[저장(Save)] 버튼을 클릭하여 저장한다.


### 계산

메뉴의 [병렬연산(Parallel)]-[환경설정(Environment)]를 클릭하고 원하는 코어수를 입력한다. 

[계산 조건(Run Conditions)]은 다음과 같이 설정하고 [계산시작(Start Calculation)] 버튼을 누르면 계산이 시작된다.

+ 시간 전진 기법(Time Stepping Method) : 적응시간기법(Adaptive)
+ Courant Number : 1
+ Courant Number for VoF : 1
+ 종료시간(End Time) : 20
+ 자동 저장 간격(Save Interval) : 0.1

계산이 시작되면 아래와 같이 모니터링 그래프가 그려진다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/run.png"><br> 유량 모니터링 화면
</p>


### 후처리

물의 흐름을 그려본다.

메뉴에서 [외부 프로그램(External tools)]-[ParaView] 버튼을 클릭해서 paraview를 실행한다.

병렬연산이면 [Case Type]을 [Decomposed Case]로 변경한다.

<p align='left'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/clip.png"> [Clip] 필터를 이용해서 Volume fraction이 0.5 이상인 영역을 잘라준다.
</p>

[Field]를 [U]로 변경한다. Pipeline Browser의 baram.foam을 [Outline]으로 표시하고, weir.stl 파일을 읽어 [Solid Color]로 표시하면 아래 그림과 같이 된다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/pvClip.png"><br>  
</p>

<p align='left'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/weir/play.png"> [Play] 아이콘을 누르면 동영상을 볼 수 있다.
</p>

<!--------------------------------------------------------------------------------------------------->
## 선박 저항 - KCS(KRISO Container Ship)

[격자 파일 다운로드](https://drive.google.com/file/d/1GC8NdJuT3Eog9FWQ9M7XyqoCrVOx1TpP/view?usp=sharing)

[계산 파일 다운로드](https://drive.google.com/file/d/185vzZC9Cc7kboGWYoLyLXaWZawQHKmvr/view?usp=sharing)

### 개요

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/kcs/kcs-wave.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/kcs/kcs-wave.png)


본 예제는 자유수면을 포함하는 선박의 저항 해석에 대한 검증 문제이다. 대상 선형은 KCS이며 아래 논문의 결과와 비교하였다. 자유수면 계산을 위해 interFoam 솔버를 사용하고, LTS(Local Time Step) 기법을 사용하는 정상상태 계산이며 난류모델은$SST$ $k-\omega$를 사용한다. 

_Measurement of flows around modern commercial ship models, Kim,W J.Kim, Van, S H, Kim, D H, Experiments in Fluids, 2001_

계산 및 실험 조건은 다음과 같다.

+ 선박의 속도 : 2.196 $m/s$
+ 기준 면적(침수면적, wetted surface area) : 9.5121 $m^2/s$
+ draft : 0.3418 $m$ (격자에서 z좌표는 0)

### 프로그램의 구동 및 격자

프로그램 실행 후 [새 작업(New Case)]를 선택한다. 시작 창에서 [솔버 유형(Solver Type)]은 [압력기반(Pressure-based)]를, [다상유동 모델(Multiphase Model)]은 [Volume of Fluid]를 선택한다. 중력은 (0 0 -9.81)로 설정한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/kcs/launcher.png"><br> launcher 설정
</p>

격자는 주어진 polyMesh 폴더를 활용한다. 상단 탭에서 [파일(File)]-[격자 불러오기(Load Mesh)]-[OpenFOAM]을 순서대로 클릭하고 polyMesh 폴더를 선택한다. 

### 기본조건(General)

정상상태 계산이기 때문에 모든 설정은 디폴트 조건을 사용한다. 시작 창에서 설정한 중력이 표시된다. 시작 창에서 중력을 주지 않았을 때 여기서 설정할 수 있다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/kcs/general.png"><br> General 설정
</p>

### 모델(Models)

난류 모델은 $SST$ $k-\omega$ 모델을 사용하고 나머지는 디폴트 조건을 사용한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/kcs/turbulence.png"><br> Turbulence Model 설정
</p>

### 물질(Materials)

본 예제는 이상유동이므로 두 개의 유체가 필요하다. [물질(Material)]의 상단 오른쪽의 (+)를 누르면 유체를 추가할 수 있다. water-liquid를 추가하고 이름을 water로 바꾸어 준다.

각 유체의 물성치는 다음과 같이 설정한다.

+ water
    + 밀도 : 1000
    + 점성계수 : 0.001
  
+ air
    + 밀도 : 1.225
    + 점성계수 : 1.79e-5

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/kcs/material.png"><br> 
</p>

### 셀존 조건(Cell zone Conditions) 

[셀존 조건(Cell Zone Conditions)]에는 region0가 있다.(다중영역일 때는 여러개의 영역이 표시된다.) 영역의 유체를 설정한다. region0를 더블 클릭하면 설정창이 열린다. [첫번째 유체(Primary Material)]은 Air, [두번재 유체(Secondary material)]은 water로 지정한다.

표면장력(Surface Tension)은 0을 사용한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/kcs/region.png"><br> 
</p>

### 경계조건(Boundary Conditions)

경계조건은 다음과 같이 설정한다.

+ far\_inlet : 입구 속도(Velocity Inlet)
    + 속도 크기 : 2.196
    + 난류 강도(Turbulent Intensity) : 1
    + 난류 점도 비율(Turbulent Viscosity Ratio) : 10
    + 체적분율(Volume Fraction, water) : 0
  
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/kcs/inlet.png"><br> 
</p>

+ far\_outlet : 개수로 출구(Open Channel Outlet)
    + 속도 크기(Umean) : 2.196
    + 난류 강도(Turbulent Intensity) : 1
    + 난류 점도 비율(Turbulent Viscosity Ratio) : 10
  
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/kcs/outlet.png"><br> 
</p>

+ far\_top : 출구 압력(Pressure Outlet)
    + 전압력(Total Pressure) : 0
    + 유입류 난류 강도(Backflow Turbulent Intensity)  : 1
    + 유입류 난류 점도 비율(Backflow Turbulent Viscosity Ratio)  : 10
    + 유입류 체적분율(Backflow Volume Fraction, water) : 0

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/kcs/top.png"><br> 
</p>

+ KCS\_dummy\_hub, KCS\_hub\_aft, KCS\_hub\_cap, KCS\_hull, KCS\_transom, KCS\_deck : 벽면(wall)
  
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/kcs/wall.png"><br> 
</p>

+ centerplane, far\_side, far\_bottom : 대칭(symmetry)

### 기준값(Reference Values)

유체력계수를 계산하기 위한 기준값을 다음과 같이 설정한다.

+ 면적 : 4.75605(대칭 조건을 사용하므로 침수면적의 절반)
+ 밀도 : 1000
+ 길이 : 7.2786
+ 속도 : 2.196

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/kcs/ref.png"><br> 
</p>

### 수치해석 기법(Numerical Conditions)

수치해석 조건은 다음과 같이 설정한다.

+ 압력-속도 연성 기법(Pressure-Velocity Coupling Scheme) : SIMPLE
+ 운동량방정식 계산(Use Momentum Predictor) : Off
+ 이산화 기법(Discretization Schemes) : 압력은 Momentum Weighted Reconstruct, 나머지는 모두 2차 상류기법(Second Order Upwind)
+ 완화계수(Under-Relaxation Factors) : 모두 1
+ 안정성 향상(Improve Stability) : Off
+ 시간당 반복계산 회수(Max Iteration per Time Step) : 1
+ 압력 보정 회수(Number of Correctors) : 2
+ 다상유동(Multiphase)와 수렴 판정 기준(Convergence Criteria) : 디폴트 사용
  
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/kcs/numerical.png"><br> 
</p>

### 모니터(Monitor)

선박의 유체력계수를 모니터링 한다.

Add-Forces 선택 후 다음과 같이 설정한다.

+ 양력 방향(Lift Direction) : (0 0 1)
+ 항력 방향(Drag Direction) : (-1 0 0)
+ 회전 중심(Center of Rotation) : (0 0 0)
+ 경계면(Boundaries) : KCS\_dummy\_hub, KCS\_hub\_aft, KCS\_hub\_cap, KCS\_hull, KCS\_transom, KCS\_deck

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/kcs/monitor.png"><br> 
</p>


### 초기화(Initialization)

초기조건은 다음과 같이 입력한다.

+ 속도 : (-2.196 0 0)
+ 압력 : 0
+ 속도 크기(Scale of Velocity) : 2.196
+ 난류 강도(Turbulent Intensity) : 1
+ 난류 점도 비율(Turbulent Viscosity Ratio) : 10
+ 체적분율(Volume Fraction, water) : 0

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/kcs/initial.png"><br> 
</p>

water 영역의 초기조건을 주기 위해 섹션을 만든다.

[초기화(Initialization)]-[추가설정(Advanced)]-[영역(Section)]-[만들기(Create)]를 클릭한 후 다음과 같이 설정한다.

* 영영 형태(Section Type) : 육면체(Hex)
+ 최소(Min.point) : (-999 -999 -999)
+ 최대(Max.point) : (999 999 0)
+ 체적분율(Volume Fraction, water)  : 1

[경계면도 포함(Override Boundary Value)] 옵션을 사용하여 경계조건의 값도 바꾸어 준다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/kcs/setFields.png"><br> 
</p>

### 계산

메뉴의 [병렬연산(Parallel)]-[환경설정(Environment)]를 클릭하고 원하는 코어수를 입력한다. 

[계산 조건(Run Conditions)]에서 [계산회수(Number of Iterations)]을 2000으로 준다.

[계산시작(Start Calculation)] 버튼을 누르면 계산이 시작된다.


<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/kcs/parallel.png"><br> 
</p>

계산이 시작되면 아래와 같이 잔차(residual) 그래프가 그려진다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/kcs/residual.png"><br> 
</p>

### 후처리

메뉴에서 [외부 프로그램(External tools)]-[ParaView] 버튼을 클릭해서 ParaView를 실행한다

병렬연산이면 [Case Type]을 [Decomposed Case]로 변경한다.

선박의 경계면들을 선택하고 [Coloring]을 [alpha.water]로 선택하면 다음과 같은 그림을 확인할 수 있다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/kcs/post.png"><br> 
</p>
<!--------------------------------------------------------------------------------------------------->
## 캐비테이션 - NACA66 하이드로포일

[격자 파일 다운로드](https://drive.google.com/file/d/1a6uUi1CKbzZEYa9e4R3-ABR8ZY6LOtKq/view?usp=sharing)

[계산 파일 다운로드](https://drive.google.com/file/d/1iaVyUwowI6nrSYlnIkcniAcM3aiDFBXu/view?usp=sharing)

### 개요 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavitation/cav-contour.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavitation/cav-contour.png)

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavitation/cav-Cp.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavitation/cav-Cp.png)

NACA66 익형 주위의 캐비테이션 해석 검증 문제이다.

캐비테이션 문제 검증용으로 많이 사용되는 아래 논문의 형상 및 조건을 사용하였다.

_Viscous and Nuclei Effects on Hydrodynamic Loadings and Cavitation of NACA66(MOD) Foil section, Y.T.Shen, P.E. Dimotakis, J. Fluids Eng. sep. 1989_

속도는 2.01 m/s 이며, 캐비테이션 수는 0.84 이다.

캐비테이션 모델은 Schnerr-Sauer를 사용하였으며 계수들은 다음과 같다.

* 증발계수(Evaporation Coefficient) : 1
* 응축계수(Condensation Coefficient) : 1
* 기포직경(Bubble Diameter) : 2e-6
* 기포의 수밀도(Bubble Number Density) : 1.6e+9
* 증기압(Vapor pressure) : 2420 Pa

### 프로그램의 구동 및 격자

프로그램 실행 후 [새 작업(New Case)]를 선택한다. 시작 창에서 [솔버 유형(Solver Type)]은 [압력기반(Pressure-based)]를, [다상유동 모델(Multiphase Model)]은 [Volume of Fluid]를 선택한다. 중력은 (0 0 0)으로 설정한다.

격자는 주어진 polyMesh 폴더를 활용한다. 상단 탭에서 [파일(File)]-[격자 불러오기(Load Mesh)]-[OpenFOAM]을 순서대로 클릭하고 polyMesh 폴더를 선택한다. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavitation/launcher.png"><br> 
</p>
<br>

### 기본조건(General)

[시간(Time)]은 [비정상상태(Transient)]로 설정한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavitation/general.png"><br> 
</p>

### 모델(Models)

난류 모델은 $standard$ $k-\epsilon$ 모델을 사용하고 나머지는 디폴트를 사용한다.

### 물질(Materials)

본 예제는 이상유동이므로 두 개의 유체가 필요하다. [물질(Material)]의 상단 오른쪽의 (+)를 누르면 유체를 추가할 수 있다. waterLiquid와 waterVapor를 추가한다. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavitation/material.png"><br> 
</p>

### 셀존 조건(Cell zone Conditions) 

[셀존 조건(Cell Zone Conditions)]에는 region0가 있다.(다중영역일 때는 여러개의 영역이 표시된다.) 영역의 유체를 설정한다. region0를 더블 클릭하면 설정창이 열린다. [첫번째 유체(Primary Material)]은 waterVapor,  [두번째 유체(Secondary material)]은 waterLiquid로 지정한다. 표면장력(Surface Tension)은 0.07을 사용한다.

캐비테이션(Cavitation) 옵션을 켠다. 모델은 [Schnerr-Sauer]를 선택하고 [증기압(Vaporization Pressure)]는 2430을 입력한다. Model Constants 는 다음의 값을 사용한다.

* 증발계수(Evaporation Coefficient) : 1
* 응축계수(Condensation Coefficient) : 1
* 기포직경(Bubble Diameter) : 2.0e-06
* 기포의 수밀도(Bubble Number Density) : 1.6e+9

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavitation/region.png"><br> 
</p>

### 경계조건(Boundary Conditions)

경계조건은 다음과 같이 설정한다.

+ B\_INLET : 입구 속도(Velocity Inlet)
    + 속도 크기(Velocity Magnitude) : 2.01
    + 난류 강도(Turbulent Intensity) : 1
    + 난류 점도 비율(Turbulent Viscosity Ratio) : 10
    + 체적분율(Volume Fraction, waterLiquid) : 1
  
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavitation/inlet.png"><br> 
</p>

+ B\_OUTLET : 출구 압력(Pressure Outlet)
    + 압력(Pressure) : 4113.788
  
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavitation/outlet.png"><br>
</p>

+ B\_SYM : 대칭(Symmetry)
  
+ FOIL\_DOWN, FOIL\_UP : 벽면(Wall)
  
+ EMPTY : 2차원 경계(Empty)


### 수치해석 기법(Numerical Conditions)

수치해석 조건은 다음과 같이 설정한다.

+ 압력-속도 연성 기법(Pressure-Velocity Coupling Scheme) : SIMPLE

+ 운동량방정식 계산(Use Momentum Predictor) : On

+ 이산화 기법(Discretization Scheme)
    + 시간 : 1차 음해법(First Order Implicit)
    + 압력 : Momentum Weighted Reconstruct
    + 운동량 : 2차 상류기법(Second Order Upwind)
    + 난류 : 1차 상류기법(First Order Upwind)
    + 체적분율 : 2차 상류기법(Second Order Upwind)
    
+ 완화계수(Under-Relaxation Factors) : 디폴트 값 사용
+ 안정성 향상(Improve Stability) : Off
+ 시간당 반복계산 회수(Max Iterations per Time Step) : 10
+ 압력보정 회수(Number of Correctors) : 2
+ 다상유동(Multiphase)와 수렴 판정 기준(Convergence Criteria) : 디폴트 사용
  
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavitation/numerical.png"><br> 
</p>

### 초기화(Initialization)

초기조건은 다음과 같이 입력한다.

+ 속도 : (2.01 0 0)
+ 압력 : 4113.788
+ 속도 크기(Scale of Velocity) : 2.01
+ 난류 강도(Turbulent Intensity) : 1
+ 난류 점도 비율(Turbulent Viscosity Ratio) : 10
+ 체적분율(Volume Fraction, waterLiquid) : 1

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavitation/initial.png"><br> 
</p>

하단의 [초기화(Initialize)] 버튼을 클릭한다. 그 후, 메뉴의 [파일(File)]-[저장(Save)] 버튼을 클릭하여 저장한다.


### 계산

메뉴의 [병렬연산(Parallel)]-[환경설정(Environment)]를 클릭하고 원하는 코어수를 입력한다. 

[계산 조건(Run Conditions)]은 다음과 같이 설정하고 [계산시작(Start Calculation)] 버튼을 누르면 계산이 시작된다.

+ 시간 전진 기법(Time Stepping Method) : 일정(Fixed)
+ 시간 전진 간격(Time Step Size) : 0.01
+ 종료시간(End Time) : 10
+ 자동 저장 간격(Save Interval) : 0.5

### 후처리

메뉴에서 [외부 프로그램(External tools)]-[ParaView] 버튼을 클릭해서 ParaView를 실행한다

병렬연산이면 [Case Type]을 [Decomposed Case]로 변경한다.

[Coloring]을 [p\_rgh] 혹은 [alpha.waterLiquid]를 선택하면 압력과 체적분율을 확인할 수 있다.

[![intro](https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavitation/cav-contour.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavitation/cav-contour.png)

하이드로포일 표면의 압력데이터를 얻으려면 원하는 경계면(FOIL\-UP)만 선택하고, 메뉴의 [File]-[Save Data]를 실행하면 csv 형식의 데이터 파일을 얻을 수 있다.







