# 설치와 실행

## 설치

BARAM-v24의 설치는 아래 사이트를 참고.

[https://baramcfd.org/docs/installation](https://baramcfd.org/docs/installation)

마이크로소프트 Azure 클라우드에서는 별도의 설치 없이 BARAM-v24를 사용할 수 있다. 아래의 사이트에 접속해서 사용 가능.

[https://azuremarketplace.microsoft.com/en-us/marketplace/apps/nextfoam.nextfoam_baram24_ubuntu_x86?tab=Overview](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/nextfoam.nextfoam_baram24_ubuntu_x86?tab=Overview)


## 실행

윈도우즈에서는 프로그램 설치가 완료되면 바탕화면에 BaramFlow, BaramMesh 아이콘이 만들어진다. 아이콘을 더블 클릭해서 실행한다.

리눅스는 터미널을 이용해서 파일을 실행한다. 터미널에서 BARAM-v24가 설치된 폴더로 이동 하고 파일을 실행하여 프로그램을 시작한다. 실행파일은 bash 스크립트이며 실행 명령어는 다음과 같다. 

* BaramFlow : bash baramFlow.sh
* BaramMesh : bash baramMesh.sh

BARAM을 실행하면 새로운 계산을 할 것인지 기존의 문제를 열 것인지를 선택하는 창이 나타난다.

창에는 최근에 해석한 폴더들이 표시되는데 선택하면 BaramFlow가 시작되고 해당 문제가 열린다. 기존의 문제를 열고 싶은데 창에 표시되지 않는 경우는 '기존 계산 열기'(Open) 버튼을 눌러 해당 폴더를 선택하면 된다.

새로운 계산을 할 때는 '새 작업'(New Case) 버튼을 누르고 원하는 위치에 이름을 입력하면 솔버유형(Solver Type), 다상유동 모델(Multiphase Model) 선택창이 차례로 나타난 후 BaramFlow가 실행된다. 


<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/launcher1.png" width="400" height="400"><br>시작화면</center>
 
<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/launcher2.png" width="400" height="400"><br>새 작업</center>

### 솔버 유형(Solver Type)

솔버유형은 압력기반(Pressure-based)와 밀도기반(Density-based) 중 하나를 선택한다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/launcher3.png" width="400" height="400"><br>솔버 유형 설정</center>

밀도기반 솔버는 유동의 속도가 음속보다 높아 충격파가 있는 천음속/초음속 유동 해석을 위한 솔버이다.

천음속/초음속 유동은 충격파 전후로 압력, 밀도, 온도 등의 유동 변수 값이 급격하게 변하는 현상을 안정적으로 계산하기 위한 기법들이 필요하며, 현재 BaramFlow에서는 이런 기법들이 밀도 기반 솔버인 _TSLAeroFoam_(Thin Shear Layer AeroFoam)에만 적용되어 있다. BARAM-v6에서는 압력기반 솔버인 _PCNFoam_ 이라는 솔버에 이런 기능이 적용되어 있었지만 v24에서는 이 솔버를 지원하지 않으며, 추후 지원할 계획이다.

**밀도기반 솔버인** ***TSLAeroFoam*** **은 현재 정상상태 계산만 가능하며, 비정상상태 해석 기능은 개발 예정이다.**

충격파가 없는 아음속 압축성 유동의 경우 밀도기반 솔버는 마하수가 낮을 수록 수렴성이 떨어져서 압력기반 솔버를 사용하는 것이 좋다.

화학종 혼합 및 다상유동 해석은 압력기반 솔버를 사용해야 한다.

### 다상유동 모델(Multiphase Model)

다상유동 모델은 단상유동(Off)과 자유수면(Volume of Fluid) 중 하나를 선택한다. 2개 이상의 상이 존재하는 경우 자유수면을 선택한다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/launcher4.png" width="400" height="400"><br>다상유동 설정</center>>

<!------------------------------------------------------------------>
## 주화면

프로그램을 실행하면 아래 그림과 같은 창이 나타난다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/maingui.png"><br>주화면</center>

주화면은 다음 5개의 영역으로 나눌 수 있다.

* 메뉴(Menu)
* 그래픽 툴바(Graphics Toolbar)
* 탐색(Navigation)
    + 물리모델, 물성치, 경계조건, 수치해석조건, 초기조건 등을 선택할 수 있다.
* 설정(Configration)
    + 물리모델, 물성치, 경계조건, 수치해석조건, 초기조건 등의 모델과 값을 설정할 수 있다.
* 그래픽창과 콘솔(Graphics \& Console)
    + 콘솔(Console) : 계산 진행 중에 나오는 잔차(residual), 메시지, 에러 등을 보여준다. 
    + 격자(Mesh) : 해석에 사용되는 격자를 보여준다.
    + 잔차(Residuals) : 계산 진행 중에 나오는 잔차를 그래프로 보여준다.
    + 모니터(Monitor) : 사용자가 지정한 모니터링 물리량의 그래프를 보여준다.

<!------------------------------------------------------------------>
## 메뉴

메뉴는 파일(File), 격자(Mesh), 창(View), 병렬연산(Parallel), 설정(Settings), 외부프로그램(External Tools), 도움말(Help) 등으로 구성된다.

### 파일(File)

#### 저장(Save)

모든 설정을 저장한다.

#### 다른이름으로 저장(Save As)

모든 설정을 다른 이름으로 저장하는데 계산결과는 저장하지 않는다

#### 격자 불러오기(Load Mesh)

격자를 불러온다. 지원하는 격자 파일 형식은 다음과 같다.

* openfoam  : constant 혹은 polyMesh 폴더를 선택한다. **복합영역 문제인 경우는 여러 개의 polyMesh 폴더가 있기 때문에 constant 폴더를 선택해야 한다**. 복합영역 문제의 격자에서 특정한 polyMesh 폴더를 선택하면 해당 부분만 읽어 온다.
* Fluent : msh/cas 파일을 변환한다. 아스키 파일만 가능하다. 넥스트폼이 개발한 _fluentToFoam_ 유틸리티를 사용하며, 복합영역 격자로 변환, 다면체 격자(polyhedral mesh)의 변환이 가능하다. 셀존이 2개 이상인 파일을 선택하면 다음 그림과 같은 창이 나타난다.(셀존이 하나일 때는 나타나지 않는다) 각 셀존의 이름이 나타나고 어떤 영역(region)인지와 유체/고체를 선택할 수 있다. (+) 아이콘을 눌러 영역을 추가하거나 휴지통 아이콘을 눌러 영역을 삭제할 수 있다.

<center><img src="https://blog.nextfoam.co.kr/wp-content/uploads/2024/10/fluentToFoam.png" width="300" height="300"><br>fluent 격자 변환 설정 창</center>
 
* StarCCM+ : ccm 파일을 변환한다. libccmio 라이브러리가 있어야 한다. 리눅스에서만 지원된다. 
* Gmsh : 오픈소스 격자 생성 프로그램인 Gmsh의 msh 파일을 변환한다. 아스키 파일만 가능하다.
* I-deas Universal : unv 파일을 변환한다. 아스키 파일만 가능하다.


#### 프로젝트 닫기(Close Case)

현재의 작업을 종료하고 BaramFlow의 초기 화면(New Case 혹은 Open 선택)으로 돌아간다.

#### 종료(Exit)

프로그램을 종료한다.

<!------------------------------------------------------------------>
### 격자(Mesh)

격자 정보 출력, 축소/확대(Scale), 이동(Translate), 회전(Rotate) 등의 동작을 수행한다.

* 정보(Mesh Info.) : 셀 정보와 계산영역의 크기를 보여준다.
* 축소/확대(Scale) : x, y, z 각 방향의 비율을 입력 받아 격자를 확대 또는 축소한다.
* 이동(Translate) : x, y, z 각 방향의 거리를 입력 받아 격자를 이동한다.
* 회전(Rotate) : 회전 중심의 좌표, 회전축, 각도를 입력 받아 격자를 회전한다.

### 보기(View)

콘솔, 격자, 잔차, 모니터 등의 창을 없애거나 만들 수 있다.

### 병렬연산(Parallel)

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/parallel.png" width="300" height="300"><br>병렬 연산 설정 창</center>

병렬 연산에 대한 설정을 한다. CPU 코어 개수와 병렬 연산 방식을 선택한다. 병렬 연산 방식은 컴퓨터 한대(Local Machine)과 클러스터(Cluster)가 있으며, 클러스터인 경우 노드 이름(Node List)가 적힌 텍스트 파일이 필요하다. 보기/편집 버튼을 누른 후 직접 입력하거나 파일을 불러올 수 있다. 파일 내용은 다음과 같다.

```
[node name1]   cpu=[number of cores]
[node name2]   cpu=[number of cores]
[node name3]   cpu=[number of cores]
...
```

### 설정(Settings)

해상도(Scale), 언어(Language), ParaView 3가지가 있다.

* 해상도(Scale) : 프로그램의 해상도를 조절하는 것으로 기본값인 1.0 보다 크면 창이 커지고 글자도 커진다. 새로운 설정을 적용하려면 프로그램을 닫고 다시 시작해야 한다.
* 언어(Language) : 현재 English, Suomi(핀란드어), 한국어만 지원 된다. 언어 번역은 Qt Linguist를 사용하며 사용자가 참여할 수 있다. [href{https://baramcfd.org/docs/internationalization](https://baramcfd.org/docs/internationalization)
* ParaView : ParaView 실행파일의 위치를 지정한다.


### 외부프로그램(External Tools)

후처리를 위해 ParaView를 구동한다.

### 도움말(Help)

정보(About)에서 BaramFlow와 서드파티(third party) 프로그램 정보를 확인할 수 있다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/about.png" width="300" height="300"><br>BaramFlow 정보</center>
</p>

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/thirdParty.png" width="300" height="300"><br>서드파티 프로그램 정보</center>


<!------------------------------------------------------------------>
## 그래픽 툴바(Graphics Toolbar)

3차원 그래픽에 대한 툴바에는 다음과 같은 아이콘들이 있다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/graphicsToolbar-1.png"><br>그래픽 툴바</center>


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
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/displayOption.png"> 격자 디스플레이 방법을 선택한다.
</p>

<p align='left'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/bg.png"> 바탕화면의 색상을 설정한다.
</p>



<!------------------------------------------------------------------>
## 그래픽창의 마우스 컨트롤

* 회전 : 마우스 왼쪽 버튼을 누르고 이동
* 이동 : 마우스 휠을 누르고 이동
* 축소/확대 : 마우스 휠을 돌리거나 마우스 오른쪽 버튼을 누르고 이동 
* 경계면 선택 : 마우스 왼쪽 버튼 - 선택한 면이 흰색으로 표시

