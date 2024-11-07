# 물질(Materials)

BaramFlow는 몇가지 물질들의 물성값을 데이터베이스로 제공한다. 해석에 사용할 물질들을 불러올 수 있고, 필요하다면 물성값을 수정해서 사용할 수 있다. 그리고 이 물질들을 조합하여 혼합물(mixture)을 구성할 수 있으며, 화학종 혼합을 계산할 때 이것을 사용한다.

BaramFlow가 제공하는 물질 데이터베이스는 다음과 같다.

* Gas : Air, Oxygen, Nitrogen, Carbon-dioxide, Hydrogen, Argon, Carbon-monoxide, Methane, Water-vapor

* Liquid : Water-liquid

* Solid : Steel, Concrete, Aluminum, Copper

아래 왼쪽 그림 오른쪽상단의 (+)버튼을 누르면 가운데 그림과 같은 창이 나타나고 물질을 추가 할 수 있다. 오른쪽 그림은 oxygen이 추가된 결과이다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/material.png"><br>물질 설정</center>


혼합물(mixture)을 추가할 때는 여러개의 물질을 한꺼번에 선택하고 아래 가운데 그림처럼 '혼합물 생성'(Create Mixture) 버튼을 이용한다. 이 버튼은 화학종 계산이 활성화 되었을 때만 나타난다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/material-maxture.png"><br>혼합물 생성</center>

물성값 수정 버튼을 누르면 아래 그림의 창이 나타난다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/materialProperties.png" width="300" height="300"><br>물성값 설정</center>

수정할 수 있는 항목은 물질이름, 밀도, 정압비열, 점성계수, 열전도도, 분자량, 흡수계수, 포화증기압, 방사율 등이며 기체/액체/고체에 따라서 그리고 에너지방정식을 계산하는지에 따라서 항목들이 조금씩 달라진다.

<!--------------------------------------------------------------------------------------------->
### 밀도(Density)

상수(Constant), 완전기체(Perfect Gas), 다항식(Polynomial), 비압축성 완전기체(Incompressible perfect gas) 등을 선택할 수 있다. 에너지방정식을 풀지 않을 때와 액체나 고체일 때는 상수만 사용할 수 있다.

완전기체는 다음 식을 사용하여 온도와 압력의 함수로 계산한다.

<center>$\rho = \frac {p}{RT}$</center>

$R$ : 기체상수(gas constant)

다항식은 온도에 대한 함수로 정의되며, 다음 식의 $a_0$, $a_1$, $a_2$ 등의 계수들은 아래 그림의 창에서 설정한다. 

<center>$S = a_0 \cdot T^0 + a_1 \cdot T^1 + a_2 \cdot T^2 + ... + a_n \cdot T^n$</center>

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/density-polynomial.png" width="300" height="300"><br>다항식 설정</center>

비압축성 완전기체는 아래 식을 이용해 밀도를 온도만의 함수로 결정한다. 압력의 변화가 크지 않은 경우 완전기체에 비해 계산의 안정성이 높아지는 장점이 있다. **현재 화학종 혼합을 계산할 때만 쓸 수 있는데, 다음 버전부터는 에너지방정식을 계산하는 모든 경우에 사용할 수 있게 될 예정이다.

<center>$\rho = \frac {p_{ref}} {RT}$</center>

$p_{ref}$ : 기준값(Reference Values)에서 설정한 압력이 사용된다.

### 정압비열(Specific Heat Capacity, $C_p$)

상수(Constant)와 다항식(Polymomial)을 선택할 수 있다.

### 점성계수(Viscosity)

상수(Constant), Sutherland, 다항식(Polynomial)을 선택할 수 있으며, 액체의 경우 Cross, Hershel-Bulkley, Bird-Carreau, Non-Newtonian-power-law 등의 비뉴턴유체 모델을 선택할 수 있다.

Sutherland 관계식은 이상기체의 점도를 온도의 함수로 표현한 것으로 에너지방정식을 계산하는 경우에만 사용할 수 있다. Sutherland Coefficient($A_s$)와 Sutherland Temperature($T_s$)로 계산한다.

<center>$\mu = \mu_0 \left ( \frac {T} {T_0} \right )^{2/3} \frac{T_{ref} + T_s}{T + T_s} = \frac{A_s T^{2/3}}{T+T_s}$</center>

공기의 경우 $T_0$=273.15K 일 때 $\mu_0$=1.716e-5, $T_s$=110.4K, $A_s$=1.458e-6 이다.

Sutherland를 사용하면 열전도도는 Chapman-Enskog approach로 계산하기 때문에 입력 부분이 비활성화 된다.

<center>$\kappa = \mu C_v \left(1.32+1.77 \frac {R}{C_v} \right)$</center>

### 비뉴턴유체의 점성

