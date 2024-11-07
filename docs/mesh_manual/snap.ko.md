# 형상구현(Snap)

형상구현은 기하학적 표면(geometry)에 격자(mesh)를 정확하게 맞추는 과정으로, 면 근처의 점들을 면의 표면으로 이동해서 형상을 구현한다. 이 과정에서 격자의 품질을 유지하기 위한 스무딩(smoothing), 완화(relaxation) 등의 조정이 진행된다.

설정 항목은 다음과 같다.

## 반복회수(Iteration Count)

반복 계산 회수에 대한 설정이다.

### 표면 스무딩(Smoothing for Surface)

표면 격자에 대한 스무딩 회수를 지정한다.

스무딩은 격자의 품질을 개선하기 위한 후처리 단계로, 불규칙한 셀이나 표면을 더 부드럽게 만드는 작업이다. 스무딩을 통해 격자의 품질이 좋아질 수 있지만 회수를 너무 많이 하면 격자의 왜곡 혹은 형상이 변형이 생길 수도 있다. 아래 그림에 표면 스무딩의 값이 0일때와 3일 때의 격자를 보여주고 있다. 0일 때는 세분화된 모양 그대로이지만 3일 때는 달라진 것을 볼 수 있다. 이 격자도 계산에 문제가 있는 것은 아니지만 경계층 격자를 넣을 때 영향을 미친다. 

_snappyHexMeshDict_ 의 _snapControls.nSmoothPatch_ 에 해당한다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_snap_smoothing.png" width="800" height="800"><br>표면 스무딩의 영향</center>

### 내부의 스무딩(Smoothing for Internal)

분할된 면의 비직교성을 줄이기 위해 공간격자의 스무딩을 수행하는 회수이다. 내부의 면을 비평면으로 만든다.

_snappyHexMeshDict_ 의 _snapControls.nSmoothInternal_ 에 해당한다.
 
### 격자 이동 완화(Mesh Displacement Relaxation)

완화(relaxation)은 격자 생성 중의 조정 단계로, 셀의 위치와 크기 변화를 제어하여 셀들이 목표 형태에 점진적으로 적응하도록 하는 방법이다. 격자 이동의 완화에 대한 반복계산 회수이다. 

_snappyHexMeshDict_ 의 _snapControls.nSolveIter_ 에 해당한다.
 
### 스내핑 완화(Snapping Relaxation)

스내핑 중에 수행되는 완화(relaxation) 회수이다.

_snappyHexMeshDict_ 의 _snapControls.nRelaxIter_ 에 해당한다.
 
## 특징 스내핑(Feature Snapping)

특징선(feature line) 스내핑에 관련된 설정이다.

### 스내핑 완화(Snapping Relaxation)

특징선에 스내핑 할 때 수행되는 완화(relaxation) 회수이다. 

_snappyHexMeshDict_ 의 _snapControls.nFeatureSnapIter_ 에 해당한다.

### 특징선 스냅 방법(Feature Snap Type)

외재적방법(explicit)와 내재적방법(implicit)이 있다. 외재적방법은 stl 파일을 읽을 때 자동으로 만들어지는 특징선(obj 파일)를 이용하는 것이고, 내재적방법은 특징각도(feature angle)을 이용해서 직접 찾는 방법이다. 
 
### 다중 표면 특징 스내핑(Multi-Surface Feature Snap)

여러 면들 사이의 특징선을 찾기 위한 옵션이다. 다중영역(multi-region) 격자를 만들 대 유용하다. 특징선 스냅 방법이 외재적방법일 때만 적용된다. 

_snappyHexMeshDict_ 의 _snapControls.multiRegionFeatureSnap_ 에 해당한다.

## 허용 오차(Tolerance)

면으로 이동시킬 점을 찾을 때 사용되는 값으로 로컬 셀 엣지의 길이에 곱해진다. 값이 너무 작으면 적절한 점을 찾을 수 없고, 너무 크면 여러 개의 점들이 선택되어 형상을 제대로 구현하지 못하게 된다. 아래 그림의 왼쪽이 너무 작은 허용 오차 때문에 형상을 제대로 구현하지 못한 예이다.

_snappyHexMeshDict_ 의 _snapControls.tolerance_ 에 해당한다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mehs_snap_tolerance.png" width="800" height="800"><br>허용 오차의 영향</center>

## 오목한 각도(Concave Angle)

오목한 각을 가진 기하학적 부분을 처리할 때, 격자가 해당 형상을 얼마나 정확하게 따를지를 결정하는 파라미터이다. 이 각이 작으면 오목한 부분을 더 정확하게 처리할 수 있지만 격자수가 많아진다. 반대로 이 값이 크면 기하학적 형상이 덜 정확히 반영될 수 있다. 

_snappyHexMeshDict_ 의 _snapControls.concaveAngle_ 에 해당한다.

## 최소 면적비(Min. Area Ratio)

격자 품질 향상을 위해 격자 셀의 면적 비율을 제한하는 파라미터이다. 이 값보다 면적 비율이 작아지면 분할하지 않는다. 값이 작으면 복잡한 형상을 잘 구현할 수 있지만 품질이 떨어질 수 있다. 값이 크면 격자의 품질은 좋아지지만 기하학적 왜곡이 생길 수 있다.

_snappyHexMeshDict_ 의 snapControls.minAreaRatio_ 에 해당한다.




