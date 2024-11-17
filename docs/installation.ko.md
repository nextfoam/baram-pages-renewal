# 다운로드

## Windows와 macOS에 다운로드
64 비트 윈도우용 설치 파일은 아래의 링크에서 다운받으세요.

[**Download BARAM v24.4.0 Installer for 64-bit Windows ›**](https://d3c6e16xufx1gb.cloudfront.net/BARAM-24.4.0-setup.exe){: .md-button .md-button--primary}
<!---{: .btn .btn-purple .text-center .fs-5 onclick="trackDownload('BARAM-24.4.0-setup.exe')"}-->

애플 실리콘의 macOS용 디스크 이미지는 아래의 링크에서 다운 받으세요.

**주의: macOS의 [*open-mpi*](https://formulae.brew.sh/formula/open-mpi) Homebrew Formula 가 사전에 설치되어 있어야 합니다.**

[**Download BARAM v24.4.0 Disk Image(.dmg) for macOS with Apple Silicon ›**](https://d3c6e16xufx1gb.cloudfront.net/BARAM-24.4.0.dmg){: .md-button .md-button--primary}
<!---{: .btn .btn-blue .text-center .fs-5onclick="trackDownload('BARAM-24.4.0.dmg')"}-->

### 지원되는 OS 
* Windows 10 or newer
* macOS 10.14 or newer (Apple Silicon only)
* Ubuntu 20.04 or newer
* CentOS 8.2 or alternatives ( Rocky Linux, AlmaLinux, ... )
* OpenSUSE Leap 15.4
* Linux Mint 21 "Vanessa"

### BARAM 설치를 위해 다음의 소프트웨어가 설치되어 있어야 합니다:
* Python *3.9.x*
* [MS-MPI](https://docs.microsoft.com/en-us/message-passing-interface/microsoft-mpi) 10.0 or newer ( Windows Only )
* OpenMPI 4.1 or newer ( Linux, macOS )
* GNU C Compiler or any other C Compiler ( Linux, macOS )

## 넥스트폼 솔버 빌드 및 설치 과정 (Linux)

### 지원되는 OS
<!--* Windows 10 or newer
* macOS 10.14 or newer (Apple Silicon only)-->
* Ubuntu 20.04 or newer
<!--* CentOS 8.2 or alternatives ( Rocky Linux, AlmaLinux, ... )-->
<!--* OpenSUSE Leap 15.4-->
<!--* Linux Mint 21 "Vanessa"-->

### BARAM 설치에 필요한 소프트웨어:

* Python *3.9.x*
<!--* [MS-MPI](https://docs.microsoft.com/en-us/message-passing-interface/microsoft-mpi) 10.0 or newer ( Windows Only )-->
* OpenMPI 4.1 or newer ( Linux, macOS )
* GNU C Compiler or any other C Compiler ( Linux, macOS )


### OpenMPI, NextFOAM, BARAM 설치

- OpenMPI는 `configure` 명령어에 `--prefix` 옵션으로 `/opt/openmpi-4.1.6` 경로에 설치됩니다.
- OpenMPI is installed in the `/opt/openmpi-4.1.6` with `--prefix` option at `configure` command

```
sudo apt-get -y update 
sudo apt-get -y install build-essential flex zlib1g-dev libgmp-dev libmpfr-dev
wget https://download.open-mpi.org/release/open-mpi/v4.1/openmpi-4.1.6.tar.gz 
tar zxf openmpi-4.1.6.tar.gz 
rm openmpi-4.1.6.tar.gz 
cd openmpi-4.1.6 
./configure --prefix=/opt/openmpi-4.1.6 
make -j 4 all 
sudo make install 
echo 'export PATH=$PATH:/opt/openmpi-4.1.6/bin' | sudo tee -a /etc/bash.bashrc
```

- 필요한 패키지와 함께 `/opt/baram`에 BARAM을 설치합니다. 기본 pip 명령 대신 **pip3** 를 사용합니다.

```
sudo apt install -y  qtbase5-dev
sudo apt-get install -y '^libxcb.*-dev' libx11-xcb-dev libglu1-mesa-dev libxrender-dev libxi-dev libxkbcommon-dev libxkbcommon-x11-dev texinfo
pip3 install --upgrade pip
cd /opt
sudo git clone https://github.com/nextfoam/baram.git
cd baram
sudo pip3 install --ignore-installed -r requirements.txt
sudo pip3 install https://d3c6e16xufx1gb.cloudfront.net/wheels/PySide6_QtAds-4.2.1.2.dev0-cp38-abi3-linux_x86_64.whl
```

- [*https://github.com/nextfoam/nextfoam-cfd*](https://github.com/nextfoam/nextfoam-cfd)에 있는 방법을 따라 최신 `NextFOAM-CFD` 솔버를 빌드합니다.

- 컴파일된 `NextFOAM-cfd` 솔버 및 `Third-Party` 라이브러리를 `/opt/baram/solvers/openfoam`에 복사합니다.

```
export BARAM_DIR="/opt/baram"
    
sudo mkdir -p $BARAM_DIR/solvers/openfoam
sudo cp -a $WM_PROJECT_DIR/platforms/linux64GccDPInt32Opt/bin $BARAM_DIR/solvers/openfoam/
sudo cp -a $WM_PROJECT_DIR/platforms/linux64GccDPInt32Opt/lib $BARAM_DIR/solvers/openfoam/
sudo cp -a $WM_PROJECT_DIR/etc $BARAM_DIR/solvers/openfoam/

sudo mkdir -p $BARAM_DIR/solvers/openfoam/tlib

sudo cp -a $WM_THIRD_PARTY_DIR/platforms/linux64GccDPInt32/lib/* $BARAM_DIR/solvers/openfoam/tlib/
sudo cp -a $WM_THIRD_PARTY_DIR/platforms/linux64GccDPInt32/lib/libscotch* $BARAM_DIR/solvers/openfoam/tlib/
sudo cp -a $WM_THIRD_PARTY_DIR/platforms/linux64GccDPInt32/lib/sys-openmpi/libptscotch* $BARAM_DIR/solvers/openfoam/tlib/
sudo cp -a $WM_THIRD_PARTY_DIR/platforms/linux64Gcc/fftw-3.3.10/lib/libfftw3* $BARAM_DIR/solvers/openfoam/tlib/
sudo cp -a $WM_THIRD_PARTY_DIR/platforms/linux64Gcc/kahip-3.15/lib/libkahip_static.a $BARAM_DIR/solvers/openfoam/tlib/
```

- Daemonizer 및 리소스 컴파일
```
cd /opt/baram
sudo gcc -o solvers/openfoam/bin/baramd misc/baramd.c
sudo python3 convertUi.py
```

- `baramMesh.sh` 수정

    
```
sudo vi /opt/baram/baramMesh.sh
```
`source`를 주석 처리하고 `python`을 `python3`으로 변경합니다.
    
```
#source venv/bin/activate
python3 -m baramMesh.main
```

- `baramFlow.sh` 수정
```
sudo vi /opt/baram/baramFlow.sh
```

`source`를 주석 처리하고 `python`을 `python3`으로 변경합니다.
    
```
#source venv/bin/activate
python3 -m baramFlow.main
```


### `paraview` 설치

- `apt-get` 명령어로 `paraview`를 설치합니다. [paraview 공식 홈페이지](https://www.paraview.org/download/)에서 `paraview`를 설치할 수도 있습니다.

```
sudo apt-get install paraview
```

### 바탕화면 바로가기 생성
- 모든 사용자에 대해 `/etc/skel`에 `Desktop`를 생성합니다.

```
sudo mkdir -p /etc/skel/Desktop
```

- `/etc/skel/Desktop` 경로 밑에 `baramMesh.desktop`을 생성합니다.

```
sudo vi /etc/skel/Desktop/baramMesh.desktop
```

```
[Desktop Entry]
Encoding=UTF-8
Name=baramMesh
Comment=baram Mesh
Icon=/opt/baram/baramMesh.png
Exec=bash -c '/opt/baram/baramMesh.sh'
Terminal=false
Type=Application
Categories=Science
```

- `/etc/skel/Desktop` 경로 밑에 `baramFlow.desktop`을 생성합니다.

```
sudo vi /etc/skel/Desktop/baramFlow.desktop
```
```
[Desktop Entry]
Encoding=UTF-8
Name=baramFlow
Comment=baram Flow
Icon=/opt/baram/baramFlow.png
Exec=bash -c '/opt/baram/baramFlow.sh'
Terminal=false
Type=Application
Categories=Science
```
    
- 바로가기 권한을 변경합니다.

```
sudo chmod +x /etc/skel/Desktop/*
```

**(NOTE)** `baramMesh.png`와 `baramFlow.png`를 `/opt/baram`에 업로드해야 합니다.

## Python 가상 환경으로 BARAM 설치하기

### 지원 OS 플랫폼
* Windows 10 or newer
* macOS 10.14 or newer (Apple Silicon only)
* Ubuntu 20.04 or newer
* CentOS 8.2 or alternatives ( Rocky Linux, AlmaLinux, ... )
* OpenSUSE Leap 15.4
* Linux Mint 21 "Vanessa"

### BARAM 설치에 필요한 소프트웨어:

* Python *3.9.x*
* [MS-MPI](https://docs.microsoft.com/en-us/message-passing-interface/microsoft-mpi) 10.0 or newer ( Windows Only )
* OpenMPI 4.1 or newer ( Linux, macOS )
* GNU C Compiler or any other C Compiler ( Linux, macOS )


### 소스코드 복제
```commandline
git clone https://github.com/nextfoam/baram.git
cd baram
```

### Python 가상 환경 설정

다운로드한 소스 코드의 최상위 디렉토리에서 다음 명령을 실행합니다.
Python **3.9** 가 필요하니 잊지 마세요.
`python3 -V` 명령으로 버전을 확인할 수 있습니다.

```commandline
python3.9 -m venv venv
```

### 가상 환경 활성화
다운로드한 소스 코드의 최상위 디렉토리에서 다음 명령을 실행합니다.

#### Windows
```commandline
.\venv\Scripts\activate.bat
```

#### Linux 또는 macOS
```commandline
source ./venv/bin/activate
```

### pip 버전 업그레이드
```commandline
pip install --upgrade pip
```

### Python 패키지 설치
다운로드한 소스 코드의 최상위 디렉토리에서 다음 명령을 실행합니다.
```commandline
pip install -r requirements.txt
```

### QT Advanced Docking System package 설치
다운로드한 소스 코드의 최상위 디렉토리에서 다음 명령을 실행합니다.

#### Windows
```commandline
pip install https://d3c6e16xufx1gb.cloudfront.net/wheels/PySide6_QtAds-4.2.1.2.dev0-cp38-abi3-win_amd64.whl
```

#### Linux
```commandline
pip install https://d3c6e16xufx1gb.cloudfront.net/wheels/PySide6_QtAds-4.2.1.2.dev0-cp38-abi3-linux_x86_64.whl
```

### 솔버 실행 파일 복사
다운로드한 솔버 실행 파일을 다운로드한 소스 코드의 최상위 디렉토리로 복사하고 압축을 해제합니다. 압축 파일에는 _**solvers**_ 폴더가 포함되어 있습니다. _**solvers**_ 폴더를 최상위 디렉토리에 배치하세요. 최종 디렉토리 구조는 다음과 같습니다.
```
($BARAM)
|
+-- requirements.txt
+-- ...
+-- solvers/
|   |
|   +-- openfoam/
|       |
|       +-- bin/
|       +-- etc/
|       +-- ...
+-- ...
```

#### Windows
[solvers_windows_24.4.0_20240923.zip](https://d3c6e16xufx1gb.cloudfront.net/solvers_windows_24.4.0_20240923.zip)

#### Linux
[solvers_linux_24.4.0_20240923.tar.xz](https://d3c6e16xufx1gb.cloudfront.net/solvers_linux_24.4.0_20240923.tar.xz)


cURL 또는 wget 명령을 사용하여 다음과 같이 파일을 다운로드 할 수 있습니다.
```commandline
wget https://d3c6e16xufx1gb.cloudfront.net/solvers_linux_24.4.0_20240923.tar.xz
```

```commandline
curl -L https://d3c6e16xufx1gb.cloudfront.net/solvers_linux_24.4.0_20240923.tar.xz -o solvers_linux_24.4.0_20240923.tar.xz
```

#### macOS (Apple Silicon 전용)
엄격한 코드 서명 정책으로 인해 아직 준비되지 않았습니다.


### Daemonizer 컴파일 ( Linux and macOS )
압축파일이 해제되면서 "solvers"디렉토리가 생성됩니다.
```commandline
gcc -o solvers/openfoam/bin/baramd misc/baramd.c
```

### 리소스 파일 컴파일
```commandline
python convertUi.py
```

### BaramFlow 실행
```commandline
python -m baramFlow.main
```

### BaramMesh 실행
```commandline
python -m baramMesh.main
```

### 수동 *venv* 활성화가 필요없는 편리한 스크립트

```commandline
baramFlow.sh
```
```commandline
baramMesh.sh
```

## Azure market place
Azure market place에서 Cloud 서비스를 이용하실 수 있습니다!!
아래 링크 접속을 통해 넥스트폼에서 제공하는 Cloud 서비스를 사용해보세요.
[**Azure Market place 링크**](https://azuremarketplace.microsoft.com/en-us/marketplace/apps?search=baram&page=1)

## Paraview

*BaramFlow* 후처리를 위해 [*ParaView*](https://www.paraview.org/)를 실행하는 메뉴가 있습니다.
사용자들은 후처리를 위해 Paraview를 다운로드 및 설치해야 합니다. 다운로드는 [*ParaView Download Site*](https://www.paraview.org/download/)에서 할 수 있습니다.
만약 Paraview가 이미 설치되어 있다면, 이 메뉴로 Paraview에서 case folder를 실행할 수 있습니다.