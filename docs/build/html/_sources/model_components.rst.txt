
====================
Model Components
====================

Using the OpenMDAO framework, the multidisciplinary IFEWs model can be decomposed into a series of smaller interconnected components.
These components are defined by their inputs/outputs and relation to other components.

--------------------
Subsystems
--------------------

The IFEW nexus model contains 5 integrated subsystems including Weather, Agriculture, Animal Agriculture, Energy, and Water. The first three subsystems have been fully implemented.
The energy subsystem will model the production of ethanol and biodiesel, they do no contribute to the nitrogen surplus but are directly related to the crop data.
The water subsystem has not been implemented yet.

^^^^^^^^^^^^^^^^^^^^
Weather
^^^^^^^^^^^^^^^^^^^^

The weather subsystem uses the inputted weather data and correlates it with a linear regression equation of past data to estimate the crop yield.
The regression equation is based on past annual weather and crop data for the state of Iowa.

^^^^^^^^^^^^^^^^^^^^
Agriculture
^^^^^^^^^^^^^^^^^^^^

The agriculture subsystem uses the inputted crop data and the computed crop yield from the weather subsystem to calculate the grain nitrogen (GN), fixed nitrogen (FN), and commercial nitrogen (CN).
These outsputs are portions of the total nitrogen surplus and are defined as follows:
 
* FN - Nitrogen fixed by the planted soybean crop

    :math:`FN = (81.1 \cdot 6.72511 \cdot 10^{-3} \cdot  y_s - 98.5) \cdot \frac{A_{soy}}{A}` [#f1]_

* GN - Nitrogen present in the harvested grain

    :math:`GN = \frac{y_c \cdot 6.72511 \cdot 0.118 \cdot AH_{corn} + y_s \cdot 6.2511 \cdot 0.64 \cdot AH_{soy}}{AH_{corn} + AH_{soy}}` [#f1]_

* CN - Nitrogen inputted from the fertilizer application rate of commercial nitrogen

    :math:`CN = RCN_{corn}` [#f2]_

The computed nitrogen components are in :math:`\frac{N \cdot kg}{ha}`

^^^^^^^^^^^^^^^^^^^^
Ethanol Production
^^^^^^^^^^^^^^^^^^^^
 
The ethanol susbsytem is a subset for the energy production facet of the IFEWs model. The computed corn yield and agricultural inputs data are used to estimate the production of ethanol from a regression equation.

:math:`P_{EtOH} = 0.02 \cdot y_c \cdot A_{corn} \cdot \frac{1.827}{1000}  \cdot (1000 \cdot 42)` [#f2]_

^^^^^^^^^^^^^^^^^^^^
Animal Agriculture
^^^^^^^^^^^^^^^^^^^^

The animal agriculture subsystem uses the inputted animal and crop data to compute the manure nitrogen (MN) [#f1]_.
The nitrogen fixated in the soil by animal manure is first calculated individually by cattle type as follows

:math:`N_{Total_{Cattle}} = Cattle_{Beef} \cdot N_{Beef} \cdot lf_{Beef} + Cattle_{Milk} \cdot N_{Milk} \cdot lf_{Milk} \\\\` 
      :math:`\ \ \ \ \ \ \ \ \ \ \ \ \ \ + 0.5 \cdot Cattle_{Other} \cdot N_{HS} \cdot lf_{HS} + 0.5 \cdot Cattle_{Other} \cdot N_{Slught_{Cattle}} \cdot lf_{Slught_{Cattle}}`
     
:math:`N_{Total_{Hog}} = Hog \cdot N_{Hog} \cdot lf_{Hog}`

It is then summed to compute the total nitrogen fixated in the soil by animal manure.

:math:`MN = \frac{N_{Total_{Cattle}} + N_{Total_{Hog}}}{A}`

^^^^^^^^^^^^^^^^^^^^
Nitrogen Surplus
^^^^^^^^^^^^^^^^^^^^

The nitrogen surplus subsystems summates the computed nitrogen components calculated from the other components to compute the total nitrogen surplus.

:math:`N_{surplus} = CN  + MN + FN - GN` [#f1]_

.. rubric:: References

.. [#f1] Raul, V., Leifsson, L., Kaleita, A. System Modeling and Sensitivity Analysis of the Iowa Food-Water-Energy Nexus. Journal of Environmental Informatics Letters, p. 73-79, jan. 2021. https://doi.org/10.3808/jeil.202000044
.. [#f2] (PLACEHOLDER) Julia's presentation.