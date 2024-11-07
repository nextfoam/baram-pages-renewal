# 모니터(Monitor)

계산 중 결과의 모니터링을 위한 설정을 하는 곳이다. 힘, 점, 면, 체적 등 4 가지를 설정할 수 있다. 추가(Add) 버튼을 눌러 해당 항목을 추가할 수 있다. 계산이 시작되면 추가된 항목은 모니터 창에 그래프가 그려진다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/monitor.png" width="300" height="300"><br>모니터 설정</center>

계산 중 결과의 모니터링을 위한 설정을 하는 곳이다. Force, Point, Surface, Volume 등 4 가지를 설정할 수 있다. Add 버튼을 눌러 해당 항목을 추가할 수 있다. 계산이 시작되면 추가된 항목은 Monitor 창에 그래프가 그려진다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/monitor1.png"><br>모니터링 그래프 예</center>

## 힘(Force)

설정 항목은 저장 간격(Write Interval), 유동 방향(Flow Direction), 회전중심(Center of Rotation), 경계면(Boundaries)이다. 

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/force.png" width="300" height="300"><br>힘 설정</center>

'저장 간격'은 몇 번 반복계산(iteration) 마다 데이터를 저장할 것인가이다. 

'유동 방향'은 직접입력(Direct)와 'AOA와 AOS'를 선택할 수 있다. 직접입력은 항력방향(Drag)와 양력방향(Lift)을 벡터로 입력하는 방식이다. 'AOA와 AOS'는 받음각과 옆미끄럼각이 0일 때의 항력과 양력 방향을 기준으로 각도를 입력한다. 회전축(Pitch axis)는 항력 및 양역 방향과 수직하게 결정된다. 

힘과 유체력계수 데이터가 저장되며 그래프는 $C_d$, $C_l$, $C_m$이 그려진다. 이 값들을 계산할 때 필요한 기준 값은 기준값(Reference Values)에서 설정한다.

## 점(Point)

점에 대한 설정창은 아래 그림과 같다. 모니터링할 필드(Field)와 좌표(Coordinate)와 모니터링 간격(Write Interval)을 입력한다. 경계면에서의 값을 모니터링하고 싶다면 Snap onto Boundary에서 해당 경계면을 선택한다. 이 경우 좌표가 정확히 경계면상에 있지 않아도 좌표와 가장 인접한 지점의 값을 모니터링한다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/point.png" width="300" height="300"><br>점 설정</center>

## 면(Surface)

면에 대한 설정창은 다음 그림과 같다. 모니터링 할 경계면(Surface), 유동변수(Field Variable), 함수(Report Type), 저장간격(Write Interval)을 입력한다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/surface.png" width="300" height="300"><br>면 설정</center>

함수는 면적가중평균(area-weighted average), 질량가중평균(mass-weighted average), 적분(integral), 질량유량(mass flowrate), 체적유량(volume flowrate), 최소값(minimum), 최대값(maximum), 변동률(Coefficient of variation, CoV)을 선택할 수 있다. 질량유량과 체적유량일 때는 경계면만 선택하면 되고, 다른 것들은 경계면과 필드를 선택한다. 

## 체적(Volume)

체적에 대한 설정창은 아래 그림과 같다. 모니터링 할 영역(Volumes), 유동변수(Field Variable), 함수(Report Type), 저장간격(Write Interval)을 입력한다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/volume.png" width="300" height="300"><br>체적 설정</center>

함수는 체적평균(volume average), 체적적분(volume integral), 최소값(minimum), 최대값(maximum), 변동률(Coefficient of variation, CoV)을 선택할 수 있다.


