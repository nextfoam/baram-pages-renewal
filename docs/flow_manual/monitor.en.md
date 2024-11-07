# Monitor

This is where you set up settings for monitoring the results during the calculation. There are four types of settings: Force, Point, Surface, and Volume. Click the Add button to add an item. When the calculation starts, the added items are graphed in the Monitor window.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/monitor.png" width="300" height="300"><br>Monitor</center>

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/monitor1.png"><br>example of monitoring graph</center>

## Force

The settings are Write Interval, Flow Direction, Center of Rotation, and Boundaries.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/force.png" width="300" height="300"><br>Force</center>

The 'Write Interval' is how many iterations to save the data.

'Flow Direction' can be selected between 'Direct' and 'Angle of Attack, Sideslip'. 'Direct' is a way to input drag and lift as vectors. 'Angle of Attack, Sideslip' enters the angle based on the drag and lift direction when the angle of attack (AOS) and angle of sideslip (AOS) are zero. The Pitch axis is determined perpendicular to the drag and lift directions. 

Force and force coefficient data are saved and graphs $C_d$, $C_l$, and $C_m$ are plotted. When calculating these values, use the reference values in [Setup-Reference Values].

## Point

The settings window for a Point looks like the image of figure below. Enter the Field and Coordinate to monitor and the Write Interval to monitor. If you want to monitor values on a boundary, select Snap onto Boundary. In this case, the value of the point closest to the coordinate is monitored, even if the coordinate is not exactly on the boundary.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/point.png" width="300" height="300"><br>Point monitoring</center>

## Surface

The settings window for a Surface looks like the image of figure below. Enter the Surface, Field Variable, Report Type, and Write Interval you want to monitor.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/surface.png" width="300" height="300"><br>Surface monitoring</center>

Report Type can be area-weighted average, mass-weighted average, integral, mass flowrate, volume flowrate, minimum, maximum, and coefficient of variation (CoV). For mass and volume flow rates, you only need to select the boundary, for the others, select the boundary and field. 

## Volume

The settings window for a Volume looks like the image of figure~\ref{fig:volume}. Enter the Volume, Field Variable, Report Type, and Write Interval you want to monitor.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/volume.png" width="300" height="300"><br>Volume monitoring</center>

Report Type can be volume average, volume integral, minimum, maximum, Coefficient of variation, CoV.


