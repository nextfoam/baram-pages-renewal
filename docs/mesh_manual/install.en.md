# Installation and Start

## Installation

Visit the installation instructions for BARAM-v24. 

[https://baramcfd.org/docs/installation](https://baramcfd.org/docs/installation)

BARAM-v24 is available in the Microsoft Azure cloud with no installation required. It is available by accessing the following sites

[https://azuremarketplace.microsoft.com/en-us/marketplace/apps/nextfoam.nextfoam_baram24_ubuntu_x86?tab=Overview](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/nextfoam.nextfoam_baram24_ubuntu_x86?tab=Overview)

## Start

On Windows, BaramFlow and BaramMesh icons are created on the desktop after the program
installation is complete. Double-click the icons to run them.

On Linux, use the terminal to run the file. In the terminal, navigate to the folder where
BARAM is installed and run the file to start the program. The executable file is a bash script
and the command is as follows.

* BaramFlow : baramFlow.sh 혹은 python -m baramFlow.main
* BaramMesh : baramMesh.sh 혹은 python -m baramMesh.main

When you run BaramMesh, a window appears where you can choose whether to run a new project or open an existing project.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_start.png" width="400" height="400"><br>Start Window</center>

The window displays the folders that have been recently used, which you can select to start
BaramMesh and open the project. If you want to open an existing project and it is not displayed in the window, you can select the folder by pressing the Open button.

To start a new project, click the New Case button and enter the name in the desired location.

### Main Window

When you run the program, you'll see a window of figure~\ref{fig:mesh_mainWindow}.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_mainWindow.png" width="800" height="800"><br>Main Window</center>

The main window can be divided into six areas

* Menu
* Progress panel : The part at the top of the screen that shows each steps
* Configration window : Setup conditions for each step
* Display Control : Located in the second left corner of the screen. Settings for the display of geometry and grids
* Graphic window : Show geometry and mesh
* Console Log : shows messages, error statements and more as the mesh generation progresses

### Menu

There are menus for File, Mesh Quality, View, Parallel, Settings, Help.

#### File

In file there are sub-menus as New, Open, Open Recent, Save, Exit. 

#### Mesh Quality

Clicking on Mesh Quality will bring up the window shown below. The mesh will be created based on the values set here.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_parameters.png" width="400" height="400"><br>Mesh Quality parameter</center>

#### Parallel

Make settings for parallel computation. Select the number of CPU cores and parallel computation method. There are Local Machine and Cluster. 

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_parallel.png" width="300" height="300"><br>Parallel setup window</center>

In the case of Cluster, you need a text file with the node list. The View/Edit button lets you type directly or load a file. The contents of the file are as follows:

```
[node name1]   cpu=[number of cores]
[node name2]   cpu=[number of cores]
[node name3]   cpu=[number of cores]
...
```
#### Settings

* Scale : This adjusts the program's resolution, and anything higher than the default of 1.0 will result in larger windows and larger text. You must close and restart the program for the new settings to take effect.
* Language : Currently, only English, Suomi (Finnish), and Korean are supported. Language translation is done using Qt Linguist. Anyone can contribute translations. 
[https://baramcfd.org/docs/internationalization](https://baramcfd.org/docs/internationalization)

### Progress panel

Seven steps are shown. The next step is currently disabled. When you finish the current step and press the Next button, the current step is locked and the next step is enabled.

If you go back to a previous step, you can see its status, but you can't edit it. If you want to edit the previous step, you must unlock it by pressing the Unlock button. Unlocking unlocks all subsequent steps and clears the mesh data that was created. 

### Configration Window

Set models and values for each step.  

### Display Control

You will see a list of geometries and mesh. There are three types: Geomerty, Boundary, and Mesh. In the [1.Geometry] and [2.Region] steps, only Geometry is displayed, and when you create a background grid in [3.Base Grid], Boundary and Mesh appear from then on. You can sort the list by clicking Name and Type.\\

If you right-click on the list, you will see Hide, Opacity, Color, Display Mode, and No Cut. This is how you can set the display appearance. You can set each item individually, or you can set several at once by dragging the mouse.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_display.png" width="300" height="300"><br>Display Control</center>

<p align='left'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/viewEnable.png"> The on/off button in the display control, is for choosing whether to show the mesh in the graphics window, to avoid the inconvenience of taking a long time to render graphics as you move through the progress steps in case of very large mesh.
</p>

Expand 'Cut' at the top to see the Clip and Slice options. Slice allows you to view a section perpendicular to the axis. Objects that have No Cut selected in the Display Options will not be cut.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/mesh_cut.png" width="800" height="800"><br>Example of display Control</center>

### Graphics Window

At the top is a toolbar that lets you set your display preferences.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/graphicsToolbar.png" width="800" height="800"><br>graphicsToolbar</center>

<p align='left'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/axis.png"> Show axis at (0, 0, 0)
</p>

<p align='left'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/ruler.png"> Show axis coordinates of whole domain
</p>

<p align='left'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/ruler-1.png"> Show the distance between two points on the boundary surface
</p>

<p align='left'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/perspective.png"> Select the Perspective/Orthogonal method
</p>

<p align='left'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/fit.png"> Fit the size of displayed model

</p>

<p align='left'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/viewNormal.png"> Set view point to axis normal closest to the current state

</p>

<p align='left'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/rotation.png"> Rotate 90 degrees
</p>

<p align='left'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/meshNumber.png"> Show number of mesh at current stage
</p>

<p align='left'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/bg.png"> Set background color
</p>

<!------------------------------------------------------------------>
### Mouse Control

* Rotation : Press the left mouse button and Move
* Translation : Press the mouse wheel and Move
* Scale : Rotate the mouse wheel or press the right mouse button and Move
* Select Boundary : click the left mouse button - selected boundary turns white

