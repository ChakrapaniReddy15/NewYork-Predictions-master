# NewYork-Predictions-master
ntroduction
In this competition, Kaggle is challenging you to build a model that predicts the total ride duration of taxi trips in New York City. Your primary dataset is one released by the NYC Taxi and Limousine Commission, which includes pickup time, geo-coordinates, number of passengers, and several other variables.

2. Getting to Know the Data¶

2.1. Reading the Data
It is very straightforward to read tabular data into Python with Pandas library's read_csv() function. The function can conveniently convert time information into datetime objects by setting infer_datetime_format parameter to True and providing the names of the time information columns to parse_dates parameter. The "id" column (unique trip identifier) can be set as the index of train and test DataFrames since it has no predictive value.


As it can be seen from the above output, there are two columns less in the testing data when compared to training data. Those columns are "dropoff_datetime" and "trip_duration". The columns are missing from the testing data because they would give out the answers since they are to be predicted in this competition. More specifically, the aim is to predict the duration of each taxi trip, which is calculated by subtracting pick-up time from drop-off time.

In the below Markdown table, there is a description of each of the 11 variables:

Variable name	Variable description
id	A unique identifier for each trip
vendor_id	A code indicating the provider associated with the trip record
pickup_datetime	Date and time when the meter was engaged
dropoff_datetime	Date and time when the meter was disengaged
passenger_count	The number of passengers in the vehicle (driver entered value)
pickup_longitude	The longitude where the meter was engaged
pickup_latitude	The latitude where the meter was engaged
dropoff_longitude	The longitude where the meter was disengaged
dropoff_latitude	The latitude where the meter was disengaged
store_and_fwd_flag	This flag indicates whether the trip record was held in vehicle memory before sending to the vendor because the vehicle did not have a connection to the server.
Y=store and forward; N=not a store and forward trip
trip_duration	Duration of the trip in seconds
Next, let's show the beginning of each dataset in an interactive Bokeh table and visualize the number of examples in each dataset with a bar chart.

2. Data Preparation

My initial thoughts on this is that from the pickup_datetime field we should be extracting hour of day and day of week and day of month. This seems logical since peak hour traffic and weekend vs non-weekend days could have a mayor effect on travel times. Similar to this there could be seasonality in the travel times to be observed between the different months of the year. Think, for example, of the effect New York's winter months might have on travel time. If the roads are wet/icy you're less likely to drive very fast, so regardless of which route you're taking, you'll take a bit longer to reach your destination.

An interesting variable and its effects to potentially explore is the `passenger_count`. One might argue that an increased number of passengers could result in a few scenarios. It could, for example, result in increased stops along the way, which ultimately extends the time from A to B (note how we're not given the number of passengers at the start and end of the trip - my thinking is that if we were to know the start and end number of passengers in a trip, the data between drops would be split into separate entries, with the last entry in a collection of rows ending with 0 passengers). Also, from a purely physical point of view, the higher the number of passengers, the heavier the vehicle, the slower the vehicle might move. Although I'm fairly certain the effect of this is neglible.

Same with `vendor` and `store_and_fwd_flag`. It is possible that looking into it, we find that there's a difference between vendor 1 and vendor 2, and that either one of the two might be corrupting the "shortest route information" because the vendor's employees are less efficient in finding the best routes through New York. This, however, I also find highly unlikely, but it is an alternative to explore (at least look into it and rule it out difinitively). As for `store_and_fwd_flag` - not having a connection to the server for a particular route to be indicative of a few things. For example, If upon inspection it is discovered that there is a strong correlation between slow trip times and server disconnects, it could be used as another feature in the training model to predict what time a particular route could take.

Getting down to it however, the `_lattitude` and `_longitute` variables is where I think the most value lies. The options here are to cluster them into "neighborhoods" as well as find the distance and directions between coordinates.

2.1 Descriptive Statistics
For analyzing the data, we use descriptive statistics and find the mean, standard deviation of the variables, and the maximum and minimum number4 of the variables for analyzing.
We check for the outliers in the data and remove them for the better understanding of the data. the outliers may skew the data.


3. Visualizing Raw Variables
In this chapter, different visualizations are created for each of the variables using matplotlib, bokeh and folium packages.

3.1. Categorical and Ordinal Variables
The datasets contain one ordinal variable "passenger_count" and two categorical variables "vendor_id" and "store_and_fwd_flag". Let's visualize the distributions of their values in training and testing data by bar charts.

4. Feature Engineering
Feature engineering is the process of using domain knowledge of the data to create features that make machine learning algorithms work.
The features in your data will directly influence the predictive models you use and the results you can achieve. You can say that the better the features that you prepare and choose, the better the results you will achieve. In this problem, we create extra features for the date time format variables.

5. Label Encoder and Standard Scaler
Label encoder: Encodes labels with value between 0 and n_classes-1

Standardize features by removing the mean and scaling to unit variance. Centering and scaling happen independently on each feature by computing the relevant statistics on the samples in the training set. Mean and standard deviation are then stored to be used on later data using the transform method. Standardization of a dataset is a common requirement for many machine learning estimators: they might behave badly if the individual feature do not more or less look like standard normally distributed data.

6. XGBoost - Training the Model and Testing the Accuracy
As mentioned you can play with the different parameters of the XGBoost algorithm to tweak the model's outcome. So below is a short, but very nice, way of itterating through model parameters to tweak the model. So it's implementation is simple: just uncomment the code and run the kernel. Again, refer to the [documentation for XGBoost](http://xgboost.readthedocs.io/en/latest/python/python_intro.html "XGBoost Documentation") to understand what each parameter does and how it alters the behaviour of the training process.
