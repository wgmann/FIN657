# Time-varying volatility and portfolio risk

Your goal in this assignment is to compare constant-volatility and GARCH estimates of risk for momentum, value, and a combined strategy. You will compare risk forecasts from two points in the dot-com episode to see how the models revise their estimates as new returns arrive and how the combined strategy compares with its two components.

You can use our class slides and code as a template and guide on how to do the analysis. The most relevant files for this assignment are the ones on **Volatility modeling**. You are also encouraged to use AI tools to help you, and they can refer to the course files as well.

## General instructions

Perform this analysis as a self-contained Python script or a Jupyter notebook that you upload back to Canvas as *just a single file*. My first step will be to try to get your code running on my end, and then I will grade the output. So check that the entire script or notebook runs, start to finish, before uploading your file back to Canvas! 

Again, you should just upload a single self-contained script with no other supporting files. The script should include the steps to download any data you use from FRED or WRDS. However, the code you submit should *not* include any database usernames, passwords, or API keys. These should be read into the script from externally, like in the examples we did in class. I will supply my own usernames and passwords when I run your code.

## 1. Obtain and prepare the data

Use the **WRDS API** to retrieve monthly factor data from **ff_all.factors_monthly**, including **date**, **umd**, and **hml**, for **July 1963 through July 2001**, inclusive. These return fields use decimal units. Record your download date and close the connection after retrieval.

Construct monthly simple returns for the following strategies:

| Strategy | Return |
| --- | --- |
| UMD | Long high-momentum stocks and short low-momentum stocks |
| HML | Long value stocks and short growth stocks |
| Combo | 50% UMD and 50% HML, rebalanced monthly: $0.5r_{UMD,t}+0.5r_{HML,t}$ |

1. Have your script print the first date, last date, and observation count. Confirm ordered monthly dates, no missing months or values, and that returns are in decimal units.
2. Report the sample mean and standard deviation of the simple monthly returns on UMD and HML, and the correlation between these two series.
3. Calculate the simple return on Combo as the simple average of the ingredient returns.
4. Convert each of the three simple returns $r_t$ (UMD, HML, and Combo) to log return via $\ln(1+r_t)$.
5. Plot the cumulative gross returns, $\exp(\sum r_t^{log})$, for all three strategies UMD, HML, and Combo, compared to a normalized starting wealth of 1.

Use the full sample through July 2001 for the diagnostics in Part 2.

## 2. Fit and assess the volatility models

For each strategy,

1. Estimate a constant mean and sample standard deviation of monthly log returns for the constant-volatility model.
2. Apply Engle's ARCH-LM test to demeaned log returns. State its null hypothesis and briefly interpret the p-value.
3. Estimate a **GARCH(1,1)** model with a constant mean and normal innovations, following class. Fit to **100 times decimal log returns** to improve numerical scaling. Report the estimated mean and volatility parameters and any convergence warnings that affect the result.
4. Plot fitted conditional monthly volatility over time, labeled in percentage points of log return, using comparable axes across strategies.
5. Apply the same ARCH-LM test to **standardized residuals** (residual divided by conditional volatility). Plot the ACF of squared standardized residuals through lag 12.

Use **three monthly lags** for both ARCH-LM tests (`nlags=3` in `het_arch`). Make a compact table or list reporting the before/after LM p-values. Briefly explain whether the results support time-varying volatility and whether detectable volatility dependence remains after fitting GARCH.

## 3. Compare 12-month risk estimates

For each strategy, calculate the **12-month, 5% Value at Risk (VaR)** under the constant-volatility and GARCH models using the method from class. Use July 1963 as the start of both estimation samples. Run the same analysis at each cutoff, fitting both models using only returns through the last month shown.

| Last month in estimation sample | 12-month forecast period |
| --- | --- |
| July 1999 | August 1999–July 2000 |
| July 2001 | August 2001–July 2002 |

1. In one table, report the July 1999 VaR, the July 2001 VaR, and the change (July 2001 minus July 1999) for each strategy and model. Label the units and use the same VaR sign convention throughout.
2. Use your table to identify and explain the patterns you see across models, dates, and strategies.
   - How does GARCH VaR differ from constant-volatility VaR at each date? How do the models' volatility assumptions help explain those differences?
   - How does VaR change from July 1999 to July 2001? Where is the change larger under GARCH, and why might GARCH respond more strongly to the returns added between the two dates?
   - How does Combo's VaR compare with UMD's and HML's across both models and dates? How does diversification help explain what you see?
