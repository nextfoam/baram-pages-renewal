# 팬

[형상 파일 다운로드](https://drive.google.com/file/d/1Z_PLXsIe-niyzrYEHUTkhZkboLs5eeSP/view?usp=sharing) 

[BaramMesh 폴더 다운로드](https://drive.google.com/file/d/1tGAmauRlTQr1k_YRxYIMAeVFqLozNoS8/view?usp=sharing)

## 개요 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/fan/intro.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/fan/intro.png)

* 본 예제는 간단한 형상의 원심팬 격자를 BaramMesh로 만드는 예제이다. 블레이드 두께가 매우 앏고 케이스와 블레이드의 간격이 매우 작기 때문에 형상을 잘 구현하기 위해 세심한 주의가 필요하다.
* MRF로 계산하기 위해 블레이드를 둘러싼 실린더가 있고 그 내부를 셀존으로 설정한다. 

## 형상(Geometry)

형상은 주어진 casing.stl, blade.stl, inlet.stl, outlet.stl 4개의 파일을 사용한다. 

[불러오기(Import)] 버튼을 클릭해서 4개의 stl 파일을 선택한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/fan/importSTL.png"  >
    <br> 
</p>

셀존(Cell zone)을 만들기 위해 [추가(Add)] 버튼을 눌러 실린더(Cylinder)를 선택한다. [유형(Type)]은 [셀존(CellZone)]으로 선택하고 다음과 같이 설정한다.

+ Axis Point 1 : (0 0 -0.001)
+ Axis Point 2 : (0 0 0.021)
+ Radius : 0.07

실린더가 만들어지면 cylinder\_1\_surface라는 면이 생긴다. 이것을 마우스 오른쪽 버튼으로 선택하고 [편집(Edit)] 버튼을 눌러 [유형(Type)]을 [없음(None)]으로 설정한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/fan/createCylinder.png"  >
    <br> Add cell zone geometry
</p>

최종 결과는 다음 그림과 같다.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/fan/geom1.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/fan/geom1.png)

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 영역(Region)

위쪽의 (+) 아이콘을 눌러 영역을 만든다. 그래픽 창에 연두색으로 나타나는 선의 교차점을 마우스로 계산영역 내부에 위치시킨다. [추가(Add)] 버튼을 누르면 설정이 완료된다.

[![intro](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/fan/region.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/fan/region.png)

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 배경격자(Base Grid)

격자수를 50, 50, 20으로 설정한다. [생성(Generate)] 버튼을 누르면 배경 격자가 생성된다.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/fan/baseGrid.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/fan/baseGrid.png)

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 격자세분화(Castellation)

### Surface/Feature 세분화

[Surface/Feature 세분화]에서 블레이드의 격자 레벨을 설정한다. (+) 아이콘을 누르고 다음과 같이 설정한다.

+ 면 격자 세분화(Surface Refinement)
    + 최소 레벨(Minimum Level) : 1
    + 최대 레벨(Maximum Level) : 4
+ 특징선 세분화 레벨(Feature Edge refinement Level) : 1
+ 면(Surfaces) : blade\_surface

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/fan/fan_refine_blade.png"><br> 
</p>

[Surface/Feature 세분화]에서 케이스와 셀존의 격자 레벨을 설정한다. (+) 아이콘을 누르고 다음과 같이 설정한다.

+ 면 격자 세분화(Surface Refinement)
    + 최소 레벨(Minimum Level) : 1
    + 최대 레벨(Maximum Level) : 2
+ 특징선 세분화 레벨(Feature Edge refinement Level) : 1
+ 면(Surfaces) : casing\_surface, Cylinder\_1\_surface

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/fan/fan_refine_case.png"><br> 
</p>

### 공간 세분화(Volume Refinement)

[공간 세분화(Volume Refinement)]의 (+) 아이콘을 누르고 casing에 대해 다음과 같이 설정한다. 

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/fan/volumeRefine.png"><br> 
</p>

나머지 설정은 디폴트를 사용하고 [분할(Refine)] 버튼을 누른다. 완료되면 아래 그림과 같이 격자를 확인할 수 있다.

[![intro](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/fan/refineResult.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/fan/refineResult.png)

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 형상구현(Snap)

[다중 표면 특징 스내핑(Multi-Surface Feature Snap)]을 활성화한다. 나머지 설정은 디폴트를 사용하고 [형상구현(Snap)] 버튼을 누른다.

[![intro](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/fan/snap.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/fan/snap.png)

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 경계층 격자(Boundary Layer)

블레이드와 케이스에 경계층 격자를 만든다. 

[설정(Configuration)]의 (+) 아이콘을 눌러 [설정(Setting)]을 추가하고 다음과 같이 설정한다.

+ 경계층 레이어 개수(Number of Layers) : 3
+ 경계층 격자 높이 설정 방법(Thickness Model Specification) : 첫번째 레이어 높이와 증가율(First and Expansion)
+ 높이 지정 방법(Size Specification) : 상대값(Relative)
+ 첫번째 레이어 높이(First Layer Thickness) : 0.1
+ 증가율(Expansion Ratio) : 1.2
+ 최소 전체 높이(Min. Total Thickness) : 0.3
+ 경계면(Boundary) : blade\_surface, casing\_surface

[고급설정(Advanced Configuration)]은 디폹트 값을 사용한다.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/fan/fan_layer.png"><br> 
</p>


[적용(Apply)] 버튼을 눌러 경계층 격자를 만든다.

최종적으로 생성된 격자는 다음과 같다. 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/fan/fan_final.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/fan/fan_final.png)

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 내보내기(Export)

[BaramFlow 프로젝트로 내보내기(Export as BaramFlow project)] 버튼을 눌러 원하는 위치에 격자를 내보낸다.
