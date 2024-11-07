# 형상(Geometry)

BaramMesh에서 형상은 다음의 3가지 목적으로 만들어진다.

* 물체의 형상을 표현하기 위해
* 격자 내부의 특정 영역을 구분 짓기 - 셀존을 만들거나 격자의 조밀도를 지정하기 위해
* 외부 유동의 경우 계산 영역을 정의하기 위해

## 불러오기(Import)

복잡한 물체의 형상은 CAD 파일(stl 파일)을 사용한다. 불러오기 버튼으로 파일을 선택할 수 있다. 파일을 불러올 때 면들의 특징각도(feature angle)를 이용해서 분리할 수 있다. 

면을 분리할 때 stl 파일의 삼각형 격자가 한 평면에 있더라도 수직벡터(normal vector)의 방향이 반대이면 다른 면으로 나누어지기 때문에 stl 파일의 수직벡터를 확인해야 한다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_split.png" width="400" height="400"><br>stl 파일 불러오기</center>

면 나누기(Split Surface) 옵션이 켜진 상태에서 OK 버튼을 누르면 아래 그림과 같은 창이 나타난다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_split-1.png" width="800" height="800"><br>면 나누기 설정 창</center>

특징각도에 따라 어떻게 면이 나누어지는지 확인할 수 있다.

최소 면적 한계(Area Threshold)는 전체 면적에 대한 나누어진 면의 면적 비율이 입력 값보다 작으면 큰 면과 합치게 된다. 아래쪽에 각 조각들의 면적 비율을 색깔과 숫자로 표시하여 참고할 수 있도록 하였다. stl 파일의 특성에 따라 인접한 큰 면과 합쳐질 수 없는 경우도 있다.

stl 파일을 읽어오면 형상 창에 표시되고 그 아래에 면들이 표시된다. 여러 개의 solid(stl 파일이 영역을 구분하는데 사용하는 용어)로 구성된 파일의 경우 각각의 solid 영역은 분리되어 하위 레벨로 따로 표시된다.

## 추가(Add)

간단한 물체의 형상은 추가(Add)를 통해 만들 수 있다. 주로 격자 내부의 영역 구분, 외부 유동의 계산 영역 정의 등에 사용된다.

육면체(Hex), 실린더(Cylinder), 구(Sphere), Hex6의 4가지 기본 형상이 제공된다. Hex6는 육면체인데 6개의 면이 분리되어 있는 것으로 외부 유동의 계산 영역을 정의할 때 사용된다. Hex6는 _snappyHexMesh_에서 형상으로 다루지 않기 때문에 물체의 형상을 표현하는 용도로는 제한이 있다.

4가지 기본 형상은 체적(volume)과 면(surface)로 만들어진다. 형상 목록에 면은 체적의 하위 레벨로 표시된다. 

## 유형(Type)

체적과 면은 다음과 같은 유형(type)을 갖는다. 리스트에서 항목을 오른쪽 마우스 버튼으로 선택하면 수정할 수 있다.

* 체적은 없음(None)과 셀존(CellZone) 선택
* 면은 경계면(Boundary), 없음(None), 인터페이스(Interface) 선택
    * 인터페이스는 불일치 격자(Non-Conformal)와 영역 경계면(Inter-Region) 선택

면의 유형이 경계면이면 격자의 경계면이 된다.

셀존을 위해 만든 형상의 면 유형이 경계면이 되면 외부와 내부가 분리되기 때문에 그 중 하나를 계산영역으로 인식하지 않아 격자를 만들지 않게 된다. 그래서 셀존이 경계면으로 구분될 필요가 없을 때는 없음(none)으로 주고, 미끄럼 격자(slidng mesh)에서 처럼 셀존이 움직일 때는 인터페이스로 설정해야 한다.

### 인터페이스 면의 유형

인터페이스 유형을 갖는 면은 같은 위치에 2개의 경계면이 만들어 진다. 해당 면 이름에 \_slave가 붙은 경계면이 추가로 만들어 진다. 

아무런 인터페이스 유형을 주지 않은 경우 두 면에 똑같은 격자가 만들어지고, 불일치 격자(non-conformal)인 경우 두 면의 격자가 달라질 수 있다.

다공성 압력 점프(porous jump), 팬(fan) 경계조건과 같이 OpenFOAM의 _cyclic_ 속성을 사용해야 하는 경우는 불일치 격자를 사용하면 안된다.

미끄럼 격자와 같이 한쪽은 움직이고 다른 쪽은 움직이지 않는 경우에는 불일치 격자를 사용해야 한다. 아무런 인터페이스 유형을 주지 않은 경우 두 개의 경계면이 공통된 점을 사용하기 때문에, 한쪽면은 움직이고 다른쪽은 움직이지 않는 조건에서는 사용할 수 없게 된다.

인터페이스의 유형은 OpenFOAM의 _snappyHexMeshDict_ 파일의 _castellatedMeshControlds_ 에 설정된다. 유형이 없는 경우는 _faceType_ 이 _baffle_ 이 되고, 불일치 격자는 _boundary_ 가 된다.

여러 개의 영역(region)이 있는 경우, 영역간의 경계면은 영역 경계면(inter-region)으로 주어야 한다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_geometry.png" width="800" height="800"><br>형상 불러오기와 추가의 예</center>


