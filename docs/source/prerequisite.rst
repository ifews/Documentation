Prerequisite Learning
============

- IFEWs model is a program built with the OpenMDAO software framework, which is a Python-based open-source platform for performing multidisciplinary optimization and systems analysis with high-performance computing capabilities. In case you lack familiarity with OpenMDAO, it is recommended that you review the OpenMDAO’s documentation (link 달기: http://openmdao.org/twodocs/versions/latest/index.html). 

- Also, the IFEWs model uses LinearRegression() from the sklearn linear_model package to approximate unmeasured and missing data using available historical data. Related information and tutorials can be found at the scikit-learn’s documentation (link 달기: https://scikit-learn.org/stable/modules/linear_model.html)

- Here, a brief overview of OpenMDAO and sklearn is provided for understanding the fundamental mechanism and the structure of the IFEWs model. 
Follow the below steps for installation::

	pip install download

OpenMDAO basics
------------

- This is a basic terminology within OpenMDAO:
    • Component: the smallest buildiing block of computational work
    • Group: a structure used to construct models hierarchically
    • Driver: A process that manages the iterative execution of models, such as an optimizer or DOE (design of experiments)
    • Problem: The top-level class that encompasses all components and provides the model execution API.
    
.. image:: figures/openmdao4.png
  :width: 400
  :alt: Alternative text
  
- In OpenMDAO, calculations are carried out inside a Component, which is the smallest unit of computation understood by the framework. Each Component produces its own set of variables. OpenMDAO provides different types of Components for various calculations.

In order to understand the different kinds of components in OpenMDAO, let us consider the following numerical model that takes ``x`` as an input:
 
.. image:: figures/openmdao1.png
  :width: 400
  :alt: Alternative text
  
- In our numerical model, we have three variables: ``x``, ``y``, and ``z``. Each of these variables needs to be defined as the output of a component. There are three basic types of components in OpenMDAO:
    • IndepVarComp : defines independent variables (e.g., ``x``)
    • ExplicitComponent : defines dependent variables that are computed explicitly (e.g., ``z``)
    • ImplicitComponent : defines dependent variables that are computed implicitly (e.g., ``y``)

- The most straightforward way to implement the numerical model would be to assign each variable its own component, as below.

.. image:: figures/openmdao2.png
  :width: 400
  :alt: Alternative text
  
- Another way that is also valid would be to have one component compute both ``y`` and ``z`` explicitly.
It would mean that this component solves the implicit equation for ``y`` internally.
  
.. image:: figures/openmdao3.png
  :width: 400
  :alt: Alternative text

- Add how to setup input & output, define a component, run the script, sellar, etc. (refer to Basic User Guides)
  
  
Sklearn linear model basics
------------

- The following is a method intended for ordinary least squares linear regression::

	class sklearn.linear_model.LinearRegression(*, fit_intercept=True, copy_X=True, n_jobs=None, positive=False)

- LinearRegression fits a linear model with coefficients w = (w1, …, wp) to minimize the residual sum of squares between the observed targets in the dataset, and the targets predicted by the linear approximation.

.. image:: figures/scikit1.png
  :width: 400
  :alt: Alternative text

- Parameters:
    • fit_interceptbool, default=True
	Whether to calculate the intercept for this model. If set to False, no intercept will be used in calculations (i.e. data is expected to be centered).

    • copy_Xbool, default=True
If True, X will be copied; else, it may be overwritten.

    • n_jobsint, default=None
	The number of jobs to use for the computation. This will only provide speedup in case of sufficiently large problems, that is if firstly n_targets > 1 and secondly X is sparse or if positive is set to True. None means 1 unless in a joblib.parallel_backend context. -1 means using all processors. See Glossary for more details.

    • positivebool, default=False
	When set to True, forces the coefficients to be positive. This option is only supported for dense arrays.
  
``` py
>>> from sklearn import linear_model
>>> reg = linear_model.LinearRegression()
>>> reg.fit([[0, 0], [1, 1], [2, 2]], [0, 1, 2])
LinearRegression()
>>> reg.coef_
array([0.5, 0.5])
```
.. image:: figures/scikit2.png
  :width: 400
  :alt: Alternative text

  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
