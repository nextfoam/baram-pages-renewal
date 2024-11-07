# 프로펠러

[형상 파일 다운로드](https://drive.google.com/file/d/1Y0-PdoUDE6MFPLRlPVwE1Q54BMvOkn2N/view?usp=sharing) 

[BaramMesh 폴더 다운로드](https://drive.google.com/file/d/1_q807VRQmB4kM47IfswJOvukThvjwI9X/view?usp=sharing)

## 개요 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/propeller/intro.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/propeller/intro.png)

* 본 예제는 OpenFOAM의 pimpleFoam tutorial에 있는 propeller의 격자를 BaramMesh로 생성하는 예제이다.
* 미끄럼 격자(Sliding mesh)로 계산하기 위해 프로펠러를 둘러싼 실린더가 있고 그 내부를 셀존으로 설정한다. 셀존을 둘러싼 면은 같은 위치에 2개가 인터페이스로 있어야 한다.

## 형상(Geometry)

형상은 주어진 propeller.stl, propelerStem.stl, far.stl 3개의 파일을 사용한다. far.stl은 원방경계를 위한 실린더인데 유동의 입출구가 구분되어 있지 않기 때문에 읽을 때 경계면을 분리해 주어야 한다. 

아래쪽의 [불러오기(import)] 버튼을 눌러 propeller.stl과 propellerStem.stl 파일을 선택한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/importSTL.png">
    <br>
</p>

아래쪽의 [불러오기(import)] 버튼을 눌러 far.stl 파일을 선택한다. [면 나누기(Split Surface)] 옵션을 활성화한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/importSTL1.png">
    <br> 
</p>

Feature Angle이 디폴트인 60인 상태에서 입구, 출구, 원방경계의 3개로 나누어지는 것을 확인할 수 있다. OK 버튼을 누른다. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/importSTL2.png"  >
    <br> split surface
</p>

[형상(Geometry)]에서 far_surface1 면을 마우스 오른쪽 버튼으로 선택하고 [편집(Edit/View)] 버튼을 눌러 이름을 inlet으로 바꾸어 준다.

[형상(Geometry)]에서 far_surface2 면을 마우스 오른쪽 버튼으로 선택하고 [편집(Edit/View)] 버튼을 눌러 이름을 outlet으로 바꾸어 준다.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/geom.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/geom.png)

미끄럼 격자(Sliding mesh)를 위한 셀존을 만들기 위해 [추가(Add)] 버튼을 눌러 실린더를 만든다. Cylinder를 선택하고 다음과 같이 설정한다.

+ Name : cellZone
+ Type : CellZone
+ Cylinder Geometry
    + Axis Point1 : (-0.06 0 0)
    + Axis Point2 : (0.08 0 0) 
+ Radius : 0.12

[형상(Geometry)]에서 cellZone\_surface 면을 마우스 오른쪽 버튼으로 선택하고 [편집(Edit/View)] 버튼을 눌러 [유형(Type)]을 [Interface]-[Non-Conformal]로 바꾸어 준다.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/cellZone.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/cellZone.png)

격자세분화를 위한 영역을 만들기 위해 [추가(Add)] 버튼을 눌러 실린더를 만든다. Cylinder를 선택하고 다음과 같이 설정한다.

+ Name : refine
+ Type : None
+ Cylinder Geometry
    + Axis Point1 : (-0.1 0 0)
    + Axis Point2 : (0.6 0 0) 
+ Radius : 0.16

[형상(Geometry)]에서 refine\_surface 면을 마우스 오른쪽 버튼으로 선택하고 [편집(Edit/View)] 버튼을 눌러 [유형(Type)]을 [없음(None)]으로 바꾸어 준다.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/refine.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/refine.png)

최종 결과는 다음 그림과 같다.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/geom1.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/geom1.png)

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 영역(Region)

위쪽의 (+) 아이콘을 눌러 영역을 만든다. 그래픽 창에 연두색으로 나타나는 선의 교차점을 마우스로 계산영역 내부에 위치시킨다. [추가(Add)] 버튼을 누르면 설정이 완료된다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/region.png"  >
    <br> 
