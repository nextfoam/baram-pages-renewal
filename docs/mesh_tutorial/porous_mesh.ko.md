# 다공성 매질

## 개요

[형상 파일 다운로드](https://drive.google.com/file/d/1Jlqrgd5BrKkAfhzNkybtb0cF3zSEAFfA/view?usp=sharing) 

[BaramMesh 폴더 다운로드](https://drive.google.com/file/d/1tN2wyi5Tae0OYhfyoWjzP522D10Uk2rx/view?usp=sharing)

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/porousMedia/intro.png"><br> 
</p>

본 에제는 다공성 매질 조건을 사용하는 유동해석을 위한 격자생성 예제이다. 상부 왼쪽에서 유동이 유입되고 다공성 영역을 지나 아래로 유동이 흐르는 문제이다.(위 그림 왼쪽에서 파란색 부분이 다공성 영역)

OpenFOAM의 다공성 매질 모델은 다공성 영역에서 불연속적인 속도 분포가 나타나고 압력손실도 입력 조건과 조금 다른 결과를 보이는 문제가 있다. 아래 그림의 왼쪽이 Baram-v24의 결과이며, 오른쪽이 OpenFOAM 2306의 standard 솔버를 사용한 결과이다. 

Baram이 사용하는 NextFOAM에서는 다공성 영역에서 압력의 내삽(interpolation) 방법을 개선하여 이 문제를 해결하였다(이에 대한 자세한 내용은 아래 링크의 문서를 참고). 결과의 정확성과 함께 수렴성도 많이 좋아진 것을 확인할 수 있다.

### *[Porous Media 참고 문헌](https://nextfoam.co.kr/proc/DownloadProc.php?fName=231101140051_yvpJhMF0nY.pdf&realfName=10thOKUCC_OpenFOAM%EC%82%AC%EC%86%8C%ED%95%9C%EB%AC%B8%EC%A0%9C%EB%93%A4.pdf)

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/porousMedia/res.png"><br> 결과 (좌)Baram v24, (우) openfoam 2306 standard solver
</p>

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/porousMedia/residual-1.png"><br> Residual (좌)Baram v24, (우) openfoam 2306 standard solver
</p>
<br/>


## 형상(Geometry)

덕트의 형상은 stl 파일을 사용한다.

아래쪽의 [불러오기(import)] 버튼을 눌러 porousMedia.stl 파일을 선택한다.

[면 나누기(Split Surface)] 옵션을 선택하여 경계면을 구분한다. 하나의 면으로 되어있는 형상 파일이 특징각도(feature angle)에 따라 8개의 면으로 나누어진다.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/porousMedia/import.png"><br> 
</p>

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/porousMedia/split.png"><br> 
</p>

8개의 면들 중에 입구와 출구를 구분하기 쉽게 이름을 바꿔준다. 그래픽창에서 마우스로 입구를 클릭하면 [형상(Geometry)] 리스트에서 해당 면이 활성화된다. 마우스 오른쪽 버튼으로 [편집(Edit/View)] 버튼을 누르고 이름을 inlet, outlet으로 바꾸어 준다.

다공성 영역은 [추가(Add)] 버튼을 눌러 [육면체(Hex)]를 이용해서 만든다. 육면체의 설정은 다음과 같다.

* 이름(Name) : porousZone
* 종류(Type) : 셀존(CellZone)
* 최소(Min.) : (-0.3 -0.2 0.8)
* 최대(Max.) : (0.3 0.2 0.9)

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/porousMedia/hex.png"><br>
</p> 

porousZone을 생성하면 그 하위에 porousZone\_surface라는 것이 생긴다. 이것을 마우스 오른쪽 버튼으로 선택하고 [편집(Edit/View)] 버튼을 눌러 [유형(Type)]을 [없음(None)]로 바꾸어 준다.

최종적인 형상 리스트는 다음 그림과 같이 된다.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/porousMedia/geom.png"><br> 
</p> 
작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 영역(Region)

위쪽의 (+) 아이콘을 눌러 영역을 만든다. 그래픽 창에 연두색으로 나타나는 선의 교차점을 마우스로 계산영역 내부에 위치시킨다. [추가(Add)] 버튼을 누르면 설정이 완료된다.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/porousMedia/region.png"><br> 
</p> 

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 배경격자(Base Grid)

격자수를 60, 40, 120으로 설정한다. [생성(Generate)] 버튼을 누르면 배경 격자가 생성된다.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/porousMedia/baseGrid.png"><br> 
</p> 

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 격자세분화(Castellation)

전체 영역의 격자를 균일하게 만들 것이기 때문에 별도의 설정 없이 [분할(Refine)] 버튼을 누른다. 

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 형상구현(Snap)

디폴트를 사용하고 [형상구현(Snap)] 버튼을 누른다.

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 경계층 격자(Boundary Layer)

덕트 벽면에 경계층 격자를 생성한다.

[설정(Configuration)]의 (+) 아이콘을 눌러 [설정(Setting)]을 추가하고 다음과 같이 설정한다.

+ 경계층 레이어 개수(Number of Layers) : 5
+ 경계층 격자 높이 설정 방법(Thickness Model Specification) : 첫번째 레이어 높이와 증가율(First and Expansion)
+ 높이 지정 방법(Size Specification) : 상대값(Relative)
+ 첫번째 레이어 높이(First Layer Thickness) : 0.15
+ 증가율(Expansion Ratio) : 1.2
+ 최소 전체 높이(Min. Total Thickness) : 0.3
+ 경계면(Boundary) : 모든 덕트 벽면

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/porousMedia/blayer.png"><br> 
</p> 


[적용(Apply)] 버튼을 눌러 경계층 격자를 만든다.

최종적으로 생성된 격자는 다음과 같다. 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/porousMedia/layer.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/porousMedia/layer.png)

작업이 끝나면 [다음단계(Next)] 버튼을 눌러 다음 단계로 넘어간다.

<!-------------------------------------------------------------------------------------------------->
## 내보내기(Export)

[BaramFlow 프로젝트로 내보내기(Export as BaramFlow project)] 버튼을 눌러 원하는 위치에 격자를 내보낸다.



