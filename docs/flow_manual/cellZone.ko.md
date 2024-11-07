# 셀존 조건(Cell Zone Conditions)

셀존 조건에는 격자의 영역(region)과 셀존이 아래 그림과 같이 표시된다. 영역이 하나인 경우 하나의 영역만 표시되고 복합영역인 경우 영역의 개수 만큼 나타난다. 각 영역에 속한 셀존들이 그 아래에 나타난다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/cellZoneUI.png" width="300" height="300"><br>셀존 설정</center>

## 영역(Region)

영역(Region)을 더블 클릭하면 아래 그림의 창이 열린다. 

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/region.png"><br>영역 설정</center>

물질(Materials)에 등록한 것들 중 하나를 해당 영역의 물질로 선택한다. 화학종 혼합을 계산하는 경우에는 해당 혼합물(mixture)를 선택한다. 전체 영역에 생성항이나 일정한 값을 주고 싶다면 해당 항목을 설정한다.

다상유동의 경우에는 하나를 '첫번째 상'(primary)으로 나머지를 '두번째 상'(secondary)으로 설정해 주고, 두 물질간의 표면장력(Surface tension)과 캐비테이션(Cavitation)을 설정한다. 아래 그림의 왼쪽 그림의 선택(Select) 버튼을 누르면 오른쪽 그림과 같은 창이 열리고 물질을 선택할 수 있다.

**캐비테이션 모델을 사용하는 경우 '첫번째 상'(primary)은 기체를 '두번째 상'(secondary)은 액체를 선택해야 한다.**

<center>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/regionVOF.png"><br>다상유동에서 유체 선택</center>

### 캐비테이션 모델

캐비테이션 모델은 증기압, 모델, 모델계수를 설정한다. 액체의 압력이 증기압보다 낮을 때 캐비테이션이 발생한다.

[캐비테이션 예제 : NACA66 하이드로포일](https://baramcfd.org/tutorials/2024/10/07/cavitation-post/)

#### Schnerr-Sauer

다음 논문에 제시된 모델이다. 증발계수(Evaporation Coefficient, $C_v$), 응축계수(Condensation Coefficient, $C_c$), 기포 직경(Bubble Diameter, dNuc), 기포의 수밀도(Number Density, n)를 입력한다.

*Schnerr, G. H., And Sauer, J., "Physical and Numerical Modeling of Unsteady Cavitation Dynamics", Proc. 4th International Conference on Multiphase Flow,         New Orleans, U.S.A., 2001.*

#### Kunz

다음 논문에 제시된 모델이다. 증발계수(Evaporation Coefficient, $C_v$), 응축계수(Condensation Coefficient, $C_c$), 평균유동시간(Mean flow time scale, $t_{Inf}$), 자유류속도(free stream velocity, $U_{Inf}$)를 입력한다.

*Kunz, R.F., Boger, D.A., Stinebring, D.R., Chyczewski, Lindau. J.W.,         Gibeling, H.J., Venkateswaran, S., Govindan, T.R., "A Preconditioned Implicit Method for Two-Phase Flows with Application to Cavitation Prediction," Computers and Fluids, 29(8):849-875, 2000.*

#### Merkle

다음 논문에 제시된 모델이다. 증발계수(Evaporation Coefficient, $C_v$), 응축계수(Condensation Coefficient, $C_c$), 평균유동시간(Mean flow time scale, $t_{Inf}$), 자유류속도(free stream velocity, $U_{Inf}$)를 입력한다.

*C. L. Merkle, J. Feng, and P. E. O. Buelow, "Computational modeling of the dynamics of sheet cavitation", in Proceedings Third International Symposium on Cavitation Grenoble, France 1998.*

#### Zwart-Gerber-Belamri

넥스트폼에서 개발한 것으로 다음 논문에 제시된 모델이다. 증발계수(Evaporation Coefficient, $C_v$), 응축계수Condensation Coefficient, $C_c$), 기포 직경(Bubble Diameter, dNuc), 기포 체적분율(Nucleate Site Volume Fraction, aNuc)를 입력한다. \textcolor[rgb]{0.6,0,0}{Baram-v24.4에서는 지원하지 않으며 다음 버전에서 지원할 예정이다}.

