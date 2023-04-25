====================
Input Data
====================

The OpenMDAO model requires several geospatial datasets that serve as inputs to the various subsystems. The three subsystem datasets are:
have number points in sphinx
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

^^^^^^^^^^^^^^^^^^^^
Weather
^^^^^^^^^^^^^^^^^^^^

The weather data is acquired from the `PRISM Climate Group`_ Database. They gathers climate observations from a wide range of monitoring networks and develop geospatial climate datasets. To get data from multiple locations, a csv file with the latitude and longitudinal coordinates must be inputted, which is provided.
The OpenMDAO model requires monthly data for the following variables:

.. _PRISM Climate Group: https://prism.oregonstate.edu/

* Precipitation in inches
* Mean temperature in |deg|F

^^^^^^^^^^^^^^^^^^^^
Agriculture & Animal Agriculture
^^^^^^^^^^^^^^^^^^^^

The agriculture & animal agriculture data are both acquired from the `National Agricultural Statistics Service`_ from the USDA. Using the Quick Stats tool you can choose the variables of interest from both the crop and animal sectors. You can also choose the geopgraphic level of data and its timespan.
The OpenMDAO model requires monthly data for the following variables:

.. _National Agricultural Statistics Service: https://quickstats.nass.usda.gov/

""""""""""""""""""""
Agriculture
""""""""""""""""""""
* Rate of commercial nitrogen fixation in :math:`N \dot kg / ha`
* Area of corn planted in acres
* Area of soybean planted in acres
* Area of corn harvested in acres
* Area of soybean harvested in acres

""""""""""""""""""""
Animal Agriculture
""""""""""""""""""""
* Number of beef cattle
* Number of milk cattle
* Number of hog cattle
* Number of other cattle