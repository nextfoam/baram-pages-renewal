# Run Conditions

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/runcondition.png"><br>Run Conditions setup - steady(left), transient(right)</center>

The number of iterations is used for steady conditions, and the End Time is used for transient conditions.

### Time Stepping Method

The time advancement method for transient conditions can be fixed or adaptive. In the case of 'fixed', a time step size is required. In the case of the adaptive time method, the time step size is determined by the Courant Number in the flow region and the Maximum Diffusion Number in the solid region.

The Courant Number (CFL) and Diffusion Number ($D_i$) are defined as follows

<center>$CFL = U \frac{\Delta t}{\Delta x}$</center>

<center>$D_i = \alpha \frac{\Delta t}{\Delta x^2}$</center>

* $\alpha$ : thermal diffusivity
* $\Delta t$ : time step size
* $\Delta x$ : grid size

For multiphase flow calculations, an additional Max Courant Number for VoF is required.

### Save Interval

Specifies at what intervals data should be automatically saved. If \textbf{0, no automatic saving is done and only the last result is saved.} When the calculation ends, the data at the end of the calculation is saved regardless of the value given here.


### Retain Only the Most Recent Files

Saves only the most recent results, as many as set in Maximum Number of Data Files, and deletes older results. 

