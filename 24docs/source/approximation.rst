Approximation
========================

By utilizing a neural network, we can approximate data for a specific county when sufficient training data from surrounding counties is available. To build a large enough dataset for the neural network, we combine data using one of two approaches. The first approach incorporates data from neighboring counties that share borders with the target county. The second method uses data from the eight surrounding counties, regardless of whether they share borders. These counties include those located to the north-east, north-west, north-central, central-east, central-west, south-east, south-west, and south-central of the target county. By aggregating data from these counties over the period from 1968 to 2019, we create a robust dataset for model training. In this case, I used RCN and corn yield data, allowing the neural network to approximate the RCN value based on corn yield inputs. However, users can substitute any variable of interest, as long as it is included in the CSV file used as the empirical data for this model.

Importing libraries 
------------------------------------
You can simply import external packages and function by using ``import``::

	import numpy as np
	import pandas as pd

	from sklearn.model_selection import train_test_split
	import numpy as np

	from imp_function import *
	from Cvg_County import *

.. note::

   ``Cvg_County.py`` is the file that funds surrounding counties of the inputted county. 
   ``imp_function.py`` is the file that has functions that are used here, which include training the neural network.
   
Prepocessing data
------------------------------------

The first step involves setting and loading values that are used to initialize creating the training data.::
	
	# Load Animal Agriculture data -----------
	loc_AA = 'animal_agriculture_data/IFEW_Counties_1997_2019.csv'

	data = pd.read_csv(loc_AA)

	# Capitalize only the first letter.Ex) Lyon, Marchall, etc. 
	user_input = input("\n County name : ")

	method_input = input("\n Choose the method \n 1. Surrounding counties \n 2. Agricultural district \n\n Method : ")
	
	if method_input == '1':
		print("\n You selected 1.Surrounding counties")
		selected_counties = get_surrounding_counties(user_input)
		formatted_string = ', '.join(selected_counties)
		print(f"\n Surrounding counties are {formatted_string}")
		aggregated_data = aggregate_surrounding_data(user_input, data)
		
	elif method_input == '2':
		print("\n You selected 2. Agricultural district")
		selected_counties = get_counties_in_same_district(user_input)
		district = get_district_by_county(user_input)
		print(f"\n The district for {user_input} is: {district}")
		aggregated_data = data[data['CountyName'].isin(selected_counties)]
		
		
	# Extract values into a list
	RCN_array = np.array(aggregated_data['CommercialN_kg_ha'].tolist())
	yield_c_array = np.array(aggregated_data['CornGrainYield_bupacre'].tolist())
	yield_s_array = np.array(aggregated_data['SoybeansYield_bupacre'].tolist())

	# Check for NaN values in any of the arrays
	has_nan_RCN = np.isnan(RCN_array).any()
	has_nan_yield_c = np.isnan(yield_c_array).any()
	has_nan_yield_s = np.isnan(yield_s_array).any()

	# Combine the results to see if any array has NaN values
	has_nan_any = has_nan_RCN or has_nan_yield_c or has_nan_yield_s

	if has_nan_any:
		print("\n At least one of the arrays contains NaN values.")
		if has_nan_RCN:
			RCN_array = fill_nan(RCN_array)
			
		if has_nan_yield_c:
			yield_c_array = fill_nan(yield_c_array)
			
		if has_nan_yield_s:
			yield_s_array = fill_nan(yield_s_array)
	
Neural network
------------------------------------
We can now train the neural network and plot the data::

	print("\n Training set size:", len(RCN_array))
	x = RCN_array.reshape(-1,1)
	y = yield_c_array.reshape(-1,1)

	'''
	plot of the data
	'''
	import matplotlib.pyplot as plt
	fig, ax = plt.subplots(figsize=(10, 6))

	sorted_indices = np.argsort(RCN_array)
	sorted_yield = yield_c_array[sorted_indices]
	sorted_RCN = RCN_array[sorted_indices]

	ax.set_ylim([80, 220])
	ax.set_yticks([80, 100, 120, 140, 160, 180, 200, 220])
	ax.set_xlim([50, 100])
	ax.set_xticks([50, 60, 70, 80, 90, 100])

	plt.plot(sorted_RCN, sorted_yield, marker='o')  # Plot x and y using a line and markers
	plt.xlabel('RCN')  # Add x-axis label
	plt.ylabel('corn yield')  # Add y-axis label
	plt.grid(True)  # Add gridlines
	plt.show()  # Display the plot


	'''
	plot of the NN approximate
	'''
	fig, ax = plt.subplots(figsize=(10, 6))

	NN_test_x = np.arange(50,100,1).reshape(-1,1)
	NN_test_x = x_transform.transform(NN_test_x)
	NN_test_y = model(NN_test_x)
	NN_test_x = x_transform.inverse_transform(NN_test_x)
	NN_test_y = y_transform.inverse_transform(NN_test_y)

	ax.set_ylim([80, 220])
	ax.set_yticks([80, 100, 120, 140, 160, 180, 200, 220])
	ax.set_xlim([50, 100])
	ax.set_xticks([50, 60, 70, 80, 90, 100])

	plt.plot(NN_test_x, NN_test_y, marker='o', color = 'red')  # Plot x and y using a line and markers
	plt.title('NN Test')  # Add a title
	plt.xlabel('RCN')  # Add x-axis label
	plt.ylabel('corn yield')  # Add y-axis label
	plt.grid(True)  # Add gridlines
	plt.show()  # Display the plot


	'''
	User RCN Input
	'''
	user_RCN = float(input("\n Amount of commercial Nitrogen (kg/ha) : "))
		
	while user_RCN < min(RCN_array) or user_RCN > max(RCN_array):
		print(f"Select between {min(RCN_array)} and {max(RCN_array)}")
		user_RCN = float(input("Amount of commercial Nitrogen (kg/ha) : "))

		
	user_RCN = np.array([user_RCN], dtype=int).reshape(-1,1)
	user_RCN = x_transform.transform(user_RCN)

	user_yield = model(user_RCN)
	user_yield = y_transform.inverse_transform(user_yield)

	print(f"\n Approximated corn yield: {user_yield[0][0]:.2f} bu/acre")