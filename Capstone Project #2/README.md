![cover_photo](https://github.com/phatakshaunak/Springboard-Data-Science/blob/master/Capstone%20Project%20%232/Readme%20Files/air_pollution_getty_images.jpg)
# **Prediction of PM2.5 in Lucknow, India**
*Air pollution is a severe health hazard causing 8.8 million deaths globally. It is a significant problem in India where 21 out of the 30 most polluted cities in the world are from India. Among types of air pollutants, particulate matter is very harmful due to its ability to enter the lungs and bloodstream owing to its microscopic size and causing respiratory & health disease. For example, 0.98 out of 1.67 million deaths in India were attributed to particulate pollution in 2019.
This project aims to create a hourly forecasting model for particulate matter (PM2.5) in Lucknow, India. The project can be further extended into an application that can provide real time forecasts warning people about hazardous conditions and providing necessary recommendations.*
  
## 1. Data
The [data](https://github.com/phatakshaunak/Springboard-Data-Science/tree/master/Capstone%20Project%20%232/Data_CPCB_Lucknow/Cleaned%20Data%20) for the project was obtained from two sources:
  * Pollution data from the Central Pollution Control Board, India (CPCB) website
  * Temperature data from the National Oceanic and Atmospheric Administration (NOAA) website
  
    
## 2. Data Wrangling and EDA
Some data cleaning steps are explained as follows:
  * Merging pollution and temperature datasets after standardizing column names, removing unnecessary columns and converting data from string to numeric format
  * Rows with missing data for PM2.5 were dropped while other column features were imputed by their means
  * Corrupted data indicated by '999' or negative entries was also imputed with column means
  * Outlier treatment such as bounding by 3 standard deviations of percentile capping significantly changed the data distribution.  
      * These methods were not applied and most of the data set was left intact. 
      * Certain outliers (~2400 micrograms/cubic metre) as seen in the figure for PM2.5 below were removed. A similar process was followed for sulphur dioxide.
      <p align="center">
      <img src = "https://github.com/phatakshaunak/Springboard-Data-Science/blob/master/Capstone%20Project%20%232/Readme%20Files/PM_25.png"></p>
  * Time features such as month, day of week, hour of day were extracted from the date time index.
      <p align="center"><img src = "https://github.com/phatakshaunak/Springboard-Data-Science/blob/master/Capstone%20Project%20%232/Readme%20Files/month_pm.png">
      </p>
  * As seen in the images for month and hour, certain cyclic variations were observed for the average values of particulate matter (PM2.5)
      <p align="center"><img src = "https://github.com/phatakshaunak/Springboard-Data-Science/blob/master/Capstone%20Project%20%232/Readme%20Files/hourly_pm.jpg">
      </p>
  * Heat map for feature correlations showed the following observations:
      *	Negative correlations for temperature and wind speed with PM2.5
      * Slight positive correlations with other pollutant features such as Sulphur dioxide, carbon monoxide and nitrogen-based compounds.
      * Multi collinearity between some pollutant features such as carbon monoxide and nitrogen compounds as well for pairs of nitrogen compounds
      <p align="center"><img src = "https://github.com/phatakshaunak/Springboard-Data-Science/blob/master/Capstone%20Project%20%232/Readme%20Files/Correlation_Map.jpg">
      </p>
## 3. Modeling and Results
Two modeling tasks were undertaken:
  * Firstly, performances were compared between random train/test splits versus time series split 
    A random train/test split is not applicable for a time series due to the inherent order in the data. This would potentially cause data leakge using a model  
    trained on future data to predict past data. This exercise however was undertaken to compare performance between these two methods.
    <p align="center"><img src = "https://github.com/phatakshaunak/Springboard-Data-Science/blob/master/Capstone%20Project%20%232/Readme%20Files/random_time_results.png">
      </p>
  * The results showed better performance for a random split when no lag features were observed whereas the performance was similar with lag features.
  * Next, model performances were compared while applying walk forward validation. In this method, each subsequent test split was added to the training set to make predictions for further test sets whose size was kept contant. This can be explained clearly in the following [schematic] (https://www.researchgate.net/publication/341618027_Forecasting_Sales_of_Truck_Components_A_Machine_Learning_Approach)
   <p align="center"><img src = "https://github.com/phatakshaunak/Springboard-Data-Science/blob/master/Capstone%20Project%20%232/Readme%20Files/forward_chaining.png"></p>
  * Further, features were tested systematically for their importance. It was observed that model performance did not improve beyond adding the first lag feature as seen below.<p align="center"><img src = "https://github.com/phatakshaunak/Springboard-Data-Science/blob/master/Capstone%20Project%20%232/Readme%20Files/lag_var.png"></p>
  * Apart from the 1st lag for PM2.5 and time features, weather and pollutant features did not further improve modeling performance
  * As seen in the model metrics comparison, the results are similar across all models
      <p align="center"><img src = "https://github.com/phatakshaunak/Springboard-Data-Science/blob/master/Capstone%20Project%20%232/Readme%20Files/walk_forward_metrics.png">
      </p>
  * This forward chaining approach can be extended to train new models for all new predictions instead of a fixed number of sequential splits. This approach though can be very time consuming. Following is such an example applying light gradient boost to make 200 hourly predictions.
      <p align="center"><img src = "https://github.com/phatakshaunak/Springboard-Data-Science/blob/master/Capstone%20Project%20%232/Readme%20Files/walk_forward_results.png"></p>
 ## 4. Final Conclusion
  * This project aimed at creating a forecasting model for particulate matter (PM2.5)
  * It was observed that the 1st lag feature in addition to transformed time features were sufficient to maximize performance
  * Weather and pollutant features did not further improve performance
  * All models tested had a very similar performance with the features being more important than the type of model used
  * This model could be extended further to create a real time application warning the public about potential high pollution days
