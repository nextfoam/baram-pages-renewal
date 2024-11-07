# 개요

## BaramMesh란

BARAM v24.4의 격자 생성 모듈 BaramMesh는 OpenFOAM의 유틸리티인 _blockMesh_ 와 _snappyHexMesh_ 를 사용하여 격자를 생성하는 모듈이다. 형상은 STL 파일을 가져올 수 있고 육면체, 구, 실린더 형상을 생성할 수 있다. 경계층 격자를 포함한 옥트리(octree) 방식의 3차원 격자를 생성할 수 있으며 영역(region), 셀존(cell zone), 인터페이스(interface), 배플(baffle)을 만들 수 있다.

OpenFOAM의 _snappyHexMesh_ 유틸리티는 다음과 같은 단계를 통해 격자를 생성한다.

1) 전체 영역을 포함하는 정렬격자 형식의 배경 격자(background mesh)를 _blockMesh_ 유틸리티를 사용하여 만든다. 

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_blockMesh.png" width="400" height="400"><br>배경 격자</center>

2) 필요한 부분에 격자를 조밀하게 만들고, 격자를 만들지 않을 부분의 격자를 삭제한다. 이 과정을 격자세분화(castellation)이라고 한다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_castellate.png" width="400" height="400"><br>격자세분화</center>

3) 면 부근의 격자점을 면의 표면으로 이동하여 정확한 형상을 구현한다. 이 과정을 형상구현(snap)이라고 한다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_snap.png" width="400" height="400"><br>형상구현</center>

4) 경계층 격자를 만든다. 이 과정을 경계층 격자(add layer)라고 한다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_layer.png" width="400" height="400"><br>경계층 격자</center>


첫번째 단계를 지나면 constant/polyMesh 폴더에 격자가 만들어지고, 2, 3, 4 단계를 지날 때 마다 1, 2, 3 폴더가 만들어 진다. 

BaramMesh에서는 위의 과정을 좀 더 세분화 해서 다음의 일곱개의 단계로 나누어 진행한다.

1. 형상(Geometry) : 형상 파일을 읽어오거나 기본 형상을 만든다.
2. 영역(Region) : 격자를 만들 영역 내부를 지정한다.
3. 배경격자(Base Grid) : 배경 격자를 만든다.
4. 격자세분화(Castellation) : 필요한 부분에 격자를 조밀하게 만들고, 격자를 만들지 않을 부분의 격자를 삭제한다.
5. 형상구현(Snap) : Surface 부근의 격자점을 surface 표면으로 이동하여 정확한 형상을 구현한다.
6. 경계층 격자(Boundary Layer) : 경계층 격자를 만든다.
7. 내보내기(Export) : BaramFlow에서 읽을 수 있는 폴더 형식으로 격자를 내보낸다. 

위의 일곱 단계는 순차적으로 진행된다.

하나의 단계가 완료되면 그 단계는 잠금 상태가 되고 다음 단계로 넘어간다. 이전 단계로 돌아가서 다른 설정으로 작업하려면 해당 단계로 가서 잠금을 해제해야 한다. 잠금을 해제하면 해당 단계 이후에 만들어진 모든 결과물은 지워진다.




 
  


