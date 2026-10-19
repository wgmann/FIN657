# Time-varying volatility and portfolio risk

Your goal in this assignment is to compare constant-volatility and GARCH estimates of risk for momentum, value, and a combined strategy. You will use the fitted results and 12-month risk forecasts to assess how current volatility and diversification affect estimated portfolio risk.

You can use our class slides and code as a template and guide on how to do the analysis. The most relevant files for this assignment are the ones on **Volatility modeling**. You are also encouraged to use AI tools to help you, and they can refer to the course files as well.

## General instructions

Perform this analysis as a self-contained Python script or a Jupyter notebook that you upload back to Canvas as *just a single file*. My first step will be to try to get your code running on my end, and then I will grade the output. So check that the entire script or notebook runs, start to finish, before uploading your file back to Canvas! 

Again, you should just upload a single self-contained script with no other supporting files. The script should include the steps to download any data you use from FRED or WRDS. However, the code you submit should *not* include any database usernames, passwords, or API keys. These should be read into the script from externally, like in the examples we did in class. I will supply my own usernames and passwords when I run your code.

## 1. Obtain and prepare the data

Use the **WRDS Python connection** to retrieve monthly factor data from **ff_all.factors_monthly**, including **date**, **umd**, and **hml**, for **January 2010 through April 2020**, inclusive. These return fields use decimal units. Record your download date and close the connection after retrieval.

Construct monthly simple returns for the following strategies:

| Strategy | Return |
| --- | --- |
| UMD | Long high-momentum stocks and short low-momentum stocks |
| HML | Long value stocks and short growth stocks |
| Combo | 50% UMD and 50% HML, rebalanced monthly: $0.5r_{UMD,t}+0.5r_{HML,t}$ |

Use the same unit-capital convention as class for these long–short strategy returns.

1. Have your script print the first date, last date, and observation count. Confirm ordered monthly dates, no missing months or values, and that returns are in decimal units.
2. Convert each simple return $r_t$ to log return $\ln(1+r_t)$, checking that $1+r_t>0$. Form Combo in simple returns **before** taking logs; do not average the component log returns.
3. Plot cumulative gross returns, $\exp(\sum r_t^{log})$, for the three strategies, normalized to 1 immediately before the first sample return. Use clear labels.
4. Report the sample mean and standard deviation of monthly log returns, and the correlation between UMD and HML **simple** monthly returns.

Treat **April 30, 2020** as the forecast origin. Estimate all models using observations through April 2020 and forecast **May 2020–April 2021**. Historical downloads may include revisions; you do not need to reconstruct the original data releases. The market series is not required.

## 2. Fit and assess the volatility models

For each strategy,

1. Estimate a constant mean and sample standard deviation of monthly log returns for the constant-volatility model.
2. Apply Engle's ARCH-LM test to demeaned log returns. State its null hypothesis and briefly interpret the p-value.
3. Estimate a **GARCH(1,1)** model with a constant mean and normal innovations, following class. Fit to **100 times decimal log returns** to improve numerical scaling. Report the estimated mean and volatility parameters and any convergence warnings that affect the result.
4. Plot fitted conditional monthly volatility over time, labeled in percentage points of log return, using comparable axes across strategies.
5. Apply the same ARCH-LM test to **standardized residuals** (residual divided by conditional volatility). Plot the ACF of squared standardized residuals through lag 12.

Use **three monthly lags** for both ARCH-LM tests (`nlags=3` in `het_arch`). Make a compact table or list reporting the before/after LM p-values. Briefly explain whether the results support time-varying volatility and whether detectable volatility dependence remains after fitting GARCH.

## 3. Compare 12-month risk estimates

Use the **normal-approximation calculation taught in class** for the 5th percentile of cumulative 12-month log return:

$$q_{0.05}=\mu_{12}-1.645\sigma_{12},\qquad VaR_{log}=-q_{0.05}.$$

Calculate this risk estimate using both methods:

- **Constant volatility:** use 12 times the sample mean of monthly decimal log returns for $\mu_{12}$ and $\sqrt{12}$ times their sample standard deviation for $\sigma_{12}$.
- **GARCH:** forecast the next 12 monthly means and variances from the end-of-April fit. Sum the mean forecasts; sum the variance forecasts and then take the square root. Convert the cumulative mean and standard deviation from percentage-point units back to decimal units before calculating the percentile. Do not sum standard deviations or hold current volatility constant over the forecast horizon.

For GARCH, this normal percentile is an approximation to the multi-period return distribution; simulation is not required.

1. Make a table showing both log VaR estimates and their difference for each strategy. Also express each threshold as a percentage loss of capital using $100[1-\exp(q_{0.05})]$, rather than simply multiplying $VaR_{log}$ by 100. Retain the sign of negative VaR estimates and briefly explain any implied positive return threshold.
2. For one strategy, display the 12 monthly GARCH mean and variance forecasts with their units and target dates, and the summed mean, summed variance, square-root step, and unit conversion. Confirm that all models use the same forecast origin and horizon.
3. At this forecast date, which method reports greater risk for each strategy? Briefly relate the differences to recent conditional volatility and the forecast volatility path.
4. How does Combo's estimated risk compare with the individual strategies? Use their simple-return correlation as supporting context for diversification.
5. Translate one percentage-loss threshold into a brief statement for a portfolio manager, identifying the horizon and probability. State one limitation of the risk estimates.
