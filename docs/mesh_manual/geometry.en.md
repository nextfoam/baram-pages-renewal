# Geometry

In BaramMesh geometry is used for the purpose of follows

* To represent ther geometry of an object
* Delimit specific space within the mesh - to create cell zones or specify mesh size level
* To define external computational domain

## Import

The geometry of complex objects uses CAD files(stl files). You can select a file with the Import button. When importing the file, you can use the feature angles of the faces to separate them.

When separating the faces, you need to check the normal vector of the surfaces because even if the triangular mesh in the STL file is in one plane, it will be divided into different faces if the normal vector is in the opposite direction.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_split.png" width="400" height="400"><br>Import stl file</center>

When the OK button is pressed with the 'Split Surface' option enabled, the window shown in the figure below appears:

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_split-1.png" width="800" height="800"><br>Split surface window</center>

You can see how the surfaces are divided based on feature anlge.

The Area Threshold will merge with the larger side if the ratio of the area of the divided side to the total area is less than the input value. At the bottom, the area ratio of each piece is displayed in color and number for reference. Depending on the characteristics of the stl file, it may not be possible to merge with a neighboring large area.

When an stl file is read, it is displayed in the Geometry window and the surfaces are shown below it. For files that consist of multiple SOLIDs(the term used by STL files to separate regions), each SOLID region is separated and displayed separately at a lower level.

## Add

Simple geometries can be created with Add button. They are mainly used for delimiting regions inside the grid, defining computational regions for external flows, etc.

Four basic geometries are provided: Hex, Cylinder, Sphere, and Hex6. Hex6 is a hexahedron with six separate faces and is used to define the computational domain for external flows. Hex6 is not treated as a shape in _snappyHexMesh_, so its use for representing the shape of an object is limited.

The four basic geometries are created from volumes and surfaces. In the geometry list, surfaces are shown as a sub-level of volumes. 

## Type

Volumes and surfaces have the following types. Right-click an item in the list to modify it.

* Volume : None, CellZone
* Surface : Boundary, None, Interface
    * Surface : Boundary, None, Interface

If type of surface is boundary, it becomes boundary surface of mesh.

If the surface type of the geometry you create for the cell zone is a boundary, it will separate the outside from the inside, so it will not recognize one of them as a computational domain and will not create a mesh. Therefore, you should set this to none when the cellzone does not need to be separated by a boundary, and to interface when the cellzone is moving, such as in a slidng mesh.

### Types of Interface

A surface with an interface type creates two boundaries at the same location. An additional boundary is created with \_slave appended to its face name.

If no interface type is given, the same mesh is created on both sides, and if it is a non-conformal mesh, the grid on the two sides can be different.

Non-conformal mesh should not be used in cases where OpenFOAM's _cyclic_ property should be used, such as porous jumps and fan boundary conditions.

If one side is moving and the other is not, such as a sliding mesh, you must use a non-conformal mesh. If you don't give it any interface type, it won't work for conditions where one side moves and the other doesn't, because the two bounding surfaces use a common point.

The type of interface is set in OpenFOAM's _castellatedMeshControls_ dictionary in _snappyHexMeshDict_ file. If there is no type, then _faceType_ becomes _baffle_, and non-conformal mesh become _boundary_.

If there are multiple regions, the boundary between regions should be given as inter-region.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_geometry.png" width="800" height="800"><br>형Example of Import and Add geometry</center>


