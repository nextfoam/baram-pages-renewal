# 설치와 실행

## 설치

BARAM-v24의 설치는 아래 사이트를 참고.

[https://baramcfd.org/docs/installation](https://baramcfd.org/docs/installation)

마이크로소프트 Azure 클라우드에서는 별도의 설치 없이 BARAM-v24를 사용할 수 있다. 아래의 사이트에 접속해서 사용 가능.

[https://azuremarketplace.microsoft.com/en-us/marketplace/apps/nextfoam.nextfoam_baram24_ubuntu_x86?tab=Overview](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/nextfoam.nextfoam_baram24_ubuntu_x86?tab=Overview)

## 실행

윈도우즈에서는 프로그램 설치가 완료되면 바탕화면에 BaramFlow, BaramMesh 아이콘이 만들어진다.

리눅스 터미널에서 실행할 때의 명령어는 다음과 같다.

* BaramFlow : baramFlow.sh 혹은 python -m baramFlow.main
* BaramMesh : baramMesh.sh 혹은 python -m baramMesh.main

baramMesh를 실행하면 새로운 격자를 만들 것인지 기존의 격자 작업을 열 것인지를 선택하는 창이 나타난다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_start.png" width="400" height="400"><br>시작 화면</center>

창에는 최근에 작업한 폴더들이 표시되는데 선택하면 baramMesh가 시작되고 해당 격자 작업이 열린다. 기존의 격자 작업을 열고 싶은데 창에 표시되지 않는 경우는 기존 계산 열기(Open) 버튼을 눌러 해당 폴더를 선택하면 된다.

새로운 격자를 만들 새 계산(New Case) 버튼을 누르고 원하는 위치에 이름을 입력하면 baramMesh가 실행된다.

### 주화면

프로그램을 실행하면 아래 그림과 같은 창이 나타난다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_mainWindow.png" width="800" height="800"><br>주화면</center>

주화면은 다음 6개의 영역으로 나눌 수 있다.

* 메뉴(Menu)
* 진행단계 패널(Progress panel) : 화면 상단의 현재 작업의 상태를 나타내는 부분
* 설정 창(Configration window) : 화면의 왼쪽에 위치. 각 단계에서 필요한 모델과 값을 설정하는 부분
* 디스플레이 컨트롤(Display Control) : 화면의 왼쪽 두번째에 위치. 형상과 격자의 디스플레이에 관한 설정 부분
* 그래픽 창 : 형상과 격자를 보여주는 부분
* 메시지 창(Console Log) : 어플리케이션 실행시 로그를 보여주는 부분

### 메뉴

파일(File), 격자 품질(Mesh Quality), 보기(View), 병렬연산(Parallel), 설정(Settings), 도움말(Help) 등의 메뉴가 있다.

#### 파일

파일에는 새 프로젝트(New), 열기(Open), 최근 작업 열기(Open Recent), 저장(Save), 종료(Exit) 등의 하위 메뉴가 있다. 

#### 격자 품질

격자 품질을 클릭하면 아래 그림과 같은 창이 나타난다. 여기에 설정된 값을 기준으로 격자가 만들어 진다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_parameters.png" width="400" height="400"><br>격자 품질 매개변수</center>

#### 병렬연산

병렬연산을 사용할 때는 병렬연산 메뉴를 사용한다. 환경설정(Environment)를 클릭하면 아래 그림의 창이 나타난다. 코어수를 입력하고 컴퓨터 한대인지 클러스터인지를 선택한다. 

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_parallel.png" width="300" height="300"><br>병렬연산 설정</center>

클러스터일 때는 호스트 파일을 만들어 주어야 하는데 보기/편집(View/Edit) 버튼을 눌러 만들 수 있다. 파일 내용은 다음과 같다.

```
[node name1]   cpu=[number of cores]
[node name2]   cpu=[number of cores]
[node name3]   cpu=[number of cores]
...
```
#### 설정

설정에는 해상도(Scale), 언어(Language) 2가지가 있다.

해상도는 기본값인 1.0 보다 커지면 창이 커지고 글자도 커진다. 새로운 설정을 적용하려면 프로그램을 닫고 다시 시작해야 한다.

언어는 현재 English, Suomi(핀란드어), 한국어만 지원 된다. 언어 번역은 Qt Linguist를 사용하며 사용자가 직접 참여할 수 있다. 참여 방법은 아래의 링크를 참조.

[https://baramcfd.org/docs/internationalization](https://baramcfd.org/docs/internationalization)

### 진행단계 패널(Progress panel)

7가지 단계가 표시되어 있다. 현재 이후의 단계는 비활성화되어 있다. 현재 단계가 끝나고 다음 단계(Next) 버튼을 누르면 현 단계는 잠금 상태가 되고 다음 단계가 활성화 된다.

이전 단계로 돌아가면 상태를 확인할 수는 있지만 수정할 수는 없다. 이전 단계를 수정하고 싶으면 잠금해제(Unlock) 버튼을 눌러 잠금을 해제해야 한다. 잠금을 해제하면 그 이후는 모든 단계의 잠금이 해제되고 만들어진 격자 데이터도 지워진다. 

### 설정 창(Configration Window)

각 단계에서 필요한 모델과 값을 설정한다.

### 디스플레이 설정(Display Control)

형상과 격자의 리스트가 표시된다. 유형(Type)은 Geomerty, Boundary, Mesh 3가지가 있다. [1.형상(Geometry)], [2.영역(Region)] 단계에서는 Geometry만 표시되고 [3.배경격자(Base Grid)]에서 배경 격자를 만들면 그 때부터 Boundary와 Mesh가 나타난다. 이름(Name)과 유형(Type)을 클릭하여 리스트를 정렬할 수 있다.

리스트를 마우스 오른쪽 버튼으로 클릭하면 숨기기(Hide), 투명도(Opacity), 색깔(Color), 디스플레이 모드(Display Mode), 축단면 절단 무시(No Cut) 등이 나타난다. 이것으로 디스플레이 형태를 설정할 수 있다. 각각의 항목별로 설정할 수도 있고 마우스 드래그로 여러 개를 한꺼번에 설정할 수 있다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_display.png" width="300" height="300"><br>디스플레이 설정</center>

<p align='left'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/viewEnable.png"> 디스플레이 설정의 on/off 버튼은 그래픽 창에 격자를 보여줄지를 선택하는 기능으로, 격자 수가 매우 많은 경우 진행 단계를 이동할 때 렌더링에 많은 시간이 소요되는 불편함을 방지하기 위한 것이다.
</p>

상단의 축단면으로 자르기(Cut)을 펼치면 잘라내기(Clip)과 단면(Slice) 옵션이 있다. 축에 수직한 단면으로 잘라서 볼 수 있다. 디스플레이 옵션에서 축단면 절단 무시(No Cut)을 선택한 개체는 잘리지 않는다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_cut.png" width="800" height="800"><br>디스플레이 설정 예</center>

### 그래픽 창

상단에 디스플레이 환경을 설정하는 툴바가 있다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/graphicsToolbar.png" width="800" height="800"><br>그래픽 툴바</center>

<p align='left'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/axis.png"> 원점(0, 0, 0)을 기준으로 해석 영역의 축 방향을 나타내준다.
</p>

<p align='left'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/ruler.png"> 그래픽창에 해석 영역의 x, y, z 좌표축을 표시한다. 
</p>

<p align='left'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/ruler-1.png"> 면 위의 두 점 사이의 거리를 확인할 수 있다. 
</p>

<p align='left'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/perspective.png"> 투영방식으로 투시(Perspective) 혹은 직교(Orthogonal)를 선택한다. 
</p>

<p align='left'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/fit.png"> 해석 모델을 그래픽창에 전체 모습이 나오게 보여준다.

</p>

<p align='left'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/viewNormal.png">  현재 상태에 가장 가까운 축 단면에서 모델을 보여준다.

</p>

<p align='left'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/rotation.png"> 해석 모델을 90도 회전한다.
</p>

<p align='left'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/meshNumber.png"> 현재 상태의 격자 수를 보여준다.
</p>

<p align='left'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/bg.png"> 바탕화면의 색상을 설정한다.
</p>

<!------------------------------------------------------------------>
### 그래픽창의 마우스 컨트롤

* 회전 : 마우스 왼쪽 버튼을 누르고 이동
* 이동 : 마우스 휠을 누르고 이동
* 축소/확대 : 마우스 휠을 돌리거나 마우스 오른쪽 버튼을 누르고 이동 
* 경계면 선택 : 마우스 왼쪽 버튼 - 선택한 면이 흰색으로 표시