</p>

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 배경격자(Base Grid)

격자수를 20, 12, 12로 설정한다. [생성(Generate)] 버튼을 누르면 배경 격자가 생성된다.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/baseGrid.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/baseGrid.png)


작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 격자세분화(Castellation)

### Surface/Feature 세분화

propeller에는 레벨 4~6을, stem에는 레벨 4를, cellZone에는 레벨 4를, refine에는 레벨 3을 사용한다.

[Surface/Feature 세분화]에서 프로펠러의 격자 레벨을 설정한다. (+) 아이콘을 누르고 다음과 같이 설정한다.

+ 면 격자 세분화(Surface Refinement)
    + 최소 레벨(Minimum Level) : 4
    + 최대 레벨(Maximum Level) : 6
+ 특징선 세분화 레벨(Feature Edge refinement Level) : 4
+ 면(Surfaces) : propeller

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/propeller_refine_propeller.png"  >
    <br> 
</p>

propellerStem과 셀존의 격자 레벨을 설정한다. (+) 아이콘을 누르고 다음과 같이 설정한다.

+ 면 격자 세분화(Surface Refinement)
    + 최소 레벨(Minimum Level) : 4
    + 최대 레벨(Maximum Level) : 4
+ 특징선 세분화 레벨(Feature Edge refinement Level) : 4
+ 면(Surfaces) : propellerStem, cellZone\_surface

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/propeller_refine_stem.png"  >
    <br> 
</p>

refine 영역의 격자 레벨을 설정한다. (+) 아이콘을 누르고 다음과 같이 설정한다.

+ 면 격자 세분화(Surface Refinement)
    + 최소 레벨(Minimum Level) : 3
    + 최대 레벨(Maximum Level) : 3
+ 특징선 세분화 레벨(Feature Edge refinement Level) : 3
+ 면(Surfaces) : refine\_surface

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/propeller_refine_refine.png"  >
    <br> 
</p>

### 공간 세분화(Volume Refinement)

cellZone과 refine의 레벨을 4와 3으로 설정한다.

[공간 세분화(Volume Refinement)]의 (+) 아이콘을 누르고 cellZone에 대해 다음과 같이 설정한다. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/propeller_refine_v1.png"  >
    <br> 
</p>

refine 영역의 격자 레벨을 설정한다. (+) 아이콘을 누르고 다음과 같이 설정한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/propeller_refine_v2.png"  >
    <br> 
</p>

나머지 설정은 디폴트를 사용하고 [분할(Refine)] 버튼을 누른다. 완료되면 아래 그림과 같이 격자를 확인할 수 있다.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/refineResult.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/refineResult.png)

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 형상구현(Snap)

[특징 스내핑(Feature Snapping)]의 [피처 스냅 방법(Feature Snap Type)]을 [implicit]으로 설정한다. 나머지 설정은 디폴트를 사용하고 [형상구현(Snap)] 버튼을 누른다.

[![intro](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/snap.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/snap.png)

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 경계층 격자(Boundary Layer)

propeller와 propellerStem에 경계층 격자를 만든다.

[설정(Configuration)]의 (+) 아이콘을 눌러 [설정(Setting)]을 추가하고 다음과 같이 설정한다.

+ 경계층 레이어 개수(Number of Layers) : 3
+ 경계층 격자 높이 설정 방법(Thickness Model Specification) : 첫번째 레이어 높이와 증가율(First and Expansion)
+ 높이 지정 방법(Size Specification) : 상대값(Relative)
+ 첫번째 레이어 높이(First Layer Thickness) : 0.1
+ 증가율(Expansion Ratio) : 1.2
+ 최소 전체 높이(Min. Total Thickness) : 0.2
+ 경계면(Boundary) : propeller, propellerStem

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/propeller/layer.png"  >
    <br> 
</p>

[고급설정(Advanced Configuration)]은 디폹트 값을 사용한다.

[적용(Apply)] 버튼을 눌러 경계층 격자를 만든다.

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 내보내기(Export)

[BaramFlow 프로젝트로 내보내기(Export as BaramFlow project)] 버튼을 눌러 원하는 위치에 격자를 내보낸다.

