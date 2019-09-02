#
# Springboard Data Science Capstone Project 1 – Predicting the Price of an Uber/Lyft Ride

[Slide Deck](https://github.com/dgokalga/Springboard-Data-Science/blob/master/Capstone-1/Capstone1_Slides.pdf)

[Blog Post](https://github.com/dgokalga/Springboard-Data-Science/blob/master/Capstone-1/Capstone1_blog.pdf)

[Project Report](https://github.com/dgokalga/Springboard-Data-Science/blob/master/Capstone-1/Capstone1_Final_Report.pdf)

## **Data Problem**

Uber and Lyft have dominated the ride-sharing industry in recent times, with combined over a 100 millions active users around the world. You can book rides easily from your phone, selecting the pickup and dropoff location, selecting the type of ride (whether it&#39;s a four-seater, six-seater, luxury car, SUV, etc.), and tracking the car to see when the driver arrived. This beats having to call a taxi service, and not knowing when the taxi would arrive from pickup. But a major factor that drove demand for Uber and Lyft over taxi services has been the price of the ride.

According to [RideGuru](https://ride.guru/), Uber and Lyft provide a significantly cheaper alternative to standard yellow taxis/cabs in many major cities in the United States. But what exactly goes into determining the price of these rides by Uber and Lyft? Is it just distance? Does the time of pickup matter? Does the weather affect the demand of rides, leading to an increase in price? I employed machine learning techniques on previous ride data to determine what exactly goes into Uber and Lyft&#39;s pricing models to estimate prices for future rides and give some interesting insights for you, the rider, to take into consideration when using these two ride-sharing apps.

## **Potential Clients**

My target audiences are:

1. Riders that frequently or casually use Uber/Lyft, who want an estimate of what a ride will cost.
2. Competing ride-service applications, who could determine estimates for rides provided by Uber/Lyft, and provide discounts and incentives accordingly, to drive demand for their application

## **Dataset Description**

1. The ride data, from [Kaggle](https://www.kaggle.com/ravi72munde/uber-lyft-cab-prices), was collected using Uber and Lyft API queries (ride data collected in time intervals of roughly 1 hour, 30 minutes apart), containing 10 features including: ride application used (Uber, Lyft), distance between pickup and dropoff, timestamp of ride, pickup location of ride and destination of ride located in twelve areas around Boston (five main neighborhds, and seven smaller areas as seen by the map below), price of ride, surge multiplier of ride (over much price was increased), specific type of ride (e.g. Uber Black, Lyft Lux XL), and corresponding id is included. There were more than 690,000 ride data instances recorded for this dataset.

2. Weather data, also from [Kaggle](https://www.kaggle.com/ravi72munde/uber-lyft-cab-prices), contained shared features with the ride dataset such as the location of the ride pickup, between November 26 to December 18, 2018, contains features such as rain, clouds, humidity, wind, and pressure measurements. An additional dataset, to measure the effect of a sports game to ride prices, was created containing sports data, with features such as start-time, stop-time, sports team (Celtics or Bruins), and location of game (North Station, as TD Garden is located just above it), in order to be combined with the ride and weather data.

Wrangling Steps:

1. The rides dataset contained approximately 50,000 records, or less than 10% of the dataset, with missing price feature, all from an Uber ride type: Taxi. Because there is not any recorded price for the Taxi ride type records, these ride instances were removed from the dataframe.

2. Weather data is merged onto the rideshare data where the weather measurement was within one hour of the ride start time. Any ride records with missing weather data, amounting to 60,000 records or another 10% of the total data set, were removed. Details regarding implementation of merging ride to weather data can be found in this [IPython](https://github.com/dgokalga/Springboard-Data-Science/blob/master/Capstone-1/AlignWeathertoRides.ipynb) notebook.

3. A dummy feature column was created representing &quot;Sports Occurrence&quot;, where a value is assigned 1 if a ride occurred within 2 hours of a sports game starting or finishing, and 0 otherwise.  Details regarding implementation of creating sports occurrence dummy column can be found in this [IPython](https://github.com/dgokalga/Springboard-Data-Science/blob/master/Capstone-1/AddSportsData.ipynb) notebook.

## **Data Exploration Insights**

To summarize the findings from the exploratory data analysis of the Uber/Lyft ride data:

1. **Ride-Types:** On average, four of the five ride-types offered by Uber are more expensive per mile than their Lyft counterparts: UberPool over Shared, UberX or Lyft, UberXL or LyftXL, Uber Black SUV over Lyft Black XL. The fifth ride-type, Lux Black, is on average more expensive than Uber Black.

2. **Rush Hour vs. Regular Hours vs. Late-Early Hours:** Uber rides, on average, are more expensive during late night to early morning hours (12:00-5:59 A.M), than regular hours and rush hours. For Lyft, however, rides are more expensive during regular hours (10:00 A.M. to 3:59 P.M. and 8:00-11:59 P.M) than rush hours and late-early hours.

3. **Surge-Multiplied:** More rides are surge-multiplied \&gt; 1.0 during late-early hours over daytime hours, and more rides are surged during the weekend (Fri, Sat, Sun) than the weekdays.

4. **Sports Occurrence:** Rides, on average, cost more per mile during and after a sports game, for rides going to and coming from the sports arena. More rides are surged \&gt; 1.0 during and after a sports game.

5. **Weather Factors:** Rides, on average, cost less per mile during rainy and cloudy weather than rides under clear weather. This could be attributed to more people remaining indoors during rainy and cold weather, decreasing the demand for ride-services and travel in general.

Details on the implementation of the data analysis and visualization can be found in this [IPython](https://github.com/dgokalga/Springboard-Data-Science/blob/master/Capstone-1/DataStorytelling.ipynb) notebook.

## **Predictive Modeling Insights**

To summarize the findings from the predictive modeling of the Uber/Lyft ride data:

1. I built a model capable of predicting ride price to within $1.40 (9.5%) of the actual price on average. This can help riders or competitors better understand Uber and Lyft&#39;s pricing models

2. To build the model, tried a variety of regression techniques, including Linear, Lasso, Ridge, and Log-Linear (log-transformation of dependent variable). Ultimately, Log-Linear regression produced the most accurate model because of the non-linear relationship between ride price and the independent features.

3. The top features, as indicated by the coefficient values from normal linear, lasso, and ridge regression, were ride-type, surge-multiplier, cab-type, distance, and rain. Lasso, as opposed to ridge and normal regression, minimized the rain&#39;s coefficient to 0, eliminating it from the model, which may attribute to its lower precision in predicting ride prices, as rain is a significant factor according to its coefficient values in normal and ridge regression.

4. Humidity and Sports Occurrence are features not relevant to the prediction of price, as they have been found to be not statistically significantly (.05 significance).

Details on the implementation of the modeling and analysis can be found in this [IPython](https://github.com/dgokalga/Springboard-Data-Science/blob/master/Capstone-1/UberLyft_Regression.ipynb) notebook.

## **Future Work**

1. In terms of the data, additional features could be added to provide better understanding of the factors that influence the price for an Uber or Lyft ride. These features include traffic accident data, latitude and longitudinal features representing the exact location to where the ride was picked up or dropped off, event occurrence (similar to sport occurrence) data occurring near ride pickup and dropoff, or Uber and Lyft additional fees or cancelation fees data if they apply to the rides. The ride and weather data should also be expanded outside Boston, to other major cities such as San Francisco and New York City to see consistency in how ride price is determined, and these could be compared to ride data outside the country, to see if the ride-service applications are similar across the world.

2. In terms of the modeling, various other complex regression algorithms should be implemented such as regression trees, to capture some of the non-linearity of the explanatory variables to the target variable, random forest (an ensemble of regression trees), or gradient boosting (ensemble of weak prediction models). These algorithms could provide a more precise model for unseen data over the normal linear regression model, and its regularization variates.
