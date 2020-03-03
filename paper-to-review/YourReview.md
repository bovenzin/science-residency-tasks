# Your review

Selected paper: Improving Subseasonal Forecasting in the Western U.S. with Machine Learning

### Summary

In this paper the authors propose a machine-learning based subseasonal forecasts of average temperature and total precipitation in Western US. Their study responds to a year-long challenge issued by national institutions in order to improve the accuracy of midterm forecasts, that are complicated as they heavily depend on both local weather and global climate variables. The contest asked participants to submit every two weeks their forecasts for each target variable (average temperature and total precipitation) and two forecast horizons (3-4 and 5-6 weeks in advance). Upon definition of the ''anomaly'' as the difference between target variable and a longterm average of that same variable over a 30 year period,  the accuracy of the forecast at a given target date is measured by the cosine similarity (also called "skill") between the predicted and observed anomaly vectors. The spatial region of interest is mapped into a grid of 514 points with a resolution of *1°x1°*: each anomaly vector has as many components as the number of grid points. Finally, for each target variable and horizon forecast, the average skill is calculated over the whole period of the challenge and the results are compared to those of other models. 

The approach presented in the paper consists in combining two regression models, both of them based on "multitask feature selection", meaning that the variables for a target date are selected jointly - instead of selecting them independently for each grid point - while the regression coefficients are fit independently for each grid point. 

The first model, the Local Linear Regression with Multitask Feature Selection (MultiLLR), has been trained on data from different sources that the authors collected in their SubseasonalRodeo Dataset, including lagged measurements of temperature, precipitation, humidity, pressure, wind, Madden-Julian oscillation. "Local" means that the training data is restricted to measurements within a range of *56* days around the target date. Since a feature can be more or less important depending on the target date (for instance, high humidity level can be a better predictor for precipitation in winter months than in summer), backward feature selection is performed for each target date. Starting with all features in the model at step zero, at each following step the feature that affect the cosine similarity the least, is removed, until when further removals are no more possible without changing the outcome of the predictive measurement beyond a tolerance value controlled by external input.     

The second model, the Multitask k-Nearest Neighbor Autoregression (AutoKNN), uses as features only historical measurements of the target variables from *l* days prior to the target date, with *l* taking three fixed values, plus a constant feature and the observed anomalies of the *k* most similar neighbors to the target date. The most similar neighbor is identified as the candidate date, within a period of one year prior to the target date, whose anomaly vector is as similar as possible to the target date's anomaly vector. The authors include *k=20* most similar neighbors when predicting temperatures, for a total of 24 features, while *k=1* when predicting precipitations (5 features in total). This difference can be explained by the fact that when predicting precipitation, the top neighbors for a target date are shown to be from the same time of year as the target date, while for temperature the top neighbors are drawn from throughout the year, regardless of the month of the target date. The authors show this difference in plots.

Finally, the two models are combined and the average of their *l2*-normalized predicted anomalies is defined as the ensemble anomaly. The authors motivate this choice by showing the mathematical proof that the so-defined ensemble anomaly is guaranteed to produce a better skill than the average of the skills of the individual models. 

The individual models and the ensemble model are evaluated over the contest period and also over each year in the period 2011-2018, and they are shown to outperform both the benchmark models provided by the organizers of the challenge and the top competitor.  

### Strengths

The study addresses the problem of forecasting on a time scale which has been defined “predictability desert” for the lack of models and methods that give good predictions. 
The models used in this paper are conceptually simple as they use linear regression.
The authors have collected data from many different sources, releasing a dataset for each target variable and each time range.


### Weaknesses and limitations

Besides considering the exact average of the two models, it would have been interesting to consider a weighted average of them when calculating the ensemble anomaly, and varying the relative weight to find the optimal ensemble skill. For instance, I believe that privileging the AutoKNN model would give a better mean skill for precipitation at 3-4 weeks.

### Implications
Societal implications: Improvement of subseasonal forecasts can lead to better management of water resources, agriculture processes, food production, reduction of the risks associated to extreme weather events.

Research implications: following this study, other multi-model combinations can be tried and the SubseasonalRodeo dataset can be used as a benchmark to train machine-learning models.


### 

