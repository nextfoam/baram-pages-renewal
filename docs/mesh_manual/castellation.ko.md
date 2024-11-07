# 격자세분화(Castellation)

이 단계는 격자를 기하학적 모델에 맞추기 위한 준비작업이다. 배경격자가 형상과 교차하는지 확인하여 격자를 분리한 다음 기하학적 경계 바깥의 격자를 제거한다. 필요한 부분의 육면체 격자를 주어진 레벨에 따라 분할하여 격자를 세분화한다. 격자의 조밀도는 사이즈 레벨(size level)로 조절하는데 배경격자 크기를 기준 0으로 사용하고, 1만큼 커질 때 마다 육면체를 4개로 분할한다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_castell.png" width="800" height="800"><br>배경격자에서 격자세분화로</center>

설정(Configuration)과 고급설정(Advance)이 있다. 격자를 나눌 부분은 면/특징선(Surface/Feature) 혹은 공간(volume)에 대해 설정할 수 있다.

## 설정(Configuration)

### 레벨 사이의 격자 개수(Number of Cells between Levels)

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_nCellsBetweenLevels.png" width="800" height="800"><br>레벨 사이의 격자 개수 (좌) 1, (중) 2, (우) 3</center>

셀 크기의 급격한 변화는 격자 시스템에 좋지 않기 때문에, 레벨 변경시 완충영역을 두는 것이다. 아래 그림에서 원의 격자 레벨이 1, 2, 3일 때의 결과를 나타내었다.

_snappyHexMeshDict_ 의 _castellatedMeshControls.nCellsBetweenLevels_ 에 해당한다.

### 특징 각도 임계값(Feature Angle Threshold)

인접한 두 격자의 법선이 이루는 각이 이 값보다 클 때, 주어진 면 격자 세분화(surface refinement)의 최대 레벨을 사용한다. 아래 그림은 두 개의 육면체가 있고 안쪽 육면체의 사이즈 레벨을 최소 레벨은 1, 최대 레벨은 2로 주었을 때, 특징 각도 임계값에 따른 격자세분화 결과를 보여준다.

_snappyHexMeshDict_ 의 _castellatedMeshControls.resolveFeatureAngle_ 에 해당한다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_resolveFeature.png" width="800" height="800"><br>특징 각도 임계값의 영향 (좌) 형상, (중) 100, (우) 30</center>

### 비매니폴드 엣지 유지(Keep Non-Manifold Edges)

면들의 특징선(feature edge)를 추출할 때 사용하는 옵션으로 3개 이상의 연결된 면을 갖는 엣지를 포함할 것인지에 대한 옵션이다.

### 열린 엣지 유지(Keep Open Edges)

면들의 특징선(feature edge)를 추출할 때 사용하는 옵션으로 연결된 면이 1개 뿐인 엣지를 포함할 것인지에 대한 옵션이다.

## 고급설정(Advanced)

### 최대 글로벌 셀 수(Max. Global Cell Count)

전체 셀 개수의 제한으로 이 값에 도달하면 셀 분할이 중단된다. 격자세분화 과정에서 계산영역이 아닌 부분의 격자를 제거하기 전의 격자수로 판단한다. 

_snappyHexMeshDict_ 의 _castellatedMeshControls.maxGlobalCells_ 에 해당한다.

### 최대 로컬 셀 수(Max. Local Cell Count)

병렬 연산시 각 코어당 최대 셀 개수이다. 병렬 연산시 refinement-followed-by 방식의 밸런싱을 사용하는데, 이 값에 도달하면 weighted balancing 방법을 사용하게 된다. 이 경우 속도가 느려질 수 있다. 

_snappyHexMeshDict_ 의 _castellatedMeshControls.maxLocalCells_ 에 해당한다.

### 최소 세분화 셀 수(Min. Refinement Cell count)

