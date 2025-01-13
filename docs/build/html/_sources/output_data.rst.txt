====================
Output Data
====================

The IFEWs model by default outputs four variables:

#. :math:`N_{surplus}` = Nitrogen surplus (:math:`\frac{kg}{ha}`)
#. :math:`y_{c, m} \ \ \ \ \ \ ` = Modelled corn yield (:math:`\frac{bushels}{Acres}`)
#. :math:`y_{s, m} \ \ \ \ \ \ ` = Modelled soybean yield (:math:`\frac{bushels}{Acres}`)
#. :math:`E_{prod} \ \ \ \ ` = Ethanol produced (:math:`gal`)

The nitrogen surplus is the overall output of the model. The ethanol production is currently an independant component calculated using the same inputs.
The modelled crop yields are computed using a regression equation and are required for future work with surrogate models.
The variables are all outputted as dataframes where the data at each index corresponds to the input data at that iteration of the batch analysis.
The dataframes are ordered such that the data for a county is computed for the entire timespan of 23 years before doing the same for the next county.
This is repeated for all 99 counties of Iowa.