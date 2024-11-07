# 고속열차

[형상 파일 다운로드](https://drive.google.com/file/d/1r9S6y9TAx7m2eyg59KGejgYFvIazv1Px/view?usp=sharing) 

[BaramMesh 폴더 다운로드](https://drive.google.com/file/d/1qZzOj1Jhs8KEwuUTqY5qBIFdF3GGnwq5/view?usp=sharing)

## 개요 

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/train/intro.png"><br>
</p>

고속열차는 마하수가 0.3~0.4 범위의 아음속 압축성 유동 영역에서 주행한다. CFD에서 저속 유동에서는 SIMPLE 게열의 압력기반 솔버를, 고속유동에서는 밀도기반의 솔버를 많이 사용한다. Baram의 비압축성 솔버인 buoyantSimpleNFoam의 아음속 압축성 유동영역에서 솔버의 안정성을 검증하기 위한 에제이다.

차량 연결부, 대차(바퀴), 판토그라프를 단순화한 고속철도 모델을 사용하였으며, 열차의 속도는 400 km/h이다. 

약 200만개 정도의 격자로 300번 정도의 iteration만에(8 코어 CPU에서 20분 정도에) 수렴된 결과를 얻을 수 있었다. OpenFOAM의 표준솔버인 buoyantSimpleFoam과 rhoSimpleFoam을 사용하여 해석한 결과와 비교했을 때 훨씬 뛰어난 수렴성을 보여준다.

<!-------------------------------------------------------------------------------------------------->
## 형상(Geometry)

차량은 stl 파일을 사용한다. 전체 영역 설정을 위한 Hex6와 차량 주위에 격자를 조밀하게 만들기 위해 하나의 육면체(Hex)를 사용하여 다음 그림과 같이 구성한다. 

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/train/geom.png"><br> 
</p>

아래쪽의 [불러오기(import)] 버튼을 눌러 train.stl 파일을 선택한다.

Visualization\_Toolkit\_generated\_SLA\_File이라는 체적과 Visualization\_Toolkit\_generated\_SLA\_File\_surface라는 면이 생긴다. 면을 선택하고 마우스 오른쪽 버튼을 눌러 [Edit/View]를 선택한다. 열린 창에서 이름을 train\_surface로 변경한다. 

[추가(Add)] 버튼을 누르면 나타나는 창에서 [Hex6]를 선택하고 전체 영역을 다음과 같이 설정한다.

+ far(전체영역) : 입구, 출구, 바닥 등의 경계를 구분하기 위해 Hex6로 생성
    + 유형(Type) : 없음(None)
    + 최소/최대(Min./Max.) : (-75 -40 0) / (260 0 40) 
    + 좌우 대칭이기 때문에 y 축으로 절반만 생성한다.
    + 각 면의 이름을 inlet(far\_xMin), outlet(far\_xMax), side(far\_yMin), symmetry(far\_yMax), ground(far\_zMin), top(far\_zMax)으로 바꿔준다.

  <p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/train/far.png"><br> 
</p>

* refine box : 육면체(Hex)로 생성, 격자 분할 설정을 위한 육면체
    + 유형(Type) : 없음(None) 
    + 최소/최대(Min./Max.) : (-18 -5 0) / (100 5 7) 
    + 생성하면 Hex\_1\_surface라는 면이 생성된다. 이것을 마우스 오른쪽 버튼으로 선택하고  [편집(Edit)] 버튼을 눌러 [유형(Type)]을 [없음(None)]으로 설정한다.

  <p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/train/hex.png"><br> 
</p>
   
작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 영역(Region)

위쪽의 (+) 아이콘을 눌러 영역을 만든다. 그래픽 창에 연두색으로 나타나는 선의 교차점을 마우스로 이동하여 차량 외부에 위치시킨다. [추가(Add)] 버튼을 누르면 설정이 완료된다.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/train/region.png"><br> 
</p>

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 배경격자(Base Grid)

[Hex6 사용(Use Hex6)]를 선택하고 격자수를 100, 20, 20으로 설정한다. [생성(Generate)] 버튼을 누르면 배경 격자가 생성된다.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/train/baseGrid.png"><br> 
</p>

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 격자세분화(Castellation)

[설정(Configuration)]과 [고급설정(Advanced)]는 디폴트 조건을 사용한다.

[Surface/Feature 세분화]에서 차량의 격자 레벨을 설정한다. (+) 아이콘을 누르고 train\_surface에 대해 다음과 같이 설정한다. 

+ train\_surface
    + 면 격자 세분화(Surface Refinement)
        + 최소 레벨(Minimum Level) : 5
        + 최대 레벨(Maximum Level) : 5
    + 특징선 세분화 레벨(Feature Edge refinement Level) : 5 
    + 면(Surfaces) : train\_surface

<p style="text-align: center">
     <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/train/train_refine_surface.png"><br> 
</p>

[공간 세분화(Volume Refinement)]의 (+) 아이콘을 누르고 Hex\_1에 대해 다음과 같이 설정한다. 

+ Hex\_1
    + 볼륨 격자세분화 레벨(Volume Refinement Level) : 4
    + 볼륨(Volume) : Hex\_1

<p style="text-align: center">
     <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/train/train_refine_volume.png"><br> 
</p>

설정을 끝내면 화면은 다음과 같이 된다.

<p style="text-align: center">
     <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/train/train_refine_level.png"><br>
</p>

[분할(Refine)] 버튼을 눌러 격자세분화를 진행한다. 메뉴의 [병렬연산(Parallel)] 설정을 사용해서 병렬로 진행할 수 있다.

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 형상구현(Snap)

디폴트 설정을 그대로 사용하고 형상구현(Snap) 버튼을 누른다.

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 경계층 격자(Boundary Layer)

차량에 경계층 격자를 생성한다.

[설정(Configuration)]의 (+) 아이콘을 눌러 [설정(Setting)]을 추가하고 다음과 같이 설정한다.

+ 경계층 레이어 개수(Number of Layers) : 5
+ 경계층 격자 높이 설정 방법(Thickness Model Specification) : 첫번째 레이어 높이와 증가율(First and Expansion)
+ 높이 지정 방법(Size Specification) : 상대값(Relative)
+ 첫번째 레이어 높이(First Layer Thickness) : 0.1
+ 증가율(Expansion Ratio) : 1.2
+ 최소 전체 높이(Min. Total Thickness) : 0.3
+ 경계면(Boundary) : train\_surface

<p style="text-align: center">
     <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/train/train_blayer.png"><br> </p>

[고급설정(Advanced Configuration)]은 디폹트 값을 사용한다.

<p style="text-align: center">
     <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/train/boundary.png"><br> 
</p>

[적용(Apply)] 버튼을 눌러 경계층 격자를 만든다.

최종적으로 생성된 격자는 다음과 같다. 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/train/train_final.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/train/train_final.png)

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 내보내기(Export)

[BaramFlow 프로젝트로 내보내기(Export as BaramFlow project)] 버튼을 눌러 원하는 위치에 격자를 내보낸다.



