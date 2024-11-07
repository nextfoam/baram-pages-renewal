# Features

## BaramFlow

### Supported Mesh Types
* OpenFOAM PolyMesh
* Fluent case (.cas)
* Fluent mesh (.msh)
* Gmsh (.msh)
* I-deas Univesl (.unv)
* StarCCM+ (.ccm) : only for linux


### Mesh manipulation
* Scale
* Translate
* Rotate

### Physical Models

* **Turbulent Models**
    * Laminar
    * Spalart-Allmaras
    * k-epsilon : Standard, RNG, Realizable
    * k-omega : SST
    * LES
    * DES

* **Heat Transfer**
    * Natural / Force convection
    * Solid conduction
    * Conjugated heat transfer    

* **Compressible flow**
    * Pressure based solver : to subsonic flow
    * Density based solver : subsonic to supersonic flow
        
* **Multiphase Models**
    * Volume of Fluid for multiple phases
    * Caviation model : Schnerr-Sauer, Kunz, Merkle

* **Chemical Species Transport**
    * Include buoyancy and diffusion

* **User Defined Scalars**
    * Arbitrary scalar transport equations can be included
    * Scalars can be simulated with or without flow calculation

* **Non-Newtonian Models**
    * Bird-Carreau
    * Cross power law
    * Hershel-Bulkley
    * Power law


### Cell Zone Configuration
* Zone Types
    * Multiple Reference Frame (MRF)
    * Porous Zone : Darcy-Forchheimer, Power law
    * Sliding Mesh
    * Actuator Disk
* Source Terms
* Fixed Values


### Monitoring / Report values
* Residual Values
* Force Coefficients
* Point Values
* Surface Values
* Volume Values

### Batch run
* Define user parameters
* csv and xlxs format can be used to define conditions

### Parallel Processing
* MPI Parallel Processing
* Support SMP and Cluster

## BaramMesh

### Geometry
* Import STL files
* No limit on the number of input surfaces
* Automatic detection of closed surface composing a volume
* Automatic surface split based on feature angle
* Support native simple geometry ( Hex, Sphere, Cylinder )
* can work with dirty surfaces, i.e. non-watertight surfaces

### Mesh generation
* Generate meshes for external flow and internal flow
* Create Cell Zones
* Support both **conformal** and **non-conformal** interfaces
* Support **Multi-Region** Mesh for OpenFOAM Multi-Region cases
* Mesh refinement based on surfaces, feature edges, volumes and gaps
* Build boundary layers
* scales well when meshing in **parallel**
* Export 3D mesh as 2D or axi-symmetric mesh