격자 세분화 과정이 소수의 셀 때문에 많은 시간이 소비되는 문제가 있을 수 있다. 남은 셀들이 여기서 지정한 값에 도달하면 격자 세분화 과정을 중단한다. 격자 세분화 과정에서 중요하지 않은 소수의 셀 때문에 너무 많은 시간이 소비되는 것을 막아주기 위한 설정이다. 

_snappyHexMeshDict_ 의 _castellatedMeshControls.minRefinementCells_ 에 해당한다.

### 최대 부하 불균형(Max. Load Unbalance)

병렬 연산에서 프로세스 당 셀 수의 상대적인 차이이다. 낮은 값은 더 자주 로드 밸런싱을 하게 되고 높은 값은 로드 밸런싱을 비활성화 할 수도 있다. 

_snappyHexMeshDict_ 의 _castellatedMeshControls.maxLoadUnbalance_ 에 해당한다.

### 독립적인 존 페이스 허용(Allow Free Standing Zone Faces)

페이스존(faceZone)이 배플(baffle)과 같이 독립적인 면일 때 true로 설정해 주어야 한다. 

_snappyHexMeshDict_ 의 _castellatedMeshControls.allowFreeStandingZoneFaces_ 에 해당한다.

## 면/특징선 세분화(Surface/Feature Refinement)

오른쪽의 (+)를 클릭하면 생성된다.

면과 특징선(feature) 각각에 대한 레벨을 지정하고 적용할 면을 선택한다. 면은 [1.형상]에서 만들어진 것들이 나타난다. 면 격자 세분화(Surface Refinement)는 최소 레벨과 최대 레벨을 줄 수 있다. 인접한 두 격자의 법선이 이루는 각이 특징 각도 임계값(Feature Angle Threshold)에 입력한 값보다 크면 최대 레벨을 사용하고 작으면 최소 레벨을 사용한다.

특징선(feature)는 형상을 만들 때 baramMesh에서 만들어 주기 때문에 따로 선택할 필요는 없다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_surfaceRefinement.png" width="400" height="400"><br>면 격자 세분화 설정</center>

## 공간 세분화(Volume Refinement)

공간의 레벨을 지정하고 적용할 볼륨을 선택한다. 볼륨은 [1.형상]에서 만들어진 것들이 니타난다. 

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_volumeRefinement.png" width="400" height="400"><br>공간 세분화 설정</center>

갭 분할(Gap Refinement) 옵션은 간격이 좁은 부분에 격자를 자동으로 세분화하는 기능이다. 여러 개의 면이 매우 가까이 인접해 있는 경우나 얇은 판이나 날개가 있는 경우 편리하게 사용할 수 있다. 아래의 다섯 가지 입력이 있다.

* 갭 내부의 최소 셀수(Min. Cell Layers in a gap) : 좁은 간격에 들어갈 최소 격자 개수로 4 이상의 정수를 입력한다.
* 갭 분할 시작 레벨(Gap detection start level) : 여기서 주어진 레벨의 격자 크기보다 간격이 작을 경우 갭 분할이 적용된다.
* 최대 분할 레벨(Max. refinement level) : 이 값보다 더 큰 레벨은 적용하지 않는다.
* 방향(Direction) : 내부(inside)는 닫힌 면 내부에 있는 좁은 간격에 적용하는 옵션으로 얇은 판이나 날개의 두께 방향으로 표면에 격자를 세분화한다. 외부(outside)는 다른 면과 면 사이의 간격에 적용된다. 내/외부(mixed)는 두 가지가 모두 적용된다.
* 같은 면에서의 갭도 포함(Include surface's own gap) : 하나의 닫힌 면들 사이에 존재하는 좁은 간격에도 적용할 것인지를 선택한다. 아래 그림에서 왼쪽이 포함하지 않는 경우이고 오른쪽이 포함하는 경우이다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_volumeRefinement-1.png" width="400" height="400"><br>갭 분할 설정</center>




