# Your review

Selected paper: Improving Subseasonal Forecasting in the Western U.S. with Machine Learning

### Summary

In this paper the authors propose a machine-learning based system to predict the average temperature and
and total precipitation with two-to-four and four-to-six weeks advance. Their study responds to a year-long challenge issued by national institutions in order to improve the accuracy of midterm forecasts, that are complicated as they depend on both local weather and global climate variables. The contest asked participants to submit every two weeks their forecasts for each target variable (average temperature and total precipitation) for each of the two forecast horizons (3-4 and 5-6 weeks respectively). Upon definition of the ''anomaly'' as the difference between target variable and a longterm average of that same variable over a 30 year period,  the accuracy of the forecast at a given target date is measured by the cosine similarity (also called "skill") between the predicted and observed anomaly vectors. The spatial region of interest was mapped into a grid of 514 points with a resolution of 1°x1° and each vector has as many components as the numer of grid points. Finally, for each target variable and horizon forecast, the average skill is calculated over the whole period of the challenge and the results are compared to those of other models. 
The approach presented in the paper consists in combining two regression models, both of them based on "multitask feature selection", meaning that the variables for a target date are selected jointly - instead of selecting them independently for each grid point - while the regression coefficients are fit independently for each grid point. 

The first model is a Local Linear Regression with Multitask Feature Selection (MultiLLR), it has been trained on data from different sources that the authors collected in their SubseasonalRodeo Dataset, including lagged measurements of temperature, precipitation, humidity, pressure, wind, Madden-Julian oscillation. “Local” means that the training data is restricted to measurements within a range of 56 days around the target date. As the importance of the features depends on the target date (for instance, high humidity level can be a better predictor for precipitation in winter months than in summer), backward feature selection is performed for each target date. Starting with all thefeatures in the model at step zero, then each step the feature that influence the predictive measurement of the cosine similarity the least, is removed, until when further removals that change the outcome predictive measurement within a tolerance range are no more possible.     

The second model is a Multitask k-Nearest Neighbor Autoregression (AutoKNN). It uses as features only historical measurements of the target variables from l days prior to the target date, with l taking three fixed values, plus the constant intercept (with value equal 1 for all datapoints) and the observed anomalies of the k most similar neighbors to the target date. The most similar neighbor is identified as the candidate date, within a period of one year prior to the target date, whose anomaly vector is as similar as possible to the target date's anomaly vector. The authors include k=20 most similar neighbors when predicting temperatures, for a total of 24 features, while k=1 when predicting precipitations (5 features in total in total). This difference can be explained by the fact that when predicting precipitation, the top neighbors for a target date are shown to be from the same time of year as the target date, while for temperature the top neighbors are drawn from throughout the year, regardless of the month of the target date. The authors show corresponding plots.

Finally, the two models are combined and the average of their l2-normalized predicted anomalies is defined as the ensemble anomaly. The authors motivate this choice by showing the mathematical proof that the so-defined ensemble anomaly is guaranteed to produce a better skill than the average of the skills of the individual models. 

The individual models and the esemble model are evaluated over the contest period and also over each year in the period 2011-2018, and they are shown to outperform both the benchmark models provided by the organizers of the challenge and the top competitor.  

Put your summary of the paper here

### Strengths

The study addresses the problem of forecasting on a time scale which has been defined “predictability desert” for the lack of models and methods that give good predictions. 
The models used in this paper are conceptually simple as they use linear regression.
The authors have collected data from many different sources, realising a dataset for each target variable and each time range.


### Weaknesses and limitations

Besides considering the exact average of the two models, it would have been interesting to consider a weighted average of them when calculating the ensemble anomaly, and varying the relative weight to find the optimal ensemble skill. For instance, I believe that privileging the AutoKNN model would give a better mean skill for precipitation at 3-4 weeks.

### Implications
Societal implications: Improvement of subseasonal forecasts can lead to better management of water resources, agriculture processes, food prodcution, it helps to reduce the risks associated to extrem weather events

Research implications: following this study, other multi-model combinations can be tried and the SubseasonalRodeo dataset can be used as a benchmark to train machine-learning models.

ll expose the ML community to an im-
portant problem ripe for ML development—improving subseasonal
forecasting for water and fire management, demonstrate that ML
tools can lead to significant improvements in subseasonal forecast-
ing skill, and stimulate future development with the release of our
user-friendly Python Pandas SubseasonalRodeo dataset.
Our experience also suggests that subseasonal forecasting is
fertile ground for machine learning development. Much of the
methodological novelty in our approach was driven by the unusual
multitask forecasting skill objective. This objective inspired our
new and provably beneficial ensembling approach and our custom
multitask neighbor selection strategy. We hope that introducing
this problem to the ML community and providing a user-friendly
benchmark dataset will stimulate the development and evaluation
of additional subseasonal forecasting approaches.

### 

