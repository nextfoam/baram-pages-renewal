# 초기화(Initialization)

초기화는 계산에 사용하는 초기값을 설정하는 것과, 계산을 통해 저장된 데이터를 모두 지우고 초기화하는 역할을 한다. x, y, z 방향의 속도 성분과 압력, 온도, 체적분율 등의 초기값을 설정한다.

난류는 속도 크기(velocity scale), 난류 강도(turbulence intensity), 난류 점도 비율(viscosity ratio)를 사용한다. Spalart-Allmaras 모델을 사용할 때는 속도 크기와 난류 점도 비율만 사용한다. 이 값을 이용해서 $\nu_t$, $k$, $\epsilon$, $\omega$ $\tilde{\nu}$ 등의 값을 계산해서 초기조건으로 사용하는데, 계산 방법은 다음과 같다.

<center>$\nu_t = viscosityRatio * \nu$</center>

<center>$k = 1.5 \cdot (velocityScale \cdot turbulentIntensity)^2$</center>

<center>$\epsilon = 0.09 \cdot k^2 / \nu_t$</center>

<center>$\omega = k / \nu_t$</center>

<center>$\tilde{\nu} = viscosityRatio * \nu$</center>

## 추가설정(Advanced)

추가설정(Advanced)은 원하는 영역의 초기값을 따로 설정하는 기능이다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/setfield.png"><br>추가 설정</center>

OpenFOAM의 _setFields_ 유틸리티를 사용한다. 영역의 지정은 육면체, 실린더, 구 등의 형상과 셀존을 이용할 수 있다. 육면체는 2개 점의 좌표로 정의된다. 실린더는 2개점의 좌표와 반지름으로 정의되며, 구는 중심점과 반지름으로 정의된다. 초기화할 유동변수는 속도, 압력, 온도, 체적분율 등이다. '경계면도 포함'(Override Boundary Value)를 선택하면 영역에 포함되어 있는 경계면의 값도 해당 초기값으로 설정된다.

[특정 영역 초기값 설정 예제 : 위어(Weir) 유동](https://baramcfd.org/tutorials/2023/09/05/weir-post/)




