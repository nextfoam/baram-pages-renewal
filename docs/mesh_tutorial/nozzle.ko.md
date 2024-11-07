# 축대칭 노즐

[형상 파일 다운로드](https://drive.google.com/file/d/1mWnujK9XPQyt5gnSD0Z4cF3hP67tHVA1/view?usp=sharing) 

[BaramMesh 폴더 다운로드](https://drive.google.com/file/d/1h3VUvhbEv6gdD8tPpWSNXwshjvHYwYch/view?usp=sharing)

## 개요 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/blood/intro.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/blood/intro.png)

* 본 예제는 축대칭 노즐 해석을 위한 격자 생성 예제이다.
* 미국 식품의약청이 주관한 비뉴턴유체 시뮬레이션 검증을 위한 벤치마크 테스트 프로그램인 FDA's Nozzle Challenge에서 사용된 다음 논문에 있는 형상이다.

_Trias, Miquel, Antonio Arbona, Joan Massó, Borja Miñano, and Carles Bona. “FDA’s nozzle numerical simulation challenge: non-Newtonian fluid effects and blood damage.” PloS one 9, no. 3 (2014): e92638._ 

## 형상(Geometry)

형상은 주어진 fda-nozzle-scaled.stl 파일을 사용한다. 이 파일은 x-y 평면 상의 축대칭 면을 z 방향으로 extrude한 3차원 형상이다. BaramMesh에서는 축대칭 형상의 격자를 직접 만드는 것이 아니라 3차원 격자를 만들고 그 중 하나의 면을 회전시켜(extrude) 축대칭 격자로 내보내는 방식을 사용한다.

아래쪽의 [불러오기(import)] 버튼을 눌러 fda-nozzle-scaled.stl 파일을 선택한다.

[면 나누기(Split Surface)] 옵션을 선택하여 경계면을 구분한다. 옵션을 선택하고 OK 버튼을 누르면 아래 그림의 오른쪽과 같은 창이 나타난다. 8개의 경계면이 만들어지는 것을 확인할 수 있다. OK 버튼을 클릭하면 형상을 불러온다.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/nonNewtonianNozzle/importSTL.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/nonNewtonianNozzle/importSTL.png)

stl 파일을 읽으면 zone0\_suface, zone0\_surface1, ...과 같이 번호가 매겨진 8개의 면을 확인할 수 있다. 각 면을 구분하기 위해 이름을 바꾸어 준다. 마우스 오른쪽 버튼으로 면을 선택하고 [편집(Edit/View)] 버튼을 클릭하여 이름을 다음과 같이 변경한다. [유형(Type)]은 디폴트인 Boundary를 그대로 사용한다.

+ zone0\_surface : back
+ zone0\_surface1 : front
+ zone0\_surface2 : bottom
+ zone0\_surface3 : wall
+ zone0\_surface4 : wall1
+ zone0\_surface5 : out
+ zone0\_surface6 : in
+ zone0\_surface7 : wall2


<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/nonNewtonianNozzle/changeName.png"  >
    <br> 
</p>

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 영역(Region)

위쪽의 (+) 아이콘을 눌러 영역을 만든다. 그래픽 창에 연두색으로 나타나는 선의 교차점을 마우스로 계산영역 내부에 위치시킨다. [추가(Add)] 버튼을 누르면 설정이 완료된다.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/nonNewtonianNozzle/region.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/nonNewtonianNozzle/region.png)

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 배경격자(Base Grid)

격자수를 600, 50, 2로 설정한다. [생성(Generate)] 버튼을 누르면 배경 격자가 생성된다.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/nonNewtonianNozzle/baseGrid.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/nonNewtonianNozzle/baseGrid.png)

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 격자세분화(Castellation)

아무런 설정 변경 없이 [분할(Refine)] 버튼을 누른다. 완료되면 아래 그림과 같이 격자를 확인할 수 있다.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/nonNewtonianNozzle/castel.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/nonNewtonianNozzle/castel.png)

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
+ 첫번째 레이어 높이(First Layer Thickness) : 0.15
+ 증가율(Expansion Ratio) : 1.2
+ 최소 전체 높이(Min. Total Thickness) : 0.3
+ 경계면(Boundary) : wall, wall1, wall2

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/nonNewtonianNozzle/nozzle_layer.png"  >
    <br> 
</p>

[고급설정(Advanced Configuration)]은 디폹트 값을 사용한다.

[적용(Apply)] 버튼을 눌러 경계층 격자를 만든다.

최종적으로 생성된 격자는 다음과 같다. 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/nonNewtonianNozzle/layer.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/nonNewtonianNozzle/layer.png)

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 내보내기(Export)

축대칭 격자로 내보내기 위해 [2차원 격자 내보내기(2D Exports)]에 있는 [축대칭 격자(Axi-Symmetry)] 버튼을 클릭하면 다음과 같은 창이 열린다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/nonNewtonianNozzle/nozzle_export.png"  >
    <br> 
</p>

+ 경로와 이름을 설정한다.
+ 회전할 경계면(Source Boundary)에 back을 선택하고, [회전할 경계면에 대응하는 경계면(Exposed Boundary)]는 front를 선택한다.
+ 각도(Angle)은 5를 입력한다.
+ Axis Origin은 (0 0 0), Direction은 (-1 0 0)을 설정한다.

OK 버튼을 누르면 BaramFlow에서 읽을 수 있는 폴더가 생성된다. 

내보낸 격자는 다음과 같다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/nonNewtonianNozzle/nozzle_exported_mesh.png"  >
    <br> 
</p>

