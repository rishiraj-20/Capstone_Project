# Capstone_Project

# Project Overview: Dynamic Parking Price Prediction System

# Objective

The goal of this project was to build an intelligent, responsive, and adaptable system to dynamically calculate parking prices in urban environments. The pricing was to be driven by real-time factors like occupancy, traffic, queue length, time, and vehicle type — ensuring fairness for users and optimization for lot managers.

Our task was to create three distinct but connected models:
 
1. Model_1  :  This is a simple trend-based prediction which predicts the price based on the trends of the occupancy in the parking lot.
2. Model_2  :  This is a multi-factor demand based model which looks on the multiple factors instead of just going for the trends of occupancy, it calculates the price based on the occupancy trends, timing, traffic, type of vehicle, and so on.
3. Pathway_Model  :  This one predicts/streams the dynamic pricing based on the demand, which is calculated using multiple factors.

# Tech Stack Used

1. Python 3.11+
2. NumPy
3. Pandas
4. Pathway (for streaming model)
5. Google Colab for execution

# Models Overview :

# 1. Model_1 : 
This is the simple baseline model in which we calculated the dynamic price by the trend of the occupancy in the parking lot.
# Trend : The trend was calculated based on the addition of 3 "change in occupancies" , this trend was further used to calculate the momentum , 
So, if we have to calculate the momentum at a time t(r) , then , 
occupancy at time t(r) = o(r)
# Momentum = [ [ { o(r) - o(r-1) } + { o(r-1) - o(r-2) } + { o(r-2) - o(r-3) } ] / capacity ]
And, the final Pricing was calculated by the linear and non linear( tanh ) addition of this momentum to the base price ( $10 )
We also did the visualization of the model using Bokeh Plot...

# 2. Model_2 : 
This is the smarter demand based model in which we estimated the dynamic pricing by multiple factors, like, Trends of occupancy, Timing, Vehicle type, Queue length, Is Special Day, Traffic conditions...
1. We have calculated the Momentum ( Trend ) in the same way as we did in Model_1. 
2. After some observation and by the stats of the given dataset , we analyzed some time frames have a lot of occupancy , so we adjusted the prices based on the timings, i.e., increased the price during the heavy demands to boost up the revenue and, reduced the price during the cold hours to increase people's interest to use the parking lot ultimately leading to more revenue..
3. We also given the value to the queue length...
4. The priority was also given to the traffic conditions nearby the area, i.e., if there is high traffic, then, people will not want to park their vehicles in that lot, so the price should be reduced to engage more customers, and increasing the price in case of low traffic to improve the revenue....
5. And, if there is any special day on that date, then the price should be increased due to heavy demand.....
6. We also categorised the prices based on the type of vehicle, i.e., more pricing more for the trucks, moderate for the cars and low pricing for bikes......

# We calculated the final raw price by the linear and non linear ( tanh, log ) additions of all the above factors.
Then , we normalise that raw price and clamp that normalised price between $5 and $20 ( as per the problem statement ) ,
and , applies a cap of ( (+/-) $3 ) per 30 minutes to negotiate with the sharp increament or decreament in the prices......
# And, that's how we calculates the final dynamic pricing of each 14 parking lots based on the demand.

# 3. Pathway_Model :
This is the linear demand based model which is designed to stream the dynamic pricing ( results ) using the Pathway.
* I've taken the help of documentaries and chatgpt to implement this model.
* This model also predicts the price by taking in considerations multiple factors, like we used in Model_2, just the difference is that i was not able to put the momentum based logic in this model.
