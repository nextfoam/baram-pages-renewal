# Porous media

## Introduction 

[Download geometry file](https://drive.google.com/file/d/1Jlqrgd5BrKkAfhzNkybtb0cF3zSEAFfA/view?usp=sharing) 

[Download BaramMesh folder](https://drive.google.com/file/d/1tN2wyi5Tae0OYhfyoWjzP522D10Uk2rx/view?usp=sharing)

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/porousMedia/intro.png"><br> 
</p>

This is an example of mesh generation for flow analysis using porous media conditions. The problem involves flow entering from the top left and flowing down through a porous region (the blue area on the left side of the figure above is the porous region).

OpenFOAM's porous media model has a problem with discontinuous velocity distributions in the porous region and pressure losses that differ slightly from the input conditions. In the figure below, the left side is the result from Baram-v24, and the right side is the result using the standard solver in OpenFOAM 2306. 

NextFOAM, which is used by Baram, solves this problem by improving the interpolation method of pressure in porous regions (see the documentation linked below for more information). You can see that the accuracy of the results is much improved, as is the convergence.

[Porous Media Reference](https://nextfoam.co.kr/proc/DownloadProc.php?fName=231101140051_yvpJhMF0nY.pdf&realfName=10thOKUCC_OpenFOAM%EC%82%AC%EC%86%8C%ED%95%9C%EB%AC%B8%EC%A0%9C%EB%93%A4.pdf)

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/porousMedia/res.png"><br> result of Baram v24(left), openfoam 2306 standard solver(right)
</p>

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/porousMedia/residual-1.png"><br> Residual Baram v24(left), openfoam 2306 standard solver(right)
</p>
<br/>


## Geometry

Use given porousMedia.stl file as geometry.

Click the [Import] button at the bottom to select the stl file. 

Turn on the [Split Surface] option to separate boundary surfaces. A single-surface geometry is divided into eight surfaces based on feature angles.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/porousMedia/import.png"><br> 
</p>

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/porousMedia/split.png"><br> 
</p>

The eight surfaces are renamed to distinguish between entrances and exits. Click on the inlet with the mouse in the graphics window to activate the surface in the [Geometry] list. Right-click the [Edit/View] button and rename it to inlet or outlet.

Click the [Add] button and select [Hex] and set values as follows for porous cell zone

+ Name : porousZone
+ Type : CellZone
+ Min. : (-0.3 -0.2 0.8)
+ Max. : (0.3 0.2 0.9)

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/porousMedia/hex.png"><br>
</p> 

In Geometry, select porousZone\_surface and right-click and select Edit/View. In the window that opens, change the type as [None].

The final list of geometries should look like the following image.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/porousMedia/geom.png"><br> 
</p> 

Click the [Next] button to move on to the next step.
<!-------------------------------------------------------------------------------------------------->
## Region

Click the (+) icon at the top to create a region. Move the mouse to the intersection of the lines that appear in light green color in the graphics window and position it inside the computation domain. Click the [Add] button to complete the setup.


<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/porousMedia/region.png"><br> 
</p> 

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Base Grid

Set the number of grids to 60, 40, and 120. Click the [Generate] button to generate the base grid.

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/porousMedia/baseGrid.png"><br> 
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

Add layer at walls.

Click the (+) icon under [Configuration] to add a [Setting], and set it as follows

+ Number of Layers : 5
+ Thickness Model Specification : First and Expansion
+ Size Specification : Relative
+ First Layer Thickness : 0.15
+ Expansion Ratio : 1.2
+ Min. Total Thickness : 0.3
+ Boundary : all walls of duct

<p style="text-align: center">
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/porousMedia/blayer.png"><br> 
</p> 

Click [Apply] button to start boundary layer step.

The final mesh looks like this

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/porousMedia/layer.png)](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/porousMedia/layer.png)

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Export

Click [Export as BaramFlow project] to export the mesh to the desired location. 

