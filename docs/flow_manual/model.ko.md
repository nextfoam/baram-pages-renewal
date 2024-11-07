# 모델(Models)

난류모델, 에너지방정식 계산 여부, 다상유동, 화학종 혼합, 사용자 정의 스칼라 등을 설정한다.

솔버유형(Solver Type)과 다상유동(Multiphase)는 프로그램 시작할 때 설정되며 바꿀 수 없다.

격자를 읽었을 때 복합영역 격자이면 에너지방정식은 자동으로 해석하는 것으로 설정되고 바꿀 수 없다. 솔버유형이 밀도기반일 때도 마찬가지이다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/models.png" width="400" height="400"><br>모델 설정</center>


## 난류(Turbulence)

난류를 더블 클릭하면 아래 그림의 설정창이 나타난다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/turbulence.png" width="400" height="400"><br>난류 설정</center>

모델을 선택하면 그에 따라 필요한 추가 설정 부분이 표시된다.

#### 비점성(Inviscid)

압축성 유동에서 Euler라고 많이 표현되는, 점성을 고려하지 않는 모델로 별도의 설정은 없다. 이 옵션을 선택하면 내부적으로 층류(laminar)가 선택되고 점성계수는 0이 된다. 물성값에서 점성계수를 직접 0으로 설정할 필요는 없다.

#### 층류(Laminar)

층류유동은 별도의 설정이 없다.

층류인지 난류인지의 판단은 관성력과 점성력의 비율인 레이놀즈 수(Reynolds number, $Re$)를 사용한다. 레이놀즈 수를 이용한 층류/난류 판별 기준은 유동 형태에 따라 다르다. 일반적으로 관내유동의 경우 Re가 2000보다 작으면 층류 4000보다 크면 난류로 볼 수 있다. 개수로 유동의 경우 500, 평판(외부) 유동에서는 $10^5$ $\sim$ $10^6$ 정도가 기준이 된다. 

<center>$Re = \frac {\rho U L} {\mu^2}$</center>

* $L$ : 특성길이(characteristic length)
* $\mu$ : 점성계수(viscosity)\\

자연대류 문제에서는 확산과 대류에 의한 열전달 비율인 레일리 수(Rayleigh number, $Ra$)를 사용한다. 일반적으로 $10^7$ 이하에서는 층류, $10^9$ 이상에서는 난류로 본다.

<center>$Ra = \frac {\rho \beta \Delta T L^3 g} {\mu \alpha}$</center>

* $\beta$ : 열팽창계수(thermal expansion coefficient)
* $L$ : 거리(distance)
* $\alpha$ : 열확산율(thermal diffusivity)

#### Spalart-Allmaras

보정 동점성 계수($\tilde{\nu}$, modified kinematic viscosity)의 수송방정식을 계산하는 1 방정식 난류모델로, 항공우주 분야에서 경계층 난류 유동을 계산할 때 많이 사용된다. 계산된 $\tilde{\nu}$ 값과 벽 근처에서의 점성 효과 보정함수를 이용하여 난류 점성계수를 계산한다. 난류 프란틀 수($Pr_t$, turbulent Prandtl Number)를 설정할 수 있다.

#### k-epsilon

난류 운동 에너지(k, turbulent kinetic energy)와 난류 소산율($\epsilon$, turbulent dissipation rate)에 대한 수송방정식을 계산하는 2 방정식 난류모델이다. Standard, RNG, Realizable 3가지 모델을 선택할 수 있으며 난류 프란틀 수를 설정할 수 있다. Realizable 모델을 선택하면 벽함수 옵션으로 표준벽함수와 two layer 모델을 선택할 수 있다.

#### k-omega

난류 운동 에너지(k, turbulent kinetic energy)와 특정 소산율($\omega$, specific dissipation rate)에 대한 수송방정식을 계산하는 2 방정식 난류모델이다.
k-omega 모델은 현재 Menter의 SST(Shear Stress Transport) 모델만 지원한다. 난류 프란틀 수를 설정할 수 있다.

