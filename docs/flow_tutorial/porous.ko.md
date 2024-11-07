# 다공성 매질

## 다공성 셀존(Porous cell zone)

[격자 파일 다운로드](https://drive.google.com/file/d/1V0pE28Q-8MEHR-VXaE6upP5mgt_cImGF/view?usp=sharing)

[계산 파일 다운로드](https://drive.google.com/file/d/1WPrNMbi0e91vl34ZO3O_HFbzCyE2QQJS/view?usp=sharing)

### 개요 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousMedia/intro.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousMedia/intro.png)

본 에제는 다공성 셀존 조건을 사용한 유동해석 예제이다. 상부 왼쪽에서 유동이 유입되고 porous 영역을 지나 아래로 유동이 흐르는 문제이다.(위 그림 왼쪽에서 파란색 부분이 다공성 매질 영역)

OpenFOAM의 다공성 셀존 모델은 다공성 매질 영역에서 불연속적인 속도 분포가 나타나고 압력손실도 입력 조건과 조금 다른 결과를 보이는 문제가 있다. 아래 그림의 왼쪽이 Baram-v24의 결과이며, 오른쪽이 OpenFOAM 2306의 표준 솔버를 사용한 결과이다. 

Baram이 사용하는 NextFOAM에서는 다공성 매질 영역에서 압력의 내삽(interpolation) 방법을 개선하여 이 문제를 해결하였다(이에 대한 자세한 내용은 아래 링크의 문서를 참고). 결과의 정확성과 함께 수렴성도 많이 좋아진 것을 확인할 수 있다.

### * [Porous Media 참고 문헌](https://nextfoam.co.kr/proc/DownloadProc.php?fName=231101140051_yvpJhMF0nY.pdf&realfName=10thOKUCC_OpenFOAM%EC%82%AC%EC%86%8C%ED%95%9C%EB%AC%B8%EC%A0%9C%EB%93%A4.pdf)

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/porousMedia/res.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/porousMedia/res.png)

<p align='center'>결과 (좌)Baram v24, (우) openfoam 2306 표준 솔버</p>

[![residual](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/porousMedia/residual-1.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/porousMedia/residual-1.png)

<p align='center'>Residual (좌)Baram v24, (우) openfoam 2306 표준 솔버</p>


### 프로그램의 구동 및 격자

프로그램 실행 후 [새 작업(New Case)]를 선택한다. 시작 창에서 [솔버 유형(Solver Type)]은 [압력기반(Pressure-based)]를, [다상유동 모델(Multiphase Model)]은 [None]을 선택한다.

격자는 주어진 polyMesh 폴더를 활용한다. 상단 탭에서 [파일(File)]-[격자 불러오기(Load Mesh)]-[OpenFOAM]을 순서대로 클릭하고 polyMesh 폴더를 선택한다. 

### 기본조건(General)

정상상태 계산이기 때문에 모든 설정은 디폴트 조건을 사용한다. 

### 모델(Models)

난류 모델은 standard $k-\epsilon$ 모델을 사용한다.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousMedia/turbulence.png"><br> 
</p>
<br/>

### 물질(Materials)

공기의 디폴트 조건을 사용한다.

### 셀존 조건(Cell zone Conditions) 

region0 영역에 셀존 porousZone이 있다. 이것을 더블 클릭하면 설정창이 열린다. [셀존 종류(Zone Type)]을 [다공성 매질(Porous Zone)]으로 선택하면 아래쪽에 세부 설정 부분이 나타나는데 다음과 같이 설정한다.

+ 모델(Model) : Power Law
+ C0 : 5000
+ C1 : 1.9

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousMedia/cellZone.png"><br> 
</p>

### 경계조건(Boundary Conditions)

경계조건은 다음과 같이 설정한다.

+ inlet : 입구 속도(Velocity Inlet)
    + 속도 크기(Velocity magnitude) : 1
    + 난류 강도(Turbulent Intensity) : 1
    + 난류 점도 비율(Turbulent Viscosity Ratio) : 10 
 
 <p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousMedia/inlet.png"><br> 
 </p>

+ outlet : 출구 압력(Pressure Outlet)
    + 압력(Pressure) : 0 
  
   <p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousMedia/outlet.png"><br> outlet 경계조건
   </p>

나머지는 모두 디폴트 조건인 벽면(wall)을 사용한다.


### 수치해석 기법(Numerical Conditions)

[이산화 기법(Discretization Schemes)]의 압력(Pressure)를 [Linear]로 설정한다. 다공성 매질과 같이 운동량 소스가 있는 경우 [Momentum Weighted]나 [Momentum Weighted Reconstruct] 기법은 안정성에 문제가 있을 수 있어 [Linear]를 사용한다. 
([user guide](https://baramcfd.org/userguidelist/2023/09/05/numericalCondition-post/)) 참조

[수렴 판정 기준(Convergence Criteria)]의 압력을 0.0001로 설정하고 나머지는 디폴트 조건을 사용한다.

### 초기화(Initialization)

초기조건은 모두 디폴트 조건을 사용한다.

### 계산

메뉴의 [병렬연산(Parallel)]-[환경설정(Environment)]를 클릭하고 원하는 코어수를 입력한다. 

[계산 조건(Run Conditions)]은 다음과 같이 설정하고 [계산시작(Start Calculation)] 버튼을 누르면 계산이 시작된다.

+ 계산회수(Number of Iterations) : 1000
+ 자동 저장 간격(Save Interval) : 1000


<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousMedia/parallel.png"><br> 
</p>

계산이 시작되면 아래와 같이 잔차(residual)과 유체력 계수의 그래프가 그려진다.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousMedia/residual.png"><br> 
</p>

### 후처리

메뉴에서 [외부 프로그램(External tools)]-[ParaView] 버튼을 클릭하여 paraview를 실행한다.

병렬연산이면 [Case Type]을 [Decomposed Case]로 변경한다.

<p style="text-align: left">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousMedia/slice.png"> [Slice] 아이콘을 선택한다.
</p>

Pipeline Browser에서 [Y Normal]을 선택하고, [Coloring]을 [U]로 선택하면 다음과 같은 그림을 확인할 수 있다.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousMedia/post.png"> <br> 중앙 단면의 속도 분포
</p>
<!---------------------------------------------------------------------------------------------------------->
## 다공성 압력 점프(Porous Jump)

[격자 파일 다운로드](https://drive.google.com/file/d/1c7RgueGF8kfG_pqA0tGbU_TbPpZ99tNG/view?usp=sharing)

[계산 파일 다운로드](https://drive.google.com/file/d/1OnXwBiE6TIIh11fSAyhqJlfJ6y9jZ72_/view?usp=sharing)

### 개요 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousJump/9.1.png"><br>
</p>

본 예제는 계산영역 내부에 있는  다공성 판 또는 팬을 두께가 없는 경계면으로 모사할 수 있는 [다공성 압력 점프(Porous Jump)] 경계조건 예제이다. 육면체 덕트 내부에 사각형의 면을 만들고 다공성 압력 점프 경계조건을 사용하여 압력과 속도가 변하는 문제이다.

계산 조건은 다음과 같다.

+ 솔버 : buoyantsimpleNFoam 
+ 난류 모델 : $Standard$ $k-\epsilon$ model
+ 밀도 : 1.225 $kg/m^3$
+ 점성 계수 : 1.79e-5 $kg/ms$
+ 다공성 압력 점프(Porous Jump) 경계 조건
    + Darcy Coefficient : -100
    + Inertial Coefficient : -5
    + 다공성 매질 두께 : 0.05 m

다공성 압력 점프(Porous Jump)를 계산하는 porousBafflePressure 경계조건은 아래 식을 사용한다.

<p align='left'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousJump/9.2.png"><br>
</p>

### 프로그램의 구동 및 격자

프로그램 실행 후 [새 작업(New Case)]를 선택한다. 시작 창에서 [솔버 유형(Solver Type)]은 [압력기반(Pressure-based)]를, [다상유동 모델(Multiphase Model)]은 [None]을 선택한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/launcher.png"><br>
</p>

격자는 주어진 polyMesh 폴더를 활용한다. 상단 탭에서 [파일(File)]-[격자 불러오기(Load Mesh)]-[OpenFOAM]을 순서대로 클릭하고 polyMesh 폴더를 선택한다. 

### 기본조건(General)

모든 설정은 디폴트 조건을 사용한다. 

### 모델(Models)

난류 모델은 $Standard$ $k-\epsilon$ 모델을 사용한다. 

### 물질(Materials)

본 예제에서는 공기의 디폴트 값을 사용한다.

### 경계조건(Boundary Conditions)

아래와 같이 경계면 타입과 경계값을 설정한다.

+ duct : 벽면(Wall)
    + 속도 조건(Velocity Condition) : 정지(No Slip)

+ inlet : Velocity Inlet
    + Velocity Specification Method : Magnitude, Normal to Boundary
    + Profile Type : Constant
    + Velocity Magnitude : 10 (m/s)
    + Turbulent Intensity : 1 (%)
    + Turbulent Viscosity Ratio : 10 (m)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousJump/9.3.png"><br>
</p>

+ outlet : Pressure Outlet
    + Total Pressure : 0 (Pa)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousJump/9.4.png"><br>
</p>

+ plane_master : Porous Jump
    + Coupled Boundary : plane_slave
    + Darcy Coefficient : -100
    + Inertial Coefficient : -5
    + Porous Media Thickness : 0.05

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousJump/9.5.png"><br>
</p>

### 수치해석 기법(Numerical Conditions)

Numerical Conditions은 다음과 같이 설정한다.

+ Pressure-Velocity Coupling Scheme : SIMPLE

+ Discretization Schemes
    + Momentum : Second Order Upwind
    + Turbulence : First Order Upwind

+ Convergence Criteria
    + Pressure : 1e-6
    + Momentum : 0.001
    + Turbulence : 0.001

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousJump/9.6.1.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousJump/9.6.2.png"><br>
</p>

### 초기화(Initialization)

+ X-Velocity : 10 (m/s)
+ Pressure : 0 (Pa)
+ Turbulence
    + Scale of Velocity : 10 (m/s)
    + Turbulent Intensity : 1 (%)
    + Turbulent Viscosity Ratio : 10

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousJump/9.7.png"><br>
</p>

### 계산

Run Conditions에서 다음과 같이 설정 후 계산을 진행한다.

+ Number of Iterations : 1000
+ Save Interval : 1000
+ Data Write Format : Binary
+ Number of Cores : 1 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousJump/9.8.png"><br>
</p>


<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousJump/9.9.png"><br>계산이 완료된 모습

</p>

### 후처리

덕트 내부 압력 분포를 확인한다. External tools의 paraview 버튼을 클릭하여 paraview를 실행한다.

Case Type을 Reconstructed Case로 변경한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousJump/9.10.png"><br>
</p>

상단 툴바의 Slice 아이콘을 선택하고 Z Normal 버튼을 클릭한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousJump/9.11.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousJump/9.12.png"><br>
</p>

이후, 상단의 Solid Color를 p_rgh로 변경하여 덕트 내부 압력 분포를 확인한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/porousJump/9.13.png"><br>
</p>

