Additional Reference
========================

Additional references that might be helpful to understand the IFEWs model.

Here is a list of the ``numpy`` and ``pandas`` functions with examples that IFEWs model use. 


Numpy
------------------------
- ``numpy.hstack``:
    • Stacks arrays in sequence horizontally (column wise).
    
    • This is equivalent to concatenation along the second axis, except for 1-D arrays where it concatenates along the first axis.

::

	a = np.array([[1],[2],[3]])
	b = np.array([[4],[5],[6]])

	>>> np.hstack((a,b))
	array([[1, 4],
	       [2, 5],
	       [3, 6]])
	
- ``numpy.ones``:
    • Returns a new array of given shape and type, filled with ones.

::

	>>> np.ones(5)
	array([1., 1., 1., 1., 1.])

- ``numpy.array``:
    • Creates a numpy array.

::

	>>> np.array([1, 2, 3])
	array([1, 2, 3])

	
Pandas
------------------------
- ``pandas.read_csv``:
    • Reads a CSV (Comma Separated Values) file and converts it into a pandas DataFrame object.
    
    • It has many parameters that users can customize it. You can check any options that pandas provide at the `pandas’ documentation <https://pandas.pydata.org/docs/reference/api/pandas.read_csv.html>`_.

::

	csv_file = 'crop_yield_data/Corn_y_model_data_v2.csv'
	dfC = pd.read_csv(csv_file)
	
- ``pandas.DataFrame``:
    • Represents a two-dimensional, size-mutable, tabular data structure with labeled axes (rows and columns). 
    
    • Store and manipulates data in a way that is similar to a spreadsheet or a SQL table.

::

	df_result = pd.DataFrame({"N2 Surplus" : ns_data, "Corn Yield" : yc_data, "Soy Yield" : ys_data})
	df_result.to_csv("Results3-3.csv", index=False)

	




















