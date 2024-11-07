# 외팔보(Cantilever Beam)

[형상 파일](https://drive.google.com/file/d/1WsbkUpeVhtj8RlEXlhHEVLmwsnjyem5v/view?usp=sharing) 

[BaramMesh 폴더 다운로드](https://drive.google.com/file/d/1-Gb9mxY1H6_ywJWfwfcqVFMCb8UFBfzA/view?usp=sharing)

## 개요 

* 본 예제는 Cantilever Beam (외팔보)의 stl파일을 이용해 외팔보 주변 격자를 생성하는 예제이다.

## 형상(Geometry)

형상은 주어진 cantileverBeam.stl 파일을 사용한다. 

아래쪽의 [불러오기(import)] 버튼을 눌러 cantileverBeam.stl 파일을 선택한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cantileverBeam/1.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cantileverBeam/2.png"><br>
</p>


외부유동 해석을 위해 원방 경계를 생성한다. 

[추가(Add)] 버튼을 누르면 나타나는 창에서 [Hex6]를 선택하고 다음과 같이 설정한다.

+ Name : Hex6_1 
+ Type : None 
+ MIn. : (-0.4 0 0)
+ Max. : (1.2 0.2 0.5) 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cantileverBeam/cantilever_hex6.png"><br>
</p>

격자 크기를 조밀하게 적용할 영역을 지정하기 위해, [추가(Add)] 버튼을 눌러 육면체(Hex)를 추가한다.

+ Name : Hex
+ Type : None 
+ MIn. : (-0.05 0 0)
+ Max. : (0.2 0.04 0.2)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cantileverBeam/3.png"><br>
</p>

[형상(Geometry)]에서 Hex\_1\_surface 면을 마우스 오른쪽 버튼으로 선택하고 [편집(Edit/View)] 버튼을 눌러 [유형(Type)]을 [없음(None)]으로 바꾸어 준다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cantileverBeam/4.png"><br>
</p>

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 영역(Region)

위쪽의 (+) 아이콘을 눌러 영역을 만든다. 그래픽 창에 연두색으로 나타나는 선의 교차점을 마우스로 계산영역 내부에 위치시킨다. [추가(Add)] 버튼을 누르면 설정이 완료된다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cantileverBeam/5.png"><br>
</p>

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 배경 격자(Base Grid)

[Hex6 사용(Use Hex6)]를 선택하고 격자수를 100, 12, 30으로 설정한다. [생성(Generate)] 버튼을 누르면 배경 격자가 생성된다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cantileverBeam/7.png"><br>
</p>

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 격자세분화(Castellation)

### Surface/Feature 세분화

[Surface/Feature 세분화]에서 외팔보 면의 격자 레벨을 설정한다. (+) 아이콘을 누르고 다음과 같이 설정한다.

+ 면 격자 세분화(Surface Refinement)
    + 최소 레벨(Minimum Level) : 3
    + 최대 레벨(Maximum Level) : 3
+ 특징선 세분화 레벨(Feature Edge refinement Level) : 3
+ 면(Surfaces) : cantileverBeam\_surface\_0

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cantileverBeam/cantilever_refine_surface.png"><br>
</p>

### 공간 세분화(Volume Refinement)

[공간 세분화(Volume Refinement)]의 (+) 아이콘을 누르고 Hex에 대해 다음과 같이 설정한다. 

+ 볼륨 격자세분화 레벨(Volume Refinement Level) : 2
+ 볼륨(Volume) : Hex\_1
    
나머지 설정은 디폴트를 사용하고 [분할(Refine)] 버튼을 누른다. 

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 형상구현(Snap)

모든 설정은 디폴트를 사용하고 [형상구현(Snap)] 버튼을 누른다.
작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 경계층 격자(Boundary Layer)

[설정(Configuration)]의 (+) 아이콘을 눌러 [설정(Setting)]을 추가하고 다음과 같이 설정한다.

+ 경계층 레이어 개수(Number of Layers) : 5
+ 경계층 격자 높이 설정 방법(Thickness Model Specification) : 첫번째 레이어 높이와 증가율(First and Expansion)
+ 높이 지정 방법(Size Specification) : 상대값(Relative)
+ 첫번째 레이어 높이(First Layer Thickness) : 0.1
+ 증가율(Expansion Ratio) : 1.2
+ 최소 전체 높이(Min. Total Thickness) : 0.01
+ 경계면(Boundary) : cantileverBeam

[고급설정(Advanced Configuration)]은 디폹트 값을 사용한다.

[적용(Apply)] 버튼을 눌러 경계층 격자를 만든다.

최종적으로 생성된 격자는 다음과 같다. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cantileverBeam/cantilever_mesh.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cantileverBeam/cantilever_mesh_1.png"><br>
</p>

## 내보내기(Export)

마지막으로 cantileverBeam이라는 이름으로 Export 하면 baramFlow v23에서 열 수 있는 Project 폴더, polyMesh 폴더 등이 생성된다. 
