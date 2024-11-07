# 파이프 유동 

[형상 파일 다운로드](https://drive.google.com/file/d/1WHF4ptAEe0RXVily3eg5GhrAGkNcRCIQ/view?usp=sharing) 

[BaramMesh 폴더 다운로드](https://drive.google.com/file/d/1AM2EolmBIfDrNDOjVM6CTRSTFPfBFbqm/view?usp=sharing)

## 개요 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/1.1.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mixingPipe/1.1.png)

본 예제는 간단한 배관의 stl 파일을 이용해 내부 격자를 생성하는 예제이다. 파이프 내부의 경계층 격자 생성과 격자 분할 수준을 조절을 하면서 격자를 생성한다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixingpipe/1.1.png"><br>
</p>

## 형상(Geometry)

형상은 주어진 mixingpipe.stl 파일을 사용한다.
 
아래쪽의 [불러오기(Import)] 버튼을 눌러 mixingpipe.stl 파일을 선택한다. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixingpipe/import.png"><br>
</p>

여기서 [면 나누기(Split Surface)] 옵션은 형상 파일의 면을 나누어 주는 기능이다. 이것을 활성화하면 특징각도(feature angle)을 기준으로 면이 다른 이름으로 분할된다. 경계면을 나누기 위해 60도를 사용하면 아래 그림과 같이 면이 4개로 분할된다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixingpipe/split.png"><br>
</p>

형상 파일을 읽으면 다음과 같이 창이 나타난다.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixingpipe/1.3.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixingpipe/1.3.png)

왼쪽에 [형상(Geometry)]와 [디스플레이 설정(Display Control)]이라는 부분이 있다. [형상(Geometry)]에서는 면 혹은 체적의 유형을 변경하거나 삭제할 수 있고 [디스플레이 설정(Display Control)]에서는 그래픽창에 표시되는 모양을 설정한다.

[형상(Geometry)]에서 면을 선택하고 마우스 오른쪽 버튼을 눌러 [Edit/View]를 선택한다. 열린 창에서 이름을 다음과 같이 변경한다. 

+ mixingpipe\_surface → wall
+ mixingpipe\_surface_1 → outlet 
+ mixingpipe\_surface_2 → inlet1 
+ mixingpipe\_surface_3 → inlet2 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixingpipe/1.4.png"><br>
</p>

inlet1 주변에 격자를 조밀하게 만들기 위해 육면체를 만든다.

[추가(Add)] 버튼을 누르면 나타나는 창에서 [육면체(Hex)]를 선택한다. 육면체는 최소와 최대 두 점의 좌표로 다음과 같이 정의한다.

+ Name : Box 
+ Type : None
+ MIn. : (-0.5 -0.3 -1.8)
+ Max. : (0.5 0.3 0) 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixingpipe/1.7.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixingpipe/1.8.png"><br>
</p>

[형상(Geometry)]에 Box라는 체적과 Box\_surface라는 면이 추가되었다.

Box\_surface를 마우스 오른쪽 버튼으로 클릭하여 [Edit/View]를 선택한다. 육면체의 면은 경계면이 아니기 때문에 [Type]을 [None]으로 변경한다. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixingpipe/1.9.png"><br>
</p>

형상 단계는 완료 되었으며 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 영역(Region)

[영역(Region)]에서는 격자를 만들 부분을 지정한다. 다중영역(multi-region) 격자를 만들 때는 여러 개의 영역을 지정할 수 있으며 유체인지 고체인지를 지정한다. 본 예제에서는 하나의 영역만 정의한다.

위쪽의 (+) 아이콘을 클릭하면 다음과 같이 화면이 변경된다. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixingpipe/1.11.png"><br>
</p>

연두색의 선이 교차하는 지점을 마우스로 옮겨 격자가 만들어질 부분을 지정한다. 파이프 내부의 임의의 지점으로 이동한다.

[추가(Add)] 버튼을 눌러 영역을 만든다. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixingpipe/1.12.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixingpipe/1.13.png"><br>
</p>

[다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.
<!-------------------------------------------------------------------------------------------------->
## 배경격자(Base Grid)

BaramMesh에서 사용하는 snappyHexMesh 유틸리티는 배경격자를 만들고, 그것을 기준으로 격자를 분할한다.

배경격자(Base Grid)는 x, y, z 축 방향별 격자의 개수로 정의한다. 

+ 방향별 격자 개수(Number of Cells per Direction)
    + X : 20
    + Y : 5
    + Z : 40

[생성(Generate)] 버튼을 누르면 배경격자가 만들어진다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixingpipe/1.14.png"><br>
</p>

배경격자가 만들어지면 [디스플레이 설정(Display Control)]에 internalMesh, xMin, xMax, yMin, yMax, zMin, zMax 등이 나타난다. internalMesh를 선택하면 배경격자가 그래픽 창에 붉은 색으로 표시된다.

[다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 격자세분화(Castellation)

격자세분화 단계에서는 [형상(Geometry)] 단계에서 만든 mixingpipe와 Box 내부의 격자를 분할한다. 

[공간 세분화(Volume Refinement)]의 (+) 아이콘을 눌러 그룹을 2개 만든다. 아래 그림과 같이 mixingpipe 내부는 레벨 1로, box 내부는 레벨 3으로 분할되도록 설정한다. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixingpipe/1.15.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixingpipe/1.16.png"><br>
</p>

[분할(Refine)] 버튼을 눌러 격자세분화를 진행한다. 

[다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 형상구현(Snap)

형상구현 단계에서는 형상의 특징선(feature line)과 면에 격자점을 이동시켜 형상을 구현한다. 

디폴트 설정으로 형상구현(Snap) 버튼을 누르면 아래와 같은 격자가 생성된다. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixingpipe/1.17.png"><br>
</p>

[다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 경계층 격자(Boundary Layer)

[설정(Configuration)]의 (+) 아이콘을 눌러 [설정(Setting)]을 추가하고 다음과 같이 설정한다.

+ 경계층 레이어 개수(Number of Layers) : 4
+ 경계층 격자 높이 설정 방법(Thickness Model Specification) : 첫번째 레이어 높이와 증가율(First and Expansion)
+ 높이 지정 방법(Size Specification) : 상대값(relative)
+ 첫번째 레이어 높이(First Layer Thickness) : 0.1
+ 증가율(Expansion Ratio) : 1.2
+ 최소 전체 높이(Min. Total Thickness) : 0.3
+ 경계면(Boundary) : wall

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixingpipe/1.18.png"><br>
</p>

[적용(Apply)] 버튼을 눌러 경계층 격자를 만든다.

아래 그림과 같이 경계층 격자가 만들어진다. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixingpipe/1.19.png"><br>
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/mixingpipe/1.20.png"><br>
</p>

[다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 내보내기(Export)

[BaramFlow 프로젝트로 내보내기(Export as BaramFlow project)] 버튼을 눌러 원하는 위치에 격자를 내보낸다.



