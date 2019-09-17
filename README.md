# Ananlysis_of_VolientCrime_Data
---
Given real-world data relating to various communities and their socio-demographics, law enforcement details, and crime statistics, your goal is to predict the community-level per capita violent crimes. The target variable is continuous and you may use any techniques at your disposal to produce a highly predictive model.

-------------
Author 
---

**Venkata Subbaraju Sagi**

All known Bugs and fixes can be sent to subbaraju.v@ou.edu

Packages required for the project : tidyverse,Amelia,caret,MASS,corrplot,psych,corrgram,mlbench,e1071,car,pls,glmnet,elasticnet,xgboost

-----
Data Details
----
The variables included in the dataset involve the community, such as the percent of the population considered urban, and the median family income, and involving law enforcement, such as per capita number of police officers, and percent of sworn full time police officers on patrol.

The per capita violent crimes variable, ViolentCrimesPerPop, is the target variable and is calculated using population and the sum of crime variables considered violent crimes in the United States: murder, rape, robbery, and assault.

All numeric data was normalized into the decimal range 0.00-1.00 using an equal-interval binning method. Attributes retain their distribution and skew (hence for example the population attribute has a mean value of 0.06 because most communities are small). E.g. An attribute described as 'mean people per household' is actually the normalized (0-1) version of that value.

The normalization preserves rough ratios of values WITHIN an attribute (e.g. double the value for double the population within the available precision - except for extreme values (all values more than 3 SD above the mean are normalized to 1.00; all values more than 3 SD below the mean are normalized to 0.00)).

However, the normalization does not preserve relationships between values BETWEEN attributes (e.g. it would not be meaningful to compare the value for whitePerCap with the value for blackPerCap for a community)

A limitation was that the LEMAS survey was of the police departments with at least 100 officers, plus a random sample of smaller departments. For our purposes, communities not found in both census and crime datasets were omitted. Many communities are missing LEMAS data.

-------

SUMMARY OF TRANSFORMATIONS PERFORMED
---
“Communityname” #has 1135 unique values. So, can be removed.

“Target variable” doesn't seem to be affected by county. So, we’ll remove it.

Predictors with Police information has 84.33% missing values, so, all these are removed.

Replaced one missing value in ‘OtherPerCap’ with mode value.


Householdsize & PersPerOccupHous both variables mean the same and also have similar value, householdsize is removed. State is converted into factor variable.

Variables with skewness >4 are transformed using symbox.

‘medIncome’ ‘medFamInc’  ‘numbUrban’ ‘NumUnderPov’ are removed as these are having high correlation with other variables.

Unemployed and PctNotHSGrad are correlated and similarly affecting target variable. So, we'll remove PctNotHSGrad to avoid overfitting.

MalePctDivorce and FemalePctDiv are highly correlated with TotPctDiv, so, these two are deleted. Also, PctKids2Par, PctTeen2Par, PctWorkMomYoungKids, PctRecImmig5, PctRecImmig8, PctImmigRec5, PctImmigRec8 are removed as they’re having correlation with other predictors.


The following Predictors are removed as they’re correlated with other variables: - PctLargHouseOccup, PersPerOccupHous, PersPerOwnOccHous, PersPerRentOccHous, MedYrHousBuilt, OwnOccHiQuart, OwnOccLowQuart, RentLowQ, RentHighQ, MedRent, PctSameHouse85 are removed.

---
Observations made from the above correlation plot
----------------
1. age12t21 and age12t29 have correlation. This may be due to overlapping in data. So, we,ll make age12t29 to age21t29

2. age65up and pctWSocSec are highly correlated and pctWSocSec is more related to ViolentCrimesPerPop. SO, we'll remove age65up to avoid overfitting.

3. perCapInc and whitePerCap are highly correlated and seems to be similarly correlated with other variables. 

4. PctBSorMore and PctOccupMgmtProf are highly correlated and PctOccupMgmtProf seems to be more related to other variables. So, we'll remove PctBSorMore. 

5. PctFam2Par and PctYoungKids2Par are highly correlated. PctYoungKids2Par is removed to avoid multicollinearity.

6. PctPersOwnOccup and PctHousOwnOcc are highly correlated and are similarly related to other variables. So, we'll remove PctPersOwnOccup

