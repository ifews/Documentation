Installation
============

To use IFEWs model, you must first install Python3. 

In case you prefer convenient access to the source code and examples, you have the option to install IFEWs model through cloning the ifews/Multidisciplinary-Model::

    git clone https://github.com/ifews/Multidisciplinary-Model.git

Then, open the terminal and ``cd`` into the downloaded repository and pip install the package::

    pip install -e .

This will install the package in editable mode. If you change anything in the source code of the package, then you don't need to re-install the package.

.. note::

	Since the packge is under constant developement, installing new updates will might be necessary. If there are any, following code will get the latest updates for the IFEWs model. 


Open the terminal and ``cd`` into the cloned repository and run::

    git pull


Requirements
------------------------------------

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


