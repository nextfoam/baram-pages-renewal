# Base Grid)

The base grid uses OpenFOAM's _blockMesh_ utility to create a structured type mesh including all domain.

The 'Grid Span' displays the min/max coordinates and the 'Number of Cells per Direction' allows you to specify the number of cells in the x, y, z directions. Depending on the number, the length of one cell in the x, y, z direction is displayed in the 'Grid Span'.

There is a 'Use Hex6' option to select whether to create the base grid using the Hex6 created in [1.Geometry]. It is recommended to turn on this option when creating a farfield boundary for external flow simulations.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_baseGrid.png" width="800" height="800"><br>Base Grid setup</center>

Clicking the Generate button will create the mesh and an internalMesh appears in the display settings, which you can see in the Graphics window. If you want to change the number of mesh, click the Reset button to modify and regenerate.







