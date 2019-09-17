# Ananlysis_of_VolientCrime_Data

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
observations made from the above correlation plot
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
List of external links that I used for help
--
https://www.digitalocean.com/community/tutorials/how-to-handle-plain-text-files-in-python-3 # understanding the basics of text file
https://stackoverflow.com/questions/35672809/how-to-read-a-list-of-txt-files-in-a-folder-in-python #used for taking all text files in the current folder
https://docs.python.org/2/library/logging.html
https://medium.com/@acrosson/extracting-names-emails-and-phone-numbers-5d576354baa #extracting the data and facing issues for installing required packages
https://towardsdatascience.com/named-entity-recognition-with-nltk-and-spacy-8c4a7d88e7da
https://stackoverflow.com/questions/31088509/identifying-dates-in-strings-using-nltk
https://stackoverflow.com/questions/3868753/find-phone-numbers-in-python-script?utm_source=zapier.com&utm_medium=referral&utm_campaign=zapier # for finding phone numbers
https://www.geeksforgeeks.org/get-synonymsantonyms-nltk-wordnet-python/
https://stackoverflow.com/questions/5319922/python-check-if-word-is-in-a-string https://stackoverflow.com/questions/30150047/find-all-locations-cities-places-in-a-text#  for getting addresses

------
------
**Assumptions/Bugs**
--
Date Format: only this format is supported: 
```
January 28, 1986 or mm-dd-yyyy or January 1996,January, yyyy
```
Address Format: Only this format is supported:
```
112 N. Bald Hill Ave, Suffolk, VA 23434

```
Names:
```
You will see that most of the names are redacted, but again using nltk, chunking I find chunking doesnt give desired values always.
```
Phones
```
I have taken only regex given in the code to identify the numbers, numbers coming out of that might not be redacted.

