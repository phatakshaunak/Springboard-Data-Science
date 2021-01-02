![cover_photo](https://github.com/phatakshaunak/Springboard-Data-Science/blob/master/Capstone%20Project%20%232/Readme%20Files/air_pollution_getty_images.jpg)
# **Prediction of PM2.5 in Lucknow, India**
*Air pollution is a severe health hazard causing 8.8 million deaths globally. It is a significant problem in India where 21 out of the 30 most polluted cities in the world are from India. Among types of air pollutants, particulate matter is very harmful due to its ability to enter the lungs and bloodstream owing to its microscopic size and causing respiratory & health disease. For example, 0.98 out of 1.67 million deaths in India were attributed to particulate pollution in 2019.
This project aims to create a hourly forecasting model for particulate matter (PM2.5) in Lucknow, India. The project can be further extended into an application that can provide real time forecasts warning people about hazardous conditions and providing necessary recommendations.*
  
## 1. Data
The data for the project was obtained from two sources:
  * Pollution data from the Central Pollution Control Board, India (CPCB) website
  * Temperature data from the National Oceanic and Atmospheric Administration (NOAA) website
    
## 2. Data Wrangling and EDA
Some data cleaning steps are explained as follows:
  * Merging pollution and temperature datasets after standardizing column names, removing unnecessary columns and converting data from string to numeric format
  * Rows with missing data for PM2.5 were dropped while other column features were imputed by their means
  * Corrupted data indicated by '999' or negative entries was also imputed with column means
  * Outlier treatment such as bounding by 3 standard deviations of percentile capping significantly changed the data distribution.  
      * These methods were not applied and most of the data set was left intact. 
      * Certain outliers (~2400 micrograms/cubic metre) as seen in the figure for PM2.5 below were removed. A similar process was followed for sulphur dioxide.\
      **Figure 1 Time series for PM2.5**
      <p align="center"> 
      <img src = "https://github.com/phatakshaunak/Springboard-Data-Science/blob/master/Capstone%20Project%20%232/Readme%20Files/PM_25.png".
      </p>
      
        ![](https://github.com/phatakshaunak/Springboard-Data-Science/blob/master/Capstone%20Project%20%232/Readme%20Files/PM_25.png)
  * Time features such as month, day of week, hour of day were extracted from the date time index.
  * As seen below for month and hour, certain cyclic variations were observed for the average values of particulate matter (PM2.5)
    Figure 2 Bar Chart for average monthly PM2.5 concentration
    ![](https://github.com/phatakshaunak/Springboard-Data-Science/blob/master/Capstone%20Project%20%232/Readme%20Files/month_pm.png)
    Figure 3 Bar Chart for average hourly PM2.5 concentration
    ![](https://github.com/phatakshaunak/Springboard-Data-Science/blob/master/Capstone%20Project%20%232/Readme%20Files/hourly_pm.jpg)
