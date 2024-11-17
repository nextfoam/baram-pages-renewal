# 주요기능

## 해석 기능

**난류 모델**

* Spalart-Allmaras
* SST $k$-$\omega$, $k$-$\epsilon$(Standard, RNG, Realizable)
* LES, DES

**열전달**

* 자연대류, 강제대류
* 고체의 열전도
* 복합열전달  

**압축성유동**

* 압력기반 솔버 : 비압축성에서 아음속유동까지
* 밀도기반 솔버 : 아음속에서 초음속유동까지

**다상유동 모델**

* Volume of Fluid
* 캐비테이션 모델 : Schnerr-Sauer, Kunz, Merkle

**화학종 혼합**

* 부력과 확산을 고려한 다양한 물질의 혼합

**사용자 정의 스칼라**

* 임의의 스칼라에 대한 수송방정식 계산 가능
* 유동과 함깨 계산하거나 유동장은 고정된 상태에서 스칼라만 계산 가능

**비뉴턴유체 모델**

* Bird-Carreau, Cross power law, Hershel-Bulkley, Power law

**회전 기계**

* 다중기준좌표계 모델(Multiple Reference Frame, MRF)
* 미끄럼격자(Sliding Mesh)
* Actuator Disk
* Fan 경계조건

**다공성 영역 모델**

* Darcy-Forchheimer, Power law
* Porous Jump 경계조건

**일괄 계산**

* 사용자가 매개변수를 선언
* csv, xlsx 파일 형식을 이용하여 계산 조건을 입력

**모니터링 / 데이터 추출**

* 잔차(Residual)
* 힘, 유체력 계수
* 점, 면, 체적에서의 값

## 격자 기능

**지원하는 격자 형식**

* OpenFOAM PolyMesh
* Fluent (.msh, .cas)
* Gmsh (.msh)
* I-deas Universal (.unv)
* StarCCM+ (.ccm) : only for linux

**형상**

* STL 파일을 이용한 형상 정의
* 닫힌 면을 자동으로 인식하여 볼륨 생성
* 특징 각도(feature angle)을 이용해서 면을 자동 분할
* 육면체, 구, 실린더 기본 형상 제작 기능
* 완벽하게 서로 연결되지 않은 형상 사용 가능(non-watertight surfaces)

**격자 생성**

* 내부유동/외부유동 격자 생성
* 셀존 생성
* **일치(conformal)** 및 **불일치(non-conformal)** 격자 인터페이스 지원
* 복합열전달 계산을 위한 **다중영역(Multi-Region)** 격자 생성 기능
* 면(surface), 특징선(feature edge), 체적(volume), 좁은 간격(gap)에서의 격자 분할 기능
* 경계층 격자 생성 기능
* **병렬연산** 을 통한 빠른 격자 생성 
* 2차원 / 축대칭 격자로 내보내기 기능

**격자 변형**

* 확대/축소, 이동, 회전

## 로드맵

**다음 버전 예정**

* 필드 생성, 자체 그래픽 후처리 기능, 적합직교분해를 이용한 차수축소모델 제작 기능

**다음 버전 이후**

* 복사열전달, Shell conduction, Discrete phase model, 열교환기 모델

**펀드가 있으면...**

* 화학반응, 오일러리안 다상유동 모델, Overset mesh, Finite Area Method, 최적화, 전자기장 해석, FSI...