*P. J. Zwart, A. G. Gerber, and T Belamri. A two-phase flow model for predicting cavitation dynamics. In Proceedings of the International Conference on Multiphase Flow (ICMF 04), Yokohama, Japan, 2004*
<!---------------------------------------------------------------------------------------------------->
## 셀존 모델

셀존을 더블 클릭하면 아래 그림의 창이 나타난다. 셀존 종류(Zone Type), 생성항(Source Terms), 고정값(Fixed Values)를 설정할 수 있다. 셀존 종류는 다중기준좌표계(MRF, Multiple Reference Frame), 다공성 매질(Porous), 미끄럼 격자(Sliding Mesh), 액추에이터 디스크(Actuator Disk) 등이 있다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/cellZone.png"><br>셀존 설정</center>

### 다중기준좌표계(Multiple Reference Frame, MRF)

다중기준좌표계는 회전하는 물체를 시뮬레이션 할 때 물체를 회전시키지 않고 회전체 주변의 유동을 회전상대좌표계에서 계산하는 방법이다. 다중기준좌표계를 선택하면 아래 그림과 같은 설정 부분이 나타난다. 세부 설정 항목은 다음과 같다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mrf.png" width="300" height="300"><br>다중기준좌표계 설정</center>

* 회전속도(Rotating Speed) : 분당 회전수(RPM, Revolution Per Minute)
* 회전축 중심 좌표(Rotating Axis Origin)
* 회전축 방향(Rotating Axis Direction) : \textcolor[rgb]{0.6,0,0}{오른손 법칙에 따라 회전 방향이 반시계 방향이 되도록 설정}
* 셀존 안의 움직이지 않는 경계면(Static Boundary) : \textcolor[rgb]{0.6,0,0}{인터페이스 면도 선택해야 한다}. 2개의 인터페이스 면 중 어느 것이 해당 셀존에 있는 것인지 알기 어려울 때는 모두 선택하면 된다. 셀존에 있지 않는 면이 포함되더라도 계산에 영향을 미치지 않는다.

