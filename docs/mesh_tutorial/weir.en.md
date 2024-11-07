# Weir

## Introduction 

[Download geometry file](https://drive.google.com/file/d/1GAW7sYRYS07-To1PhH5QCLTAK29bbbuu/view?usp=sharing) 

[Download BaramMesh folder](https://drive.google.com/file/d/1oKVpiZEEfw-jEpjWlJTnWwcLP8zEztjg/view?usp=sharing)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/weir/main.png"><br> wave height
</p>

In hydraulic engineering, a weir is a structure set in a waterway that allows water to overflow and is used to maintain a specific water level or measure flow. In CFD, they are often used for code validation by comparing analytical results with theoretical flow rates.

This example shows an example of mesh generation and calculation for the analysis of a constant water level in a square weir.

A simple geometry of a square weir is read into an stl file to generate a grid.

To use the pressure condition corresponding to the head of water at the inlet, the inlet boundary is divided into water and air regions. The water level at the inlet is 1.6 meters.

## Geometry

The geometry of the weir is constructed as shown using an stl file and two [Hex6]s to separate the water and air boundary at the inlet. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/weir/geom.png"><br> Geomerty 설정
</p>

Click the [Import] button at the bottom to select the weir.stl file. 

Click the [Add] button and select [Hex6] and set values as follows

+ Name : Hex6_1 
+ Type : None 
+ MIn. : (-2 -1 0)
+ Max. : (2 1 1.6), Generate z coordinates up to 1.6 because the water level is 1.6. 

This will create six surfaces. Right-click on each surface and select [Edit/View] to rename them to
 
+ Hex6\_1\_xMin : water-in
+ Hex6\_1\_xMax : out
+ Hex6\_1\_yMin : front
+ Hex6\_1\_yMax : back
+ Hex6\_1\_zMin : bottom

In Geometry, select Hex6\_1\_zMax and right-click and select Edit/View. In the window that opens, change the type as [None] 

Once more, click the [Add] button and select [Hex6] and set values as follows

+ Name : Hex6_2 
+ Type : None 
+ MIn. : (-2 -1 1.6)
+ Max. : (2 1 2)

This will create six surfaces. Right-click on each surface and select [Edit/View] to rename them to
 
+ Hex6\_2\_xMin : air-in
+ Hex6\_2\_xMax : out-1
+ Hex6\_2\_yMin : front-1
+ Hex6\_2\_yMax : back-1
+ Hex6\_2\_zMax : top

In Geometry, select Hex6\_2\_zMix and right-click and select Edit/View. In the window that opens, change the type as [None] 


Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Region

Click the (+) icon at the top to create a region. Move the mouse to the intersection of the lines that appear in light green color in the graphics window and position it inside the computation domain. Click the [Add] button to complete the setup.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/weir/region.png"><br>
</p>

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Base Grid

Set the number of grids to 100, 50, and 50. Click the [Generate] button to generate the base grid.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/weir/baseGrid.png"><br> 
</p>

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Castellation

Since we want to make the mesh uniform across the entire area, we don't need to set any settings, so we press the [Refine] button.

Click the [Next] button to move on to the next step.
<!-------------------------------------------------------------------------------------------------->
## Snap

Use the default settings and click the [Snap] button to start snap.

Click the [Next] button to move on to the next step.
<!-------------------------------------------------------------------------------------------------->
## Boundary Layer

Does not create a boundary mesh. 

Click the [Apply] and [Next] button to move on to the next step.

The final mesh looks like this

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/weir/finalMesh.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/weir/finalMesh.png)

<!-------------------------------------------------------------------------------------------------->
## Export

Click [Export as BaramFlow project] to export the mesh to the desired location. 





