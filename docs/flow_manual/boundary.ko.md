# 경계조건(Boundary Conditions)

경계조건을 선택하면 설정부에 아래 그림처럼 영역 별로 경계면이 표시된다. 경계면을 선택하면 그래픽창에 그 경계면이 붉은색으로 표시된다. 경계면을 선택하고 마우스 오른쪽 버튼을 누르면 경계조건을 선택할 수 있다. 

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/boundaryCondition.png" width="300" height="300"><br>경계조건 설정</center>

필터링을 위한 문자열 입력(filter string) 기능을 이용하여 특정 문자열이 포함된 경계면만 표시할 수 있다.

경계조건은 입구(Inlet), 출구(Outlet), 벽면(Wall), 기타(Misc)의 4개 범주로 구분되어 있으며, 각 범주에는 다음과 같은 조건들이 있다. 다상유동, 압축성유동 등의 조건에 따라 나타나는 것이 다르다. 경계면을 선택하고 더블 클릭을 하거나 Edit 버튼을 누르면 경계조건의 세부 설정창이 나타난다.

* 입구
      + 입구 속도(Velocity Inlet)
      + 입구 유량(Flow Rate Inlet)
      + 입구 전압력(Pressure Inlet)
      + 대기경계층 입구(ABL Inlet, Atmospheric Boundary Layer Inlet)
      + 비압축성 자유류(Free Stream)
      + 개수로 입구(Open Channel Inlet) : 다상 유동에만 사용
      + 압축성 리만(Far-field Riemann) : 압축성 유동에만 사용
      + 아음속 입구(Subsonic Inlet) : 압축성 유동에만 사용
      + 초음속 입구(Supersonic Inflow) : 압축성 유동에만 사용
* 출구
      + 출구 압력(Pressure Outlet)
      + 유출(Outflow)
      + 개수로 출구(Open Channel Outlet) : 다상 유동에만 사용
      + 아음속 출구(Subsonic Outflow) : 압축성 유동에만 사용
      + 초음속 출구(Supersonic Outflow) : 압축성 유동에만 사용
* 벽면
      + 벽면(Wall)
      + 연결 벽면(Thermo-Coupled Wall)
* 기타
      + 대칭(Symmetry)
      + 인터페이스(Interface)
      + 2차원 경계(Empty)
      + 축대칭 경계(Wedge)
      + Cyclic
      + 다공성 압력 점프(Porous Jump)
      + 팬(Fan)

## 입구 속도(Velocity Inlet)

Velocity Inlet 조건은 유동의 입구에 속도, 난류, 온도, 화학종 질량분율 값 등을 주는 조건이다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/velocityInlet.png" width="300" height="300"><br>입구 속도 설정</center>

속도는 x, y, z 각 성분을 주는 방법(Component)와 경계면에 수직한 속도를 주는 방법(Magnitude, Normal to Boundary)이 있다.

난류는 난류 필드의 값(k, epsilon, omega)을 상수로 주는 방법과 '난류 강도와 점도비율'(Intensity and Viscosity Ratio)를 주는 방법이 있다. Spalart-Allmaras 모델에서는 '보정 난류 점성계수'(Modified Turbulent Viscosity, nuTilda) 혹은 '난류 점도비율'(Turbulent Viscosity Ratio)를 상수로 줄 수 있다.

온도는 일정한 값을 준다.

화학종 질량분율은 전체 화학종의 합이 1이 되도록 주어야 한다.

각 필드가 사용하는 openfoam의 경계조건은 다음과 같다.

* 속도 : x,y,z 성분을 줄때는 _fixedValue_, 수직한 속도를 줄때는 _surfaceNoramlVelocity_
* 압력 : _zeroGradient_
* 온도 : _fixedValue_
* 난류운동에너지 : _turbulentIntensityInletOutletTKE_
* 난류소산율($\epsilon$), 특성소산율($\omega$) : _viscosityRatioInletOutletTDR_
* 보정동점성계수($\tilde{\nu}$) : _fixedValue_
* 화학종 : _fixedValue_

### 속도 분포(boundary profile) 조건 설정

입구 속도 조건에서 속도와 온도를 시간에 따른 변화(temporal) 혹은 공간 상의 분포(spatial)로 지정할 수 있다.


#### 시간에 따른 변화(Temporal Distribution)

