# Installation and Start

## Installation

Visit the installation instructions for BARAM-v24.

[https://baramcfd.org/docs/installation](https://baramcfd.org/docs/installation)

BARAM-v24 is available in the Microsoft Azure cloud with no installation required. It is available by accessing the following sites

[https://azuremarketplace.microsoft.com/en-us/marketplace/apps/nextfoam.nextfoam_baram24_ubuntu_x86?tab=Overview](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/nextfoam.nextfoam_baram24_ubuntu_x86?tab=Overview)


## Start

On Windows, BaramFlow and BaramMesh icons are created on the desktop after the program installation is complete. Double-click the icons to run them.

On Linux, use the terminal to run the file. In the terminal, navigate to the folder where BARAM is installed and run the file to start the program. The executable file is a bash script and the command is as follows. 

* BaramFlow : bash baramFlow.sh
* BaramMesh : bash baramMesh.sh

When you run BARAM, a window appears where you can choose whether to run a new calculation or open an existing problem.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/launcher1.png" width="400" height="400"><br>Start Window</center>

The window displays the folders that have been recently solved, which you can select to start BARAM and open the problem. If you want to open an existing case and it is not displayed in the window, you can select the folder by pressing the Open button.

To start a new calculation, click the New Case button and enter the name in the desired location, then the Solver Type, Multiphase Model selection window will appear, and then BARAM will be executed. 
 
<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/launcher2.png" width="400" height="400"><br>New Case</center>

### Solver Type

The solver type is selected between Pressure-based and Density-based.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/launcher3.png" width="400" height="400"><br>Solver Type setup</center>

The Density-based solver is a solver for analyzing transonic/supersonic flows with shock waves, where the velocity of the flow is higher than the speed of sound. Transonic/supersonic flows require techniques to reliably compute the rapid changes in flow variables such as pressure, density, and temperature before and after the shock wave. These techniques are currently only available in \textit{TSLAeroFoam}, the density-based solver in Baram. In Baram v6, the pressure-based solver _PCNFoam_ had this capability, but this solver is not supported in v24 and is planned to be included in the future.

***TSLAeroFoam*** **is currently only capable of steady-state calculations, and transient analysis is planned**.

For subsonic compressible flows without shock waves, density-based solvers are less convergent at lower Mach numbers, so it is better to use pressure-based solvers.

Pressure-based should be chosen for chemical species mixing and multiphase flow simulations.
 
### Multiphase Model

For multiphase flow models, choose between Off and Volume of Fluid. If more than two phases are present, select Volme of Fluid.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/launcher4.png" width="400" height="400"><br>Multiphase Model setup</center>>

<!------------------------------------------------------------------>
## Main Window

When you run the program, you'll see a window of figure below.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/maingui.png"><br>Main window</center>

The main window can be divided into five areas

* Menu
* Graphics Toolbar
* Navigation
    + Select items such as Models, Materials, Boundary Conditions, Numerical Conditions, Initial Conditions
* Configration
    + Setup conditions such as Models, Materials, Boundary Conditions, Numerical Conditions, Initial Conditions
* Graphics \& Console
    + Console : shows residuals, messages, error statements and more as the calculation progresses
    + Mesh : show mesh
    + Residuals : show residuals graph as the calculation progresses
    + Monitor : show graphs of monitoring values as the calculation progresses

<!------------------------------------------------------------------>
## Menu

Th Menu consists of File, Mesh, View, Parallel, Settings, External Tools and Help.

### File

#### Save

Save all settings.

#### Save As

Save all settings under a different name, but not the calculation results.

#### Load Mesh

Import a mesh. The following grid file formats are supported.

* openfoam  : Select either the constant or polyMesh folder. **For multiregion problems, you should select the constant folder because there are multiple polyMesh folders**. If you select a specific polyMesh folder in the grid of a multiregion problem, only that part of the grid will be read.
* Fluent : Convert msh and cas file. Only ASCII files are supported. It uses the _fluenToFoam_ utility developed by NEXTFoam and can convert to a multiregion mesh and polyhedral mesh. If you select a file with more than one cellzone, the window shown in the following figure appears (it does not appear when there is only one cellzone). The name of each cellzone appears, and you can select which region it is and whether it is fluid or solid. You can add a region by clicking the (+) icon or delete a region by clicking the trash can icon.

<center><img src="https://blog.nextfoam.co.kr/wp-content/uploads/2024/10/fluentToFoam.png" width="300" height="300"><br>fluent mesh convert window</center>

* StarCCM+ : StarCCM+ : Convert ccm file. Requires the libccmio library to be present. Supported on Linux only.
* Gmsh : Converts msh files from Gmsh, an open source lattice generation program. Only ASCII files are supported.
* I-deas Universal : I-deas Universal : Convert unv files. Only ASCII files are supported.

#### Close Case

Close the current task and returns to the initial screen of BaramFlow (select New Case or Open).

#### Exit

Exit program.

<!------------------------------------------------------------------>
### Mesh

show grid information and  Scale/Translate/Rotate mesh.

* Mesh Info. : show mesh information and domain size
* Scale : input x, y, z scale ratio
* Translate : input translate distance of x, y, z direction
* Rotate : input rotation center, axis and angle

### View

On/Off Console, Mesh, Residual, Monitor window.

### Parallel

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/parallel.png" width="300" height="300"><br>Parallel setup window</center>

Make settings for parallel computation. Select the number of CPU cores and parallel computation method. There are Local Machine and Cluster, and in the case of Cluster, you need a text file with the node list. The View/Edit button lets you type directly or load a file. The contents of the file are as follows:

```
[node name1]   cpu=[number of cores]
[node name2]   cpu=[number of cores]
[node name3]   cpu=[number of cores]
...
```

### Settings

There are 3 items as Scale, Language, ParaView.

* Scale : This adjusts the program's resolution, and anything higher than the default of 1.0 will result in larger windows and larger text. You must close and restart the program for the new settings to take effect.
* 언어(Language) : Language : Currently, only English, Suomi (Finnish), and Korean are supported. Language translation is done using Qt Linguist. Anyone can contribute translations. 
[href{https://baramcfd.org/docs/internationalization](https://baramcfd.org/docs/internationalization)
* ParaView : Specifies the location of the ParaView executable.


### External Tools

External Tools has You can find program information and third party program information in About. ParaView entry, which drives ParaView for post-processing.

### Help

External Tools has a ParaView entry, which drives ParaView for post-processing.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/about.png" width="300" height="300"><br>About window</center>
</p>

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/thirdParty.png" width="300" height="300"><br>third party software information</center>

<!------------------------------------------------------------------>
## Graphics Toolbar

The toolbar for three-dimensional graphics has the icons shown in the figure~\ref{fig:graphicsToolbar}.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/graphicsToolbar-1.png"><br>graphics tool bar</center>


<p align='left'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/axis.png"> Axis : Show axis at (0, 0, 0)
</p>

<p align='left'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/ruler.png"> Axis Grid : Show axis coordinates of whole domain 
</p>

<p align='left'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/ruler-1.png"> Ruler : Show the distance between two points on the boundary surface
</p>

<p align='left'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/perspective.png"> Perspective : Select the Perspective/Orthogonal method 
</p>

<p align='left'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/fit.png"> Fit : Fit the size of displayed model

</p>

<p align='left'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/viewNormal.png">  Axis View : Set view point to axis normal closest to the current state

</p>

<p align='left'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/rotation.png"> Rotation View : Rotate 90 degrees
</p>

<p align='left'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/displayOption.png"> Display Options : Select mesh display method
</p>

<p align='left'>
    <img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/bg.png"> Backgroud Color : Set background color
</p>

<!------------------------------------------------------------------>
## Mouse Control

* Rotation : Press the left mouse button and Move
* Translation : Press the mouse wheel and Move
* Scale : Rotate the mouse wheel or press the right mouse button and Move
* Select Boundary : click the left mouse button - selected boundary turns white