#### LES, Large Eddy Simulation

큰 와류는 직접 계산하고 작은 와류는 서브그리드 스케일(SGS, Subgrid Scale)모델을 사용하여 에너지를 난류 점성으로 변환하는 모델이다. 비정상상태로 설정되었을 때만 활성화 된다. 서브그리드 스케일 모델과 와류의 크기를 구분하는 필터링 방법인 Length-Scale 모델을 선택할 수 있다.

#### DES, Detached Eddy Simulation

LES와 RANS(Reynolds Averaged Navier-Stokes)를 결합한 하이브리드 난류 모델로, 자유류 영역은 LES를 경계층 유동은 RANS를 사용하는 모델이다. 비정상상태로 설정되었을 때만 활성화 된다. 벽면의 RANS 모델과 DES 옵션, Length-Scale 모델을 선택할 수 있다. RANS 모델로 Spalart-Allmaras와 SST k-omega를 선택할 수 있다. Spalart-Allmaras 옵션으로 Low Reynolds Damping을 선택할 수 있다. DES 옵션으로 Delayed DES를 선택할 수 있으며, 이 옵션이 활성화되면 DDES와 IDDES를 선택할 수 있다.

#### Two layer 모델

넥스트폼이 아래 논문을 바탕으로 만든 벽함수 모델로, 블렌딩(blending) 함수를 사용한다. 표준벽함수는 $y^+$가 버퍼 레이어(buffer layer)에 있는 경우 정확도가 문제될 수 있는데, 이 모델은 $y^+$에 상관없이 사용할 수 있는 장점이 있다. **지금은 realizable k-epsilon 모델만 지원한다. 블렌딩 함수는 다음의 식을 사용한다.

<center>$\lambda = {\frac 1 2} \left[1 + tanh \left( \frac{Re_y - {Re_y}^* } {A} \right) \right ]$</center>

<center>$A = \frac {\Delta Re_y } {tanh(0.98)}$</center>

*Ref) Shih, T. H., Liou, W. W., Shabbir, A., Yang, Z., & Zhu, J. (1995). A New k-epsilon eddy Viscosity Model for High Reynolds Number Turbulent Flows. Computers and Fluids, 24(3), 227-238.*


#### 난류 프란틀 수($Pr_t$, Turbulent Prandtl Number)

난류 프란틀 수는 난류 흐름에서 운동량 확산과 열 확산 간의 비율을 나타내는 무차원수이다. '내부 유동변수에 사용'(Internal Field)와 '벽함수에 사용'(Wall Function) 두 가지를 설정할 수 있다. 내부 유동변수에 사용되는 값은 난류모델에서 사용하고, 벽함수에 사용되는 값은 난류 열확산 계수($\alpha_t$, turbulent thermal diffusivity)의 벽함수에 사용된다. DES/LES 모델에서는 사용되지 않는다.

<center>$Pr_t = \frac {\nu_t } {\alpha_t}$</center>

* $\nu_t$ : 난류 점성 계수(turbulent viscosity)
* $\alpha_t$ : 난류 열확산 계수(turbulent thermal diffusivity)

#### 난류 슈미트 수($Sc_t$, Turbulent Schmidt Number)

난류 슈미트 수는 난류 흐름에서 물질 확산과 운동량 확산 간의 비율을 나타내는 무차원수이다. 화학종의 난류 확산에 사용되며, 화학종을 계산하지 않을 때는 사용되지 않는다.

<center>$Sc_t = \frac {\nu_t } {D_t}$</center>

* $\nu_t$ : 난류 점성 계수(turbulent viscosity)
* $D_t$ : 난류 확산 계수(turbulent diffusivity)\\

화학종의 확산 플럭스는 다음식으로 계산되는데, 난류 슈미트 수가 작을 수록 혼합이 더 잘 일어난다. 

