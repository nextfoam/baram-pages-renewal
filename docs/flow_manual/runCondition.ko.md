# 계산 조건(Run Conditions)

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/runcondition.png"><br>계산 조건 설정 - 정상상태(좌), 비정상상태(우)</center>

얼마나 계산할 것인지는 정상상태일때는 계산회수(Number of iteration)을 사용하고, 비정상상태일 때는 종료 시간(End Time)을 사용한다.

### 시간 전진 방법(Time Stepping Method)

비정상상태일 때 시간 전진 방법은 일정(fixed)와 적응시간기법(adaptive)를 선택할 수 있다. 일정할 때는 시간전진간격(time step size)가 필요하다. 적응시간기법일 때는 유동영역은 Courant Number, 고체영역은 Maximum Diffusion Number에 의해 시간전진간격이 결정된다.

Courant Number(CFL)와 Diffusion Number($D_i$)는 다음과 같이 정의된다.

<center>$CFL = U \frac{\Delta t}{\Delta x}$</center>

<center>$D_i = \alpha \frac{\Delta t}{\Delta x^2}$</center>

* $\alpha$ : thermal diffusivity
* $\Delta t$ : time step size
* $\Delta x$ : grid size

다상유동 계산시에는 추가로 Max Courant Number for VoF가 필요하다.

### 자동 저장 간격(Save Interval)

어떤 간격으로 데이터를 자동으로 저장할 것인가를 지정한다. 0이면 자동 저장을 하지 않고 마지막 결과만 저장한다. 계산이 종료되면 여기서 주어진 값과 상관없이 종료된 시점의 데이터는 저장된다.


### 가장 최근 파일만 저장(Retain Only the Most Recent Files)

최대 저장 개수(Maximum Number of Data Files)에 설정된 개수 만큼의 가장 최근 결과만 저장하고 이전 결과는 삭제한다. 

