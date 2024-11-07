# 경계층 격자(Boundary Layer)

경계층 격자를 생성한다.

## 설정(Configurations)

오른쪽의 (+)를 클릭하면 경계층 격자를 생성하기 위한 조건을 설정하는 창이 열린다. 

경계층 레이어 개수(Number of Layers)에서 몇 층을 쌓을지 결정한다.

경계층격자 높이 설정 방법{Thickness Model Specification}을 선택한다. 층의 높이를 첫번째 격자, 마지막 격자, 전체 격자의 높이로 지정하는 옵션이 있다. 격자 높이의 증가율은 증가율을 지정하거나 전체 높이를 지정할 수 있다. 그리고 첫번째 높이와 마지막 높이를 지정하는 옵션이 있다.

층의 높이를 나타낼 때 절대값(Absolute)과 상대값(Relative)을 선택할 수 있다. 상대값은 경계층 격자를 쌓을 경계면의 격자 크기를 기준으로한 상대값이며, 절대값은 미터 단위의 값이다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_layerConfig.png" width="400" height="400"><br>경계층격자 설정</center>

* 마지막 레이어 높이와 증가율(Final and Expansion)
* 마지막 레이어 높이와 전체 높이(Final and Total)
* 첫번째 레이어 높이와 증가율(First and Expansion)
* 첫번째 레이어 높이와 전체 높이(First and Total)
* 전체 높이와 증가율(Total and Expansion)
* 첫번째 높이와 마지막 높이(First and Relative Final) : 이 옵션을 선택하면 높이 지정 방법이 상대값으로 자동으로 설정된다.

최소 전체 높이(Min. Total Thickness)는 전체 높이의 최소값이다. 전체 높이가 이 값보다 작으면 그 부분에 경계층 격자를 만들지 않는데 경계층 격자 높이가 너무 작을 때 격자의 품질이 낮아지는 것을 방지하기 위한 장치이다.

## 고급 설정(Advanced Configuration)

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_layerAdv.png" width="300" height="300"><br>고급 설정</center>

### Number of Grow

모서리 등의 점에서 경계층 격자를 쌓지 못할 때, 그 점과 연결된 면에서 레이어를 쌓지 않을 개수이다. 큰 값일 때 경계층 격자를 만드는데 소요되는 시간을 줄일 수 있다.

_snappyHexMeshDict_ 의 _addLayersControls.nGrow_ 에 해당한다.

아래의 그림은 특징각도 임계값(Feature Angle Threshold)의 영향으로 모서리의 점에서 경계층을 쌓지 않을 때, Number of Grow의 값에 따른 경계층 격자의 모양을 보여준다. 

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/ngrow.png" width="800" height="800"><br>Number of Grow의 영향, 0(좌), 1(중), 2(우)</center>

### 시작격자의 정적 분석(Static Analysis of Starting Mesh)

#### 특징각도 임계값(Feature Angle Threshold)

각 면의 법선 벡터가 이루는 각도가 이 값보다 크면 모서리 부분에 경계층 격자를 만들지 않는다.

아래 그림의 왼쪽은 이 값이 60일 때이다. 두 면의 법선 벡터가 이루는 각도가 90이기 때문에 이보다 작은 60일 때는 모서리 부분에 경계층 격자를 만들지 않으며, 이보다 큰 120일 때는 오른쪽 그림과 같이 모서리 부분에도 경계층 격자가 만들어진다.

이 값이 120 정도로 큰 값일 때 격자 품질 뿐 아니라 경계면의 형상이 왜곡되는 문제가 발생하는 경우도 있어 유의해야 한다.

_snappyHexMeshDict_ 의 _addLayersControls.featureAngle_ 에 해당한다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_layerFeatureAngle.png" width="800" height="800"><br>특징각도 임계값, 60(좌), 120(우)</center>


#### 최대 두께 비율(Max. Thickness Ratio)

왜곡된(warped) 셀에서 경계층 격자를 만들지 않기 위한 파라미터이다. 이 값보다 크면 경계층 격자를 만들지 않는다. 큰 값은 더 자유로운 격자 생성이 가능하지만 격자의 품질이 떨어질 수 있다. 

