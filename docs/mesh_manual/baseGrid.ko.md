# 배경격자(Base Grid)}

배경격자에서는 OpenFOAM의 \textit{blockMesh} 유틸리티를 이용해서 정렬격자 형태의 배경 격자를 만들어 준다.

격자 범위(Grid Span)에 최소/최대 좌표가 표시되고 방향별 격자 개수(Number of Cells per Direction)에서 x,y,z 방향으로 격자의 개수를 지정할 수 있다. 개수에 따라 한개 셀의 x, y, z 방향의 길이가 격자 범위에 표시된다.

[1.형상(Geometry]에서 만든 Hex6를 이용해서 배경격자를 만들 것인지 선택하는 Hex6사용(Use Hex6) 옵션이 있다. 외부유동 해석을 위해 원방 격자를 만들 때는 이 옵션을 켜주는 것이 좋다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_baseGrid.png" width="800" height="800"><br>배경격자 설정</center>

생성(Generate) 버튼을 누르면 격자가 만들어지고 디스플레이 설정에 internalMesh가 생겨서 그래픽 창에서 확인할 수 있다. 격자 개수를 바꾸고 싶으면 재설정(Reset) 버튼을 누르고 수정한 후 다시 만든다.







