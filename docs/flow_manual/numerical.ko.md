# 수치해석 기법

해석에 사용되는 수치해석 조건들을 설정한다.

## 압력-속도 연성기법(Pressure-Velocity Coupling Scheme)

SIMPLE과 SIMPLEC를 선택할 수 있으며, fvSolution 파일의 SIMPLE 딕셔너리의 _consistent_에 적용된다. SIMPLEC를 사용할 때 relaxation factor를 0.9 이상의 값으로 설정해서 수렴 속도를 높일 수 있다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/scheme00.png" width="300" height="300"><br>압력-속도 연성기법 설정</center>

## 운동량방정식 계산(Momentum Predictor)

비정상상태(transient)일 때만 나타난다. 레이놀즈 수가 낮은 유동이나 다상유동 문제에서 운동량방정식 계산을 끄면 안정성이 향상되는 경우가 있다.

## Formulation

밀도 기반 압축성 솔버(솔버 유형을 밀도기반으로 선택)에서만 나타난다. 현재 Implicit만 선택할 수 있다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/scheme0.png" width="300" height="300"><br>Formulation, 플럭스 계산 기법 설정</center>

## 플럭스 계산 기법(Flux Type)

밀도 기반 압축성 솔버(Solver Type을 Density-based로 선택)에서만 나타난다.

Roe-FDS, AUSM, AUSM-up 3가지를 선택할 수 있다. Roe-FDS를 선택하면 Entropy Fix Coefficient, $\epsilon$을 입력할 수 있다. 0과 1 사이의 값을 입력한다. AUSM-up를 선택하면 Cut-off Mach Number, $M_\infty$를 설정할 수 있다. 0과 10 사이의 값을 입력한다. AUSM을 선택하면 별도의 설정이 없다.

## 이산화 기법(Discretization Schemes)

시간, 압력, 운동량, 에너지, 난류, 체척분율에 대한 이산화 기법을 선택한다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/scheme1.png" width="300" height="300"><br>이산화 기법 설정</center>


### 시간(Time)

1차 음해법(First Order Implicit)과 2차 음해법(Second Order Implicit)을 선택할 수 있다. fvSchemes 파일의 ddtSchemes 딕셔너리에 사용된다. 1차 음해법일 때 _Euler_를 사용하고 2차 음해법일 때 _backward_를 사용한다.

### 압력(Pressure)

셀 중심의 압력으로부터 셀면의 압력을 계산하는 내삽 기법을 선택하는 것으로 Linear, Momentum Weighted Reconstruct, Momentum Weighted를 선택할 수 있다. 

* Linear : 면 양쪽 셀의 중심값을 사용하여 선형 내삽하는 방법으로 안정성이 뛰어나다. 다중기준프레임(MRF)나 다공성 모델과 같은 운동량 소스가 있는 경우 이 기법의 사용을 권장한다.
* Momentum Weighted : 운동량방정식의 계수($a_p$)를 weighting factor로 사용하는 방법으로 수렴성이 뛰어니다. 운동량 소스가 있는 경우 안정성에 문제가 있을 수 있다.
* Momentum Weighted Reconstruct : 2차 정확도 기법이라고 할 수 있는데, 셀 중심의 압력구배를 이용한 외삽으로 셀면 좌우의 값을 계산하고 이를 평균해서 셀면의 값으로 사용하는 방법이다. 평균할 때 운동량방정식의 계수($a_p$)를 가중 계수로 사용한다. 좀 더 정확한 결과를 얻을 수 있는 대신 안정성이 낮아질 수 있다.

### 운동량(Momentum), 에너지(Energy), 난류(Turbulence), 체적분율(Volume Fraction)

1차 상류기법(First Order Upwind)와 2차 상류기법(Second Order Upwind)를 선택할 수 있다. fvSchemes 파일의 divSchemes 딕셔너리에 사용된다. 1차 상류기법일 때 '_Gauss upwind_'를 사용하고, 2차 상류기법일 때는 _Gauss linearUpwind_ 와 Venkatakrishnan 제한자(_VKLimited Gauss linear 1.0_)을 사용한다.

## 완화계수(Under-Relaxation Factors)

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/relaxation.png" width="300" height="300"><br>완화계수 설정</center>

정상상태에서는 디폴트 값만, 비정상상태에서는 디폴트 값과 최종 값을 입력한다. fvSolution 파일의 _relaxationFactors_ 에 해당한다. 값이 클수록 수렴 속도가 빨라질 수 있으나 불안정해질 수 있다. SIMPLEC를 사용하는 경우 0.9 이상의 값을 추천한다.

밀도 기반 압축성 솔버를 사용할 때는 난류 부분만 활성화 된다.

## 안정성 향상(Improve Stability)

안정성 향상을 위해 라플라시안 기법에 사용된다. 옵션을 끄면 fvSchemes 파일의 laplacianScheme은 "_Gauss linear corrected_"를 사용하고, 옵션이 켜지면 "_Gauss linear limited corrected [limiting factor]_' 를 사용한다. Limiting factor 값이 크면 좀 더 안정적이지만 정확도가 떨어질 수 있다.

