# Forecasting payrolls with exponential smoothing

Your goal in this assignment is to compare smoothing methods for payroll series with different trend and seasonal features. You will use the fitted results and a short forecast evaluation to recommend an approach for each series.

You can use our class slides and code as a template and guide on how to do the analysis. The most relevant files for this assignment are the ones on **Moving-average smoothing** and **Exponential smoothing**. You are also encouraged to use AI tools to help you, and they can refer to the course files as well.

## General instructions

Perform this analysis as a self-contained Python script or a Jupyter notebook that you upload back to Canvas as *just a single file*. My first step will be to try to get your code running on my end, and then I will grade the output. So check that the entire script or notebook runs, start to finish, before uploading your file back to Canvas! 

Again, you should just upload a single self-contained script with no other supporting files. The script should include the steps to download any data you use from FRED or WRDS. However, the code you submit should *not* include any database usernames, passwords, or API keys. These should be read into the script from externally, like in the examples we did in class. I will supply my own usernames and passwords when I run your code.

## 1. Obtain and prepare the data

Download monthly nonfarm payrolls through the **FRED API**, from **April 1, 2020 through December 31, 2025**:

- **PAYEMS:** seasonally adjusted payrolls.
- **PAYNSA:** non-seasonally-adjusted payrolls.

The raw data are in thousands of persons. Divide by 1000 to restate them in millions of persons.

1. Have your script print the first date, last date, and observation count for each of these series.
2. Plot the two series with clear labels. Briefly describe the trend and seasonal features relevant to forecasting.
3. Separate the data into **April 2020–December 2024** as the training sample, and **January–December 2025** as the evaluation sample. Report the dates and counts of both the training and evaluation samples.

## 2. Fit and compare the smoothing methods, and make a recommendation

Estimate the following smoothing models on only the *training* sample:

| Series | Model |
| --- | --- |
| PAYEMS | Simple exponential smoothing and double exponential smoothing |
| PAYNSA | Double exponential smoothing and Holt–Winters |

For each model estimation,

- Report the level smoothing parameter, the trend smoothing parameter (where applicable), and the sum of squared errors (SSE). Label the parameters clearly. Explain what the estimated smoothing parameters tell us about how the best smoothing fit, in terms of how it responds to new information.
- Plot the fitted values against the actual training observations.
- Highlight any ways that the model in question systematically misses the data, and any ways in which it performs better than the other model under consideration.
- For each of the two series, recommend one of the two models under consideration as the best choice for modeling the data. Justify your choice in terms of both training fit, and basic appropriateness of the model to the data given the features that you see.

## 3. Evaluation

For each of the two series, and for each of the two estimated models in the previous section (not just the one you recommended!),

1. Generate 12 monthly forecasts for **January–December 2025**, all made as of the end of December 2024. Do not yet use the evaluation sample (the 2025 data) in any way.
2. Then make plot that includes the training sample *and* the forecast sample, the fitted values from the model, and the forecasts that you made in step 1. Visually, in what ways do the forecasts seem to succeed or fail?
3. Report the RMSE in the evaluation sample. Does the ranking of the two models on RMSE agree with the recommendation that you made in Question 2? If not, how would you describe what is going on?