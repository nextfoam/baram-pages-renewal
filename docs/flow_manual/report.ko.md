# 데이터 추출(Report)

계산 종료 후 결과 값을 추출하는 곳이다. 힘, 점, 면, 체적 등 4가지에 대해 값을 추출할 수 있다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/report.png"><br>데이터 추출</center>

## 힘(Force)

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/forceReport.png"><br>힘 확인</center>

유동방향(Flow Direction)은 직접입력(Direct)와 'AOA와 AOS'을 선택할 수 있다. 직접입력은 항력과 양력의 방향을 벡터로 입력하는 방식이다. 'AOA와 AOS'는 받음각과 옆미끄럼각이 0일 때의 항력, 양역 방향을 기준으로 각도를 입력한다. 회전축(Pitch axis)는 항력, 양력 방향과 수직하게 결정된다.

회전중심(Center of Rotation)과 경계면을 선택하고 계산(Compute) 버튼을 누르면 결과를 확인할 수 있다. 항력/양력/모멘트 계수와 함께 힘과 모멘트를 압력항과 점성항으로 나누어 방향별 값을 확인할 수 있다.

$C_d$, $C_l$, $C_m$을 계산할 때 필요한 기준값은 기준값(Reference Values)에서 설정한다.

모니터에서와 달리 데이터 추출에서는 저장된 데이터를 이용해서 계산하기 때문에 저장된 데이터의 유효자리수에 따라 모니터의 값과 미소한 차이가 있을 수 있다.

## 점(Point)

확인할 유동변수(Field)와 좌표(Coordinate)를 입력한다. 경계면에서의 값을 확인하고 싶다면 '경계면 상의 값'(Snap onto Boundary)에서 해당 경계면을 선택한다. 이 경우 좌표가 정확히 경계면상에 있지 않아도 좌표와 가장 인접한 지점의 값을 보여준다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/pointReport.png"><br>점의 값 확인</center>

## 면(Surface)

확인할 경계면(Surface), 유동변수(Field Variable), 함수(Report Type)을 설정한다.

함수는 면적가중평균(area-weighted average), 질량가중평균(mass-weighted average), 적분(integral), 질량유량(mass flowrate), 체적유량(volume flowrate), 최소값(minimum), 최대값(maximum), 변동율(Coefficient of variation, CoV)을 선택할 수 있다. 질량유량과 체적유량일 때는 경계면만 선택하면 되고, 다른 것들은 경계면과 필드를 선택한다. 

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/surfaceReport.png"><br>면의 값 확인</center>

## 체적(Volume)

확인할 영역(Volumes), 유동변수(Field Variable), 함수(Report Type)을 입력한다.

함수는 체적평균(volume average), 체적적분(volume integral), 최소값(minimum), 최대값(maximum), 변동율(Coefficient of variation, CoV)을 선택할 수 있다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/volumeReport.png"><br>체적 값 확인</center>



