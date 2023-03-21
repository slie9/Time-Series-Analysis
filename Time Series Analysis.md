Sydney Lieske

Final Project

STA4853.01I&T Time Series Analysis

4/20/2022

# 



## Abstract

For my Time Series Analysis project in Business, Data Science, and Economics, I leveraged data from St. Louis Federal Reserve Economic Data (FRED) and focused on the Orlando-Kissimmee-Sanford area (MSA) in Florida. Using Stata, I applied time series analysis tools to forecast the March non-seasonally adjusted estimates for the total employment number and average weekly earnings of private employers.

To achieve accurate forecasts, I employed several techniques such as vselect and rolling windows for model selection. These techniques allowed me to identify the best model and forecast its outcome. Overall, this project aimed to showcase the effectiveness of time series analysis tools in business and economics and provide valuable insights into the future trends of employment and earnings in the Orlando-Kissimmee-Sanford area.


## Introduction

In this final project, I have applied the time series modeling and forecasting techniques that I learned in class to predict two variables for the Orlando-Kissimmee-Sanford area: total private employment and average weekly earnings. The aim of this project is to forecast the March non-seasonally adjusted estimates for these variables. The project involves using various techniques, such as summary statistics, auto-correlograms, partial auto-correlograms, tsline, vselect, rolling windows, and forecasting the best model. Overall, this project aims to demonstrate my understanding of time series analysis and its application in forecasting economic trends.

## Data

**Summary Statistics**

Table 1 Summary Statistics for All Standard Variables

| **Variable** | **Obs** | **Mean** | **Std. dev.** | **Min** | **Max** |
|--------------|---------|----------|---------------|---------|---------|
| TotalPriv    | 387     | 857.3328 | 202.0049      | 495.2   | 1225.5  |
| WeeklyHrs    | 183     | 35.44645 | .9223572      | 33      | 37.9    |
| HourlyEarn   | 183     | 22.97038 | 2.483777      | 19.33   | 30.18   |
| WeeklyEarn   | 183     | 812.7143 | 75.05616      | 684.74  | 1029.14 |

Table 2 Summary Statistics for All Log Transformed Variables

| **Variable** | **Obs** | **Mean** | **Std. dev.** | **Min**  | **Max**  |
|--------------|---------|----------|---------------|----------|----------|
| lnTotalPriv  | 387     | 6.723484 | .2532416      | 6.204962 | 7.111104 |
| lnWeeklyHrs  | 183     | 3.56769  | .0258066      | 3.496508 | 3.634951 |
| lnHourlyEarn | 183     | 3.128612 | .1051165      | 2.961658 | 3.407179 |
| lnWeeklyEarn | 183     | 6.696303 | .0897021      | 6.529039 | 6.936479 |

Upon examining the data, it is observed that the standard variables have a wide range of values, with maximum values being significantly higher than their corresponding minimum values.

Figure 1 Time Series Plots of lnTotalPriv and lnWeeklyEarn

<img src = Images/lntsline.png>

The time series plot on the left shows the log transform of total private employment in the Orlando-Kissimmee-Sanford area. It displays a general upward trend, except for periods in the early 1990s, early-mid 2000s, 2010~, and in 2020. On the right is the log transform of weekly earnings, with data available only from a bit before 2010. The plot reveals a decline before 2010, followed by a sharp increase and subsequent fluctuation until around 2014. Subsequently, the data experiences a steady rise until 2020, when it registers a significant spike.

Figure 2 ac_pac_lntotalpriv

<img src = Images/ac_pac_total.png>

Figure 3 ac_pac_lnweekly

<img src = Images/ac_pac_weekly.png>
The autocorrelation (AC) charts exhibit a decreasing trend starting from a high value, indicating the presence of an autoregressive term in the data. On the other hand, the partial autocorrelation (PAC) chart displays an alternating pattern with non-significant positive and negative values. The pattern suggests the possibility of a seasonal trend in the data.

## Model estimation and selection

For modeling total private employment and weekly earnings in the Orlando-Kissimmee-Sanford area, lags 1/12 for lnTotalPriv and lnWeeklyEarn were generated using vselect. Several models were selected, and their LOOCV is shown below. Models with 2 - 6 predictors had similar performance. Model 2 had the best AIC at -919.9073, while model 1 had the best BIC at **-**908.0097. Model 6 had the best adjusted R-squared at .1155997. Although model 1 had the lowest RMSE at .01289857, models 4 and 5 were the next lowest with .02288133 and .02285259, respectively. These models included lags 1,2,12dlnTotalPriv and lags 1,2 dlnWeekly. Model 1 only included lags 1,2 dlnTotalPriv.

**LOOCV of Models**

Table 3 LOOCV of Models

