Requirements
============

IFEWs model depends external packages to do multidisciplinary analysis and optimization (MDAO). 
The following packages are required to download

#. Openmdao
#. Sklearn
#. Numpy
#. Pandas

.. note::

   Numpy and Pandas will be automatically installed along with scikit-learn because they are dependencies of Sklearn.

Installation instructions:
You can simply download the requirements by using ``pip`` from your python environment (we recommend Anaconda)::

	pip install 'openmdao[all]'

	pip install scikit-learn
	
To ensure the installation of optional dependencies, use the ``[all]`` suffix with the install command. However, it is not necessary for a basic installation.


