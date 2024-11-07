# Report

This is where the results are extracted after the calculation. Values can be extracted for four categories: Force, Point, Surface, and Volume.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/report.png"><br>Report</center>

## Force

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/forceReport.png"><br>Force Report</center>

For Flow Direction, you can choose between 'Direct' and 'Angle of Attack, Sideslip.' 'Direct' involves inputting the directions of Drag and Lift as vectors. 'Angle of Attack, Sideslip' allows you to input angles based on the directions of Drag and Lift when the Angle of Attack (AOA) and Sideslip Angle (AOS) are both zero. The pitch axis is determined perpendicular to the directions of Drag and Lift.

By selecting the 'Center of Rotation' and the boundary surface, and then clicking the 'Compute' button, you can view the results. Along with the Drag/Lift/Moment coefficients, the Force and Moment can be divided into pressure and viscous components, allowing you to check the values for each direction.

Reference values to calculate $C_d$, $C_l$, $C_m$ are set at [Setup-Reference Values].

Unlike in Monitoring, in Report, calculations are performed using saved data, so there may be slight differences from the values in Monitoring depending on the write precision of the data.

## Point

Enter the Field and Coordinate to be reported. If you want to report values on a boundary surface, select the relevant boundary in 'Snap onto Boundary.' In this case, even if the coordinates are not exactly on the boundary, it will display the values from the point closest to the specified coordinates.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/pointReport.png"><br>Point Report</center>

## Surface

Enter the Surfae, Field Variable and Report Type to be reported. For Report Type, you can choose from area-weighted average, mass-weighted average, integral, mass flowrate, volume flowrate, average, minimum, maximum, and Coefficient of Variation (CoV). When selecting mass flowrate or volume flowrate, only the boundary surface needs to be selected, while for the others, both the boundary surface and the field must be selected.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/surfaceReport.png"><br>Surface Report</center>

## Volume

Enter the Volumes, Field Variable and Report Type to be reported. For Report Type, you can choose from volume average, volume integral, minimum, maximum, Coefficient of variation(CoV). 

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/volumeReport.png"><br>Volume Report</center>



