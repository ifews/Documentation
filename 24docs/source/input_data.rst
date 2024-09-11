====================
Input Data
====================

The OpenMDAO model requires several geospatial datasets that serve as inputs to the various subsystems. The three subsystem datasets are:

* Weather
* Agriculture
* Animal Agriculture

Additionally, two more datasets are required for the visualization tools that are provided with the model:

* County Geographic dataset
* FIPS (Federal Information Processing Standards) Geospatial dataset

--------------------
Data Source 
--------------------

All the datasets are provided and presorted as needed by the model. However, it is helpful to know the source of the data when tuning the model.
The provided datasets for the inputs contain the data for all the 99 counties of Iowa from the years 1968-2019.
Beyond 1997 the agricultural data at the desired geographic level is not consistently avaiable at the same resolution as the weather data.
Therefore, the datasets are limited to this timeframe to ensure the consistency.

^^^^^^^^^^^^^^^^^^^^
Weather
^^^^^^^^^^^^^^^^^^^^

The weather data is acquired from the `PRISM Climate Group`_ Database. They gathers climate observations from a wide range of monitoring networks and develop geospatial climate datasets.
To get data from multiple locations, a csv file with the latitude and longitudinal coordinates must be inputted, which is provided. Although the PRISM database
The OpenMDAO model requires monthly data for the following variables:

.. _PRISM Climate Group: https://prism.oregonstate.edu/

* Precipitation in :math:`inches`
* Mean temperature in :math:`\deg F`

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Agriculture & Animal Agriculture
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The agriculture & animal agriculture data are both acquired from the `National Agricultural Statistics Service`_ from the USDA.
Using the Quick Stats tool you can choose the variables of interest from both the crop and animal sectors. You can also choose the geopgraphic level of data and its timespan.
The OpenMDAO model requires monthly data for the following variables:

.. _National Agricultural Statistics Service: https://quickstats.nass.usda.gov/

""""""""""""""""""""
Agriculture
""""""""""""""""""""
* Rate of commercial nitrogen fixation in :math:`N \cdot kg / ha`
* Area of corn planted in :math:`acres`
* Area of soybean planted in :math:`acres`
* Area of corn harvested in :math:`acres`
* Area of soybean harvested in :math:`acres`

""""""""""""""""""""
Animal Agriculture
""""""""""""""""""""
* Number of beef cattle
* Number of milk cattle
* Number of hog cattle
* Number of other cattle

--------------------
Input Variables 
--------------------

The IFEWs model takes two arrays as inputs to compute the nitrogen surplus value at each year for the corresponding geopgraphic level.
If the input data is at the district level, the output data will also be at the district level.
The scope of our project is to make a database for the estimated nitrogen surplus level at the county level, so all the input databases available in the GitHub correspond to the county data for the state of Iowa.
The input arrays are defined as follows:

* x (Array) - Contains the agriculture and animal data for desired geographic level and year.
    #. :math:`RCN_{corn} \ \ \ \ `    = Rate of commercial N for corn (:math:`\frac{kg}{ha}`)
    #. :math:`A_{corn} \ \ \ \ \ \ \ \ \ \ `   = Area of corn planted (Acres)
    #. :math:`A_{soy} \ \ \ \ \ \ \ \ \ \ \ `    = Area of corn planted (Acres)
    #. :math:`AH_{corn} \ \ \ \ \ \ \ `  = Area of corn harvested (Acres)
    #. :math:`AH_{soy} \ \ \ \ \ \ \ \ `   = Area of soybean harvested (Acres)
    #. :math:`y_c \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ `      =  Corn yield (:math:`\frac{bushels}{Acres}`) 
    #. :math:`y_s \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ `      =  Soybean yield (:math:`\frac{bushels}{Acres}`) 
    #. :math:`Cattle_{Hog} \ \ ` = Hogs cattle population
    #. :math:`Cattle_{Beef} \ ` = Beef cattle population
    #. :math:`Cattle_{Milk} \ ` = Milk cattle population
    #. :math:`Cattle_{Other}` = Other cattle population
* w (Array) - Contains the weather data  for desired geographic level and year.
    #. :math:`May_P \ \ \ \ \ `    = Average May plantation progress
    #. :math:`T_{July} \ \ \ \ \ \ \ `   = July temperature (:math:`^{\circ} F`) 
    #. :math:`PPT_{July} \ ` = July precipitation (inches) 
    #. :math:`PPT_{June}` = June precipitation (inches)
