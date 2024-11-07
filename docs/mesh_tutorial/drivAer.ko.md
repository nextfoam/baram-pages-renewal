# DrivAer

[형상 파일 다운로드](https://drive.google.com/file/d/1vgJ6DTyna02EOOMAj8kZMxf6eDoM8mr8/view?usp=sharing) 

[BaramMesh 폴더 다운로드](https://drive.google.com/file/d/1dawD37cqRtg8aPnHIgEOPY3hRzSS33OA/view?usp=sharing)

## 개요 

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/drivAer/main.png"><br>
</p>

DrivAer는 자동차 공학 분야에서 사용되는 차량 외부 디자인 및 공기역학 테스트를 위한 실제 차량 모델로 차량의 외부 형태와 공기역학적 특성을 시뮬레이션하고 평가하기 위해 사용된다. 단순화된 모델과 매우 복잡한 양산차 사이의 격차를 줄이기 위해 도입된 모델이다. 

공개되어 있는 CAD 파일을 이용하여 BARAM에서 격자를 만들고 계산하여 풍동시험 결과와 비교하는 예제이다.

형상 파일은 아래의 링크된 페이지에서 Fastback, Smooth Underbody, with Mirrors, with Wheels에 해당하는 모델을 다운 받을 수 있다.

https://www.epc.ed.tum.de/en/aer/research-groups/automotive/drivaer/geometry/


## 형상(Geometry)

형상은 차량, 앞바퀴, 뒤바퀴의 3개 stl 파일을 사용한다. 전체 영역 설정을 위한 Hex6와 차량 주위에 격자를 조밀하게 만들기 위해 3개의 육면체를 사용하여 다음 그림과 같이 구성한다. 

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/drivAer/geom.png"><br> 
</p>

[불러오기(Import)] 버튼을 클릭해서 body\_no\_wheel.stl, Wheels\_Front\_Smooth.stl, Wheels\_Rear\_Smooth.stl 파일을 선택한다.

[추가(Add)] 버튼을 클릭해서 전체 영역과 격자 조밀화를 위한 3개의 박스를 만들어준다. 

* far(전체영역) : 입구, 출구, 바닥 등의 경계를 구분하기 위해 Hex6로 생성
    + 유형(Type) : 없음(None) 
    + 최소/최대(Min./Max.) : (-14 0 -0.3) / (30 4 6) 
    + 좌우 대칭이기 때문에 y축으로 절반만 생성한다. z축의 최소값은 stl 파일의 최소값을 사용하면 바퀴의 원형과 바닥의 평면이 접하는 부분의 격자가 너무 나빠지기 때문에 조금 높게 잡아준다.

* refineBox1 : 육면체(Hex)로 생성
    + 유형(Type) : 없음(None) 
    + 최소/최대(Min./Max.) : (-2 0 -0.3) / (15 2 2.5)
    + 생성하면 refineBox1\_surface라는 면이 생성된다. 이것을 마우스 오른쪽 버튼으로 선택하고 [편집(Edit)] 버튼을 눌러 [유형(Type)]을 [없음(None)]으로 설정한다.
  
* refineBox2 : 육면체(Hex)로 생성
    + 유형(Type) : 없음(None) 
    + 최소/최대(Min./Max.) : (-1 0 -0.3) / (7 1.2 1.5)
    + 생성하면 refineBox2\_surface라는 면이 생성된다. 이것을 마우스 오른쪽 버튼으로 선택하고 [편집(Edit)] 버튼을 눌러 [유형(Type)]을 [없음(None)]으로 설정한다.
    
* refineBox3 : Hex로 생성
    + 유형(Type) : 없음(None) 
    + 최소/최대(Min./Max.) : (3 0 -0.3) / (5 1 1)
    + 생성하면 refineBox3\_surface라는 면이 생성된다. 이것을 마우스 오른쪽 버튼으로 선택하고 [편집(Edit)] 버튼을 눌러 [유형(Type)]을 [없음(None)]으로 설정한다.
  
작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 영역(Region)

위쪽의 (+) 아이콘을 눌러 영역을 만든다. 그래픽 창에 연두색으로 나타나는 선의 교차점을 마우스로 이동하여 차량 외부에 위치시킨다. [추가(Add)] 버튼을 누르면 설정이 완료된다.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/drivAer/region.png"><br> 
</p>

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 배경격자(Base Grid)

[Hex6 사용(Use Hex6)]를 선택하고 격자수를 220, 20, 32로 설정한다. [생성(Generate)] 버튼을 누르면 배경 격자가 생성된다.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/drivAer/baseGrid.png"><br> 
</p>

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->

## 격자세분화(Castellation)

[설정(Configuration)]과 [고급설정(Advanced)]는 디폴트 조건을 사용한다.

[Surface/Feature 세분화]에서 바퀴와 차체의 격자 레벨을 각각 설정한다.

+ wheel
    + 면 격자 세분화(Surface Refinement)
        + 최소 레벨(Minimum Level) : 5 
        + 최대 레벨(Maximum Level) : 5 
    + 특징선 세분화 레벨(Feature Edge refinement Level) : 1 
    + 면(Surfaces) : Wheels\_Front\_Smooth\_surface\_0, Wheels\_Rear\_Smooth\_surface\_0

<p style="text-align: center">
     <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/drivAer/drivAer_refine_wheel.png"><br> 
</p>
  
+ body
    + 면 격자 세분화(Surface Refinement)
        + 최소 레벨(Minimum Level) : 4 
        + 최대 레벨(Maximum Level) : 4 
    + 특징선 세분화 레벨(Feature Edge refinement Level) : 5 
    + 면(Surfaces) : body\_no\_wheel\_surface

<p style="text-align: center">
     <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/drivAer/driAer_refine_body.png"><br> 
</p>

[공간 세분화(Volume Refinement)]의 (+) 아이콘을 누르고 refineBox1에 대해 다음과 같이 설정한다. 

+ 그룹이름(Group Name) : refine1
+ 볼륨 격자세분화 레벨(Volume Refinement Level) : 1
+ 볼륨(Volume) : refineBox1 

다시 한번 (+) 아이콘을 누르고 refineBox2에 대해 다음과 같이 설정한다.

+ 그룹이름(Group Name) : refine2
+ 볼륨 격자세분화 레벨(Volume Refinement Level) : 2
+ 볼륨(Volume) : refineBox2 

다시 한번 (+) 아이콘을 누르고 refineBox3에 대해 다음과 같이 설정한다.

+ 그룹이름(Group Name) : refine3
+ 볼륨 격자세분화 레벨(Volume Refinement Level) : 3
+ 볼륨(Volume) : refineBox3

설정을 끝내면 화면은 다음과 같이 된다.

<p style="text-align: center">
     <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/drivAer/drivAer_refine_level.png"><br>
</p>

[분할(Refine)] 버튼을 눌러 격자세분화를 진행한다. 메뉴의  [병렬연산(Parallel)] 설정을 사용해서 병렬로 진행할 수 있다.

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 형상구현(Snap)

디폴트 설정을 그대로 사용하고 형상구현(Snap) 버튼을 누른다.

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 경계층 격자(Boundary Layer)

차량과 바퀴에 첫번째 경계층 높이는 0.001, 증가율(expansion ratio)는 1.2로 5개의 경계층 격자를 생성한다.

[설정(Configuration)]의 (+) 아이콘을 눌러 [설정(Setting)]을 추가하고 다음과 같이 설정한다.

<p style="text-align: center">
     <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/drivAer/drivAer_blayer.png"><br> 
</p>

+ 경계층 레이어 개수(Number of Layers) : 4
+ 경계층 격자 높이 설정 방법(Thickness Model Specification) : 첫번째 레이어 높이와 증가율(First and Expansion)
+ 높이 지정 방법(Size Specification) : 절대값
+ 첫번째 레이어 높이(First Layer Thickness) : 0.001
+ 증가율(Expansion Ratio) : 1.2
+ 최소 전체 높이(Min. Total Thickness) : 0.0001
+ 경계면(Boundary) : body_no_wheel_surface_0, Wheels_Front_Smooth_surface_0, Wheels_Rear_Smooth_surface_0

[고급설정(Advanced Configuration)]은 디폹트 값을 사용한다.

<p style="text-align: center">
     <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/drivAer/drivAer_boundary.png"><br> 
</p>

[적용(Apply)] 버튼을 눌러 경계층 격자를 만든다.

최종적으로 생성된 격자는 다음과 같다. 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/drivAer/finalMesh.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/drivAer/finalMesh.png)

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 내보내기(Export)

[BaramFlow 프로젝트로 내보내기(Export as BaramFlow project)] 버튼을 눌러 원하는 위치에 격자를 내보낸다.



