# 위어(Weir)

## 개요

[형상 파일 다운로드](https://drive.google.com/file/d/1GAW7sYRYS07-To1PhH5QCLTAK29bbbuu/view?usp=sharing) 

[BaramMesh 폴더 다운로드](https://drive.google.com/file/d/1oKVpiZEEfw-jEpjWlJTnWwcLP8zEztjg/view?usp=sharing)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/weir/main.png"><br> wave height
</p>

위어(Weir)는 수공학에서 수로에 설지하는 구조물로 물이 넘치게 만들어 특정 수위를 유지하거나 유량을 측정하는데 사용한다. CFD에서는 이론식으로 구할 수 있는 유량과 해석 결과를 비교하여 코드 검증용으로 사용하기도 한다.

본 예제는 사각 위어에서 수위가 일정한 경우에 대한 해석을 위한 격자 생성 및 계산 예제이다.

사각위어의 단순한 형상을 stl 파일로 읽어 격자를 생성한다.

물의 입구에 수두에 해당하는 압력조건을 사용하기 위해 입구 경계면을 물과 공기의 영역을 나누어 만든다. 입구의 수위는 1.6m이다.

## 형상(Geometry)

위어의 형상은 stl 파일을 사용하고, 입구의 물과 공기의 경계면을 구분하기 위해 두 개의 Hex6를 사용하여 그림과 같이 구성한다. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/weir/geom.png"><br> Geomerty 설정
</p>

아래쪽의 [불러오기(import)] 버튼을 눌러 weir.stl 파일을 선택한다.

[추가(Add)] 버튼을 누르면 나타나는 창에서 [Hex6]를 선택하고 다음과 같이 설정한다.

+ Name : Hex6_1 
+ Type : None 
+ MIn. : (-2 -1 0)
+ Max. : (2 1 1.6), 수위가 1.6인 조건이기 때문에 z 좌표를 1.6까지 생성

6개 면이 만들어 진다. 각 면을 마우스 오른쪽 버튼으로 [편집(Edit/View)]을 선택해서 이름을 다음과 같이 바꿔 준다
 
+ Hex6\_1\_xMin : water-in
+ Hex6\_1\_xMax : out
+ Hex6\_1\_yMin : front
+ Hex6\_1\_yMax : back
+ Hex6\_1\_zMin : bottom

Hex6\_1\_zMax은 경계면이 아니기 때문에 [유형(Type)]을 [없음(None)]으로 설정한다.

한번 더 [추가(Add)] 버튼을 눌러 [Hex6]를 선택하고 다음과 같이 설정한다.

+ Name : Hex6_2 
+ Type : None 
+ MIn. : (-2 -1 1.6)
+ Max. : (2 1 2)

6개 면이 만들어 진다. 각 면을 마우스 오른쪽 버튼으로 [편집(Edit/View)]을 선택해서 이름을 다음과 같이 바꿔 준다
 
+ Hex6\_2\_xMin : air-in
+ Hex6\_2\_xMax : out-1
+ Hex6\_2\_yMin : front-1
+ Hex6\_2\_yMax : back-1
+ Hex6\_2\_zMax : top

Hex6\_2\_zMin은 경계면이 아니기 때문에 [유형(Type)]을 [없음(None)]으로 설정한다.

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 영역(Region)

위쪽의 (+) 아이콘을 눌러 영역을 만든다. 그래픽 창에 연두색으로 나타나는 선의 교차점을 마우스로 계산영역 내부에 위치시킨다. [추가(Add)] 버튼을 누르면 설정이 완료된다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/weir/region.png"><br>
</p>

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 배경격자(Base Grid)

격자수를 100, 50, 50으로 설정한다. [생성(Generate)] 버튼을 누르면 배경 격자가 생성된다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/weir/baseGrid.png"><br> 
</p>

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 격자세분화(Castellation)

전체 영역의 격자를 균일하게 만들 것이기 때문에 별도의 refinement 설정 없이 Refine 버튼을 누르면 castellation이 진행된다.

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.
<!-------------------------------------------------------------------------------------------------->
## 형상구현(Snap)

디폴트 설정을 그대로 사용하고 [형상구현(Snap)] 버튼을 누른다.

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.
<!-------------------------------------------------------------------------------------------------->
## 경계층 격자(Boundary Layer)

따로 경계층 격자를 만들지 않을 것이기 때문에 [적용(Apply)] 버튼을 누르고 작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

최종적으로 생성된 격자는 다음과 같다. 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/weir/finalMesh.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/weir/finalMesh.png)

<!-------------------------------------------------------------------------------------------------->
## 내보내기(Export)

[BaramFlow 프로젝트로 내보내기(Export as BaramFlow project)] 버튼을 눌러 원하는 위치에 격자를 내보낸다.





