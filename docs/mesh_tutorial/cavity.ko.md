# 공동(Cavity)

[형상 파일 다운로드](https://drive.google.com/file/d/1eJMxycnNdELkT0YJRAqvojJDZTsTstOE/view?usp=sharing) 

[BaramMesh 폴더 다운로드](https://drive.google.com/file/d/1skTN_uD0E8Gh7_Ku9ggTvKUnDp2cWCTo/view?usp=sharing)

## 개요 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavity/intro.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/cavity/intro.png)

* 본 예제는 3차원 천음속 공동(Cavity) 유동 해석을 위한 격자 생성 예제이다.
* 풍동 내부에 공동 모델이 있으며 다음 논문에 있는 형상이다.
  
  _Numerical Simulation of High-Speed Turbulent Cavity Flows, G.N.Barakos, S.J.Lawson, R.Steijil, P.Nayyar, Flow Turbulence Combust, 2009_

## 형상(Geometry)

형상은 주어진 7개의 stl 파일을 사용한다. 

아래쪽의 [불러오기(import)] 버튼을 눌러 7개의 stl 파일을 선택한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cavity/cavity-importSTL.png"  >
    <br> 
</p>

공동 모델과 원방 경계면의 6개 면으로 구성된다. 체적의 이름을 Cavity로 바꾼다. 마우스 오른쪽 버튼으로 [편집(Edit/View)]을 선택해서 각 면들의 이름을 다음과 같이 바꾸어 준다. 

+ Visualization\_Toolkit\_generated\_SLA\_file\_surface : right
+ Visualization\_Toolkit\_generated\_SLA\_file\_surface1 : forward
+ Visualization\_Toolkit\_generated\_SLA\_file\_surface2 : down
+ Visualization\_Toolkit\_generated\_SLA\_file\_surface3 : left
+ Visualization\_Toolkit\_generated\_SLA\_file\_surface4 : up
+ Visualization\_Toolkit\_generated\_SLA\_file\_surface5 : wall
+ Visualization\_Toolkit\_generated\_SLA\_file\_surface6 : back

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cavity/cavity-geom.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cavity/cavity-geom.png)

격자를 조밀하게 생성할 영역에 육면체를 만든다. 공동 부분과 모델의 벽면 근처에 다음과 같이 2개의 육면체를 만든다.

[추가(Add)] 버튼을 누르면 나타나는 창에서 [육면체(Hex)]를 선택하고 다음과 같이 설정한다.

+ Name : Hex\_1 
+ Type : None 
+ MIn. : (1.75 1.1 -0.2)
+ Max. : (2.5 1.3 0.1)

만들어진 Hex\_1\_surface 면을 마우스 오른쪽 버튼으로 선택하고 [편집(Edit/View)] 버튼을 눌러 [유형(Type)]을 [없음(None)]으로 바꾸어 준다.

다시 한번 [추가(Add)] 버튼을 누르면 나타나는 창에서 [육면체(Hex)]를 선택하고 다음과 같이 설정한다.

+ Name : Hex\_2 
+ Type : None 
+ MIn. : (1 1 0)
+ Max. : (3 1.5 0.05)

만들어진 Hex\_2\_surface 면을 마우스 오른쪽 버튼으로 선택하고 [편집(Edit/View)] 버튼을 눌러 [유형(Type)]을 [없음(None)]으로 바꾸어 준다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cavity/cavity-refineZone.png"  >
    <br> 
</p>

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 영역(Region)

위쪽의 (+) 아이콘을 눌러 영역을 만든다. 그래픽 창에 연두색으로 나타나는 선의 교차점을 마우스로 계산영역 내부에 위치시킨다. [추가(Add)] 버튼을 누르면 설정이 완료된다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cavity/cavity-region.png"  >
    <br> 
</p>

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 배경 격자(Base Grid)

격자수를 36, 24, 26으로 설정한다. [생성(Generate)] 버튼을 누르면 배경 격자가 생성된다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cavity/cavity-baseGrid.png"  >
    <br> 
</p>

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 격자세분화(Castellation)

### Surface/Feature 세분화

[Surface/Feature 세분화]에서 공동(cavity)의 격자 레벨을 설정한다. (+) 아이콘을 누르고 다음과 같이 설정한다.

+ 면 격자 세분화(Surface Refinement)
    + 최소 레벨(Minimum Level) : 2
    + 최대 레벨(Maximum Level) : 2
+ 특징선 세분화 레벨(Feature Edge refinement Level) : 2
+ 면(Surfaces) : wall

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cavity/cavity-wall.png"  >
    <br> 
</p>

### 공간 세분화(Volume Refinement)

두 개의 육면체에 대한 설정이다.

[공간 세분화(Volume Refinement)]의 (+) 아이콘을 누르고 Hex\_1에 대해 다음과 같이 설정한다. 

+ 볼륨 격자세분화 레벨(Volume Refinement Level) : 5
+ 볼륨(Volume) : Hex\_1

다시 한번 (+) 아이콘을 누르고 Hex\_2에 대해 다음과 같이 설정한다. 

+ 볼륨 격자세분화 레벨(Volume Refinement Level) : 4
+ 볼륨(Volume) : Hex\_2

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cavity/cavity-hex1.png"  >
    <br> 
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cavity/cavity-hex2.png"  >
    <br> 
</p>

나머지 설정은 디폴트를 사용하고 [분할(Refine)] 버튼을 누른다. 완료되면 아래 그림과 같이 격자를 확인할 수 있다.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cavity/cavity-refine.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cavity/cavity-refine.png)


작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 형상구현(Snap)

[특징 스내핑(Feature Snapping)]의 [피처 스냅 방법(Feature Snap Type)]을 [implicit]으로 설정한다.

나머지 설정은 디폴트를 사용하고 [형상구현(Snap)] 버튼을 누른다.

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 경계층 격자(Boundary Layer)

[설정(Configuration)]의 (+) 아이콘을 눌러 [설정(Setting)]을 추가하고 다음과 같이 설정한다.

+ 경계층 레이어 개수(Number of Layers) : 5
+ 경계층 격자 높이 설정 방법(Thickness Model Specification) : 첫번째 레이어 높이와 증가율(First and Expansion)
+ 높이 지정 방법(Size Specification) : 상대값(Relative)
+ 첫번째 레이어 높이(First Layer Thickness) : 0.1
+ 증가율(Expansion Ratio) : 1.2
+ 최소 전체 높이(Min. Total Thickness) : 0.3
+ 경계면(Boundary) : wall

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cavity/cavity-layer-setup.png"  >
    <br> 
</p>

[고급설정(Advanced Configuration)]은 디폹트 값을 사용한다.

[적용(Apply)] 버튼을 눌러 경계층 격자를 만든다.

최종적으로 생성된 격자는 다음과 같다. 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cavity/cavity-layer.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/cavity/cavity-layer.png)

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 내보내기(Export)

[BaramFlow 프로젝트로 내보내기(Export as BaramFlow project)] 버튼을 눌러 원하는 위치에 격자를 내보낸다.

