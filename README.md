# Time Series Analysis and Linear Regression Forecasting of the Japanese Yen

This project tests several time-series tools and a linear regression model to predict future movements in the value of the Japanese yen versus the U.S. dollar.

---

# Time Series Analysis

---
Historical JPY/USD futures data was loaded from a csv. The below graph plots the Yen Futures Settle Prices.

![YenSettlePrice](Images/YenSettlePrice.png)


 Time series analysis models ARMA, ARIMA, and Volatility with GARCH were applied to determine whether there is any predictable behavior.

 ## Hodrick-Prescott Filter

The Hodrick-Prescott Filter was used to decompose the "Settle" price into two seperate series: noise and trend. Smoothing with the Hodrick-Prescott Filter and plotting the resulting trend against the actual futures returns, we can see that there's a lot of short term fluctuations that deviate around this trend.

![HPF Trend v Price](Images/HPFTrendvPrice.png)

Adding back the noise to the trend plot would re-create the original price plot. Plotting the noise on its own produces the following chart.

![HPF Noise](Images/HPFNoise.png)

## ARMA Model

An ARMA model was created and fit to the returns of the "Settle" price data. For autoregressive (AR) the parameter p was set to 2 and, for the moving average (MA) the parameter q was set to 1 making order=(2,1).

Based on the summary table output, the p-values for each lag did not show to be statistically significant - none are less than 0.05 indicating the variable inputs or lag do not deliver statistically significant impact.

![ARMA Summary](Images/ARMASummary.png)

Plotting the 5 day forecast for returns suggests the Yen will increase in value relative the USD.

![Five Day ARMA Forecast](Images/FiveDayARMA.png)

## ARIMA Model

An ARIMA model was estimated using the "Settle" price data. For autoregressive (AR) the parameter p was set to 5, for differences the parameter d was set to 1 and, for the moving average (MA) the parameter q was set to 1 making order=(5,1,1).

Again, based on the summary table output, the p-values for each lag did not show to be statistically significant - none are less than 0.05.

![ARIMA Summary](Images/ARIMASummary.png)

The Yen is forecasted to appreciate in value over the next 5 days relative to the US dollar. Based on the ARIMA model, a 0.04% increase in the Yen over the dollar is expected for the 5 day period.

![Five Day ARIMA Forecast](Images/FiveDayARIMA.png)

## GARCH Model

A GARCH model was created and fit to the returns of the "Settle" price data to forecast volatility. Similar to ARMA, the parameter p was set to 2 and the parameter q was set to 1. 

Based on the summary table output, the p-values for the alpha[1] and beta[1] lag are greater than 0.05 and therefore statistically significant. 

![GARCH Summary](Images/GARCHSummary.png)

Based on the GARCH forecast plot for the next 5 days, volatility risk of the Yen will increase each day.

![Five Day GARCH Forecast](Images/FiveDayGARCH.png)

## Conclusions

Both ARMA and ARIMA models suggest that the Yen will increase relative to the US dollar, however, taking into consideration the p-values, I would not buy the Yen. The p-values greater than 0.05 are reason for low confidence in these models.

Further, based on the GARCH forecast, volatility risk of the Yen will increase each day.

Again, due to p-values that do not come in below .05 there is not a high enough confidence interval to trade based on these models.

---

# Regression Analysis: Seasonal Effects with Sklearn Linear Regression

The same historical JPY/USD futures data was loaded from a csv. Percentage returns were calculated based on the "Settle" prices. The returns were then shifted to create a new column of lagged returns. The data was then split into training and test DataFrames. The training DataFrame included all data up to 2018. The testing DataFrame included all data from 2018 through 2019. 

An SKLearn linear regression was created and fit using the training DataFrame. A predicton was done using just the test DataFrame (X_test). Then the predicted returns were put into a DataFrame (Results) alongside the actual return data. Plotting the first 20 predictions vs the true values it is difficult to decifer how well the linear regression model predicted.

![First 20 Predictions](Images/First20Predictions.png)

To better assess the predictive ability of the model the Root Mean Squared Error (RMSE) was calculated for both out-of-sample performance and in-sample performance. The higher the Root Mean Squared Error (RMSE) the poorer the prediction is. The out-of-sample RMSE was 0.4152158107228894 while the in-sample RMSE was 0.5661029233587197. 

## Conclusions
Based on the prediction performance, the model performed better with the out-of-sample data (test data) with an RMSE of 0.415% compared to the in-sample data (training data) with an RMSE 0.566%. Generally you would expect the model to perform best on the data it trained on (in-sample data).
