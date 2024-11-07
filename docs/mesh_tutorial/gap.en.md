# Gap Refinement

[Download geometry file](https://drive.google.com/file/d/1aNHAqI2Ab7C0sDQgrjo5Nc2gRofLCCyZ/view?usp=sharing) 

[Download BaramMesh folder](https://drive.google.com/file/d/10zOv3OgFp_HgjPt-Q_3c2vdeQdsu8AYd/view?usp=sharing)

## Introduction 

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/intro.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/intro.png)

This example is to help you understand the function of [Gap Refinement] added in BARAM-v24.2.

Gap Refinement has the following inputs and options: [Min. Cell Layers in a gap], [Gap Detection Start Level], [Max. Refinement Level], [Direction], and [Include surface's own gap]. Let's see how the mesh is generated based on these settings.

## Geometry

Use given hex\_with\_self\_gap.stl file, a sphere and a hexahedron as geometry.

Click the [Import] button at the bottom to select the hex\_with\_self\_gap.stl file. 

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/geom.png"  >
    <br> 
</p>

### Create sphere and hexahedron

+ Sphere : Click [Add] button and in new window select [Sphere]
    + Center point : (0 6.5 0)
    + Raidus : 2.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/sphere.png"  >
    <br>
</p>

+ Hexahedron : Click [Add] button and in new window select [Hex]. 
    + Min. : (-9 9 -2)
    + Max. : (10 9.5 2)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/hex.png"  >
    <br>
</p>

+ Outer boundary(Hex6) : Click [Add] button and in new window select [Hex8]
    + Min. : (-20 -10 -5)
    + Max. : (20 16 5)

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/hex6.png"  >
    <br> 
</p>

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/fullGeom.png"  >
    <br> 
</p>

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Region

Click the (+) icon at the top to create a region. Move the mouse to the intersection of the lines that appear in light green color in the graphics window and position it inside the computation domain. Click the [Add] button to complete the setup.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/region.png"  >
    <br> 
</p>

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Base Grid

Select [Use Hex6] option and set the number of grids to 20, 13, and 5. Click the [Generate] button to generate the base grid.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/baseGrid.png"  >
    <br> 
</p>

Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Castellation

Click the (+) icon under [Volume Refinement] and set the following settings 

+ Volume Refinement Level : 1
+ Gap Refinement
    + Min. Cell Layers in a gap : 4
    + Gap Detection Start Level : 1
    + Max. Refinement Level : 4
    + Direction : Mixed
    + Include surface's own gap : On
+ Volume : Hex6\_1

The idea is that any area smaller than [Gap Detection Start Level] (in this case, 1) is considered a [gap] and must contain at least 4 grids. The [Max. Refinement Level] is given as 4, so even if the level is 4 and it doesn't fit more than 4, it doesn't refine any more. The direction is both inward and outward, and includes gaps caused by surface's own side.

<p align='center'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/castel1.png"  >
    <br> 
</p>

Use the default settings, click the [Refine] button and the castellation step starts.

The result looks like this

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/refine.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/refine.png)


Click the [Next] button to move on to the next step.

<!-------------------------------------------------------------------------------------------------->
## Snap

Change the [Feature Snap Type] of [Feature Snapping] to [implicit] and click the [Snap] button to start snap.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/snap.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/snap.png)

<!-------------------------------------------------------------------------------------------------->
## Effects of Gap Refinement settings

Try creating a mesh by changing the input variables and options for Gap Refinement.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/gaps.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/gaps.png)

### Effect of [Gap Detection Start Level]

If we change [Gap Detection Start Level] to 2, the result is as follows

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/gap-detect.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/gap-detect.png)

Because the size of the gap is greater than size level 2, no refinement is done.

### Effect of [Direction]

If we change [Direction] to inside and outside, the result is as follows.

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/gap-direction.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/gap-direction.png)

### Effect of [Include surface's own gap]

The impact of this option is as follows

[![](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/gap-self.png "")](https://github.com/nextfoam/baram-pages/raw/main/screenshots/mesh/gap/gap-self.png)

