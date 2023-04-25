.. IFEWs documentation master file, created by
   sphinx-quickstart on Mon Apr 17 15:15:56 2023.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to IFEWs's documentation!


=========================
Iowa Food-Energy-Water system (IFEWs)
=========================
The model for the Iowa Food-Energy-Water system (IFEWs) is a Python-based model that encodes the interdependencies and evolution of Food-Energy-Water (FEW) resources using NASA’s OpenMDAO framework. This model uses a theory of multidisciplinary analysis and optimization (MDAO) with a focus on balancing the independent food, water, and energy disciplines. Its primary objective is to develop an interface that aids in decision-making for the long-term sustainability and resilience of Iowa's ecosystem while meeting the state's socioeconomic needs and considering the environmental impact of nitrogen export.

- The proposed IFEWs system model:

.. image:: docs/figures/IFEWs model.png
  :width: 400
  :alt: Alternative text

- The analysis can be visualized through the tools provided, which generate the following figure:

.. image:: figures/XDSM2.png
  :width: 400
  :alt: Alternative text


Introduction
=========================
The initial pages of the documentation provide a thorough explanation of the setup and basic instructions of required packages. It is recommended to review this section at least to gain an understanding of how IFEWs model is constructed.

.. toctree::
   :maxdepth: 3

   install


IFEWs Model
=========================
After you have successfully set up requirements and examined the fundamental guides, you can progress to explore the main IFEWs model with several independent subsystems presented below.

.. toctree::
   :maxdepth: 3

   IFEWsmodel
   getting_data


Visualization
=========================
Once you generate and acceess the output, you can visualize it to various formats with following methods:

.. toctree::
   :maxdepth: 3

   visualization


Visualization
=========================
Once you generate and acceess the output, you can visualize it to various formats with following methods:

.. toctree::
   :maxdepth: 3

   user_reference


