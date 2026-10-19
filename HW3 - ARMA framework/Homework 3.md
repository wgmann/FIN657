# Modeling and forecasting inventory growth

Your goal in this assignment is to compare AR(1), MA(1), and ARMA(1,1) models for monthly business inventory growth. You will use the fitted results and a forecast evaluation to recommend an approach for this series.

You can use our class slides and code as a template and guide on how to do the analysis. The most relevant files for this assignment are the ones on **ARMA part 2 - Model selection** and **ARMA part 3 - Forecasting**. The stationarity material from Homework 2 is also useful. You are also encouraged to use AI tools to help you, and they can refer to the course files as well.

## General instructions

Perform this analysis as a self-contained Python script or a Jupyter notebook that you upload back to Canvas as *just a single file*. My first step will be to try to get your code running on my end, and then I will grade the output. So check that the entire script or notebook runs, start to finish, before uploading your file back to Canvas! 

Again, you should just upload a single self-contained script with no other supporting files. The script should include the steps to download any data you use from FRED or WRDS. However, the code you submit should *not* include any database usernames, passwords, or API keys. These should be read into the script from externally, like in the examples we did in class. I will supply my own usernames and passwords when I run your code.

## 1. Obtain and prepare the data

Download **Total Business Inventories**, FRED mnemonic **BUSINV**, through the **FRED API**, from **January 1, 1993 through December 31, 2015**. The series is seasonally adjusted and reported in millions of dollars.

1. Have your script print the first date, last date, and observation count. Confirm that observations are monthly and that there are no missing months or values within the requested window.
2. Plot inventory levels. Calculate monthly log growth as the difference between consecutive natural logarithms of inventory levels, and plot the resulting growth series with clear labels. Use decimal log growth throughout the analysis.
3. Separate the **growth series** into training data ending in **December 2010** and evaluation data covering **January 2011–December 2015**. Report the dates and counts of both samples. Calculate growth before splitting: January 2011 growth must use the December 2010 inventory level.
4. Apply ADF and KPSS tests to the training growth series, using a constant but no deterministic time trend, AIC-based automatic lag selection for ADF, and automatic lag selection for KPSS. State each null hypothesis and briefly explain whether the results support proceeding with a stationary model of inventory growth.

## 2. Fit and compare the models, and make a recommendation

Use the Box–Jenkins approach from class to compare **AR(1), MA(1), and ARMA(1,1)**, using only the *training* sample. Display all the diagnostics that we used in class as part of this approach. Based only on this evidence from the training sample, recommend one of the three models as most appropriate for the data.

## 3. Evaluation

For each of the three models you considered in Question 2,

1. Generate 60 monthly forecasts of log inventory growth for **January 2011–December 2015**, all made as of the end of **December 2010**. Do not update the forecasts or re-estimate the models as evaluation observations become available.
2. Plot the forecast mean and 95% **prediction intervals**, together with the training data that came before the forecasts. Use one figure per model, with the same date range and axis scales. 
3. Describe any differences across the models in how quickly each forecast approaches its fitted long-run mean. How are these differences connected to the structure of the model being estimated? In general, which one seems to perform the best in the evaluation period?
4. Report the RMSE for each model over the same 60 evaluation months. Confirm that forecasts and actual observations align on the same dates, with no missing values entering RMSE. Which model has the lowest RMSE, and how much lower is it than the alternatives?