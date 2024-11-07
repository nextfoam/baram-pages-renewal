# AhmedBody

[형상 파일 다운로드](https://drive.google.com/file/d/1gxuKBcN6puyEF6Yv6VEYSNUz-tlHyAuD/view) 

[BaramMesh 폴더 다운로드](https://drive.google.com/file/d/1A0bp1w8HKox4DbQBxmaG1kNvZeCbMrxF/view?usp=sharing)

## 개요 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/intro.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/ahmedBody/intro.png)

S.R. Ahmed는 단순화된 자동차 모형을 이용해 후방 경사각에 따른 유동 구조의 변화를 실험을 통해 관찰하였다. 이후 이 문제는 자동차 외부 공력해석의 검증용으로 많이 사용되고 있다. 이 예제에서는 후방 경사각도가 25°인 형상에 대해 외부유동 해석을 위한 격자를 만든다.

ref : S.R. Ahmed, G. Ramm, Some Salient Features of the Time-Averaged Ground Vehicle Wake, SAE-Paper 840300, 1984

## 형상(Geometry)

형상은 주어진 ahmed.stl 파일을 사용한다.

아래쪽의 [불러오기(import)] 버튼을 눌러 ahmed.stl을 선택한다. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/ahmedBody/1.png"><br>
</p>

형상 파일을 읽으면 다음과 같이 창이 나타난다.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/ahmedBody/ahmed_import.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/ahmedBody/ahmed_import.png)

자동차의 외부유동 해석을 위해 원방 경계를 생성한다. 

[추가(Add)] 버튼을 누르면 나타나는 창에서 [Hex6]를 선택하고 다음과 같이 설정한다.

+ Name : Hex6_1 
+ Type : None 
+ MIn. : (-4.5 -0.05 0)
+ Max. : (10 5 3) 

좌우 대칭 형상이기 때문에 +z 방향으로 절반만 격자를 만들기 위해 최소 z 좌표를 0으로 준다. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/ahmedBody/ahmed_hex6.png"><br>
</p>

이후 생성된 면들의 이름을 다음과 같이 변경한다. 

+ Hex6\_1\_xMin → inlet 
+ Hex6\_1\_xMax → outlet 
+ Hex6\_1\_yMin → wall 
+ Hex6\_1\_yMax → sky 
+ Hex6\_1\_zMin → left\_side 
+ Hex6\_1\_zMax → right\_side 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/ahmedBody/4.png"><br>
</p>

격자 크기를 조밀하게 적용할 영역을 지정하기 위해, [추가(Add)] 버튼을 눌러 육면체(Hex)를 추가한다.

+ Name : Refinement1
+ Type : None 
+ MIn. : (-0.6 -0.05 0)
+ Max. : (1 0.4 0.27)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/ahmedBody/5.png"><br>
</p>

[형상(Geometry)]에 Refinement1\_surface이 생긴다. 이 면을 선택하고 마우스 오른쪽 버튼으로 편집을 누른다. [유형(Type)]을 [없음(None)]으로 바꿔준다.

다시 한 번 [추가(Add)] 버튼을 눌러 육면체(Hex)를 추가한다.

+ Name : Refinement2 
+ Type : None
+ MIn. : (-0.9 -0.05 0)
+ Max. : (1.7 0.6 0.5)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/ahmedBody/6.png"><br>
</p>

[형상(Geometry)]에 Refinement2\_surface이 생긴다. 이 면을 선택하고 마우스 오른쪽 버튼으로 편집을 누른다. [유형(Type)]을 [없음(None)]으로 바꿔준다.

최종 형상은 다음과 같다.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/ahmedBody/ahmed_add.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/ahmedBody/ahmed_add.png)

[다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 영역(Region)

위쪽의 (+) 아이콘을 눌러 영역을 만든다. 그래픽 창에 연두색으로 나타나는 선의 교차점을 마우스로 이동하여 차량 외부에 위치시킨다. [추가(Add)] 버튼을 누르면 설정이 완료된다.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/ahmedBody/ahmed_region.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/ahmedBody/ahmed_region.png)

[다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.
<!-------------------------------------------------------------------------------------------------->
## 배경격자(Base Grid)

[Hex6 사용(Use Hex6)]를 선택하고 격자수를 100, 35, 20으로 설정한다. [생성(Generate)] 버튼을 누르면 배경 격자가 생성된다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/ahmedBody/ahmed_basegrid.png"><br>
</p>


[다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.
<!-------------------------------------------------------------------------------------------------->
## 격자세분화(Castellation)

[설정(Configuration)]과 [고급설정(Advanced)]는 디폴트 조건을 사용한다.

차량 주변의 육면체 Refinement1, Refinement에 대한 격자 분할을 정의한다.

[공간 세분화(Volume Refinement)]의 (+) 아이콘을 누르고 Refinement1에 대해 다음과 같이 설정한다. 

+ 그룹이름(Group Name) : Refinement1
+ 볼륨 격자세분화 레벨(Volume Refinement Level) : 5 
+ 볼륨(Volume) : refinement1

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/ahmedBody/10.png"><br>
</p>

다시 한번 (+) 아이콘을 누르고 Refinement2에 대해 다음과 같이 설정한다.

+ 그룹이름(Group Name) : Refinement2
+ 볼륨 격자세분화 레벨(Volume Refinement Level) : 3 
+ 볼륨(Volume) : refinement2

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/ahmedBody/11.png"><br>
</p>

병렬연산을 하고 싶다면 메뉴의 [병렬연산(Parallel)]-[환경설정(Environment)]를 클릭하고, [코어수(Number of Cores)]에 원하는 값을 입력한다. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/ahmedBody/parallel.png"><br>
</p>

[분할(Refine)] 버튼을 눌러 격자세분화를 진행한다. 

[다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 형상구현(Snap)

디폴트 설정을 그대로 사용하고 형상구현(Snap) 버튼을 누른다.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/ahmedBody/ahmed_snap.png"><br>
</p>


작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 경계층 격자(Boundary Layer)

차량에 경계층 격자를 만든다. 

[설정(Configuration)]의 (+) 아이콘을 눌러 [설정(Setting)]을 추가하고 다음과 같이 설정한다.

+ 경계층 레이어 개수(Number of Layers) : 5
+ 경계층 격자 높이 설정 방법(Thickness Model Specification) : 첫번째 레이어 높이와 증가율(First and Expansion)
+ 높이 지정 방법(Size Specification) : 상대값(relative)
+ 첫번째 레이어 높이(First Layer Thickness) : 0.1
+ 증가율(Expansion Ratio) : 1.2
+ 최소 전체 높이(Min. Total Thickness) : 0.3
+ 경계면(Boundary) : nose1, nose2, nose3, nose5, rear, side, slant, top, leg, bottom

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/ahmedBody/ahmed_layer.png"><br>
</p>

[적용(Apply)] 버튼을 눌러 경계층 격자를 만든다.

최종적으로 생성된 격자는 다음과 같다. 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/ahmedBody/ahmed_final.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/ahmedBody/ahmed_final.png)

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 내보내기(Export)

[BaramFlow 프로젝트로 내보내기(Export as BaramFlow project)] 버튼을 눌러 원하는 위치에 격자를 내보낸다.