_snappyHexMeshDict_ 의 _addLayersControls.maxFaceThicknessRatio_ 에 해당한다.

### 경계면 변위 스무딩(Patch Displacement smoothing)

#### 반복계산 회수(Number of Iterations)

면에 수직 방향의 스무딩 반복계산 회수이다. 디폴트는 1이며 원하는 격자가 만들어지지 않는 경우 이 값을 높인다. 

_snappyHexMeshDict_ 의 _addLayersControls.nSmoothSurfaceNormals_ 에 해당한다.

#### 레이어 두께 스무딩(Smooth Layer Thickness)

표면 전체에 걸쳐 층 두께를 부드럽게 조절하기 위한 반복계산 회수이다. 보통 10이면 충분하다. 

_snappyHexMeshDict_ 의 _addLayersControls.nSmoothThickness_ 에 해당한다.

### 중심축(Medial Axis)

SnappyHexMesh는 Mesh shrinking 알고리즘으로 _displacementMedialAxis}와 _displacementMotionSolver}를 제공하는데 BaramMesh에서는 _displacementMotionSolver} 방법만 지원한다. 아래의 설정들은 이에 대한 입력이다.

#### 최소 축 각도(Min. Axis Angle)

중심축의 점들(medial axis points)을 선택하는 최소 각도 

_snappyHexMeshDict_ 의 _addLayersControls.minMedialAxisAngle_ 에 해당한다.

#### 최대 두께 비율(Max. Thickmess Ratio)

층 두께와 중앙 거리(medial distance)의 비율이 이 값보다 크면 경계층 격자의 성장을 줄인다. 

_snappyHexMeshDict_ 의 _addLayersControls.maxThicknessToMedialRatio_ 에 해당한다.

#### 스무딩 반복계산 회수(Number of Smoothing Iter.)

내부 격자 이동 방향의 스무딩 반복계산 회수이다. 

_snappyHexMeshDict_ 의 _addLayersControls.nSmoothNormals_ 에 해당한다.

#### 최대 스내핑 완화 반복회수(Max. Snapping Relaxation Iter.)

완화(relaxation) 단계의 회수이다. 

_snappyHexMeshDict_ 의 _addLayersControls.nRelaxIter_ 에 해당한다.

### 격자 축소(Mesh Shrinking)

격자 생성 과정에서 특정 영역의 격자를 축소하는 방법으로, 경계층 격자를 생성할 때 경계면 근처의 셀 크기를 줄여 좋은 품질의 격자를 얻기 위해 수행한다.

#### 버퍼 셀의 개수(Num. of Buffer Cells)

층의 수를 점진적으로 감소시키기 위해, 새로운 층 쌓기를 종료하기 위한 버퍼 영역의 개수이다. 

_snappyHexMeshDict_ 의 _addLayersControls.nBufferCellsNoExtrude_ 에 해당한다.

#### 최대 레이어 추가 반복회수(Max. Layer Addition Iter)

격자 축소(mesh shrinking)을 위한 전체 반복계산 회수이다. 이 회수에 도달하면 격자 품질과 관계없이 격자 생성이 종료된다.

아래 그림에서 모서리에 경계층 격자가 생성되지 않은 것은 격자 품질 유지를 위해 격자 축소가 적용된 결과이다. 오른쪽 그림은 격자 축소가 적용되지 않은(1인 경우) 것으로 모서리에도 경계층 격자가 만들어졌지만 격자 품질이 좋지 않게 된다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_layerShrinking.png" width="800" height="800"><br>최대 레이어 추가 반복회수, 50(좌), 1(우)</center>

_snappyHexMeshDict_ 의 _addLayersControls.nLayerIter_ 에 해당한다.

#### 완화 전 최대 반복회수(Max. Iter. Before Relax)

Relaxed mesh quality control이 적용되기 시작하는 반복계산 회수이다. 이 값의 이전까지는 메뉴의 격자품질(Mesh Quality)에 설정된 기준을 사용하고, 그 다음부터는 Relaxed mesh quality control이 사용된다. 

_snappyHexMeshDict_ 의 _addLayersControls.nRelaxedIter_ 에 해당한다.






