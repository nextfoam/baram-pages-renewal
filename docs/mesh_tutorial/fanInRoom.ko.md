# 팬이 있는 실내 공간

[형상 파일 다운로드](https://drive.google.com/file/d/1Om4XvnHL5X1ck6v6JQ2PWTik_nZRK0Jv/view?usp=sharing) 

[BaramMesh 폴더 다운로드](https://drive.google.com/file/d/1nEadrr6ku_lxiWWS5YHk4JTqxrjHjkuZ/view?usp=sharing)

## 개요 

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/room/room-intro.png" ><br>
</p>

* 본 예제는 OpenFOAM의 pimpleFoam tutorial에 있는 rotatingFanInRoom의 격자를 BaramMesh로 생성하는 예제이다.
* 실내에 회전하는 팬이 있고 이것을 미끄럼 격자(sliding mesh)로 계산하기 위해 팬을 둘러싼 실린더가 있고 그 내부를 셀존으로 설정한다. 셀존을 둘러싼 면은 같은 위치에 2개가 인터페이스면으로 있어야 한다.

## 형상(Geometry)

형상은 주어진 5개의 stl 파일을 사용한다.

아래쪽의 [불러오기(import)] 버튼을 눌러 desk.stl, door.stl, fan.stl, outlet.stl, room.stl 파일을 선택한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/room/fanInRoom-importSTL.png"  >
    <br> 
</p>

desk라는 볼륨에 desk\_surface라는 면이 생기고, fan이라는 볼륨에 fan\_surface라는 면이 생긴다. room과 door와 outlet이 하나의 볼륨을 구성하기 때문에 outlet이라는 볼륨에 outlet\_surface, door, room이라는 면이 생긴다.

팬 주위에 셀존과 인터페이스면을 만들기 위해 실린더를 생성한다. 

[추가(Add)] 버튼을 눌러 실린더를 선택한다. [유형(Type)]은 [셀존(CellZone)]을 선택하고 다음과 같이 설정한다.

+ Axis Point1 : (-3 2 2.3)
+ Axis Point2 : (-3 2 2.8)
+ Radius : 0.8

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/room/fanInRoom-cylinder.png"  >
    <br> 
</p>

[형상(Geometry)]에서 Cylinder\_1\_surface 면을 마우스 오른쪽 버튼으로 선택하고 [편집(Edit/View)] 버튼을 눌러 [유형(Type)]을 [Interface]-[Non-Conformal]로 바꾸어 준다.


[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/room/fanInRoom-geom.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/room/fanInRoom-geom.png)

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 영역(Region)

위쪽의 (+) 아이콘을 눌러 영역을 만든다. 그래픽 창에 연두색으로 나타나는 선의 교차점을 마우스로 계산영역 내부에 위치시킨다. [추가(Add)] 버튼을 누르면 설정이 완료된다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/room/fanInRoom-region.png">
    <br> 
</p>

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 배경격자(Base Grid)

격자수를 65, 55, 30으로 설정한다. [생성(Generate)] 버튼을 누르면 배경 격자가 생성된다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/room/fanInRoom-baseGrid.png"  >
    <br> 
</p>

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 격자세분화(Castellation)

fan, desk 면과 cylinder 볼륨에 격자 레벨을 설정한다.

### Surface/Feature 세분화

[Surface/Feature 세분화]에서 팬의 격자 레벨을 설정한다. (+) 아이콘을 누르고 다음과 같이 설정한다.

+ 면 격자 세분화(Surface Refinement)
    + 최소 레벨(Minimum Level) : 3
    + 최대 레벨(Maximum Level) : 4
+ 특징선 세분화 레벨(Feature Edge refinement Level) : 3
+ 면(Surfaces) : fan\_surface

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/room/fanInRoom-ami.png" >
    <br> 
</p>

desk 면 설정을 위해 (+) 아이콘을 누르고 다음과 같이 설정한다.

+ 면 격자 세분화(Surface Refinement)
    + 최소 레벨(Minimum Level) : 1
    + 최대 레벨(Maximum Level) : 1
+ 특징선 세분화 레벨(Feature Edge refinement Level) : 1
+ 면(Surfaces) : desk\_surface

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/room/fanInRoom-desk.png" >
    <br> 
</p>

### 공간 세분화(Volume Refinement)

[공간 세분화(Volume Refinement)]의 (+) 아이콘을 누르고 실린더에 대해 다음과 같이 설정한다. 

+ 볼륨 격자세분화 레벨(Volume Refinement Level) : 3
+ 볼륨(Volume) : Cylinder\_1
  
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/room/fanInRoom-amiVol.png">
    <br> 
</p>

나머지 설정은 디폴트를 사용하고 [분할(Refine)] 버튼을 누른다. 완료되면 아래 그림과 같이 격자를 확인할 수 있다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/room/fanInRoom-refine.png">
    <br> 
</p>


작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 형상구현(Snap)

[특징 스내핑(Feature Snapping)]의 [피처 스냅 방법(Feature Snap Type)]을 [implicit]으로 설정한다. 나머지 설정은 디폴트를 사용하고 [형상구현(Snap)] 버튼을 누른다.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/room/fanInRoom-snap.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/room/fanInRoom-snap.png)

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 경계층 격자(Boundary Layer)

경계층격자는 생성하지 않는다. 

[적용(Apply)]-[다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.
<!-------------------------------------------------------------------------------------------------->
## 내보내기(Export)

[BaramFlow 프로젝트로 내보내기(Export as BaramFlow project)] 버튼을 눌러 원하는 위치에 격자를 내보낸다.

