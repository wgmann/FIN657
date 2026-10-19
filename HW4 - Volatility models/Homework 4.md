# Homework 4: Time-varying volatility and portfolio risk

Compare constant-volatility and GARCH estimates of risk for momentum, value, and a combined strategy. Explain what your results say about risk at a particular forecast date and about the diversification observed in this sample.

Use the course Python environment and create a Jupyter notebook for your analysis. You may use the lecture materials and AI tools for code, debugging, and explanations. You are responsible for checking that the code performs the requested analysis and that your discussion agrees with your results. Simple, readable code is sufficient; loops, custom functions, elaborate formatting, and prompt histories are not required. Use the API credential setup established in class, and keep credentials out of your submission.

Use the public **Volatility modeling** lecture notebook as your main reference. Restrict attention to the models and risk calculation used in class; alternative volatility models, distribution fitting, and simulation are not required.

## 1. Obtain and prepare the returns (20%)

Use the **WRDS Python connection** to retrieve monthly factor data from **ff_all.factors_monthly**, including **date**, **umd**, and **hml**, for **January 2010 through April 2020**, inclusive. Record your download date and close the connection after retrieval. These WRDS return fields use decimal units; confirm that you have not confused decimal returns with percentage-point returns.

Treat **April 30, 2020** as the forecast origin. Use all observations through April 2020 to estimate the models; forecast the following 12 months, **May 2020–April 2021**. This fixes the timing explicitly rather than describing the exercise loosely as a forecast “in April.” Historical downloads may include revisions; this is not a reconstruction of original data releases.

Construct monthly simple returns for:

- **UMD:** a strategy long high-momentum stocks and short low-momentum stocks.
- **HML:** a strategy long value stocks and short growth stocks.
- **Combo:** 50% UMD and 50% HML, rebalanced monthly, so its simple return is $0.5r_{UMD,t}+0.5r_{HML,t}$.

Use the same unit-capital convention as class for these long–short strategy returns. Convert each simple return $r_t$ to log return $\ln(1+r_t)$ **after** forming the combination. Do not average the component log returns to obtain Combo.

Confirm ordered monthly dates, the first and last date, observation counts, no missing months or values, and $1+r_t>0$ for every log transformation. Plot cumulative gross returns, $\exp(\sum r_t^{log})$, for the three strategies, normalized to 1 immediately before the first sample return. Report sample mean and standard deviation of monthly log returns. The market series is not required for this assignment.

## 2. Estimate and assess volatility (30%)

For each strategy:

1. Estimate a constant mean and calculate constant-volatility risk as described in Section 3.
2. Apply Engle's ARCH-LM test to demeaned log returns. State its null hypothesis and interpret the p-value; do not assume rejection in advance.
3. Estimate a **GARCH(1,1)** model with a constant mean and normal innovations, following class. Fit to **100 times decimal log returns** to improve numerical scaling. Report the mean and volatility parameters and note any estimation/convergence warnings that affect the result.
4. Plot fitted conditional monthly volatility over time, labeled in percentage points of log return, using comparable axes across strategies.
5. Apply the same ARCH-LM test to **standardized residuals** (residual divided by conditional volatility), not raw residuals. Plot the ACF of squared standardized residuals through lag 12.

For a common ARCH-LM diagnostic, use **three monthly lags** before and after fitting GARCH (`nlags=3` in the class `het_arch` function); no lag search is required. Present before/after LM p-values in one table. Explain whether the diagnostics provide evidence against constant volatility and whether detectable volatility dependence remains after standardization. Failure to reject after fitting GARCH does not establish that every aspect of risk is captured.

## 3. Compare 12-month risk estimates (30%)

Use the **normal-approximation calculation taught in class** for the 5th percentile of cumulative 12-month log return:

$$q_{0.05}=\mu_{12}-1.645\sigma_{12},\qquad VaR_{log}=-q_{0.05}.$$

Keep this same convention for both methods. If a reported VaR is negative, retain its sign and explain the implied positive return threshold rather than silently taking an absolute value.

- **Constant volatility:** use 12 times the sample mean of monthly decimal log returns for $\mu_{12}$ and $\sqrt{12}$ times their sample standard deviation for $\sigma_{12}$.
- **GARCH:** forecast the next 12 monthly means and variances from the end-of-April fit. Sum the mean forecasts; sum the variance forecasts and then take the square root. Convert the resulting cumulative mean and standard deviation from percentage-point units back to decimal units before calculating the percentile. Do not sum standard deviations or treat current volatility as constant for all 12 months.

For GARCH, using these forecast moments in a normal percentile formula is an approximation, not an exact multi-period return distribution. You do not need to derive the distribution or replace this approach with simulation.

Make one table showing both risk estimates and their difference for each strategy. To express the threshold as a percentage loss of capital, convert the log-return percentile to $100[1-\exp(q_{0.05})]$; distinguish this from simply multiplying $VaR_{log}$ by 100.

Discuss briefly:

- At this forecast date, which approach reports greater risk for each strategy? Relate the differences to recent conditional volatility and the forecast volatility path, without assuming the ordering must always hold.
- How does Combo's estimated risk compare with the individual strategies? Use the sample correlation between their **simple** monthly returns as supporting context for diversification. Do not assume in advance that every risk measure must rank the strategies the same way.

## 4. Verify the analysis and explain your findings (20%)

### A short verification check

Display the first three months of simple UMD, HML, and Combo returns, alongside Combo log returns, so the portfolio weights and transformation order can be checked.

For one strategy, display all 12 monthly GARCH mean and variance forecasts with their units and target dates. Show the summed mean, summed variance, square-root step, and unit conversion used to obtain its 12-month percentile. Confirm that all three models use data ending in April 2020 and the same 12 future months. Explain in two or three sentences what these checks establish; there is no requirement to find an error.

### Risk summary (150–250 words)

Explain to a portfolio manager what changes when current volatility is taken into account, whether combining these strategies reduces the estimated risk in your results, and one limitation of the evidence. Translate one reported percentage-loss threshold into a plain-language statement with the horizon and probability clearly identified. Support your conclusions with your own numbers and diagnostics rather than claiming GARCH or diversification must always produce a particular ordering.

## Submission

Submit your completed Jupyter notebook with code, displayed outputs, plots, and discussion. Restart the kernel and run all cells in order before submitting. The notebook should reproduce the analysis using your normal course credentials without containing credentials itself.

Grading emphasizes correct implementation, readable evidence, and explanations tied to that evidence. Code sophistication, writing length beyond the requested discussion, and the choice of AI tool do not earn extra credit.
