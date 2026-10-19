# Time-varying volatility and portfolio risk

Your goal in this assignment is to compare constant-volatility and GARCH estimates of risk for momentum, value, and a combined strategy. You will use the fitted results and 12-month risk forecasts to assess how current volatility and diversification affect estimated portfolio risk.

You can use our class slides and code as a template and guide on how to do the analysis. The most relevant files for this assignment are the ones on **Volatility modeling**. You are also encouraged to use AI tools to help you, and they can refer to the course files as well.

## General instructions

Perform this analysis as a self-contained Python script or a Jupyter notebook that you upload back to Canvas as *just a single file*. My first step will be to try to get your code running on my end, and then I will grade the output. So check that the entire script or notebook runs, start to finish, before uploading your file back to Canvas! 

Again, you should just upload a single self-contained script with no other supporting files. The script should include the steps to download any data you use from FRED or WRDS. However, the code you submit should *not* include any database usernames, passwords, or API keys. These should be read into the script from externally, like in the examples we did in class. I will supply my own usernames and passwords when I run your code.

## 1. Obtain and prepare the data

Use the **WRDS API** to retrieve monthly factor data from **ff_all.factors_monthly**, including **date**, **umd**, and **hml**, for **January 2010 through April 2020**, inclusive. These return fields use decimal units. Record your download date and close the connection after retrieval.

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

Estimate all models using observations through April 2020 and forecast **May 2020–April 2021**. 

## 2. Fit and assess the volatility models

For each strategy,

1. Estimate a constant mean and sample standard deviation of monthly log returns for the constant-volatility model.
2. Apply Engle's ARCH-LM test to demeaned log returns. State its null hypothesis and briefly interpret the p-value.
3. Estimate a **GARCH(1,1)** model with a constant mean and normal innovations, following class. Fit to **100 times decimal log returns** to improve numerical scaling. Report the estimated mean and volatility parameters and any convergence warnings that affect the result.
4. Plot fitted conditional monthly volatility over time, labeled in percentage points of log return, using comparable axes across strategies.
5. Apply the same ARCH-LM test to **standardized residuals** (residual divided by conditional volatility). Plot the ACF of squared standardized residuals through lag 12.

Use **three monthly lags** for both ARCH-LM tests (`nlags=3` in `het_arch`). Make a compact table or list reporting the before/after LM p-values. Briefly explain whether the results support time-varying volatility and whether detectable volatility dependence remains after fitting GARCH.

## 3. Compare 12-month risk estimates

For each of the strategies, calculate a **12-month, 5\% Value at Risk**, following the definitions and formulas in class. Do this two ways: First use a **constant volatility** model, and then use a **GARCH** model.

1. Comparing across the two volatility models, which method reports greater risk at this forecast date? Why does this make sense? Describe an important takeaway from this result for a portfolio manager.
2. Comparing across the strategies, how does Combo's estimated risk compare with the individual strategies? Why does this make sense? Describe an important takeaway from this result for a portfolio manager.
