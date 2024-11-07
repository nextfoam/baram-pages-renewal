# 개요

## BARAM 이란

BARAM-v24는 압축성 유동, 비압축성 유동, 다상유동, 열전달 해석, 화학종 혼합 계산을 위한 전산유체역학 프로그램 패키지이며, 공개 소스 CFD 도구 상자인 OpenFOAM 기반으로 개발되었다.

BARAM-v24에서 사용하는 OpenFOAM은 NextFOAM-v24이다. NextFOAM-v24는 ESI의 OpenFOAM-v2406 기반으로 (주)넥스트폼에서 개발한 포크로, 안정성, 수렴성, 정확성을 향상시켰으며 새로운 기능을 위한 다양한 라이브러리, 유틸리티, 솔버를 포함한다. BARAM-v24를 설치하면 NextFOAM-v24의 바이너리가 설치된다. NextFOAM-v24의 소스코드는 github에서 다운 받을 수 있다. [https://github.com/nextfoam/nextfoam-cfd](https://github.com/nextfoam/nextfoam-cfd) 

BARAM-v24는 격자 생성 모듈인 BaramMesh와 해석 모듈인 BaramFlow로 구성된다.

격자 생성 모듈 BaramMesh는 OpenFOAM의 유틸리티인 blockMesh와 snappyHexMesh를 사용하여 격자를 생성하는 모듈이다. 형상은 STL 파일을 가져올 수 있고 육면체, 구, 실린더 형상을 생성할 수 있다. 경계층 격자를 포함한 octree 방식의 3차원 격자를 생성할 수 있으며 영역, 셀존, 인터페이스 등을 만들 수 있다. 만들어진 3차원 격자를 이용해 2차원 혹은 축대칭 격자로 내보낼 수 있다.

해석 모듈 BaramFlow는 전산유체역학 해석을 위한 모듈로 비압축성유동, 압축성유동, 열전달, 다상유동, 화학종 혼합을 해석할 수 있다.

BARAM-v24는 다양한 프로그램을 사용하여 개발되었으며 모두 공개소스 프로그램만 사용되었다. 그래픽 사용자 환경은 python, QT, VTK, matplotlib 등을 이용하여 개발 되었으며, 공개소스 프로그램인 ParaView를 이용하여 후처리 작업을 할 수 있다.

(주)넥스트폼이 개발하여 GNU GPL 라이선스로 공개하였다.

BARAM-v24에서 사용하는 솔버는 다음과 같다. 

* buoyantSimpleNFoam : 정상상태 아음속 유동, 열전달, 화학종 혼합 해석
* buoyantPimpleNFoam : 비정상상태 아음속 유동, 열전달, 화학종 혼합 해석
* chtMultiRegionSimpleNFoam : 정상상태 복합열전달 해석
* chtMultiRegionPimpleNFoam : 비정상상태 복합열전달 해석
* interFoam : 2상 유동의 자유수면 해석
* interPhaseChangeFoam : 캐비테이션 해석
* multiphaseInterFoam : 3상 이상의 물질에 대한 자유수면 해석
* TSLAeroFoam : 밀도기반 정상상태 압축성 유동 해석

  
## 주요 기능

* 난류모델
    + 비점성(Euler)
    + 층류(Laminar)
    + Standard $k-\epsilon$, RNG $k-\epsilon$, Realizable $k-\epsilon$ with standard & two-layer wall function
    + SST $k-\omega$
    + Spalart-Allmara 
    + LES/DES
  
* 열전달
    + 강제대류
    + 자연대류
    + 복합열전달

* item 다상유동
    + VOF를 이용한 자유수면
    + 캐비테이션 
    
* 셀존 모델
    +  다중기준프레임(MRF, Multiple Reference Frame)
    +  미끄럼 격자(Sliding Mesh)
    +  다공성 모델(Porous Media) : Darcy-Forchheimer, power law
    +  일정 속도(Fixed velocity)
    +  스칼라 소스(Scalar source)
    +  스칼라 값 고정(Scalar fixed value)


* 파라미터를 이용한 일괄 계산(batch run)
  
* 사용자 정의 스칼라 계산
  
* 격자 관련 기능
    + snappyHexMesh, blockMesh 유틸리티를 이용한 격자 생성
    + 2차원, 축대칭 격자로 변환
    + 격자 파일 변환 - Fluent, Star-CCM+, IdeasUnv, gmsh 파일 변환
    + 격자의 정보, 계산영역의 범위 표시
    + 격자의 이동, 회전, 축소, 확대

* 후처리
    + 모니터링 - 힘, 점/면/체적에서의 값 
    + 데이터 추출 - 힘, 점/면/체적에서의 값 
    + ParaView를 이용한 후처리

## 프로젝트 폴더 구조

BaramFlow는 프로젝트를 폴더 단위로 관리한다. 프로젝트 폴더에는 case라는 폴더와 baram.cfg, baram.log, configuration, configuration.h5 등의 파일이 있다.

case 폴더는 openfoam 계산 폴더이다. openfoam에서 사용하는 0, constant, system, postProcessing, [time] 등의 폴더가 있으며 baram.foam, stderr.log, stdout.log 파일이 있다. baram.foam은 후처리를 위한 paraview 실행을 위한 파일이며 stdout.log는 솔버를 포함한 각종 실행파일 실행시 생성되는 로그 파일이며, stderr.log는 실행시 나타난 에러 메세지이다.

그래픽 인터페이스의 각종 설정은 HDF5 파일 형식으로 configuration.h5라는 파일에 있다. HDF5는 Hierarchical Data Format version 5의 약어로, 계층적 데이터 형식으로 대량의 데이터를 저장하고 관리하기 위한 파일 형식 및 라이브러리이다. [https://www.hdfgroup.org/solutions/hdf5](https://www.hdfgroup.org/solutions/hdf5) 



 


 
  


