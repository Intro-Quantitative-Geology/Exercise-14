# Lab-exercise-7
This exercise is part 2 of the exercises on thermochronology. In this exercise you will modify your Python code to load a data file of measured thermochronometer ages, compare the measured ages to predicted ages from your Python code using a goodness-of-fit equation, and plot the results. The goal is to try to minimize the misfit between the predicted and measured thermochronometer ages, and quantify the long-term exhumation rates in the Bhutan Himalaya from different thermochronometers.

## Overview
![Bhutan Himalaya](Images/Bhutan_Himalaya.png)<br/>
*The Himalaya in Bhutan. [Image source](http://commons.wikimedia.org/wiki/File:View_of_Gasa_Dzong.JPG).*

The overall goal of this exercise is to interpret a thermochronometer dataset from the Himalaya of Bhutan. The interpretation entails determining long-term average rock exhumation rates from rock samples analyzed using apatite and zircon (U-Th)/He and muscovite <sup>40</sup>Ar/<sup>39</sup>Ar thermochronology. As you will recall, our exercise last week used a simple 1-D transient solution to the advection-diffusion equation to calculate a temperature-depth profile in the Earth, which was then used to predict thermochronometer ages based on Dodson's method. In that model we specified a rock exhumation rate and observed variations in the thermochronometer ages as a function of exhumation rate. This week we will compare those predicted thermochronometer ages to data from Bhutan with the goal of minimizing the misfit between the measured and predicted thermochronometer ages by varying the specified rock exhumation rate, which will allow us to define a best-fit exhumation rate (or exhumation history) for the Himalaya of Bhutan. For this exercise, we will be using data from [Coutand et al., 2014](https://dx.doi.org/10.1002/2013JB010891) and [Stüwe and Foster, 2001](https://dx.doi.org/10.1016/S1367-9120(00)00018-3) (PDFs available on the [course Moodle page](https://moodle.helsinki.fi/course/view.php?id=12453#section-4)).

## Problem 1 - Comparing measured and predicted thermochronometer ages, again
In order to be able to compare our predicted thermochronometer ages to some data, we'll first need to load the data file. You are welcome to use the example script file above to make your modifications (I have noted where to make changes), but it might be better to copy your Python script from last week and make changes to that code.

1. Using the past exercises as examples (look at those codes :smile:), you should add code to bottom your Python script (after the predicted thermochronometer ages are calculated) to read a data file and store the contents in different variables in the code. The data file has a header that lists the data contained in each column. You may want to look at the data file in a text editor in order to give logical names to the variables in the data file.
2. Once you have added the code to read the data file, you will want to calculate goodness-of-fit values for each thermochronometer system, as well as a total goodness-of-fit value for all of the age data. You should use the same goodness-of-fit equation we have used in the previous exercises, and be sure to divide the goodness-of-fit value by the number of measured ages for each thermochronometer. The goodness of fit equation you should use is

    ![Goodness-of-fit equation](Images/Equation1.png)<br/>
    where *n* is the number of ages, *O*<sub>*i*</sub> is the *i*th measured age, *E*<sub>*i*</sub> is the *i*th predicted age and *σ*<sub>*i*</sub> is the *i*th standard deviation.
    
    :heavy_exclamation_mark: **NOTE**: Be aware that measured ages are listed for each sample location in the data file, but not every sample has been analyzed for each different thermochronometer system. Thus, there are ages of '-9999' listed in the data file. Those ages are listed there to indicate there is no corresponding measured age for that thermochronometer system at that sample location. Those ages should be ignored in your goodness-of-fit calculation using an `if` statement.
3. The last task is to produce a useful plot. For this, we will again use the `plt.subplot()` command to make a plot that shows (1) the predicted geotherm and particle temperature-depth history in the upper panel (i.e., the plot from Laboratory Exercise 6), and (2) the predicted and measured age data on the lower panel.
  - For the lower plot, the *x*-axis should plot latitude and the *y*-axis should plot thermochronometer age, including error bars for the measured ages.
  - It may help to plot the different thermochronometer systems with different color symbols.
  - The predicted ages can be shown using lines of constant age that have a range that includes the entire range of latitude of the measured age data.
  - This plot should also list the goodness-of-fit values for each thermochronometer system, as well as the total goodness-of-fit for the age dataset.

## Problem 2 - "Fitting" thermochronometer data












For this exercise, submit a copy of your Python code and the plot you produce for point 3 above with an advection velocity of 0.5 mm/a. Include a descriptive figure caption as well.


This two-part set of exercises is designed to give you a better understanding of thermochronology and the thermal field in the Earth's crust.
Thermochronology combines many aspects of what has been studied in this course, including the advection and diffusion equations, tectonic and surface (erosional) processes and some basic geostatistics.
The first part of the laboratory exercises on thermochronology is intended to provide an understanding of heat advection and diffusion in the Earth's crust, the time-dependence of thermal processes, and how a thermal history can be used to predict thermochronometer ages.

## Getting started
1. You can start by making a folder to store files for this week's exercises in a Terminal.

    ```bash
    $ cd Desktop
    $ mkdir Lab-6
    $ cd Lab-6
    ```
**Reminder**: the `$` symbol above represents the command prompt in the Terminal window.
2. Now you can open **Spyder**.

    ```bash
    $ spyder
    ```

Now we are ready to start.

## Time-dependent temperature in the Earth
In this exercise we will use an analytical solution to the 1-D time-dependent thermal advection-diffusion equation to simulate erosion of rock at the Earth's surface, the upward transport of the underlying rock toward the surface and the changes in a 1-D geotherm with time.
The basic equation for temperature *T* as a function of depth *z* and time *t* was originally published by Carslaw and Jaeger (1959)

![Equation 1](Images/Equation1.png)<br/> *Equation 1. 1D time-dependent heat advection-diffusion equation.*

where *G* is the initial geothermal gradient (increase in temperature with depth), *v*<sub>*z*</sub> is the vertical advection velocity (positive upward), *κ* is the thermal diffusivity and `erfc()` is the complementary error function, defined as

![Equation 2](Images/Equation2.png)<br/> *Equation 2. The complementary error function.*

With this equation, the temperature initially increases linearly with depth (*T*(*z*,*t*=0) = *Gz*) and the geotherm will evolve with time as a function of the advection velocity and thermal diffusivity.

## Problem 1 - The time dependence of rock advection
We will begin by plotting geotherms to get a sense of how our temperature equation works.

1. If you [download a copy of the Python script `age_predict_1D.py`](age_predict_1D.py) you should be able to run it without making any changes to produce a plot like that shown below.

    ![Time-dependent heat transfer with advection](Images/1D_transient_plot1.png)<br/>
    *Figure 1. 1D transient thermal solution including advection.*<br/><br/>
To start, please add axis labels and a title to this plot. **What is the advection velocity for this thermal solution?** Add a text label listing the advection velocity on your the plot using the `plt.text()` function, then save a copy of the plot at the end of this document.
2. **How does the thermal solution change when you alter the advection velocity?** Increase the advection velocity to 1.0 mm/a and save the resulting plot at the end of this document. Do the same thing for an advection velocity of 0.1 mm/a. **What is the effect of changing the advection velocity on temperatures in the shallow crust (<10 km depth)?**
3. Reset the advection velocity to the starting value. Now increase the total simulation time to 100 Ma. Once again, save a copy of this plot at the end of this document. **What happens to temperatures in the model as the simulation time increases?** You might want to run some additional calculations (you don't need to save the plots) with even longer simulation times (1000 Ma, 5000 Ma, etc.). **What do you observe for the temperatures in the model? Are there any potential problems with these temperatures? Does the model approach a steady-state thermally?**
4. Reset the simulation time to 50 Ma. Now we'll explore the effect of the initial thermal gradient. Increase the initial thermal gradient to 20°C/km, run the model and save the plot. Do the same for a thermal gradient of 5°C/km. **How does the thermal gradient affect the temperatures in the model? Is there any clear relationship between the initial thermal gradient and the maximum temperature in the model at *t* = 0 Ma?**
5. Lastly, reset the initial thermal gradient to 10°C/km. Increase the number of temperature calculations to plot from 1 to 5 and run the thermal model. Save this plot and insert it at the end of this document. **In your opinion, is it helpful to see the temperature calculations at different times?**

## Predicting thermochronometer ages
Thermochronometers record the time since a rock or mineral was at a given temperature (closure temperature) in the Earth.
Above, we have calculated temperature solutions assuming 1-D vertical advection of rock.
Here, we will track rocks through the thermal field with time and use their recorded thermal history to predict thermochronometer ages.
The key to understanding what is done here is to understand that we will be simulating the position of a parcel of rock at depth in the earth, and at each time step the position of the rock parcel will be moved upward according to the length of the time step multiplied by the advection velocity.
In mathematical terms, this relationship is

![Equation 3](Images/Equation3.png)<br/> *Equation 3. Equation for calculating rock particle depth as a function of time.*

Thus, we will track a parcel of rock from some depth in the model at time *t*=0 to the surface when the simulation is complete at 0 Ma.
At each depth position, the temperature of the parcel of rock will be recorded, which will allow the thermal history of the rock parcel to be used to predict different thermochronometer ages.
We will consider the apatite (U-Th)/He, zircon (U-Th)/He and muscovite <sup>40</sup>Ar/<sup>39</sup>Ar thermochronometers that were presented briefly in lecture.

Thermochronometer closure temperatures will be predicted using Dodson's method, which was also discussed briefly in lecture.
According to Dodson's method, the closure temperature *T*<sub>c</sub> of a thermochronometer is

![Equation 4](Images/Equation4.png)<br/> *Equation 4. The effective closure temperature according to Dodson's method.*

where *E*<sub>a</sub> is the activation energy, *R* is the universal gas constant, *A* is a geometric factor (*A* = 25 for a sphere, *A* = 8.7 for a planar sheet), *τ* is time for the diffusivity to decrease by a factor of 1/e, *D*<sub>0</sub> is the diffusivity at infinite temperature and *a* is the diffusion domain (we'll assume this is the size of the mineral). The value of *τ* can be calculated as a function of the cooling rate *dT*/*dt*

![Equation 5](Images/Equation5.png)<br/> *Equation 5. The characteristic time for a change in diffusivity.*

By simulating cooling of the minerals by iterating over the values of the recorded temperature history from depth to the surface, thermochronometer ages can be predicted for various systems.

## Problem 2 - Cooling ages and their relationship to exhumation rate
One of the main interests for scientists using thermochronology is to determine the exhumation rate of a study area based on the ages of thermochronometer data at the surface.

1. At the top of the Python script [`age_predict_1D.py`](age_predict_1D.py), there are three flags (`True`/`False` variables) that allow you to enable the calculation of different thermochronometers.
Set the value for `calc_AHe` to `True` to enable prediction of apatite (U-Th)/He ages and run the model with the default parameters.
**What is the predicted apatite (U-Th)/He age?**
**What does this age mean?**
**What is the closure temperature for the model in this case?**
If you look carefully through the code, you can find where the closure temperature is calculated.
Add text to your plot to display the predicted apatite (U-Th)/He age and predicted closure temperature using the `plt.text()` function.
I suggest that you add the text functions at the bottom of the script where the predicted apatite (U-Th)/He age is written to the screen (if requested).
Save a copy of the plot and insert it at the end of this document.
2. Similar to above, set the flags for `calc_ZHe` and `calc_MAr` to `True` and add the corresponding `plt.text()` functions to display the predicted zircon (U-Th)/He and muscovite <sup>40</sup>Ar/<sup>39</sup>Ar ages and their predicted closure temperatures.
As above, I suggest that you add the text functions at the bottom of the script where the predicted apatite (U-Th)/He age is written to the screen (if requested).
Save a copy of the plot and insert it at the end of this document.
**Do all of the predicted thermochronometer ages make sense?**
3. To understand more about how the thermochronometer ages are predicted, it can he helpful to look at the temperature-depth history of the parcel of rock as it travels from depth to the surface.
You can see this history visually by setting the flag `plot_Tzhist` to `True` at the top of the Python script.
This will add a set of symbols to the plot that display how the temperature and depth of the rock parcel changes with time.
**Considering the predicted thermochronometer ages and closure temperatures, do you see any problems with the predicted ages now?**
Save a copy of this plot and insert it at the end of this document.
4. Finally, increase the advection velocity in the thermal model to 1.0 mm/a and produce a similar plot to above.
**Do the predicted thermochronometer ages make more sense now?**
**What was the problem in questions 2 and 3?**
Save a copy of this plot and insert it at the end of this document.

## What to submit
**For this exercise, your modifications to the end of this document should include**

1. The 7 plots requested for problem 1 and the 4 plots requested for problem 2.
2. Figure captions for each plot describing the plot as if it were in a scientific journal article.
3. Answers to all of the questions in bold
4. Copies of your modified Python scripts for Problems 1 and 2

**NOTE**: You may want to reference these plots in your final report on these exercises, so be sure to keep the copies of the plot files.

## References
Carslaw, H. S., & Jaeger, J. C. (1959). Conduction of heat in solids. Oxford: Clarendon Press.

Dodson, M. H. (1973). Closure temperature in cooling geochronological and petrological systems. Contributions to Mineralogy and Petrology, 40(3), 259–274.

# Answers
## Problem 1
This is some text. You can use *italics* or **bold** text easily. You may want to read a bit more about [formatting text in Github-flavored Markdown](https://help.github.com/articles/basic-writing-and-formatting-syntax/). You can see an example of how to display an image with a caption below.

![Text shown if image does not load](Images/sine.png)<br/>
*Figure 2: Sine wave calculated from 0 to 2π*