입구 속도 조건에서 속도 분포(Profile Type)을 '시간에 따른 변화'로 선택하면 다음 그림의 창에서 조각별 선형 함수(Piecewise linear)를 사용하여 지정할 수 있다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/profile.png" width="300" height="300"><br>시간에 따른 변화 조건 설정 - 조각별 선형 함수</center>

입구 속도 조건에서 속도 분포(Profile Type)을 '시간에 따른 변화'로 선택하면 다음 그림의 창에서 조각별 선형 함수(Piecewise linear)를 사용하여 지정할 수 있다.

<center>$S = a_0 \cdot t^0 + a_1 \cdot t^1 + a_2 \cdot t^2 + ... + a_n \cdot t^n$</center>

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/polynomial.png" width="300" height="300"><br>시간에 따른 변화 조건 설정 - 다항식</center>

[시간에 따라 변하는 경계조건 예제](https://baramcfd.org/tutorials/2023/09/05/ProfileBC-post/)

#### 공간 분포(Spatial Distribution)

입구 속도 조건에서 속도나 온도의 분포를 공간분포로 선택하면 csv 파일을 사용할 수 있다. 입구 속도 조건에서 속도 지정 방법이 경계면에 수직한 속도인 경우는 속도에 대해 적용할 수 없다.

csv 파일은 'x좌표, y좌표, z좌표, 값'이 콤마(,)로 구분되어 있어야 한다. 입구 속도 조건에서 속도는 다음과 같은 형식이 된다(x, y, z, Ux, Uy, Uz). x, y, z 좌표는 경계면 격자점의 좌표와 일치할 필요는 없다. 데이터는 하나의 평면에 있어야 하며, 하나의 직선상에 있으면 안된다. 만일 2차원 문제와 같이 한쪽 방향으로만 분포를 갖더라도 최소한 1개 점은 직선 밖에 있어야 한다.

```
0.0, 0.0, 0.0, 0.1, 0.2, 0.0
0.0, 0.7, 0.0, 0.1, 0.3, 0.0
0.0, 1.2, 0.0, 0.1, 0.4, 0.0
...
```

## 입구 유량(Flow Rate Inlet)

입구 유량 조건은 유동의 입구에 유량, 난류, 온도 값을 주는 조건이다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/flowrate.png" width="300" height="300"><br>입구 유량 설정</center>

유량은 질량유량(mass flow rate)과 체적유량(volume flow rate)을 줄 수 있다.

속도(U)가 사용하는 openfoam의 경계조건은 _flowRateIneltVelocity_ 이며 압력, 난류, 온도는 '입구 속도' 조건과 동일하다. 

## 입구 전압력(Pressure Inlet)

입구 전압력 조건은 유동의 입구에 전압력(total pressure)과 난류, 온도 값을 주는 조건이다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/pressureInlet.png" width="300" height="300"><br>입구 전압력 설정</center>

전압력은 상수로 줄 수 있으며 난류와 온도는 '입구 속도' 조건과 동일하게 준다.

openfoam의 경계조건은 압력은 _totalPressure_, 속도는 _pressureInletOutletVelocity_를 사용한다.

## 대기경계층 입구(ABL Inlet)

대기경계층 입구 조건은 유동의 입구에 대기경계층의 속도와 난류 분포를 주는 조건이다.

[대기경계층 조건 예제](https://baramcfd.org/tutorials/2023/09/05/Atomospheric-post/)

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/ablInlet.png" width="300" height="300"><br>대기경계층 입구 설정</center>

입력 항목은 다음과 같다.

* 바람 방향(Flow Direction)
* 지면에 수직한 방향(Ground-Normal Direction)
* 기준 고도(Reference Height, $z_{ref}$)
* 기준 풍속(Reference Flow Speed, $U_{ref}$) : 기준 고도에서의 속도
* 지표면 조도(Surface Roughness Length, $z_0$)
* 지표면 최소 z 좌표(Minimum z-coordinate, d) : 지표면의 높이 방향 좌표 중 최소값(지표면에서 높이가 z-d 로 계산됨)

속도와 난류 분포는 다음의 식을 사용한다.

<center>$u = \frac{u^*}{\kappa} ln\left(\frac{z - d + z_0}{z_0}\right)$</center>

<center>$k = \frac{(u^* )^2}{\sqrt{C_\mu}} \sqrt{C_1 ln \left( \frac{z - d + z_0}{z_0} \right) + C_2}$</center>

<center>$\epsilon = \frac{(u^* )^3}{\kappa (z - d + z_0)} \sqrt{C_1 ln \left( \frac{z - d + z_0}{z_0} \right) + C_2}$</center>

<center>$\omega = \frac{u^*}{\kappa \sqrt{C_\mu}} \frac{1}{z - d + z_0}$</center>

<center>$u^* = \frac{u_{ref} \kappa} {ln \left( \frac{z_{ref} + z_0}{z_0} \right)}$</center>

* $z$ : 수직 방향 좌표
* $d$ : 지표면의 최소 z 좌표
* $\kappa$ : Von Karman's constant, 0.41
* $C_\mu$ : constant, 0.09
* $C_1$ : constant, 0
* $C_2$ : constant, 1

각 필드가 사용하는 openfoam의 경계조건은 다음과 같다.

* 속도 : _atmBoundaryLayerInletVelocity
* 압력 : _zeroGradient
* 난류운동에너지 : _atmBoundaryLayerInletK
* 난류소산율($\epsilon$), 특성소산율($\omega$) : _atmBoundaryLayerInletEpsilon_, _atmBoundaryLayerInletOmega_
* 화학종 : _fixedValue_

## 비압축성 자유류(Free Stream)

비압축성 자유류 조건은 유동이 계산 영역으로 들어오는 경우는 일정한 속도를, 나가는 경우는 속도 구배가 0이 되는 조건이다. 비압축성 외부 유동의 원방 경계조건으로 많이 사용된다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/freestream.png" width="300" height="300"><br>비압축성 자유류 설정</center>

자유류 속도(Free Stream Velocity)는 속도 벡터를 입력하고 압력은 상수로 주며 난류와 온도는 '입구 속도' 조건과 동일하게 준다.

각 필드가 사용하는 openfoam의 경계조건은 다음과 같다.

* 속도 : _freestreamVelocity_
* 압력 : _freestreamPressure_
* 온도 : _freestream_
* 난류 : _freestream_
* 화학종 : _fixedValue_

## 개수로 입구(Open Channel Inlet)

개수로 입구 조건은 자유수면을 계산할 때 입구에 일정한 유량을 주고 그에 따라 수면의 높이가 변할 수 있는 조건이다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/openChannelInlet.png" width="300" height="300"><br>개수로 입구 설정</center>

체적유량을 상수로 입력하고, 난류는 '입구 속도' 조건과 동일하게 준다.

각 필드가 사용하는 openfoam의 경계조건은 다음과 같다.

* 속도 : _variableHeightFlowRateInletVelocity_
* 압력 : _zeroGradient_
* 체적분율 : _variableHeightFlowRate_
* 난류운동에너지 : _turbulentIntensityInletOutletTKE_
* 난류소산율($\epsilon$), 특성소산율($\omega$) : _viscosityRatioInletOutletTDR_
* 보정동점성계수($\tilde{\nu}$) : _fixedValue_

## 압축성 리만(Far-field Riemann)

압축성 유동의 원방경계에 사용하는 Riemann 경계조건이다.

[압축성 리만 경계조건 예제 : RAE2822 에어포일](https://baramcfd.org/tutorials/2024/03/21/rae2822-post/)

유동의 방향, 원방에서의 마하수(Mach Number), 압력(static pressure), 온도(static temperature)를 입력하고, 난류는 '입구 속도' 조건과 동일하게 준다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/riemann.png"><br>압축성 리만 설정</center>

유동의 방향을 결정하는 방법은 직접입력(Direct)과 '받음각과 옆미끄럼각'(AOA and AOS)' 두 가지가 있다. 직접입력일 때는 방향벡터를 입력한다. '받음각과 옆미끄럼각'일 때는 받음각과 옆미끄럼각이 0일 때의 항력 및 양력 방향과 받음각, 옆미끄럼각을 입력한다.

속도, 압력, 온도의 openfoam 경계조건은 _farfieldRiemann_이며 난류는 '입구 속도' 조건과 동일하다.

## 아음속 입구(Subsonic Inlet)

아음속 입구 조건은 압축성 유동에서 유체기계와 같은 내부유동의 입구 아음속 경계조건이다.

유동의 방향 벡터, 전압력(total pressure), 전온도(total temperature)를 입력하고, 난류는 '입구 속도' 조건과 동일하게 준다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/subsonicInflow.png" width="300" height="300"><br>아음속 입구 설정</center>

속도, 압력, 온도의 openfoam 경계조건은 _subsonicInflow_이며 난류는 '입구 속도' 조건과 동일하다.

## 초음속 입구(Supersonic Inflow)

초음속 입구 조건은 유동의 입구가 초음속인 경우 사용하는 경계조건이다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/supersonicInflow.png" width="300" height="300"><br>초음속 입구 설정</center>

속도 벡터, 압력(static pressure), 온도(static temperature)를 입력하고, 난류는 '입구 속도' 조건과 동일하게 준다.

속도, 압력, 온도의 openfoam 경계조건은 _fixedValue_이며 난류는 '입구 속도' 조건과 동일하다.

## 출구 압력(Pressure Outlet)

출구 압력 조건은 출구 경계면에 일정한 압력을 주는 조건이다. 입력한 압력은 유동이 빠져나가는 경우는 정압(static pressure)로, 들어오는 경우는 전압력(total pressure)로 사용된다. 압력을 입력하고 유동이 유입될 때의 조건과 'Non-Reflecting Boundary'를 옵션으로 선택할 수 있다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/pressureOutlet.png" width="300" height="300"><br>출구 압력 설정</center>

아무런 옵션을 사용하지 않을 때, 각 필드가 사용하는 openfoam의 경계조건은 다음과 같다.

* 속도 : _pressureInletOutletVelocity_
* 압력 : _totalPressure_ (이 조건은 유동이 유입될 때는 전압력을 사용하고, 유출될 때는 정압(static pressure)를 사용하기 때문에 이 조건에서 사용하는 압력은 정압이다)
* 온도 : _zeroGradient_
* 난류 : _zeroGradient_
* 화학종 : _zeroGradient_

#### 유입류 조건(Specify Backflow Properties) 옵션

유입류 조건(Specify Backflow Properties) 옵션을 선택하면 계산영역으로 유입되는 유동의 전온도(total temperature)와 난류 조건을 줄 수 있다.

이 옵션을 사용하면 온도, 난류 필드, 화학종이 사용하는 openfoam의 경계조건은 다음과 같다.

* 온도 : _inletOutletTotalTemperature_
* 난류운동에너지 : _turbulentIntensityInletOutletTKE_
* 난류소산율($\epsilon$), 특성소산율($\omega$) : _viscosityRatioInletOutletTDR_
* 보정동점성계수($\tilde{\nu}$) : _inletOutlet_
* 화학종 : _inletOutlet_

#### Non-Reflecting Boundary 옵션

경계면에서 압력파가 반사되지 않는 조건으로, 에너지방정식을 계산하고 밀도는 완전기체, 정압비열은 상수인 경우에만 나타난다.

속도와 압력에는 openfoam의 _waveTransmissive_ 경계조건을 사용하고 나머지 필드는 옵션이 없을 때와 동일하다. 

[Non-Reflecting Boundary 예제 : 압축성 캐비티 유동](https://baramcfd.org/tutorials/2024/09/19/2d_cavity-post/)

## 개수로 출구(Open Channel Outlet)

개수로 출구 조건은 자유수면을 계산할 때 출구에 일정한 평균 속도를 주고 그에 따라 수면의 높이가 변할 수 있는 조건이다.

[개수로 출구 경계조건 예제 : 선박 저항 해석](https://baramcfd.org/tutorials/2023/09/05/KCS-post/)

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/openChannelOutlet.png" width="300" height="300"><br>개수로 출구 설정</center>

평균 속도를 상수로 입력하고, 난류는 '입구 속도' 조건과 동일하게 준다.

각 필드가 사용하는 openfoam의 경계조건은 다음과 같다.

* 속도 : _outletPhaseMeanVelocity_
* 압력 : _zeroGradient_
* 체적분율 : _variableHeightFlowRate_
* 난류운동에너지 : _turbulentIntensityInletOutletTKE_
* 난류소산율($\epsilon$), 특성소산율($\omega$) : _viscosityRatioInletOutletTDR_
* 보정동점성계수($\tilde{\nu}$) : _inletOutlet_ 

## 유출(Outflow)

유출 조건은 출구의 모든 필드에 대해 구배(gradient)가 0인 조건을 사용한다.

## 아음속 출구(Subsonic Outflow)

아음속 출구 조건은 압축성 유동에서 유체기계와 같은 내부유동의 출구 아음속 경계조건이다. 압력(static pressure)을 입력한다.

속도, 압력, 온도의 openfoam 경계조건은 _subsonicOutflow_이며 난류는 _zeroGradient_이다. 

## 초음속 출구(Supersonic Outflow)

초음속 출구 조건은 압축성 유동의 출구속도가 초음속일 때 사용하는 경계조건이다.

입력할 것은 없으며, 모든필드의 openfoam 경계조건은 구배(gradient)가 0인 조건을 사용한다.

<!------------------------------------------------------------------------------------------------>
## 벽면(Wall)

벽면은 유동이 통과하지 못하는 면을 의미하며 벽면의 운동에 의한 속도조건과 온도조건을 지정할 수 있다. 압력과 난류는 별도의 조건 설정이 필요없다.

### 속도 조건

속도 조건은 정지(No Slip), 미끄럼 벽(Slip), 움직이는 벽(Moving Wall), 대기경계층 지표면(Atmospheric Wall), 직선 속도(Translational Moving Wall), 회전속도(Rotational Moving Wall) 조건을 선택할 수 있다.

정지 조건은 벽변 점착 조건으로 속도가 (0, 0, 0)으로 설정된다.

'미끄럼 벽'은 마찰이 없는 벽면으로 벽면에 수식한 속도 성분이 없다는 조건이다.

**'움직이는 벽'은 움직이는 물체에 대한 조건으로 미끄럼 격자(Sliding Mesh)와 같은 이동격자계를 사용할 때 움직이는 물체에 적용된다**.

'대기경계층 지표면'은 대기경계층 문제에서 지표면에 사용된다.

'직선 속도'와 '회전 속도'는 격자가 움직이지는 않지만, 벽면이 일정한 속도로 움직이는 조건으로 벽에서의 속도는 움직이는 속도와 같게 준다.

### 온도 조건

온도 조건은 단열(Adiabatic), 일정 온도(Constant Temperature), 일정 열유속(Constant Heat Flux), 외부로 대류열전달(Convection) 조건을 선택할 수 있다.

#### 단열(Adiabatic)

단열 조건으로 별도로 설정할 것은 없다.

복사열전달이 없는 경우는 온도 경계조건으로 _zeroGradient_를 사용하고, 복사열전달이 있는 경우는 복사를 포함한 열유속이 0이라는 조건을 사용한다.(_externalWallHeatFluxTemperatur_})

#### 일정 온도(Constant Temperature)

온도가 일정한 조건이다. _fixedValue_ 조건을 사용한다.

#### 일정 열유속(Constant Heat Flux)

열유속이 일정한 조건이다. _externalHextFluxTemperature_ 조건을 사용한다.

#### 외부로 대류열전달(Convection)

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/convectionWallLayer.png" width="300" height="300"><br>외부로 대류열전달 설정</center>

일정한 외부 온도와 열전달계수를 사용하는 조건으로, 벽에서의 열유속은 다음 식을 사용한다. _externalHextFluxTemperature_ 조건을 사용한다. 

<center>$q_{external} = h(T_{wall}-T_a)$</center>

* $h$ : 열전달계수(Heat Transfer Coefficient)
* $T_a$ : 외부온도(Free Stream Temperature) 

벽의 두께와 열전도도를 이용하여 고체의 열저항을 설정할 수 있으며, 여러 층의 고체에 대해 각각의 두께와 열전도도를 설정할 수 있다. 고체를 벽으로 모델링하여 복합영역 문제로 계산하지 않고도 고체의 열전도를 고려할 수 있는 방법이다. 그러나 벽에 수직방향의 열전도만 고려할 뿐 옆으로의 열전도를 고려할 수는 없으며, 비정상상태 계산에서 시간에 따른 고체의 온도 변화를 고려할 수는 없다.

이 때 열저항은 다음 식으로 계산된다.

<center>$h = \frac {1} {\frac{1}{h_{convection}} + \Sigma \frac{l_{layer}}{\kappa_{layer}}}$</center>

### 난류 경계조건

난류 벽함수(wall function)은 난류 모델에 따라 다음과 같은 조건을 사용한다.

#### standard $k-\epsilon$, realizable $k-\epsilon$, RNG $k-\epsilon$ 모델

* $k$ : _kqRWallFunction_
* $\epsilon$ : standard일 때는 _epsilonWallFunction_, two layer일 때는 _epsilonBlendedWallFunction_
* $\nu_t$ : standard일 때는 _nutkWallFunction_, two layer일 때는 _nutSpaldingWallFunction_

#### SST $k-\omega$ 모델

* $k$ : _kqRWallFunction_
* $\omega$ : _omegaBlendedWallFunction_
* $\nu_t$ : _nutSpaldingWallFunction_

#### Spalart-Allmaras 모델

* $\tilde{\nu}$ : _zeroGradient_
* $\nu_t$ : _nutSpaldingWallFunction_

#### 대기경계층 지표면일 때

* $k$ : _kqRWallFunction_
* $\epsilon$ : _atmEpsilonWallFunction_
* $\nu_t$ : _atmNutkWallFunction_

#### $\alpha_t$

$\alpha_t$ 조건은 열전달 경계조건에 따라 달라진다.

* 단열(Adiabatic)일 때 : _compressible::alphatWallFunction_
* 나머지 조건일 때 : _compressible::alphatJayatillekeWallFunction_

## 연결 벽면(Thermo-Coupled Wall)

연결 벽면 조건은 계산 영역 내부에 있는 두께가 없는 벽면인 배플(baffle)에 대한 조건이다. 배플은 2개의 경계면이 쌍을 이루고 있으며 모두 연결 벽면 조건을 사용한다. 복합영역 문제에서 영역 사이(유체-고체, 고체-고체, 유체-유체)의 경계면에도 사용된다.

각 필드가 사용하는 openfoam의 경계조건은 다음과 같다.

* 속도 : _noSlip_
* 압력 : _fixedFluxPressure_
* 온도 : _turbulentTemperatureCoupledBaffleMixed_
* 난류 : 벽면과 같은 조건 사용
* 화학종 : _zeroGradient_

## 기타 경계조건(Misc)

### 2차원 경계(Empty), 축대칭 경계(Wedge)

OpenFOAM은 별도의 2차원 격자가 없으며, 3차원 격자의 높이 방향으로 하나의 격자가 있을 때 그 위와 아래면에 2차원 경계 조건을 주면 2차원 문제가 된다. 마찬가지로 축대칭 격자도 없으며 wedge 모향의 3차원 격자에 회전방향으로 하나의 격자가 있을 때 그 양쪽면에 축대칭 경계 조건을 주면 축대칭 문제가 된다.

### 대칭(Symmetry)

대칭 조건의 경계면에 사용한다. 

### 인터페이스(Interface)

2개 경계면의 관계를 나타내는 조건이다. 계산 영역 내부의 두께가 없고 유동이 통과하는 경계면이나, cell zone 경계면에서 같은 위치에 2개의 경계면이 있을 때 사용한다. 쌍을 이루는 2개 경계면의 격자가 일치할 필요는 없다.

내부 인터페이스(Internal Interface), 회전주기조건(Rotational Periodic), 병진주기조건(Translational Periodic)의 3가지가 있다.

openfoam의 _cyclicAMI_ 조건을 사용한다.

[회전주기조건 예제](https://baramcfd.org/tutorials/2024/06/28/mixer-post/)

### Cyclic

인터페이스와 비슷하지만 2개의 쌍을 이루는 격자가 완전히 일치해야 한다. 

### 다공성 압력 점프(Porous Jump)

다공성 압력 점프 조건은 계산영역 내부에 있는 cyclic 면에서 압력 변화를 주는 조건이다. 

[다공성 압력 점프 예제](https://baramcfd.org/tutorials/2023/09/05/porousJump-post/)

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/porousJump.png" width="300" height="300"><br>다공성 압력 점프 설정</center>

입력항목은 다음과 같다.

* Darcy Coefficient, $D$
* Inertia Coefficient, $I$
* 다공성 매질 두께(Porous media thickness, $L$)
* 연결된 경계면(Coupled boundary)

**Darcy Coefficient와 Inertial Coefficient 값이 양(+)의 값일 때는 유동이 지나가면서 압력이 낮아지고, 음(-)의 값일 때 압력이 높아진다**.

압력 변화는 다음 식으로 계산된다. 

<center>$\Delta p = -\left(D \mu U + \frac{1}{2} I \rho U^2 \right)L$</center> 

* $\mu$ : 점성계수
* $\rho$ : 밀도
* $U$ : 속도

openfoam에서 사용하는 경계조건은 압력은 _porousBafflePressure_ 이고, 나머지는 모두 _cyclic_ 이다.

### 팬(Fan)

팬 조건은 계산영역 내부에 있는 cyclic 면에 팬의 속도-압력 관계를 파일로 주는 조건이다. 

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/fan.png" width="300" height="300"><br>팬 설정</center>

**속도-압력 파일은 csv 형식으로 첫번째 열에 속도, 두번째 열에 압력이 콤마(,)로 구분된 파일이어야 한다**.

openfoam에서 사용하는 경계조건은 압력은 _fan_ 이고, 나머지는 모두 _cyclic_ 이다.


