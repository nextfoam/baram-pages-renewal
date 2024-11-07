# 혼합기 - baffle, periodic, cell zone

[형상 파일 다운로드](https://drive.google.com/file/d/1K_DUBphQi3Xqhy7E7dTtYR-s_D46DmZQ/view?usp=sharing) 

[BaramMesh 폴더 다운로드](https://drive.google.com/file/d/1IX5tscEZjO3u0pbtilcwwPb9--RIZzYS/view?usp=sharing)

## 개요 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixer/intro.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixer/intro.png)

* 본 예제는 단순한 형상의 혼합기 격자 생성 예제이다. 다중기준좌표계(MRF)를 이용하여 정상상태 계산을 하기 위한 격자를 만든다. 유동의 입출구는 없으며, 전체 형상의 1/4만 모델링하여 회전 주기조건을 사용한다. 임펠러는 두께가 없는 평면(baffle)로 처리한다.
* 형상 파일을 다운로드 하고 압축을 풀면 혼합기의 형상(mixer.stl)과 셀존 형상(cellZone.stl) 파일이 있다. 

## 형상(Geometry)

형상은 주어진 2개의 파일을 사용한다. 

아래쪽의 [불러오기(import)] 버튼을 눌러 mixer.stl과 cellZone.stl 파일을 선택한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixer/importSTL.png"  >
    <br> 
</p>

mixer.stl 파일은 wall, hub, hub_rot, impeller, periodic1, periodic2, top 등의 7개의 솔리드로 구성되어 있다. 

[형상(Geometry)]에서 cellZone을 마우스 오른쪽 버튼으로 선택하고 [편집(Edit/View)] 버튼을 눌러 [유형(Type)]을 [셀존(CellZone)]으로 바꾸어 준다.

[형상(Geometry)]에서 cellZone_surface를 마우스 오른쪽 버튼으로 선택하고 [편집(Edit/View)] 버튼을 눌러 [유형(Type)]을 [없음(None)]으로 바꾸어 준다.

[형상(Geometry)]에서 impeller를 마우스 오른쪽 버튼으로 선택하고 [편집(Edit/View)] 버튼을 눌러 [유형(Type)]을 [인터페이스(interface)]로 바꾸어 준다.(Non-Conformal, Inter-Region은 활성화하지 않는다)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixer/geom.png"  >
    <br> 
</p>


작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 영역(Region)

위쪽의 (+) 아이콘을 눌러 영역을 만든다. 그래픽 창에 연두색으로 나타나는 선의 교차점을 마우스로 계산영역 내부에 위치시킨다. [추가(Add)] 버튼을 누르면 설정이 완료된다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixer/region.png"  >
    <br> 
</p>

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 배경격자(Base Grid)

격자수를 60, 40, 80으로 설정한다. [생성(Generate)] 버튼을 누르면 배경 격자가 생성된다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixer/baseGrid.png"  >
    <br> 
</p>

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 격자세분화(Castellation)

hub, hub_rot, impeller 면에는 레벨 2를, cellZone 볼륨에는 레벨 1을 사용한다.

### Surface/Feature 세분화

[Surface/Feature 세분화]에서 면의 격자 레벨을 설정한다. (+) 아이콘을 누르고 다음과 같이 설정한다.

+ 면 격자 세분화(Surface Refinement)
    + 최소 레벨(Minimum Level) : 2
    + 최대 레벨(Maximum Level) : 2
+ 특징선 세분화 레벨(Feature Edge refinement Level) : 2
+ 면(Surfaces) : hub, hub\_rot, impeller

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixer/surfaceRefine.png"  >
    <br> 
</p>


### 공간 세분화(Volume Refinement)

[공간 세분화(Volume Refinement)]의 (+) 아이콘을 누르고 cellZone에 대해 다음과 같이 설정한다. 

+ 볼륨 격자세분화 레벨(Volume Refinement Level) : 1
+ 볼륨(Volume) : cellZone

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixer/volumeRefine.png"  >
    <br> 
</p>

[설정(Configuration)]의 [비매니폴드 엣지 유지(Keep Non-Manifold Edges)]를 활성화한다.

나머지 설정은 디폴트를 사용하고 [분할(Refine)] 버튼을 누른다. 완료되면 아래 그림과 같이 격자를 확인할 수 있다.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixer/refineResult.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixer/refineResult.png)


작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 형상구현(Snap)

디폴트 설정을 그대로 사용하고 형상구현(Snap) 버튼을 누른다.

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixer/snap.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixer/snap.png)

<!-------------------------------------------------------------------------------------------------->
## 경계층 격자(Boundary Layer)

wall, hub, hub_rot, impeller, impeller_slave에 경계층 격자를 만든다. 

[설정(Configuration)]의 (+) 아이콘을 눌러 [설정(Setting)]을 추가하고 다음과 같이 설정한다.

+ 경계층 레이어 개수(Number of Layers) : 3
+ 경계층 격자 높이 설정 방법(Thickness Model Specification) : 첫번째 레이어 높이와 증가율(First and Expansion)
+ 높이 지정 방법(Size Specification) : 상대값(Relative)
+ 첫번째 레이어 높이(First Layer Thickness) : 0.1
+ 증가율(Expansion Ratio) : 1.2
+ 최소 전체 높이(Min. Total Thickness) : 0.2
+ 경계면(Boundary) : wall, hub, hub\_rot, impeller, impeller\_slave

[고급설정(Advanced Configuration)]은 다음과 같이 설정한다.

+ 경계면 변위 스무딩(Patch Displacement Smoothing)
    + 반복계산 회수(Number of Iteration) : 0
    + 레이어 두께 스무딩(Smooth Layer Thickness) : 0 
+ 나머지는 디폴트 값을 사용한다.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixer/layer.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixer/layer.png)

[적용(Apply)] 버튼을 눌러 경계층 격자를 만든다.

<!-------------------------------------------------------------------------------------------------->
## 내보내기(Export)

[BaramFlow 프로젝트로 내보내기(Export as BaramFlow project)] 버튼을 눌러 원하는 위치에 격자를 내보낸다.