## 시간당 반복계산 회수(Max Iterations per Time Step), 압력보정 회수(Number of Correctors)

’시간당 반복계산 회수’는 비정상상태 계산에서 각 시간 단계에서 수행할 최대 반복계산(iteration) 회수를 의미하며, ’압력보정 회수’는 각 반복계산마다 압력방정식을 몇번 계산할 것인지이다. fvSolution.PIMPLE 딕셔너리의 _nOuterCorrectors_와 _nCorrectors_에 적용된다.

OpenFOAM은 비정상상태 계산에 SIMPLE과 PISO 알고리즘이 결합된 PIMPLE 알고리즘을 사용한다. '시간당 반복계산 회수'가 1이고 '압력보정 회수'가 2이면 PISO 알고리즘과 같다. '시간당 반복계산 회수'가 1보다 크고 '압력보정 회수'가 1이면 transient SIMPLE 알고리즘과 같다. 

## 다상유동(Multiphase)

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/multiphase.png" width="300" height="300"><br>다상유동 설정</center>

’시간당 반복계산 회수’는 비정상상태 계산에서 각 시간 단계에서 수행할 체적분율(alpha, volume fraction) 방정식의 최대 반복계산(iteration) 회수를 의미하며, ’보정 회수’는 각 반복계산마다 체적분율(alpha, volume fraction) 방정식을 몇번 계산할 것인지이다. fvSolution 파일의 solvers-alpha(volume fraction)의 _nAlphaSubCycles_ 과 _nAlphaCorr_ 값이다.

MULES 기법은 MUlti-dimensionsal Limiter for Explicit Solution의 방법을 선택하는 것으로 fvSolution 파일의 solvers-alpha(volume fraction)의 _MULESCorr_ 값이다. '외재적 기법'(Explicit)이면 no, '준 내재적 기법'(Semi-Implicit)이면 yes이다.

'상 경계면 압축계수'(Phase Interface Compression Factor)는 상의 경계면 압축을 제어하는 요소이다. 0은 압축이 없음을 1은 보존적 압축에 해당된다. 값이 클수록 경계면이 얇게 포착되지만 불안정해 질 수 있다. fvSolution 파일의 solvers-alpha(volume fraction)의 _cAlpha_ 값이다.

'제한자에 대한 MULES 반복 회수'(Number of MULES iterations over the limiter)는 fvSolution 파일의 solvers-alpha(volume fraction)의 _nLimiterIter_ 값이다.

## 수렴 판정 기준(Convergence Criteria)

'수렴 판정 기준'은 각 필드의 수렴 조건을 나타낸다. fvSolution 파일의 SIMPLE 혹은 PIMPLE 딕셔너리의 _residualControl_에 해당한다. absolute 값들은 _tolerance_에 적용되고 비정상상태에서 활성화되는 relative는 _relTol_에 적용된다.

## 고급설정(Advanced)

값의 제한(Limits)와 방정식(Equations) 두 가지로 구성된다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/limit.png" width="300" height="300"><br>고급설정</center>

#### 값의 제한(Limits)

계산의 안정성을 위해 최소와 최대값을 제한한다. 온도와 점도 비율을 제한할 수 있다. 

온도의 제한은 fvOptions의 _limitTemperature_ 를 사용한다.

최대 점도 비율(Maximum Viscosity Ratio)는 turbulenceProperties 파일의 난류모델 딕셔너리에 설정된다. 디폴트는 1e5이며 값이 작을 때 안정성이 좋아지는 경우가 있으나 결과의 정확도에 문제가 될 수 있다.

#### 방정식(Equations)

유동, 에너지, 사용자 정의 함수, 화학종 등의 방정식을 비활성화 할 수 있다. 

에너지방정식은 점성가열(Viscous dissipation), 운동에너지(Kinetic energy), 압력에의한 일(Pressure Work) 항을 비활성화할 수 있다. 점성가열을 포함하는 경우에는 나머지 두 개는 모두 포함된다. 이들의 영향이 매우 작은 경우 생략하면 안정성 향상 효과를 볼 수 있지만, 고속, 고온, 고점도 유동의 경우에는 정확도에 문제가 있을 수 있다. 계산여부는 thermophysicalProperties 파일에 설정된다.

점성가열의 중요성을 판단할 때 브링크만 수(Brinkman Number, $Br$)를 사용할 수 있다. 브링크만 수는 점성에 의해 소산된 에너지와 전도된 에너지의 비율을 나타내는 무차원수이며, 1보다 매우 작으면 점성가열은 무시할 수 있다.

<center>$Br = \frac{U^2 \mu}{k \Delta T}$</center>

운동에너지는 유체의 속도가 빠를 때 중요한 항목이 된다. 압력에의한 일은 유체가 압축되거나 팽창할 때의 일을 의미하며 펌프, 터빈, 압축기 등의 문제에서 중요한 항목이 된다. 

미끄럼 격자를 사용하는 경우 모든 방정식을 끄고 계산하면, 유동 계산 없이 격자의 운동만 확인할 수 있다.


