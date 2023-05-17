Application
============

Once you can run the model and generate the output, we have two scripts that post-process and analyze the output with visualizations. The tools are coded in Jupyter Notebook and the code blocks are executed in series. Additonal pthon packages required by the tools are listed at the top and can be found in counda-forge. Both tools load the output file from the model and make visualizations based on that output variables, which are:

* Nitrogen Surplus
* Corn Yield
* Soybean Yield
* Ethanol Production

--------------------
Plotting Tool
--------------------

This tool generates the validation plot for the Energy subsystem using EIA data of ethanol produced in Iowa and compares it with the modelled production of Ethanol. Additionally, the tool also generates an interactive widget, which can plot the time series of variables outputted by the model for a specific county. The widget includes all 99 counties of Iowa and can plot all the modelled output variables.

--------------------
Choropleth Map Tool
--------------------

This tool generates choropleth maps, which is a type of statistical thematic map that uses pseudocolor to visualize the modelled data geographically. The plot is made by an interactive widgets with inputs for the desired year and can plot all the output variables. Additionally, the tool can also save the maps for all variables as a set of images to generate a time series GIF.

.. toctree::
   :maxdepth: 3

   contour_plot
   making_color_map
   
   
