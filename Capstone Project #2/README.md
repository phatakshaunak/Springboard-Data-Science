![](https://github.com/phatakshaunak/Springboard-Data-Science/blob/master/Capstone%20Project%20%232/Readme%20Files/air_pollution_getty_images.jpg)
# **Prediction of PM2.5 in Lucknow, India**
*Air pollution is a severe health hazard causing 8.8 million deaths globally. It is a significant problem in India where 21 out of the 30 most polluted cities in the world are from India. Among types of air pollutants, particulate matter is very harmful due to its ability to enter the lungs and bloodstream owing to its microscopic size and causing respiratory & health disease. For example, 0.98 out of 1.67 million deaths in India were attributed to particulate pollution in 2019.
This project aims to create an hourly forecasting model for particulate matter (PM2.5) in Lucknow, India. The project can be further extended into an application that can provide real time forecasts warning people about hazardous conditions and providing necessary recommendations.*\
\
The notebook used for this work can be located [here](https://github.com/phatakshaunak/Springboard-Data-Science/blob/master/Capstone%20Project%20%232/Notebooks/Capstone_Project_PM_25_Prediction.ipynb)
## 1. Data
The [data](https://github.com/phatakshaunak/Springboard-Data-Science/tree/master/Capstone%20Project%20%232/Data_CPCB_Lucknow/Cleaned%20Data%20) for the project was obtained from two sources:
  * Pollution data from the Central Pollution Control Board, India (CPCB) website
  * Temperature data from the National Oceanic and Atmospheric Administration (NOAA) website
  
    
## 2. Data Wrangling and EDA
Some data cleaning steps are detailed as follows:
  * Merging pollution and temperature datasets after standardizing column names, removing unnecessary columns and converting data from string to numeric format
  * Dropping rows with missing data for PM2.5 while imputing other column features by their means
  * Imputing corrupted data indicated by '999' or negative entries by their column means
  * Outlier treatment such as bounding by 3 standard deviations of percentile capping significantly changed the data distribution.  
      * These methods were not applied and most of the data set was left intact due to lack of enough domain knowledge
      * Certain outliers (~2400 micrograms/cubic metre) as seen in the figure for PM2.5 below were removed. A similar process was followed for sulphur dioxide.
      ![](https://github.com/phatakshaunak/Springboard-Data-Science/blob/master/Capstone%20Project%20%232/Readme%20Files/PM_25.png)<p align="center">
  * Time features such as month, day of week, hour of day were extracted from the date time index.
      ![](https://github.com/phatakshaunak/Springboard-Data-Science/blob/master/Capstone%20Project%20%232/Readme%20Files/month_pm.png)
  * As seen in the images for month and hour, certain cyclic variations were observed for the average values of particulate matter (PM2.5)
      ![](https://github.com/phatakshaunak/Springboard-Data-Science/blob/master/Capstone%20Project%20%232/Readme%20Files/hourly_pm.jpg)
  * Cyclic features such as month, hour were transformed into sine and cosine components to retain their cyclical information, ex. ensuring 11 pm and 12 am were mapped close to each other.
  * Heat map for feature correlations showed the following observations:
      *	Negative correlations for temperature and wind speed with PM2.5
      * Slight positive correlations for PM2.5 with other pollutant features such as sulphur dioxide, carbon monoxide and nitrogen compounds.
      * Multi collinearity between some pollutant features such as carbon monoxide and nitrogen compounds as well for pairs of nitrogen compounds
      ![](https://github.com/phatakshaunak/Springboard-Data-Science/blob/master/Capstone%20Project%20%232/Readme%20Files/Correlation_Map.jpg)
## 3. Modeling and Results
Two modeling tasks were undertaken:
  * Firstly, performances were compared between random train/test splits versus time series split 
    A random train/test split is not applicable for a time series due to the inherent order in the data. It can cause data leakage leading to a model trained on future data be used for predicting past data. This exercise was undertaken simply to compare performances between the two methods and check if random splitting outperformed sequential splits.
    ![](https://github.com/phatakshaunak/Springboard-Data-Science/blob/master/Capstone%20Project%20%232/Readme%20Files/random_results.png)
  * The results showed better performance for a random split when no lag features were observed whereas the performance was similar with lag features.
  * Next, model performances were compared while applying walk forward validation. In this method, each subsequent test split was added to the training set to make predictions for further test sets whose size was kept contant.
   ![](https://github.com/phatakshaunak/Springboard-Data-Science/blob/master/Capstone%20Project%20%232/Readme%20Files/forward_chaining.png)\
  This method can be explained in the [schematic](https://www.researchgate.net/publication/341618027_Forecasting_Sales_of_Truck_Components_A_Machine_Learning_Approach) above 
  * Further, features were tested systematically to check their usefulness for the model. It was observed that the first lag for PM.5 was sufficient  and that performance did not improve beyond adding the first lag feature as seen below.
![](https://github.com/phatakshaunak/Springboard-Data-Science/blob/master/Capstone%20Project%20%232/Readme%20Files/lag_var.png)
  * Apart from the 1st lag, the transformed time features provided a slight improvement in performance whereas weather and other pollutant features did not improve the results
  * Following are the model comparison results when applying walk forward validation
![](https://github.com/phatakshaunak/Springboard-Data-Science/blob/master/Capstone%20Project%20%232/Readme%20Files/walk_forward_metric.png)   
  * As seen in the table above, the average test RMSE and MAE values are similar for all models indicating that the right features played a more important part than the type of model
  * All the modeling tasks detailed above were conducted with three train/test splits (either random or sequential) and 3 splits for hyper-parameter tuning applying nested cross-validation. Nested cross-validation eliminates the problem of arbitrarily choosing a test set to evaluate a model
  * Forward chaining can be taken a step further by training a model for each new prediction instead of a fixed number of splits. Although it is useful using all the available data to make a new prediction, this approach can be time-consuming. An example for 200 predictions with Light Gradient Boosting is shown below:  
  ![](https://github.com/phatakshaunak/Springboard-Data-Science/blob/master/Capstone%20Project%20%232/Readme%20Files/walk_forward_results.png)  
  * As seen in the plots above, the 1st lag feature for PM2.5 has the most importance. A rolling mean (previous 2 values) feature although added to the model did not significantly change the RMSE/MAE.

  ## 4. Conclusions
  
  * This project aimed at applying supervised learning to a multivariate time series to create hourly forecasts for PM2.5 in Lucknow, India
  * Random splitting outperformed time based splits when no lag features were used whereas the performance was similar when lag feeatures were added to the model
  * Results showed that the 1st lag feature for PM2.5 contributed the most to model performance
  * Apart from transformed time features, weather and pollutant variables were not useful in improving the model
  * Results were similar across all models tested indicating that choosing the right features was more important than the type of model
  
  ## 5. References
  
  1. https://www.theguardian.com/environment/2019/mar/12/air-pollution-deaths-are-double-previous-estimates-finds-research
  1. https://ourworldindata.org/air-pollution
  1. https://www.epa.gov/pm-pollution/particulate-matter-pm-basics#PM
  1. https://www.thelancet.com/journals/lanplh/article/PIIS2542-5196(20)30298-9/fulltext
  1. https://www.researchgate.net/publication/341618027_Forecasting_Sales_of_Truck_Components_A_Machine_Learning_Approach
  1. https://towardsdatascience.com/time-series-nested-cross-validation-76adba623eb9
  1. https://machinelearningmastery.com/backtest-machine-learning-models-time-series-forecasting
  1. https://medium.com/mongolian-data-stories/ulaanbaatar-air-pollution-part-1-35e17c83f70b
  1. https://machinelearningmastery.com/convert-time-series-supervised-learning-problem-python/
  