[다중기준좌표계 예제 : fan](https://baramcfd.org/tutorials/2024/03/21/simpleFan-post/)

### 미끄럼 격자(Sliding Mesh)

미끄럼 격자는 운동하는(현재 버전에서는 회전 운동만 지원, 병진 운동은 추후 지원 예정) 물체 주위를 셀존으로 구성하고 격자를 움직이는 방법이다. 운동하는 셀존과 정지한 셀존의 각 경계면은 인터페이스로 구성해야 한다. **셀존 내부에 있는 움직이는 경계면은 움직이는 벽(moving wall) 경계조건을 사용해햐 한다**.

설정 항목은 회전속도(RPM), 회전축 중심 좌표, 회전 방향이며 회전 방향은 다중기준좌표계에서와 같다.

[미끄럼 격자 예제 : 프로펠러](https://baramcfd.org/tutorials/2024/06/17/propeller-post/)

### 다공성 매질(Porous Zone)

다공성 물질이나 작은 영역에 매우 복잡한 형상이 있을 때, 형상을 직접 구현하지 않고 유속에 따른 압력 손실로 모델링하는 방법이다. 압력 손실의 모델링 방법으로 Darcy-Forchheimer 모델과 Power law 모델을 사용할 수 있다.

OpenFOAM의 다공성 매질 모델은 다공성 영역에서 불연속적인 속도 분포가 나타나고 압력손실도 입력 조건과 조금 다른 결과를 보이는 문제가 있다. BaramFlow가 사용하는 NextFOAM에서는 다공성 영역에서 압력의 내삽(interpolation) 방법을 개선하여 이 문제를 해결하였다.([이에 대한 자세한 내용은 아래 링크의 문서를 참고](https://nextfoam.co.kr/proc/DownloadProc.php?fName=231101140051_yvpJhMF0nY.pdf&realfName=10thOKUCC_OpenFOAM%EC%82%AC%EC%86%8C%ED%95%9C%EB%AC%B8%EC%A0%9C%EB%93%A4.pdf))

[다공성 매질 예제 : 덕트 유동](https://baramcfd.org/tutorials/2023/09/05/porousMedia-post/)

#### Darcy-Forchheimer 모델

3가지 방향에 대한 압력 손실을 각각 설정할 수 있다. 방향은 두 개의 벡터에 의해 결정된다. 두 벡터는 입력값(Direction-1 Vector, Direction-2 Vector)에 의해 결정되고 세번째 방향은 두 벡터에 수직한 방향이다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/darcy.png" width="300" height="300"><br>Darcy-Forchheimer 모델 설정</center>

관성 저항 계수(Inertial Resistance Coefficient(f, Forchheimer coefficient))와 점성 저항 계수(Viscous Resistance Coefficient(d, Darcy coefficient))를 벡터로 입력한다. 압력손실은 다음 식으로 계산한다.

<center>$S_i = \left( \mu d + \rho |U| \frac{f}{2} \right) U$</center>

#### Power law 모델

$C_0$와 $C_1$ 두 개의 값을 이용해서 다음 식으로 계산한다.

<center>$S_i = - \rho C_0 |U|^{C_1 -1} U$</center>

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/porous-powerlaw.png" width="300" height="300"><br>Power law 모델 설정</center>

### 액추에이터 디스크(Actuator Disk)

액추에이터 디스크 모델은 프로펠러와 같은 회전체를 디스크로 모델링하여 속도의 생성항으로 처리한다. 설정 항목은 다음과 같다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/actuatorDisk.png" width="300" height="300"><br>액추에이터 디스크 설정</center>

* 디스크 축 방향(Disk Direction)
* 파워 계수(Power Coefficient, $C_p$)
* 추력 계수(Thrust Coefficient, $C_t$)
* 디스크 면적(Disk Area, $A$)
* 상류쪽 점의 좌표(Upstream Point) : 디스크 상류 임의 지점의 좌표
* 힘 계산 방법(Force Computation method) : Froude's method와 Variable-Scaling method 중 선택

추력을 계산하는 방법 중 Froud's method는 다음 식을 사용한다.

<center>$T = 2 \rho A |u_m \cdot n|^2 a(1-a)$</center>

<center>$a = 1 - \frac{C_p}{C_t}$</center>


추력을 계산하는 방법 중 Variable-Scaling method는 다음 식을 사용한다.

<center>$T = 0.5 \rho A |u_0 \cdot n|^2 C_T ^*$</center>

<center>${C_T}^* = C_T \left( \frac{|u_m|}{|u_0|}\right)^2$</center>

* T : 추력의 크기
* $\rho$ : 모니터링 된 유입 유체의 밀도
* A : 액추에이터 디스크의 단면적
* $U_m$ : 모니터링된 영역에서의 평균 유입 속도
* $U_0$ : 액추에이터 디스크에서의 평균 유입 속도
* n : 액추에이터 디스크에 수직한 방향 벡터(상류를 향한 방향)
* a : 축방향 유입 계수(Axial induction factor)
* $C_p$ : 파워 계수
* $C_T$ : 추력 계수
* $C_T*$ : 보정된 추력 계수

## 생성항(Source Terms)

에너지, 질량, 난류 관련 필드 등에 생성항을 줄 수 있다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/source.png" width="300" height="300"><br>생성항 설정</center>

생성항의 크기를 주는 방법은 '전체 체적에 대한 값'(Value for Entire Cell Zone)과 '단위 체적당 값'(Value per Unit Volume)을 주는 두 가지가 있다.

비정상상태 문제일 때 생성항을 시간에 따라 변하는 값으로 줄 수 있는데, 조각별 선형함수(piecewise linear)와 다항식(polynomial) 방법이 제공된다.

[시간에 따라 변하는 생성항 예제 : 실내 화재(Fire in Room)](https://baramcfd.org/tutorials/2024/06/16/fireInRoom-post/)

## 고정값(Fixed Values)

셀존의 속도 벡터, 온도, 난류 등의 평균값을 고정할 수 있다. 속도의 경우 계산의 불안정성이 발생할 수 있어 완화계수(relaxation factor)를 사용한다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/fixedValue.png" width="300" height="300"><br>고정값 설정</center>


