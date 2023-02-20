Sydney Lieske

Final Project

STA4853.01I&T Time Series Analysis

4/20/2022

# 



## Abstract

In this project for Time Series Analysis for Business, Data Science, & Economics, I used data from St Louis Federal Reserve Economic Data (FRED) and am using available data for the Orlando-Kissimmee-Sanford area (MSA) in Florida. Using Stata this project allows application with time series tools to forecast the March non-seasonally adjusted estimates for the number of total employment for private employers and their average weekly earnings. This project includes the use of vselect and rolling windows for model selection and a forecast of the best model selection.


## Introduction

With this final project, we take what we learned in class about time series modeling and forecasting to predict two variables for the Orlando-Kissimmee-Sanford area. These are, the total private employment and average weekly earnings for this area. We are to forecast the March non-seasonally adjusted estimates in this project. This project includes summary statistics, auto correlograms, partial autocorrelograms , tsline, vselect, rolling windows, and forecast of best model.

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

With this data we can see that the maximum values for the standard variables have extreme values compared to its lowest values.

Figure 1 Time Series Plots of lnTotalPriv and lnWeeklyEarn

![Chart, line chart Description automatically generated](media/c3ba3ee986f91f7766ab9ff1451f16f2.png)

On the left, this time series plot shows the log transform of total private employment in Orlando-Kissimmee-Sanford area. We can see here there is a general an upwards trend for the most part except in early 1990s, early-mid 2000s, 2010\~, and in 2020. On the right is the log transform of the weekly earnings, it does not start until a bit before 2010. There is a decrease before 2010 before it spikes back up, and down throughout till about 2014. From there, the data has an increase and stays balanced until 2020, where it spikes majorly.

Figure 2 ac_pac_lntotalpriv

![Chart Description automatically generated](media/c844d79cbbabc6e12982eb781250216d.png)

Figure 3 ac_pac_lnweekly

![Chart Description automatically generated](media/eca04728c5b7f371e8dee5a35cd1a0e7.png)

The AC charts start high and decreases steadily. This suggests that in the data there is an autoregressive term. The PAC has quite an alternating pattern with positive and negative values that are not significant. It seems to have a pattern in the trends, possibly a seasonal trend.

## Model estimation and selection

To model the total private employment and weekly earnings in Orlando-Kissimmee-Sanford area, I had used vselect to generated lags 1/12 for lnTotalPriv and lnWeeklyEarn. Below are the reasonable set of models from vselect along with the LOOCV of each. The models with 2 – 6 predictors show similar performance. Model 2 has the best AIC at -919.9073. Model 1 has the best BIC at **-**908.0097. Model 6 has the best adjusted R-squared at .1155997. Model 1 has the lowest RMSE at .01289857 but models, 4 and 5 are the next lowest with .02288133 and .02285259 and include lags 1,2,12dlnTotalPriv and lags 1,2 dlnWeekly. Model one just has l(1,2) dlnTotalPriv.

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

Now using rolling windows programs for each model, when looking at all models, model 1 has the best BIC and RMSE while models 4 and 5 do not have much going for them. In the table below is the respective model, window size, and RMSE for each. When run through with rolling windows, the optimal window size was 84 months and it resulted in RMSE of 0.1755403.

Table 5 Rolling Windows

| **Model**                                   | **Window Size** | **RMSE**      |
|---------------------------------------------|-----------------|---------------|
| l(1,2,12)dlnTotalPriv                       | 84              | **.01755403** |
| l(1,2,12)dlnTotalPriv                       | 84              | . 02484131    |
| l(1,2,12)dlnTotalPriv l(12)dlnWeeklyEarn    | 84              | .02450599     |
| l(1,2,12)dlnTotalPriv l(1,2)dlnWeeklyEarn   | 84              | .02378657     |
| l(1,2,9,12)dlnTotalPriv l(1,2)dlnWeeklyEarn | 84              | .02375954     |

Figure 4 Error Distribution

![Chart, histogram Description automatically generated](media/d29e9a57e776d962b941dff4cc6a5f10.png)

This is the error distribution.

Figure 5 RW 95%

![Diagram Description automatically generated with medium confidence](media/53e8f0ef7f4cc674c235f05ad1a5c92a.png)

This is the rolling window forecast with 95% intervals for normal and empirical.

Figure 6 RW 90% and 99%

![Diagram Description automatically generated](media/62d654bc1139164cafa5eed353269ddc.png)

This is the rolling windows forecast with 90% and 95% intervals for empirical and normal.

## Final Results

**Conclusion**

This project was complicated, but we are able to see and understand the data with time series models. With the summary statistics and AC/PAC plots the data was easier to understand and interpret. With the use of vselect I was able to identify several potential models for the variables to forecast. Next, with the use of those models, they were put into a rolling windows forecast to find the best window width. The plan was to forecast for March 2022, I still had some issues figuring out the rest after the rolling windows but I have a better understanding of time series.
