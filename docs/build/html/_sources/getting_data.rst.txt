Getting data
========================

The ``batch_analysis.py script`` executes the ``IFEWs_model.py`` analysis by processing a series of inputs extracted from an Excel file. 


Importing requirements 
------------------------------------
You can simply import external packages and function by using ``import``::

	import pandas as pd
	
	import math

	from IFEWs_model import *

.. note::

   ``IFEWs_model`` is the latest stable version of the IFEWs model. 
   The filename may be modified following any updates to the model. 
   

Loading data
------------------------------------

The first step involves setting and loading values that are used to initialize the ``IFEWs model``. The Excel file from which data is loaded can be modified to account for different data of interest. 

Here, we import data from two Excel files: one containing weather data and the other containing animal agriculture data. The weather data encompasses monthly data for each county from 1997 to 2019. For the animal agriculture, we possess annual data per county for the same time span. 

The following piece of code offers an example::

	# Constants
	May_P = 80 # May planting progress averaged at 80%  for 2009 - 2019

	## Read CSV Files

	# Load Weather data -----------
	loc_W = 'Data_Input/weather_data/PRISM_199701_201912_sorted.csv' # Weather dataframe
	df_W = pd.read_csv(loc_W)

	# Load Animal Agriculture data -----------
	loc_AA = 'Data_Input/animal_agriculture_data/IFEW_Counties_1997_2019.csv'
	df_AA = pd.read_csv(loc_AA) # Animal Agricultural dataframe

	# Parse Weather Data
	T_July = df_W['tmean (degrees F)'][(df_W['Month']==7)]
	PPT_June = df_W['ppt (inches)'][(df_W['Month']==6)]
	PPT_July = df_W['ppt (inches)'][(df_W['Month']==7)]

	# Parse Agricultural Data
	RCN_c = df_AA["CommercialN_kg_ha"]
	A_corn = df_AA["CornAcresPlanted"]
	A_soy = df_AA["SoybeansAcresPlanted"]
	AH_corn = df_AA["CornGrainAcresHarvested"]
	AH_soy = df_AA["SoybeansAcresHarvested"]

	# Parse Animal Agricultural Data
	cattle_B = df_AA["BeefCows"]
	cattle_M = df_AA["MilkCows"]
	cattle_H = df_AA["Hogs"]
	cattle_O = df_AA["OtherCattle"]


The subsequent step is data preprocessing. In this case, there are instances of missing ``A_corn``, ``A_soy``, ``AH_corn``, and ``AH_soy`` data for certain counties across some years. To avoid gaps in our data of interest, we address these missing entries by using the average of the data from the two preceding years::

    # Checks for missing data and replaces it with the average of the previous two entries
    if pd.isna(RCN_c.iloc[i]) == True:
        df_AA.at[i, "CommercialN_kg_ha"] = (RCN_c.iloc[i - 1] + RCN_c.iloc[i - 2]) / 2
    if pd.isna(A_corn.iloc[i]) == True:
        df_AA.at[i, "CornAcresPlanted"] = (A_corn.iloc[i - 1] + A_corn.iloc[i - 2]) / 2
    if pd.isna(A_soy.iloc[i]) == True:
        df_AA.at[i, "SoybeansAcresPlanted"] = (A_soy.iloc[i - 1] + A_soy.iloc[i - 2]) / 2
    if pd.isna(AH_corn.iloc[i]) == True:
        df_AA.at[i, "CornGrainAcresHarvested"] = (AH_corn.iloc[i - 1] + AH_corn.iloc[i - 2]) / 2
    if pd.isna(AH_soy.iloc[i]) == True:
        df_AA.at[i, "SoybeansAcresHarvested"] = (AH_soy.iloc[i - 1] + AH_soy.iloc[i - 2]) / 2

.. note::

	If the input data file is complete and does not contain any missing data, you can skip the preprocessing step.


Generating output
------------------------------------
We've loaded monthly data for 99 counties over a 23-year period, resulting in a total of 2,277 data points for each variable. In this instance, we've classified animal data as ``x``, weather data as ``w``, and agriculture data as ``e``. 

Subsequently, we call the IFEW function for each set of grouped input data and log the output data of interest.
In this case, we've recorded the ``Nitrogen Surplus``, ``Corn Yield``, ``Soybean Yield``, and ``Ethanol Production`` in a csv file ``Results.csv``::

	for i in range(len(df_AA)): 	
		# Checks for missing data and replaces it with the average of the previous two entries
		if pd.isna(RCN_c.iloc[i]) == True:
			df_AA.at[i, "CommercialN_kg_ha"] = (RCN_c.iloc[i - 1] + RCN_c.iloc[i - 2]) / 2
		if pd.isna(A_corn.iloc[i]) == True:
			df_AA.at[i, "CornAcresPlanted"] = (A_corn.iloc[i - 1] + A_corn.iloc[i - 2]) / 2
		if pd.isna(A_soy.iloc[i]) == True:
			df_AA.at[i, "SoybeansAcresPlanted"] = (A_soy.iloc[i - 1] + A_soy.iloc[i - 2]) / 2
		if pd.isna(AH_corn.iloc[i]) == True:
			df_AA.at[i, "CornGrainAcresHarvested"] = (AH_corn.iloc[i - 1] + AH_corn.iloc[i - 2]) / 2
		if pd.isna(AH_soy.iloc[i]) == True:
			df_AA.at[i, "SoybeansAcresHarvested"] = (AH_soy.iloc[i - 1] + AH_soy.iloc[i - 2]) / 2

		x = [RCN_c.iloc[i], A_corn.iloc[i], A_soy.iloc[i], AH_corn.iloc[i], AH_soy.iloc[i], cattle_H.iloc[i], cattle_B.iloc[i], cattle_M.iloc[i], cattle_O.iloc[i]] 
		w = [May_P, T_July.iloc[i], PPT_July.iloc[i], PPT_July.iloc[i] ** 2, PPT_June.iloc[i]]
	    
		raw_results = IFEW(x, w, False)
		
		if i == 0:
			ns_data = [raw_results[0]]
			yc_data = [raw_results[1]]
			ys_data = [raw_results[2]]
			Et_data = [raw_results[3]]
		else:
			ns_data =  ns_data + [raw_results[0]]
			yc_data =  yc_data + [raw_results[1]]
			ys_data =  ys_data + [raw_results[2]]
			Et_data =  Et_data + [raw_results[3]]

	# Creates Directory
	if not os.path.exists("Data_Output"):
		os.mkdir("Data_Output")

	# Saves outputs as text file
	df_result = pd.DataFrame({"N2 Surplus" : ns_data, "Corn Yield" : yc_data, "Soybean Yield" : ys_data, "Ethanol Production" : Et_data})
	df_result.to_csv("Data_Output/Results.csv", index=False)


Assessing output
------------------
The csv file, named ``Results.csv`` as pre-determined, contains the 2,277 data points for each output variable that you've recorded. The rows in the file are arranged in alphabetical order and by year, ascending from 1997 to 2019.

For instance, the first 23 rows hold data for the ``Adair`` county spanning from 1997 to 2019, and the final 23 rows contain data from the ``Wright`` county for the same time period.
