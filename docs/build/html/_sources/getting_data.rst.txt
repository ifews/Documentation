Getting data
========================

This script executes the ``IFEWs model`` analysis by processing a series of inputs extracted from an Excel file. 
It employs the ``IFEWs_model_v3_2`` for this purpose.


Importing requirements 
------------------------------------
You can simply import external packages and function by using ``import``::

	import pandas as pd
	
	import math

	from IFEWs_model_v3_2 import IFEW

.. note::

   ``IFEWs_model_v3_2`` is the newest version of the ``IFEWs model``. 
   The filename may be modified following any updates to the model. 
   

Loading data
------------------------------------

The first step involves setting and loading values that are used to initialize the ``IFEWs model``. The Excel file from which data is loaded can be modified to account for different data of interest. 

Here, we import data from two Excel files: one containing weather data and the other containing animal agriculture data. The weather data encompasses monthly data for each county from 1997 to 2019. For the animal agriculture, we possess annual data per county for the same time span. 

The following piece of code offers an example::

	# Constants
	May_P = 80 # May planting progress averaged at 80%  for 2009 - 2019
	RCN_c = 185  # Sawyer(2018) Avg (155-215)
	RCN_s = 17 # Avg = 17.7 kg/ha std = 4.8kg/ha based on the fertilizer use and price data between 2008-2018 (USDA, 2019)

	# Read CSV Files

	# Load Weather data
	loc_W = 'weather_data/PRISM_199701_201912_sorted.csv' # Weather dataframe
	df_W = pd.read_csv(loc_W)

	# Load Animal Agriculture data
	loc_AA = 'animal_agriculture_data/IFEW_Counties_1997_2019.csv'
	df_AA = pd.read_csv(loc_AA) # Animal Agricultural dataframe

	# Parse Weather Data
	T_July = df_W['tmean (degrees F)'][(df_W['Month']==7)]
	PPT_June = df_W['ppt (inches)'][(df_W['Month']==6)]
	PPT_July = df_W['ppt (inches)'][(df_W['Month']==7)]

	# Parse Animal Data
	cattle_B = df_AA["BeefCows"]
	cattle_M = df_AA["MilkCows"]                
	cattle_H = df_AA["Hogs"]
	cattle_O = df_AA["OtherCattle"]	
	
	# Parse Agricultural Data
	A_soy = df_AA["SoybeansAcresPlanted"]
	AH_soy = df_AA["SoybeansAcresHarvested"] 
	A_corn = df_AA["CornAcresPlanted"]
	AH_corn = df_AA["CornGrainAcresHarvested"]	


The subsequent step is data preprocessing. In this case, there are instances of missing ``A_corn``, ``A_soy``, ``AH_corn``, and ``AH_soy`` data for certain counties across some years. To avoid gaps in our data of interest, we address these missing entries by using the average of the data from the two preceding years::

	# Fill up the missing data in csv file
	for i in range(len(A_corn)): 
	    if(math.isnan(A_corn[i])):
	        A_corn[i] = (A_corn[i-1] + A_corn[i-2]) / 2
	        
	for i in range(len(AH_corn)): 
	    if(math.isnan(AH_corn[i])):
	        AH_corn[i] = (AH_corn[i-1] + AH_corn[i-2]) / 2
	        
	for i in range(len(A_soy)): 
	    if(math.isnan(A_soy[i])):
	        A_soy[i] = (A_soy[i-1] + A_soy[i-2]) / 2
	        
	for i in range(len(AH_soy)): 
	    if(math.isnan(AH_soy[i])):
	        AH_soy[i] = (AH_soy[i-1] + AH_soy[i-2]) / 2

.. note::

	If the input data file is complete and does not contain any missing data, you can skip the preprocessing step.


Generating output
------------------------------------
We've loaded monthly data for 99 counties over a 23-year period, resulting in a total of 2,277 data points for each variable. In this instance, we've classified animal data as ``x``, weather data as ``w``, and agriculture data as ``e``. 

Subsequently, we call the IFEW function for each set of grouped input data and log the output data of interest.
In this case, we've recorded the ``Nitrogen Surplus``, ``Corn Yield``, ``Soybean Yield``, and ``Ethanol Production`` in a csv file ``Results.csv``::

	for i in range(len(df_AA)): 	
	    x = [RCN_c, RCN_s, cattle_H.iloc[i], cattle_B.iloc[i], cattle_M.iloc[i], cattle_O.iloc[i]
	    w = [May_P, T_July.iloc[i], PPT_July.iloc[i], PPT_July.iloc[i] ** 2, PPT_June.iloc[i]]
	    e = [A_corn.iloc[i], AH_corn.iloc[i], A_soy.iloc[i], AH_soy.iloc[i]]
	    
	    raw_results = IFEW(x, w, e, False)
	    
	    if i == 0:
	        ns_data = [raw_results[0]]
        	yc_data = [raw_results[1]]
        	ys_data = [raw_results[2]]
        	P_EtOH_data = [raw_results[7]]
        	
	    else:   	 
	        ns_data =  ns_data + [raw_results[0]]
	        yc_data =  yc_data + [raw_results[1]]
	        ys_data =  ys_data + [raw_results[2]]
	        P_EtOH_data = P_EtOH_data + [raw_results[7]]

	df_result = pd.DataFrame({"N2 Surplus" : ns_data, "Corn Yield" : yc_data, "Soy Yield" : ys_data, "Ethanol Production" : P_EtOH_data })
	df_result.to_csv("Results.csv", index=False)


Assessing output
------------------
The csv file, named ``Results.csv`` as pre-determined, contains the 2,777 data points for each output variable that you've recorded. The rows in the file are arranged in alphabetical order and by year, ascending from 1997 to 2019.

For instance, the first 23 rows hold data for the ``Adair`` county spanning from 1997 to 2019, and the final 23 rows contain data from the ``Wright`` county for the same time period.
