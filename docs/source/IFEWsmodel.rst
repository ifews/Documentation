IFEWs Model
============

After you have successfully set up requirements and examined the fundamental guides, your folder should contain the following scripts:

* OpenMDAO model of the IFEWs subsystem ::

   IFEWs_model.py

* Batch analysis script that calls the IFEWs model iteratively for all the 99 counties of Iowa from the years 1997- 2019 ::

   batch_analysis.py

* First visualization tool ::

   tool_data_plot.py

* Second visualization tool ::

   tool_contour_map.py


You can progress to explore the main IFEWs model with several independent subsystems presented below.
  
.. toctree::
   :maxdepth: 3

   ::

   Calls the IFEWs model iteratively for all the 99 counties of Iowa from the years 1997- 2019.
   
   ::
      
      IFEWs_model.py
      
   This script is the OpenMDAO model of the IFEWs subsystem
   ::
      
      tool_data_plot.py
      
   This script is the first visualization tool    batch_analysis.py
 
   model_components
   output_data
   