7. PctRecentImmig and PctForeignBorn are highly correlated. PctForeignBorn is removed to avoid multicollinearity.

8. OwnOccMedVal and RentMedian are highly correlated. OwnOccMedVal is removed to avoid multicollinearity.

9. Also, other variables having correlation with ViolentCrimesPerPop are racepctblack, racepctwhite,pctWInvInc, pctWPubAsst,PctPopUnderPov, PctUnemployed,PctFam2Par, PersPerOwnOccHous, HousVacant.

---
Modelling
--

**I)	PLS**
We have chosen the PLS as our regression model. Partial Least squares regression is a supervised technique that is known to perform better than “OLS” and “PCR” Regression techniques in most cases.
This method finds components that explain high variability as well as high correlation with the response variable i.e. Violent Crimes in our case. To avoid overfitting the model to the training dataset, we used in-built cross-validation features.
We fitted the data in the basic "PLS" model. When we looked at the summary of the fit function we found that max variance can be explained around 7 components and then it stabilizes.
The Validation plot to check the validity of our assumptions and it represents the same. 
We did Hyper Parameter Turning with the PLS Model to understand which component is fitting better. To find the components at which we obtain minimum "RMSE" Value, we used the plot and found the minimum value of 7 components from the Plot results.
We used this value to plot the fit. We Obtained a very good fit with all the points following the diagonal line. We fitted the tuned model with the test data and obtained our final predictions after the hyper parameter tuning improved a lot and we got good score.

**II)	RIDGE REGRESSION**
The regression model we used here is RIDGE Regression model. Variable selection and regularization will be done by this model.
We observed that in our data we have lot of variables and in order to find the variables that matter most we used 10-fold cross validation from. We reduced the dimensionality as a result. The plot below shows the coefficients for different lambda values, we will be choosing the Lambda minimum, which gives minimum Cross validation error.


**III)	LASSO**

The regression model we used here is LASSO (Least Absolute Shrinkage and Selection Operator) model. Variable selection and regularization will be done by LASSO model.
We observed that in our housing data we have lot of variables and in order to find the variables that matter most we used 10-fold repeated cross validation. We reduced the dimensionality as a result.
The Mean-Squared Error and Lambda plot states, It includes the cross-validation curve (red dotted line), and upper and lower standard deviation curves along the λλ sequence (error bars). Two selected lambdas are indicated by the vertical dotted lines.

lambda.min is the value of the one that gives minimum mean cross-validated error.

The main tuning parameter for the lasso model is alpha - a regularization parameter that measures how flexible our model is. The higher the regularization the less prone our model will be to overfit. However, it will also lose flexibility and might not capture all of the signal in the data.
One thing to note here however is that the features selected are not necessarily the "correct" ones especially since there are few collinear features in this dataset. One idea to try here is run Lasso a few times on bootstrapped samples and see how stable the feature selection is.
 Here we used our own Grid, cross validation method, used glmnet package and we are using tenfold cross validation.

**IV)	ELASTIC NET**

We are using caret package for this modelling, The Elastic Net addresses the aforementioned “over-regularization” by balancing between LASSO and ridge penalties. In particular, a hyper-parameter, namely Alpha, would be used to regularize the model such that the model would become a LASSO in case of Alpha = 1 and a ridge in case of Alpha = 0. In practice, Alpha can be tuned easily by the cross-validation. 
In our case we want to find the optimal lambda and alpha jointly. For that we will need to use the caret package.  Using the train function in the caret package we can set up a grid of alpha and lambda values and perform cross validation to find the optimal parameter values.A higher value of lambda shrinks the model’s coefficients towards zero.
Caret package sets up everything beautifully, we get our best tuned results with which we can fit the model, the Error on the trained dataset also has best value for elastic net

**V)	SVM**
The next learning model we are interested in is support vector machine. This model is supervised learning algorithm that analyze data used for classification and regression analysis.
This model project the low dimensional data into high dimensional data and generates multiple hyper planes such that the data space is divided into segments and each segment contains only one kind of data.we have used caret package with linear kernel, for this model.

The Best Model that we have got from all above is Elastic Net.
------
