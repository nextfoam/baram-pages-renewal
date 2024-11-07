# 주요기능

## BaramFlow

### 지원하는 격자 형식
* OpenFOAM PolyMesh
* Fluent case (.cas)
* Fluent mesh (.msh)
* Gmsh (.msh)
* I-deas Univesl (.unv)
* StarCCM+ (.ccm) : only for linux


### 격자 변형
* 확대/축소
* 이동
* 회전

### 물리 모델

* **난류 모델**
    * Laminar
    * Spalart-Allmaras
    * k-epsilon : Standard, RNG, Realizable
    * k-omega : SST
    * LES
    * DES

* **열전달**
    * 자연대류, 강제대류
    * 고체의 열전도
    * 복합열전달    

* **압축성유동**
    * 압력기반 솔버 : 비압축성에서 아음속유동까지
    * 밀도기반 솔버 : 아음속에서 초음속유동까지
        
* **다상유동 모델**
    * Volume of Fluid for multiple phases
    * Caviation model : Schnerr-Sauer, Kunz, Merkle

* **화학종 혼합**
    * 부력과 확산을 고려한 다양한 물질의 혼합

* **사용자 정의 스칼라**
    * 임의의 스칼라에 대한 수송방정식 계산 가능
    * 유동과 함깨 계산하거나 유동장은 고정된 상태에서 스칼라만 계산 가능

* **비뉴턴유체 모델**
    * Bird-Carreau
    * Cross power law
    * Hershel-Bulkley
    * Power law


### 셀존 조건
* 셀존 모델
    * 다중기준좌표계 모델(Multiple Reference Frame, MRF)
    * 다공성 영역 모델(Porous Zone : Darcy-Forchheimer, Power law)
    * 미끄럼격자(Sliding Mesh)
    * Actuator Disk
* 생성항(Source Terms)
* 일정한 값으로 고정(Fixed Values)


### 모니터링 / 데이터 추출
* 잔차(Residual)
* 유체력 계수
* 점에서의 값
* 면에서의 값
* 체적에서의 값

### 일괄 계산
* 사용자가 매개변수를 선언
* csv, xlxs 파일 형식을 이용하여 계산 조건을 입력

### 병렬 연산
* MPI 병렬연산
* SMP와 클러스터 장비 지원

## BaramMesh

### 형상
* STL 파일을 이용한 형상 정의
* 면의 개수에 제한 없음
* 닫힌 면을 자동으로 인식하여 볼륨 생성
* 특징 각도(feature angle)을 이용해서 면을 자동 분할
* 육면체, 구, 실린더 기본 형상 제작 기능
* 완벽하게 서로 연결되지 않은 형상 사용 가능(non-watertight surfaces)

### 격자
* 내부유동/외부유동 격자 생성
* 셀존 생성
* **일치(conformal)** 및 **불일치(non-conformal)** 격자 인터페이스 지원
* 복합열전달 계산을 위한 **다중영역(Multi-Region)** 격자 생성 기능
* 면(surface), 특징선(feature edge), 체적(volume), 좁은 간격(gap)에서의 격자 분할 기능
* 경계층 격자 생성 기능
* **병렬연산**을 통한 빠른 격자 생성 
* 2차원 / 축대칭 격자로 내보내기 기능


