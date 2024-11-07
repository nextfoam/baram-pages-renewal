# What is BARAM

BARAM-v24 is a computational fluid dynamics program package for compressible flows, incompressible flows, multiphase flows, heat transfer, and chemical species mixing. It is based on OpenFOAM, an open source CFD toolbox.

The version of OpenFOAM used by BARAM-v24 is NextFOAM-v24. NextFOAM-v24 is a fork of ESI's OpenFOAM-v2406 and includes various libraries, utilities, and solvers for improved stability, convergence, accuracy, and new features. Installing BARAM-v24 will install the binaries of NextFOAM-v24. The source code for NextFOAM-v24 can be downloaded from github. [https://github.com/nextfoam/nextfoam-cfd](https://github.com/nextfoam/nextfoam-cfd) 

BARAM-v24 consists of a mesh generation module, BaramMesh, and an analysis module, BaramFlow.

The mesh generation module BaramMesh is a module that uses OpenFOAM's utilities blockMesh and snappyHexMesh to generate mesh. STL files can be imported as geometry. It can create hexahedron, sphere, and cylinder geometries. It can generate octree-style three-dimensional mesh, including boundary layer mesh, and can create regions, cell zones, interfaces, and more. From generated 3D mesh, it can export 2D or axi-symmetric mesh.

Simulation module BaramFlow is a module for computational fluid dynamics analysis that can analyze incompressible flows, compressible flows, heat transfer, multiphase flows, and chemical species mixing.

BARAM-v24 was developed using a variety of programs, all of which are open source. The graphical user interface is developed using python, QT, VTK, matplotlib, etc. and the open source program ParaView can be used for post-processing.


It is developed by NEXTform Inc. and released under the GNU GPL license.

The solvers used in BARAM-v24 are as follows. 

* buoyantSimpleNFoam : Steady subsonic flow, heat transfer, species mixing
* buoyantPimpleNFoam : Transient subsonic flow, heat transfer, species mixing
* chtMultiRegionSimpleNFoam : Steady conjugated heat transfer
* chtMultiRegionPimpleNFoam : Transient conjugated heat transfer
* interFoam : two phases Volume of Fluid
* interPhaseChangeFoam : Cavitation
* multiphaseInterFoam : Volume of fluid for more than 2 phases
* TSLAeroFoam : Density based coupled solver for compressible flow
  
# Features

* Turbulence Model
    + Euler
    + Laminar
    + Standard $k-\epsilon$, RNG $k-\epsilon$, Realizable $k-\epsilon$ with standard & two-layer wall function
    + SST $k-\omega$
    + Spalart-Allmara 
    + LES/DES
  
* Heat Transfer
    + Forced Convection
    + Natural Convection
    + Conjugated Heat Transfer

* item Multiphase
    + Free surface by VOF
    + Cavitation 
    
* Cell Zone Model
    +  MRF, Multiple Reference Frame
    +  Sliding Mesh
    +  Porous Media : Darcy-Forchheimer, power law
    +  Fixed velocity
    +  Scalar source
    +  Scalar fixed value

* Batch run
  
* User Defined Scalar
  
* Mesh
    + Mesh generation using snappyHexMesh and blockMesh
    + Export as 2-dimentsional, axi-symmetric mesh
    + Convert mesh - Fluent, Star-CCM+, IdeasUnv, gmsh format
    + Display mesh infomation, domain range
    + Translate, Rotate, Scale

* Post-processing
    + Monitoring - force, point/surface/volume value
    + Data extract - force, point/surface/volume value
    + ParaView support

# Project Folder Structure

BaramFlow organizes projects into folders.

The project folder contains a folder named case and files such as baram.cfg, baram.log, configuration, configuration.h5, and so on. 

The case folder is the openfoam simulation folder. There are folders as 0, constant, system, postProcessing, and [time] that are used by openfoam and contain the files baram.foam, stderr.log, and stdout.log. baram.foam is a file used in paraview for post-processing, stdout.log is a log file generated when executing various executables including the solver, and stderr.log is an error message displayed during execution.

The graphical interface is configured in a file named configuration.h5 in the HDF5 file format. HDF5 stands for Hierarchical Data Format version 5, which is a file format and library for storing and managing large amounts of data in a hierarchical data format.
[https://www.hdfgroup.org/solutions/hdf5](https://www.hdfgroup.org/solutions/hdf5) 



 


 
  


