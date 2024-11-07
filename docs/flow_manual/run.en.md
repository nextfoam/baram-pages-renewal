# Run

## 계산 시작 및 중지

Click Start Calculation to start the calculation. Once the calculation has started, Cancel Calculation, Save and Stop Calculation, and Update Configuration are enabled.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/run.png"><br>Run</center>

'Stop Calculation' forces the calculation to stop. 'Save and stop calculation' completes an ongoing iteration, then saves the data and stops the calculation.

When the calculation starts, only 'Numerical Method', 'Monitor' and 'Calculation Conditions' are enabled and the rest are disabled. **If you modify the settings in the enabled items and click 'Apply setting changes now'**, the changes will be applied and the calculation will continue.

## User Parameter

User Parameter can be specified and used. This feature is mainly used in Batch Running Mode.

The figure below is an example of specifying two variables, UX and UY. The number in parentheses in Name indicates how many places this variable is being used, here it is 0, which means it is not being used anywhere. Default Value is the value assigned to this variable when not in batch mode.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/userParameter0.png"><br>example of User Parameter</center>

Declaring, modifying, and deleting variables uses the window shown in the figure~\ref{fig:userParameter}, which appears when the Edit button is pressed. Click the (+) next to Parameter Values to add a new one. **Variable names can only contain uppercase letters, numbers, and underscores ('\_') and must start with an uppercase letter**. If click the trash icon right of the variable, variable will be deleted. But variables that are being used in your code will not be deleted.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/userParameter.png"><br>User Parameter Edit</center>

You can enter variables with a dollar sign in the same place you enter numbers for boundary conditions, initial conditions, etc. If you want to use the UX variable for the X-Velocity of the 'Inlet Velocity' boundary condition, you would write \$UX.

## Batch Running Mode

If click 'Switch to Batch Running Mode' button, you will see 'Batch Cases' appears and the button changes to 'Switch to Live Running Mode', as figure below. 'Live Running Mode' is the normal way to calculate for a single condition rather than batch calculations.

[User Parameter and Batch Run tutorial](https://baramcfd.org/tutorials/2024/03/21/batchRunRAE2822-post/)

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/batchCases.png"><br>Batch Running Mode</center>

The figure above shows a conditions that conditions was set by pressing the Import button and reading the csv file as shown below. The file types that can be read are csv and xlsx files. In the file, var, case1, case2, case3, etc. can be written arbitrarily by the user, but the variable names UX, UY must be defined in User Parameter. An example of a csv file looks like this:

```
var,UX,UY
case1,1,0
case2,2,1
case3,3,2
...
```

In the figure above, there are columns named 'Calc.' and 'Result'. Select a case (you can select multiple cases by dragging) and press the right mouse button to select Schedule Calculation, and 'Calc.' will be checked. If you select Cancel Calculation, the check mark will disappear. 

When you press Start Calculation button, the cases checked in 'Calc.' are calculated sequentially. The completed calculation is displayed in green color in 'Result', and the divergent cases are displayed in red color. 

After all calculations are finished, right-click the case and select Load to display the residual and monitoring graphs of the case and check the results in ParaView.


