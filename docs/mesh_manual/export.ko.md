# 내보내기(Export)

내보내기에서는 만들어진 격자를 BaramFlow에서 읽을 수 있는 형식의 폴더로 내보낸다. 셀존(cell zone)과 영역(region) 설정이 이 단계에서 이루어진다. 2차원 격자 내보내기(2D Exports)는 2차원 격자와 축대칭 격자로 내보낸다.

## 2차원 격자(2D Plane)

OpenFOAM에서 2차원 문제는 한쪽 방향으로 하나의 볼륨 격자가 있고 그 양쪽면을 _empty_ 경계조건을 사용해서 처리한다. baramMesh가 사용하는 snappyHexMesh는 국부적으로 refine을 하기 때문에 한쪽 방향으로 하나의 격자만 만드는 것이 쉽지 않다.

2차원 격자로 내보낼 때는 3차원 격자의 한쪽 면의 표면 격자를 격자가 없는 쪽으로 끌어당겨 한 층의 볼륨 격자를 만들고 기존 3차원 격자를 삭제해서 만든다. 따라서 사용자는 원하는 면을 선택하고 끌어당길 두께(thickness)를 입력하면 된다. 

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/2d-export.png" width="400" height="400"><br>2차원 격자로 내보내기</center>

아래 그림에서 왼쪽 그림은 baramMesh에서 만든 3차원 격자이다. 가운데 그림은 zMin 면을 -z 방향으로 extrude하여 한 층의 격자를 만든 것이다. 오른쪽 그림은 기존 3차원 격자를 삭제하고 한 층의 격자만 내보낸 것이다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/2d-export-1.png" width="800" height="800"><br>2차원 격자로 내보내기의 단계</center>

## 축대칭 격자(Axi-Symmetry)

OpenFOAM에서 축대칭 문제는 한쪽 방향으로 하나의 볼륨 격자가 있는 쐐기 형태의 격자에서 양쪽면을 _wedge_ 경계조건을 사용해서 처리한다. 

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/axi-export.png" width="400" height="400"><br>축대칭 격자로 내보내기</center>

축대칭 격자로 내보낼 때는 3차원 격자의 한쪽 면의 표면 격자를 회전하여 한 층의 볼륨 격자를 만들고 기존 3차원 격자를 삭제해서 만든다. 따라서 사용자는 회전시킬 면(Source boundary)과 그에 대응하는 면(Exposed boundary)를 선택하고 회전 각도, 회전 중심, 회전축을 입력하면 된다. 회전방향은 오른손 법칙으로 결정된다.

아래 그림은 붉은 색으로 표시된 3차원 격자의 왼쪽면과 오른쪽면을 회전시켰을 때 만들어지는 격자의 차이를 보여주고 있다. 회전방향은 오른손 법칙으로 결정되기 때문에 왼쪽면(zMin)을 회전시킬 면(source boundary)로 주었을 때 회전축은 -x 방향인 (-1 0 0)이 되어야 한다. 오른쪽면(maxZ)를 회전시킬 면(source boundary)로 주었을 때는 회전축이 +x 방향인 (1 0 0)이 되어야 한다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/axi-export-1.png" width="400" height="400"><br>회전시킬 면의 영향</center>

아래 그림은 회전 중심에 따른 결과를 보여준다. 회전중심의 y 좌표를 3차원 격자의 최소 y 값인 0 보다 작은 값인 (0 -0.2 0)로 주었을 때의 결과이다. 

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/axi-export-2.png" width="400" height="400"><br>회전 중심에 따른 결과</center>







