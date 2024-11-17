# Features

## Simulation Capabilities

**Turbulence**

* Spalart-Allmaras
* SST $k$-$\omega$, $k$-$\epsilon$(standard, RNG, realizable)
* LES, DES

**Heat Transfer**

* Natural/Forced Convection
* Conduction
* Conjugated Heat Transfer

**Compressible Flow**

* Pressure based solver : incompressible to subsonic flow
* Density based solver : subsonic to supersonic flow

**Multiphase**

* Volume of Fluid for multiple phases
* Cavitation model : Schnerr-Sauer, Kunz, Merkle

**Species Transport**

* Include buoyancy and diffusion

**User Defined Scalar**

* Arbitrary scalar transport equations can be included
* Scalars can be simulated with or without flow calculation

**Non-Newtonian Models**

* Bird-Carreau, Cross power law, Hershel-Bulkley, Power law

**Rotating Machinery**

* Multiple Reference Frame(MRF)
* Sliding Mesh
* Actuator Disk
* Fan boundary condition

**Porous Media**

* Darcy-Forchheimer, Power-law model
* Porous jump boundary condition

**Batch Run**

* Define user parameters
* csv and xlsx format can be used to define conditions

**Monitoring / Report values**

* Residual Values
* Force and coefficients
* Point / Surface / Volume Values

## Mesh Capabilities

**Available Mesh Types**

* OpenFOAM PolyMesh
* Fluent (.msh/cas)
* Gmsh (.msh)
* I-deas Universal (.unv)
* StarCCM+ (.ccm) : only for linux

**Geometry**

* Import STL files
* Automatic detection of closed surface composing a volume
* Automatic surface split based on feature angle
* Support native simple geometry ( Hex, Sphere, Cylinder )
* can work with dirty surfaces, i.e. non-watertight surfaces

**Mesh generation**

* Generate meshes for external flow and internal flow
* Create Cell Zones
* Support both **conformal** and **non-conformal** interfaces
* Support **Multi-Region** Mesh for OpenFOAM Multi-Region cases
* Mesh refinement based on surfaces, feature edges, volumes and gaps
* Build boundary layers
* scales well when meshing in **parallel**
* Export 3D mesh as 2D or axi-symmetric mesh

**Mesh manipulation**

* Scale, Translate, Rotate

## Road Map

**Planned to next version**

* Create fields, Built-in post-processor, Create ROM using POD

**Planened to near future**

* Radiation, Shell conduction, Discrete phase model, Heat  exchanger model

**If we secure a fund**

* Chemical reaction, Eulerian multi-phase, Overset mesh, Finite Area Method, Optimization, Electro-magnetics, FSI...




