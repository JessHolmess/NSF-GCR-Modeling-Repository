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

It is useful to generate a large set of simulations to compare directly. The file 4_Multi_Simulation.ipynb is a walk-through showing how to run multiple simulations for different initial conditions and animate the solutions for easy comparison. It includes two types of animation schemes, a simple scheme with visuals of progressing plots and a complex scheme that includes key metrics printed underneath the progressing plots. The key metrics include the number of simulations, functional calls regarding the numerical integration scheme, final state values at the end of the time span, and the variable parameterization that the simulations were run with. For reasons unbeknownst to me, the complex animation scheme is finicky and often does not render correctly on its first run; Therefore, if using the complex animation scheme, run each code block individually before the animation block, then run the animation block and if it renders incorrectly (spacing of text overlaps with figures), close the animation pop-up and run the animation code block again. The spacing should be fixed after the second run. The file also includes a code block for saving your animation as an mp4 to a given directory on your computer; be sure you are content with the animation visual before saving it as an mp4. The mp4 format allows for much more control when referencing the simulations within the animation later on (pausing, moving forward, moving backward in initial condition space; things the `matplotlib.animation` does not allow for).

## Particular Investigations - Ready to Run

The remaining files within the repository were created for investigating specific questions or generating pertinent figures. Most of the files below pull from previously simulated solution(s) which can be generated and stored by running 3_Single_Simulation.ipynb or 4_Multi_Simulation.ipynb (animation and file writing block needed). Below are short descriptions of each remaining file and their intended use.

Comparative_Bifurcation_Plots.ipynb - A file that runs simulations for both NPZ and NPZV configurations, pulls values representing the late time behavior of each solution, and plots them on comparative graphs. The resulting plots show the nature of the changing equilibrium the system experiences under its base parameterization; in particular the plots illustrate a Hopf Bifurcation wherein a (in this case) stable equilibrium transforms into a stable limit cycle. 

Dynamic_Initial_State_Animation.ipynb - A file that runs multiple simulations with various combinations of initial state conditions for a specified total N level. Animation capabilities are included for ease of comparison. This file is intended to help diagnose model sensitivity to initial conditions.

gVOD_data_parsing.ipynb - File importing and analyzing data from the [Global Viral Oceanography Database (gVOD)](https://doi.pangaea.de/10.1594/PANGAEA.915758). Key parameters of interest are virus decay rates, virus-to-prokaryote ratio, burst size, and lysis rate. Must download data in csv format to run the file. 

Reduced_model_examples.ipynb - A file that runs reduced versions of the coupled model to emulate the inspirational models it is based off of. Has examples simulating the equations in the following references: 

 - Sarmiento, Jorge L., and Nicolas Gruber. Ocean Biogeochemical Dynamics. Princeton University Press, 2006. https://doi.org/10.2307/j.ctt3fgxqx.

- Thamatrakoln, Kimberlee, David Talmy, Liti Haramaty, Christopher Maniscalco, Jason R. Latham, Ben Knowles, Frank Natale, Marco J. L. Coolen, Michael J. Follows, and Kay D. Bidle. “Light Regulation of Coccolithophore Host–Virus Interactions.” New Phytologist 221, no. 3 (February 2019): 1289–1302. https://doi.org/10.1111/nph.15459.


TotalN_vs_VHRatio_&_BurstSize.ipynb - A file that estimates and visualizes free virus-to-host ratio and burst size for steady-state solutions over changing total N.

Transfer_Rates.ipynb - Calculates and visualizes the sources and sinks over time for a single simulation, as well as calculates and visualizes the transfer rates at late time for multiple simulations at various total N levels.





