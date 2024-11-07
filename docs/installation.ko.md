# 설치

## Supported Platforms
* Windows 10 or newer
* macOS 10.14 or newer (Apple Silicon only)
* Ubuntu 20.04 or newer
* CentOS 8.2 or alternatives ( Rocky Linux, AlmaLinux, ... )
* OpenSUSE Leap 15.4
* Linux Mint 21 "Vanessa"

## Windows와 macOS에 설치
64 비트 윈도우용 설치 파일은 아래의 링크에서 다운받으세요.

[**Download BARAM v24.4.0 Installer for 64-bit Windows ›**](https://d3c6e16xufx1gb.cloudfront.net/BARAM-24.4.0-setup.exe){: .md-button .md-button--primary}
<!---{: .btn .btn-purple .text-center .fs-5 onclick="trackDownload('BARAM-24.4.0-setup.exe')"}-->

애플 실리콘의 macOS용 디스크 이미지는 아래의 링크에서 다운 받으세요.

**주의: macOS의 [*open-mpi*](https://formulae.brew.sh/formula/open-mpi) Homebrew Formula 가 사전에 설치되어 있어야 합니다.**

[**Download BARAM v24.4.0 Disk Image(.dmg) for macOS with Apple Silicon ›**](https://d3c6e16xufx1gb.cloudfront.net/BARAM-24.4.0.dmg){: .md-button .md-button--primary}
<!---{: .btn .btn-blue .text-center .fs-5onclick="trackDownload('BARAM-24.4.0.dmg')"}-->


### BARAM 설치를 위해 다음의 소프트웨어가 설치되어 있어야 합니다:
* Python *3.9.x*
* [MS-MPI](https://docs.microsoft.com/en-us/message-passing-interface/microsoft-mpi) 10.0 or newer ( Windows Only )
* OpenMPI 4.1 or newer ( Linux, macOS )
* GNU C Compiler or any other C Compiler ( Linux, macOS )

## 리눅스에서 소스코드를 이용한 설치 방법

[**Installation with NextFOAM build ›**](https://baramcfd.org/installation/2024/04/15/installationSourceCode-post/)

[**Installation with Python Virtual Environment ›**](https://baramcfd.org/installation/2024/04/15/installationPythonEnv-post/)

## Paraview [Optional]

*BaramFlow* has a menu that can launch [*ParaView*](https://www.paraview.org/) for convenience.
If *ParaView* is installed in the system, this menu launches *ParaView* in the case foler.
