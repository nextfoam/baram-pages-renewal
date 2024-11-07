# Gap Refinement

[형상 파일 다운로드](https://drive.google.com/file/d/1aNHAqI2Ab7C0sDQgrjo5Nc2gRofLCCyZ/view?usp=sharing) 

[BaramMesh 폴더 다운로드](https://drive.google.com/file/d/10zOv3OgFp_HgjPt-Q_3c2vdeQdsu8AYd/view?usp=sharing)

## 개요 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/intro.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/intro.png)

본 예제는 BARAM-v24.2에 추가된 [갭 분할(Gap Refinement)]의 기능에 대한 이해를 돕기 위한 것이다.

갭 분할에는 [갭 내부의 최소 셀 수(Min. Cell Layers in a gap)], [갭 분할 시작 레벨(Gap Detection Start Level)], [최대 분할 레벨(Max. Refinement Level)], [방향(Direction)], [같은 면에서의 갭도 포함(Include surface's own gap)] 등의 입력 및 옵션이 있다. 이것들의 설정에 따라 격자가 어떻게 생성되는지 알아본다.

## 형상(Geometry)

형상은 주어진 hex\_with\_self\_gap.stl 파일과 BaramMesh에서 구와 육면체를 만들어서 사용한다.

아래쪽의 [불러오기(import)] 버튼을 눌러 hex\_with\_self\_gap.stl 파일을 선택한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/geom.png"  >
    <br> 
</p>

### 형상 생성

+ 구 생성 : [추가(Add)] 버튼을 눌러 창이 나타나면 [구(Sphere)]를 선택한다. 중심점은 (0 6.5 0)을 입력하고 반경은 2를 입력한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/sphere.png"  >
    <br>
</p>

+ 육면체 생성 : [추가(Add)] 버튼을 눌러 창이 나타나면 [육면체(Hex)]를 선택한다. 최소, 최대 좌표는 (-9 9 -2), (10 9.5 2)를 입력한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/hex.png"  >
    <br>
</p>

+ 원방 경계면 생성 : [추가(Add)] 버튼을 눌러 창이 나타나면 Hex6를 선택한다. 최소, 최대 좌표는 (-20 -10 -5), (20 16 5)를 입력한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/hex6.png"  >
    <br> 
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/fullGeom.png"  >
    <br> 
</p>

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 영역(Region)

위쪽의 (+) 아이콘을 눌러 영역을 만든다. 그래픽 창에 연두색으로 나타나는 선의 교차점을 마우스로 계산영역 내부에 위치시킨다. [추가(Add)] 버튼을 누르면 설정이 완료된다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/region.png"  >
    <br> 
</p>

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 배경격자(Base Grid)

격자수를 20, 13, 5로 설정한다. [생성(Generate)] 버튼을 누르면 배경 격자가 생성된다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/baseGrid.png"  >
    <br> 
</p>

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 격자세분화(Castellation)

[공간 세분화(Volume Refinement)]의 (+) 아이콘을 누르고 다음과 같이 설정한다. 

+ 볼륨 격자세분화 레벨(Volume Refinement Level) : 1
+ 갭 분할(Gap Refinement)
    + 갭 내부의 최소 셀 수(Min. Cell Layers in a gap) : 4
    + 갭 분할 시작 레벨(Gap Detection Start Level) : 1
    + 최대 분할 레벨(Max. Refinement Level) : 4
    + 방향(Direction) : Mixed
    + 같은 면에서의 갭도 포함(Include surface's own gap) : On
+ 볼륨(Volume) : Hex6\_1

사이즈 레벨이 [갭 분할 시작 레벨] (여기서는 1)의 크기 보다 작은 영역을 [갭(gap)]으로 판단하고 여기에 최소 4개의 격자가 들어가게 한다는 것이다. [최대 분할 레벨]을 4로 주었기 때문에, 레벨이 4일 때 4개 이상이 들어가지 않더라도 더이상 분할하지 않는다. [방향]은 안쪽과 바깥쪽 모두 적용하고, 자신의 면에 의한 gap도 포함한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/castel1.png"  >
    <br> 
</p>

나머지 설정은 디폴트를 사용하고 [분할(Refine)] 버튼을 누른다. 완료되면 아래 그림과 같이 격자를 확인할 수 있다.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/refine.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/refine.png)


작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 형상구현(Snap)

[특징 스내핑(Feature Snapping)]의 [피처 스냅 방법(Feature Snap Type)]을 [implicit]으로 설정한다.

나머지 설정은 디폴트를 사용하고 [형상구현(Snap)] 버튼을 누른다.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/snap.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/snap.png)

<!-------------------------------------------------------------------------------------------------->
## 갭 분할(Gap Refinement) 설정 변경의 영향

갭 분할의 입력변수와 옵션을 바꿔서 격자를 만들어 본다.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/gaps.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/gaps.png)

### 갭 분할 시작 레벨(Gap Detection Start Level)의 영향

기본 조건에서 갭 분할 시작 레벨을 2로 바꾸면 결과는 다음과 같다.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/gap-detect.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/gap-detect.png)

갭의 크기가 사이즈 레벨 2보다 크기 때문에 아무런 분할이 되지 않는다.

### 방향(Direction의 영향

방향을 inside와 outside로 바꾸면 결과는 다음과 같다.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/gap-direction.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/gap-direction.png)

### 같은 면에서의 갭도 포함(Include surface's own gap)의 영향

이 옵션에 따른 영향은 다음과 같다

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/gap-self.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/gap-self.png)