Cross, Hershel-Bulkley, Bird-Carreau, Non-Newtonian-power-law 등은 비뉴턴유체(non-Newtonial fluid) 점성 모델로, **물질이 액체이고 난류 모델이 층류일 때만 사용할 수 있다**. 각 모델은 다음의 식을 사용한다.

[비뉴턴유체 예제 : 비뉴턴유체 혈액 유동 - FDA Nozzle](https://baramcfd.org/tutorials/2024/08/29/blood-post)

#### Cross

Cross power law 모델을 사용한다.

<center>$\nu = \nu_\infty + \frac {\nu_0 - \nu_\infty}{1+(m \gamma)^n}$</center>

* $\nu_0$ : zero shear viscosity
* $\nu_\infty$ : infinite shear viscosity
* $m$ : natural time
* $n$ : power law index
* $\gamma$ : shear strain rate

#### Hershel-Bulkley

<center>$\nu = min (\nu_0 , \tau_0 / \gamma + k \gamma^{n-1})$</center>

* $\nu_0$ : zero shear viscosity
* $\tau_0$ : yield stress threshold
* $k$ : consistency index
* $n$ : power law index
* $\gamma$ : shear strain rate


#### Bird-Carreau

<center>$\nu = \nu_\infty + (\nu_0 - \nu_\infty )[1+(k \gamma)^a]^{\frac {n-1}{a}}$</center>

* $\nu_0$ : zero shear viscosity
* $\nu_\infty$ : infinite shear viscosity
* $k$ : relaxation time
* $n$ : power law index
* $a$ : linearity deviation
* $\gamma$ : shear strain rate


#### Non-Newtonian-power-law

<center>$\nu = k \gamma ^{n-1}$</center>

* $k$ : consistency index
* $n$ : power law index
* $\gamma$ : shear strain rate

이 모델은 최대, 최소 값인 $\nu_0$, $\nu_\infty$을 사용하여 값을 제한할 수 있다.

### 열전도도(Thermal Conductivity)

상수(Constant)와 다항식(Polymomial)을 사용할 수 있으나, **점성계수를 설정하는 방법에 따라 결정된다**. 점성계수가 상수나 비뉴턴유체 모델인 경우 열전도도는 상수로 자동으로 설정된다. 점성계수가 다항식일 때는 열전도도도 다항식이 된다. 점성계수가 Sutherland일 때 열전도도는 Chapman-Enskog approach로 계산하기 때문에 입력 부분이 비활성화 된다.

### 분자량(Molecular Weight)

액체와 기체일 때만 나타난다.

### 흡수계수(Absorption Coefficient)

기체에서만 나타난다. 복사열전달 계산에만 사용된다.(현재는 불필요)

### 포화증기압(Saturation Pressure)

액체에서만 나타난다. 상변화 계산에 사용된다.(현재는 불필요)

### 방사율(Emissivity)

고체에서만 나타난다. 복사열전달 계산에만 사용된다.(현재는 불필요)

## 혼합물(Mixture)의 물성값 설정

Mixture의 물성값 수정 버튼을 누르면 아래 그림의 창이 나타난다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mixture-properties.png" width="300" height="300"><br>혼합물 물성값 설정</center>

수정할 수 있는 항목은 이름(Name), 밀도 계산방법(Density Spec.), 정압비열 계산방법(Specific Heat Spec.), 전달 물성값 계산방법(Transport Spec.), 물질확산계수(Mass Diffusivity), 주 화학종(Primary Specie) 등이다.

### 밀도 계산방법(Density Spec.)

혼합물을 구성하는 각 물질의 밀도를 계산하는 방법이다. 상수, 완전기체, 비압축성 완전기체, 다항식을 선택할 수 있다. 에너지방정식을 풀지 않을 때와 액체나 고체일 때는 상수만 사용할 수 있다. 혼합물의 밀도는 각 물질의 질량분율에 의해 결정된다.

### 정압비열 계산방법(Specific Heat Spec.)

상수와 다항식을 선택할 수 있다. 혼합물을 구성하는 각 물질의 값을 설정해 준다. 혼합물의 값은 각 물질의 질량분율에 의해 결정된다.

### 전달 물성값 계산방법(Transport Spec.)

점성계수와 열전도도를 결정하는 방법으로 상수, 다항식, Sutherland를 선택할 수 있다.

상수나 다항식을 선택한 경우 혼합물을 구성하는 각 물질의 점성계수와 열전도도를 상수나 다항식으로 설정해 준다. Sutherland인 경우 각 물질의 Sutherland Coefficient($A_s$)와 Sutherland Temperature($T_s$)를 설정해 준다. 혼합물의 값은 각 물질의 질량분율에 의해 결정된다.

### 물질확산계수(Mass Diffusivity)

현재는 상수로만 입력할 수 있다.

### 주 화학종(Primary Specie)

이 화학종에 대한 전달방정식은 계산하지 않고 1에서 나머지 화학종들의 질량분율 합을 뺀 값으로 결정된다.







