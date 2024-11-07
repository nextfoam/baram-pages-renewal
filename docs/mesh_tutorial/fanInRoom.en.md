# Fan in room

[Download geometry file](https://drive.google.com/file/d/1Om4XvnHL5X1ck6v6JQ2PWTik_nZRK0Jv/view?usp=sharing) 

[Download BaramMesh folder](https://drive.google.com/file/d/1nEadrr6ku_lxiWWS5YHk4JTqxrjHjkuZ/view?usp=sharing)

## Introduction 

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/room/room-intro.png" ><br>
</p>

* This example uses BaramMesh to generate mesh for the rotatingFanInRoom from the pimpleFoam tutorial in OpenFOAM.
* We have a rotating fan in a room and to compute it as a sliding mesh, we have a cylinder surrounding the fan and set its interior as a cellzone. The faces surrounding the cellzone must have two interface faces at the same location.

## Geometry

Use given 5 stl files as geometry.

Click the [Import] button at the bottom and select the desk.stl, door.stl, fan.stl, outlet.stl, room.stl files. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/room/fanInRoom-importSTL.png"  >
    <br> 
</p>

The volume named desk has a surface named desk\_surface, the volume named fan has a surface named fan\_surface, and the volume named outlet has a surface named outlet\_surface, door, and room because room, door, and outlet make up a single volume.

### Create cylinder for cell zone and interface

Click the [Add] button and select [Cylinder] and set values as follows

+ Type : cellZone
+ Axis Point1 : (-3 2 2.3)
+ Axis Point2 : (-3 2 2.8)
+ Radius : 0.8

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/room/fanInRoom-cylinder.png"  >
    <br> 
</p>

In Geometry, select Cylinder\_1\_surface and right-click and select Edit/View. In the window that opens, change the type as [Interface]-[Non-Conformal]. 


[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/room/fanInRoom-geom.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/room/fanInRoom-geom.png)

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Region

Click the (+) icon at the top to create a region. Move the mouse to the intersection of the lines that appear in light green color in the graphics window and position it inside the computation domain. Click the [Add] button to complete the setup.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/room/fanInRoom-region.png">
    <br> 
</p>

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Base Grid

Set the number of grids to 65, 55, and 30. Click the [Generate] button to generate the base grid.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/room/fanInRoom-baseGrid.png"  >
    <br> 
</p>

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Castellation(Castellation)

Set the levels for the fan, desk surfaces, and cylinder volumes.

### Surface/Feature Refinement

Click the (+) icon under [Surface/Feature Refinement] and set the following settings for fan

+ Surface Refinement
    + Minimum Level : 3
    + Maximum Level : 4
+ Feature Edge refinement Level : 3
+ Surfaces : fan\_surface

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/room/fanInRoom-ami.png" >
    <br> 
</p>

Click the (+) icon under [Surface/Feature Refinement] and set the following settings for desk

desk 면 설정을 위해 (+) 아이콘을 누르고 다음과 같이 설정한다.

+ Surface Refinement
    + Minimum Level : 1
    + Maximum Level : 1
+ Feature Edge refinement Level : 1
+ Surfaces : desk\_surface

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/room/fanInRoom-desk.png" >
    <br> 
</p>

### Volume Refinement

Click the (+) icon under [Volume Refinement] and set the following settings for cylinder. 

+ Volume Refinement Level : 3
+ Volume : Cylinder\_1
  
<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/room/fanInRoom-amiVol.png">
    <br> 
</p>

Click the [Refine] button and the castellation step starts. 

The result looks like this

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/room/fanInRoom-refine.png">
    <br> 
</p>


Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Snap

Change the [Feature Snap Type] under [Feature Snapping] to [implicit] and click the [Snap] button to start snap.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/room/fanInRoom-snap.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/room/fanInRoom-snap.png)

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Boundary Layer

Does not create a boundary mesh. 

Click the [Apply] and [Next] button to move on to the next step.
<!-------------------------------------------------------------------------------------------------->
## Export

Click [Export as BaramFlow project] to export the mesh to the desired location. 