<center>$\vec{J_i} = - \left(  \rho D_i + \frac{\mu_t}{Sc_t}  \right) \nabla Y_i = - \rho (D_i + K) \nabla Y_i$</center>

<!--------------------------------------------------------------------------------------------------------->
## 에너지(Energy)

에너지를 더블 클릭하면 아래 그림의 설정창이 나타난다. 에너지방정식을 계산할 것인지 아닌지를 선택한다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/energy.png" width="200" height="200"><br>에너지 설정</center>

OpenFOAM의 표준솔버(standard solver)는 에너지방정식을 계산하지 않는 _simpleFoam_ 과 계산하는 _buoyantSimpleFoam_ 을 제공한다. _simpleFoam_ 은 압력 대신 압력을 밀도로 나눈 변수를 사용하고, _buoyantSimpleFoam_ 은 압력에서 정수압($\rho g h$)을 뺀 $p\_rgh$ 변수를 사용한다. 그래서 각 솔버에서 계산한 압력 값이 밀도 만큼 다르다. BaramFlow의 압력기반 단상유동 솔버에서는 에너지방정식 계산 여부와 관계없이 넥스트폼이 개발한 하나의 솔버(_buoyantSimpleNFoam_)을 사용하는데, 이 솔버는 압력계산에 $p\_rgh$를 사용하고 각 지배방정식의 계산여부를 제어할 수 있다.

## 화학종 혼합(Species)

화학종 혼합을 더블 클릭하면 아래 그림의 설정창이 나타난다. 화학종방정식을 계산할 것인지 아닌지를 선택한다. 두 경우 모두 같은 솔버(_buoyantSimpleNFoam_)을 사용하고 선택에 따라 방정식 계산 여부를 제어한다.

[화학종 혼합 예제 : 덕트 유동](https://baramcfd.org/tutorials/2024/07/24/species-post/)

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/species.png" width="200" height="200"><br>화학종 혼합 설정</center>

## 사용자 정의 스칼라(User-defined Scalars)

사용자 정의 스칼라는 사용자가 임의로 정의하고 수송방정식을 계산하는 변수로, 유동장에 의해 분포가 결정되지만 이것이 유동에 영향을 미치지는 않기 때문에 passive scalar라고도 불린다.

[사용자 정의 스칼라 예제 : 실내 화재(Fire in Room)](https://baramcfd.org/tutorials/2024/06/16/fireInRoom-post/)

이것을 선택하고 편집(Edit) 버튼을 누르면 아래 그림의 창이 나타난다. 창 위쪽의 (+) 아이콘을 누르면 새로운 스칼라를 추가할 수 있다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/uds0.png" width="300" height="300"><br>사용자 정의 스칼라 설정</center>

각 스칼라는 스칼라이름(Field Name)과 확산계수(Diffusivity)를 설정할 수 있다. 확산계수 설정 방법은 상수(Constant)와 '층류와 난류 점도'(Laminar and Turbulent Viscosity) 두 가지가 제공된다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/uds1.png" width="300" height="300"><br>사용자 정의 스칼라의 확산계수 설정</center>

상수는 고정값을 입력하고, '층류와 난류 점도' 방법은 층류와 난류 두 계수를 입력 받아 아래의 식으로 확산계수를 구한다.

<center>$D = D_{laminar} \cdot \nu + D_{turbulent} \cdot \nu_t$</center>

* $\nu$ : 물질의 동점성계수(kinematic viscosity)
* $\nu_t$ : 난류 동점성계수(turbulent kinematic viscosity)

사용자 정의 스칼라는 OpenFOAM의 function object 기능을 사용하여 계산한다. 여기서 스칼라를 정의하면 다른 유동변수와 마찬가지로 경계조건에 값을 입력할 수 있고 모니터링과 데이터 추출 등에서 사용할 수 있다.


