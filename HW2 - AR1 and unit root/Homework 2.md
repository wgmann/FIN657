# Assessing stationarity in financial time series

Your goal in this assignment is to construct five quarterly S&P 500 series, assess their stationarity, and explain which modeling assumptions the evidence supports. You will interpret the tests alongside the data to recommend which series to model as stationary.

You can use our class slides and code as a template and guide on how to do the analysis. The most relevant files for this assignment are the ones on **AR1 and unit root processes**. You are also encouraged to use AI tools to help you, and they can refer to the course files as well.

## General instructions

Perform this analysis as a self-contained Python script or a Jupyter notebook that you upload back to Canvas as *just a single file*. My first step will be to try to get your code running on my end, and then I will grade the output. So check that the entire script or notebook runs, start to finish, before uploading your file back to Canvas! 

Again, you should just upload a single self-contained script with no other supporting files. The script should include the steps to download any data you use from FRED or WRDS. However, the code you submit should *not* include any database usernames, passwords, or API keys. These should be read into the script from externally, like in the examples we did in class. I will supply my own usernames and passwords when I run your code.

## 1. Obtain and prepare the data

Use the **WRDS Python connection** to retrieve monthly S&P 500 index observations from **CRSP**, table **crsp_a_indexes.msp500**. Retrieve **caldt**, **vwretd**, **vwretx**, and **spindx**, covering **December 1999 through December 2024**. The extra December 1999 observation supplies the lag needed to construct January 2000 dividends. 

Then, use the following code to construct the five quarterly series. Here, `data` is the DataFrame returned by your WRDS query, and `pd` and `np` refer to pandas and NumPy.

```python
# Put the monthly observations in chronological order before calculating lags.
data['month'] = pd.to_datetime(data['caldt']).dt.to_period('M')
data = data.set_index('month').drop(columns='caldt').sort_index()

# Calculate the monthly index-dividend proxy.
data['dividends_mth'] = (data['vwretd'] - data['vwretx']) * data['spindx'].shift()

# Sum monthly dividends within each quarter; retain the last monthly index level.
dataq = data.reset_index()
dataq['quarter'] = dataq['month'].dt.to_timestamp().dt.to_period('Q')
dataq = dataq.set_index('quarter')
dataq['dividends_qtr'] = dataq.groupby(level=0)['dividends_mth'].sum()
dataq = dataq.groupby(level=0).last()
dataq = dataq.loc['2000Q1':'2024Q4']

# Calculate log levels and quarterly log growth.
log_spindx = np.log(dataq['spindx']).astype('float64')
log_spindx_growth = log_spindx.diff().dropna()

log_dividends = np.log(dataq['dividends_qtr']).astype('float64')
log_dividends_growth = log_dividends.diff().dropna()

# Quarterly dividend yield, without annualizing or differencing it.
log_DP = np.log(dataq['dividends_qtr'] / dataq['spindx']).astype('float64')
```

Plot all five series in separate figures, with clearly labeled axes.

## 2. Apply and interpret the tests, and make a recommendation

For each of the five series, conduct ADF and KPSS tests using a constant but no deterministic time trend, following the specifications used in class, and allowing for automatic lag selection.

Make a compact table or list that reports, for each series:

- The ADF statistic, p-value, and selected lag count.
- The KPSS statistic, p-value, and selected lag count.
- Whether each test rejects its own null at 5%.
- A short assessment: evidence favoring stationarity, evidence against it, or inconclusive/mixed evidence under these specifications.

For each series, give a brief interpretation tied to the table and plot: would you proceed with a stationary model **of that series**, and what qualification would you attach? 

## 3. Connect ADF to its regression

For log dividend yield, run the auxiliary regression behind the ADF test, and display a summary of the regression results.

- Identify the number in the regression summary table that matches the ***ADF test statistic***, or else explain why there is no such number.
- Identify the number in the regression summary table that matches the ADF test ***p-value***, or else explain why there is no such number.
