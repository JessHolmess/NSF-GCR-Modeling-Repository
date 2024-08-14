# NSF GCR Computational Model + Investigations

Welcome to the NSF Growing Convergence Research (GCR) GitHub repository, which holds any code used to aid the research goals of the biogeochemical modeling sector. This document will act as an introductory map describing the intended use of key documents within the repository.

## Where to Start
The required background and motivation regarding the formulation of the numerical model are held [elsewhere](https://linktr.ee/nsf_gcr_project?utm_source=linktree_profile_share&ltsid=a69ffa86-f170-4fec-948f-d30d795412ce). While background is not necessarily required to run the code files, it provides helpful context as to the decisions made within. 

### Function Master

The 1_Function_Master.ipynb file holds all dependencies and major functions used to generate simulations. 

The first and most important function representing the system of ordinary differential equations related to the project is defined as the function `model`. The `model` function contains the equation definitions along with logistical adjustments that are deployed during numerical integration. Numerical integration is performed with the `scipy.integrate.solve_ivp` function in subsequent files, and thus the `model` function is formulated to the specifications of `solve_ivp`. 

The output of the `solve_ivp` function does not include the evaluated derivatives using the solutions it provides. Thus to evaluate the rate of change for each state at a given time step, we provide a function, `svs`, which stands for Sources vs. Sinks, which provides a `pandas` data frame tracking individual transfer terms, collective sources and sinks, and the total differential for each state variable evaluated using the relevant `solve_ivp` solution.   

Frequently used functions within the file also include a function `print_parameterization` which nicely displays your choice of parameterization used when integrating the `model`, a function `ss_or_osc` which determines if a solution set converges to a Steady State or is OSCillatory, and more. 

### Parameterization

The 2_Parameterization.ipynb file holds the numerical and literary definitions for essential variables included in the differential equations. Each variable has a definition, which is then culminated into a `param` array, intended to be passed into the numerical integrator. 

The Function_Master.ipynb and Parameterization.ipynb files are essential for the function of subsequent files. 

## Generating a Simulation 

If you wish to run an example simulation for a specific parameterization, run the 3_Single_Simulation.ipynb file. This file numerically integrates our model for a given parameterization (defined in 2_Parameterization.ipynb) and initial condition (defined in 3_Single_Simulation.ipynb), then visualizes the solutions using the plotting module, `matplotlib.pyplot`. Details about the object returned from the integration scheme, plotting, and how to pull each state value at the end of the time span are included in this file. 

## Generating Multiple Simulations - Animation of Solutions

It is useful to generate a large set of simulations to compare directly. The file 4_Multi_Simulation.ipynb is a walk-through showing how to run multiple simulations for different initial conditions and animate the solutions for easy comparison. It includes two types of animation schemes, a simple scheme with visuals of progressing plots and a complex scheme that includes key metrics printed underneath the progressing plots. The key metrics include the number of simulations, functional calls regarding the numerical integration scheme, final state values at the end of the time span, and the variable parameterization that the simulations were run with. For reasons unbeknownst to me, the complex animation scheme is finicky and often does not render correctly on its first run; Therefore, if using the complex animation scheme, run each code block individually before the animation block, then run the animation block and if it renders incorrectly (spacing of text overlaps with figures), close the animation pop-up and run the animation code block again. The spacing should be fixed after the second run. The file also includes a code block for saving your animation as an mp4 to a given directory on your computer; be sure you are content with the animation visual before saving as an mp4. The mp4 format allows for much more control when referencing the simulations within the animation later on (pausing, moving forward, moving backward in initial condition space; things the `matplotlib.animation` does not allow for).

## Particular Investigations

The remaining files within the repository were created for investigating specific questions or generating pertinent figures. Below are short descriptions of each remaining file and their intended use.

TotalN_vs_VHRatio_&_BurstSize.ipynb - A file which estimates and visualizes free virus to host ratio and burst size for steady state solutions over changing total N.

Dynamic_Initial_State_Animation.ipynb - A file which runs multiple simulations with various combinations of initital state conditions for a specified total N level. Animation cappabilities are included for ease of comparison. Inteanded to help diagnose model sensitivity to initial condition.

gVOD_data_parsing.ipynb - File importing and analysing data from the Global Viral Oceanography Database (gVOD). Key parameters of interests are virus decay rates, virus-to-prokaryote ratio, burst size, and lysis rate. These data can help inform our models parameterization.

Comparative_Bifurcation_Plots.ipynb - A file which runs simulations for both NPZ and NPZV configurations, pulls values representing the late time behavior of each solution, and plots them on comparitive graphs. 