| **Models**                                                      | **RMSE**    |
|-----------------------------------------------------------------|-------------|
| reg d.lnTotalPriv l(1,2)d.lnTotalPriv                           | .01289857   |
| reg d.lnTotalPriv l(1,2,12)d.lnTotalPriv                        | . 01385695  |
| reg d.lnTotalPriv l(1,2,12)d.lnTotalPriv l(3)d.lnWeeklyEarn     | .0181307    |
| reg d.lnTotalPriv l(1,2,12)d.lnTotalPriv l(1,2)d.lnWeeklyEarn   | .01821613   |
| reg d.lnTotalPriv l(1,2,9,12)d.lnTotalPriv l(1,2)d.lnWeeklyEarn |  .01818075  |

Table 4 Vselect

| **Models**                                  | **Preds** | **R2ADJ**    | **C**     | **AIC**       | **AICC**      | **BIC**       |
|---------------------------------------------|-----------|--------------|-----------|---------------|---------------|---------------|
| l(1,2)dlnTotalPriv                          | 2         | .0863858     | -5.312001 | -917.4171     | -917.1747     | **-908.0097** |
| l(1,2,12)dlnTotalPriv                       | 3         | .1048409     | -7.448541 | **-919.9073** | **-919.5415** | -907.3641     |
| l(1,2,12)dlnTotalPriv l(12)dlnWeeklyEarn    | 4         | .1078058     | -6.888386 | -919.4985     | -918.9832     | -903.8195     |
| l(1,2,12)dlnTotalPriv l(1,2)dlnWeeklyEarn   | 5         | .1143199     | -6.927468 | -919.7777     | -919.0864     | -900.9629     |
| l(1,2,9,12)dlnTotalPriv l(1,2)dlnWeeklyEarn | 6         | **.1155997** | -6.065608 | -919.0633     | -918.1689     | -897.1127     |

After implementing rolling windows for each model, it was found that model 1 had the lowest BIC and RMSE values, while models 4 and 5 did not perform as well. The table below displays the corresponding model, window size, and RMSE for each. The optimal window size, when tested, was 84 months and resulted in an RMSE of 0.1755403.

Table 5 Rolling Windows

| **Model**                                   | **Window Size** | **RMSE**      |
|---------------------------------------------|-----------------|---------------|
| l(1,2,12)dlnTotalPriv                       | 84              | **.01755403** |
| l(1,2,12)dlnTotalPriv                       | 84              | . 02484131    |
| l(1,2,12)dlnTotalPriv l(12)dlnWeeklyEarn    | 84              | .02450599     |
| l(1,2,12)dlnTotalPriv l(1,2)dlnWeeklyEarn   | 84              | .02378657     |
| l(1,2,9,12)dlnTotalPriv l(1,2)dlnWeeklyEarn | 84              | .02375954     |

Figure 4 Error Distribution

<img src = Images/Error_Disc.png>

This is the error distribution.

Figure 5 RW 95%

<img src = Images/TotalPriv_RW_95.png>

This is a visualization that shows the results of the rolling window forecast for the time series data. The forecast is displayed along with 95% intervals for both normal and empirical distributions. The normal distribution assumes that the data follows a normal distribution, while the empirical distribution is calculated using the actual data and is therefore more accurate. The 95% intervals represent the range of values within which we can be 95% confident that the actual data will fall. This is useful for assessing the accuracy of the forecast and identifying any potential outliers or unexpected trends in the data. By comparing the results of the normal and empirical distributions, we can also gain insights into the underlying distribution of the data and identify any potential deviations from normality.

Figure 6 RW 90% and 99%

<img src = Images/TotalPriv_FW_90_99.png>

The rolling windows forecast is generated with 90% and 95% intervals for both empirical and normal distributions. The choice of confidence level (90% or 95%) determines the width of the intervals and the degree of certainty in the forecast. T

## Final Results

**Conclusion**
This project presented several challenges, but through the use of time series models and various techniques, I was able to gain valuable insights into the behavior of two key economic variables for the Orlando-Kissimmee-Sanford area. The summary statistics and AC/PAC plots provided a clear picture of the trends and patterns in the data, which helped identify potential models for forecasting.

By using vselect, I generated a set of candidate models for the variables, and then employed rolling windows forecasting to determine the optimal window width. This technique allowed us to account for the changing behavior of the variables over time and refine predictions accordingly.

While there were some difficulties, this project has deepened my understanding of time series modeling and forecasting techniques. I have learned how to identify key patterns and trends in the data, choose appropriate models for forecasting, and adjust predictions based on changing conditions.

Overall, this project has been a valuable learning experience, and I hope to apply these skills and techniques to future projects and data analysis tasks.
