# Download

## Download BARAM for Windows and macOS
Binary installation package for 64-bit windows and disk image for macOS with Apple Silicon are here for convenience.
Download them from following links.

[**Download BARAM v24.4.0 Installer for 64-bit Windows ›**](https://d3c6e16xufx1gb.cloudfront.net/BARAM-24.4.0-setup.exe){: .md-button .md-button--primary}

**NOTE: For macOS, [*open-mpi*](https://formulae.brew.sh/formula/open-mpi) Homebrew Formula should be installed in advance.**

[**Download BARAM v24.4.0 Disk Image(.dmg) for macOS with Apple Silicon ›**](https://d3c6e16xufx1gb.cloudfront.net/BARAM-24.4.0.dmg){: .md-button .md-button--primary}

### Supported Platforms
* Windows 10 or newer
* macOS 10.14 or newer (Apple Silicon only)
* Ubuntu 20.04 or newer
* CentOS 8.2 or alternatives ( Rocky Linux, AlmaLinux, ... )
* OpenSUSE Leap 15.4
* Linux Mint 21 "Vanessa"

### BARAM requires following installed software:
* Python *3.9.x*
* [MS-MPI](https://docs.microsoft.com/en-us/message-passing-interface/microsoft-mpi) 10.0 or newer ( Windows Only )
* OpenMPI 4.1 or newer ( Linux, macOS )
* GNU C Compiler or any other C Compiler ( Linux, macOS )

## Installation with NextFOAM build (Linux)

### Supported Platforms
<!--* Windows 10 or newer
* macOS 10.14 or newer (Apple Silicon only)-->
* Ubuntu 20.04 or newer
<!--* CentOS 8.2 or alternatives ( Rocky Linux, AlmaLinux, ... )-->
<!--* OpenSUSE Leap 15.4-->
<!--* Linux Mint 21 "Vanessa"-->

### BARAM requires following installed software:

* Python *3.9.x*
<!--* [MS-MPI](https://docs.microsoft.com/en-us/message-passing-interface/microsoft-mpi) 10.0 or newer ( Windows Only )-->
* OpenMPI 4.1 or newer ( Linux, macOS )
* GNU C Compiler or any other C Compiler ( Linux, macOS )


### Install BARAM, NextFOAM, OpenMPI

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

- Install BARAM at `/opt/baram` with required packages. **pip3** should be used instead of default `pip` command
    
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

- Build latest `NextFOAM-CFD` solver according to the instruction at [*https://github.com/nextfoam/nextfoam-cfd*](https://github.com/nextfoam/nextfoam-cfd)

- Copy compiled `NextFOAM-cfd` solvers and `Third-Party` libraries to `/opt/baram/solvers/openfoam`
    
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

- Compile Daemonizer and resource
```
cd /opt/baram
sudo gcc -o solvers/openfoam/bin/baramd misc/baramd.c
sudo python3 convertUi.py
```

- Edit `baramMesh.sh`

    
```
sudo vi /opt/baram/baramMesh.sh
```
Comment out `source` and change `python` to `python3`
    
```
#source venv/bin/activate
python3 -m baramMesh.main
```

- Edit `baramFlow.sh`
```
sudo vi /opt/baram/baramFlow.sh
```

Comment out `source` and change `python` to `python3`
    
```
#source venv/bin/activate
python3 -m baramFlow.main
```


### Install `paraview`

- Install paraview using `apt-get` command. You can install `paraview` from the [paraview official download](https://www.paraview.org/download/) page.

```
sudo apt-get install paraview
```

### Create desktop shortcuts
- Create `Desktop` directory under `/etc/skel` for all users

```
sudo mkdir -p /etc/skel/Desktop
```

- Create `baramMesh.desktop` under `/etc/skel/Desktop`

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

 - Create `baramFlow.desktop` under `/etc/skel/Desktop`


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
    
Change permission of shortcuts

```
sudo chmod +x /etc/skel/Desktop/*
```

**(NOTE)** You should upload `baramMesh.png` and `baramFlow.png` to `/opt/baram` directory.


## Installation with Python virtual environment

### Supported Platforms
* Windows 10 or newer
* macOS 10.14 or newer (Apple Silicon only)
* Ubuntu 20.04 or newer
* CentOS 8.2 or alternatives ( Rocky Linux, AlmaLinux, ... )
* OpenSUSE Leap 15.4
* Linux Mint 21 "Vanessa"

### BARAM requires following installed software:

* Python *3.9.x*
* [MS-MPI](https://docs.microsoft.com/en-us/message-passing-interface/microsoft-mpi) 10.0 or newer ( Windows Only )
* OpenMPI 4.1 or newer ( Linux, macOS )
* GNU C Compiler or any other C Compiler ( Linux, macOS )


### Clone the source code
```commandline
git clone https://github.com/nextfoam/baram.git
cd baram
```

### Setup Python virtual environment

Run following command in the top directory of downloaded source code.
Please don't forget that Python **3.9.x** is required.
You can check it with the command of `python3 -V`.

```commandline
python3.9 -m venv venv
```

### Enter into virtual environment
Run following command in the top directory of downloaded source code

#### On Windows
```commandline
.\venv\Scripts\activate.bat
```

#### On Linux or macOS
```commandline
source ./venv/bin/activate
```

### Upgrade pip version
```commandline
pip install --upgrade pip
```

### Install Python packages
Run following command in the top directory of downloaded source code
```commandline
pip install -r requirements.txt
```

### Install QT Advanced Docking System package
Run following command in the top directory of downloaded source code

#### On Windows
```commandline
pip install https://d3c6e16xufx1gb.cloudfront.net/wheels/PySide6_QtAds-4.2.1.2.dev0-cp38-abi3-win_amd64.whl
```

#### On Linux
```commandline
pip install https://d3c6e16xufx1gb.cloudfront.net/wheels/PySide6_QtAds-4.2.1.2.dev0-cp38-abi3-linux_x86_64.whl
```

### Copy Solver Executables
Download and uncompress solver executables into the top directory of downloaded source code.
The compressed files have _**solvers**_ folder in it.
Put _**solvers**_ folder into the top directory.
The final directory structure may look like following.
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


You can download the file on command line with cURL or wget command like following.
```commandline
wget https://d3c6e16xufx1gb.cloudfront.net/solvers_linux_24.4.0_20240923.tar.xz
```

```commandline
curl -L https://d3c6e16xufx1gb.cloudfront.net/solvers_linux_24.4.0_20240923.tar.xz -o solvers_linux_24.4.0_20240923.tar.xz
```

#### macOS (Apple Silicon only)
Not yet prepared due to the strict code signing policy from Apple


### Compile Daemonizer ( only for Linux and macOS )
"solvers" directory was created when the compressed file was uncompressed.
```commandline
gcc -o solvers/openfoam/bin/baramd misc/baramd.c
```

### Compile Resource Files
```commandline
python convertUi.py
```

### Run BaramFlow
```commandline
python -m baramFlow.main
```

### Run BaramMesh
```commandline
python -m baramMesh.main
```

### Convenient Scripts that do not require manual *venv* activation

```commandline
baramFlow.sh
```
```commandline
baramMesh.sh
```

## Azure market place
You can use BARAM Cloud service with Azure makret place!!
Visit below link and use BARAM cloud service!!
[**Azure market place link**](https://azuremarketplace.microsoft.com/en-us/marketplace/apps?search=baram&page=1)

## Paraview

*BaramFlow* has a menu that can launch [*ParaView*](https://www.paraview.org/) for post-process.
Users should download and install Paraview for post-process. And users can download it in [*ParaView Download Site*](https://www.paraview.org/download/)
If *ParaView* is installed in the system, this menu launches *ParaView* in the case foler.