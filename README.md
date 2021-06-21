# Ames Housing Data and Kaggle Challenge

### Problem Statement

Ames is a city in Story County, Iowa, United States, located approximately 48 km north of Des Moines in central Iowa. The city is best known as the home of Iowa State University (ISU), with leading agriculture, design, engineering, and veterinary medicine colleges. As of 2019, the city had a population of 66,258 (including 33,391 students from ISU which makes up close to half of the city's population. Given its prime location, Ames was ranked ninth on CNNMoney's "Best Places to Live" list in 2010 (Source: Wikipedia)

As a junior data scientist in Ames Estates, one of the biggest real estate companies in Ames, you have been tasked by your CEO to develop an appropriate regression model to predict sale prices for properties in the city. With this model, it is your boss' wish that your company could make well-informed pricing decisions (for the properties our company is selling) without having to rely too heavily on the opinions and expertise of your realtors. The model will also serve as a systematic, data-driven way of estimating a sale price which we can share with house owners when they entrust us with the responsibility of selling their properties.

### Executive Summary

To predict house prices for our real estate company, we conducted the following regressions: 

1. Model 1 -> linear regression model on all variables in the dataset

    Train score is 0.9366346271941195
    Test score is -311160100964.22577
    Cross val score is -4.067246025762798e+22
    RMSE on training set: 20358.666275765623
    RMSE on testing set: 41617217847.05091

Observation: From the various test scores, they seem suggest that there is overfitting (training scores and training RMSE are performing way better than their counterparts for the test set). This also illustrates why it is important that we curate our list of independent variables. 


2. Model 2 -> linear regression model using selected variables chosen via correlation heatmap

    Train score is 0.8692612230936698
    Test score is 0.8335421318134438
    Cross val score is 0.5691852660050847
    RMSE on training set: 29243.243131786607
    RMSE on testing set: 30439.201539651465

Observation: Significant improvements in scores and RMSE vis-a-vis Model 1. As the scores and RMSE for the training and test sets are rather close to each other, this suggests that our model (in which the independent variables are the ones we selected using the correlation heatmap) is quite decent at predict sale price.

3. Model 3 -> linear regression model using selected variables from model 2 but with numerical variables in logarithmic form to correct for normality

    Train score is 0.8742048187833016
    Test score is 0.8277034548816071
    Cross val score is -14.461077471962852
    RMSE on training set: 0.14464084832779214
    RMSE on testing set: 0.16169805187338904
   

Observation: Not much changes in scores and RMSE vis-a-vis Model 2 despite taking the logarithmic form of the continuous variables. This is likely due to the fact that we have already scaled our variables using standard scaler. However, cross val score is negative and this suggests that the model may not be idea. Considering so, we will just stick with model 2 as the baseline model and perform Ridge and Lasso regressions to see if they further improve the model scores.


4. Model 4 -> LASSO regression model using selected variables from model 2

    LASSO model train score is 0.8692611095937676
    LASSO model test score is 0.8335573927416169
    Cross val score is 0.854750481491165
    RMSE on training set: 29243.255825436474
    RMSE on testing set: 30437.806168195082
    
5. Model 5 -> Ridge regression model using selected variables from model 2

    Ridge model train score is 0.8691463521715395
    Ridge model test score is 0.8341660911356079
    Cross val score is 0.8550701358837143
    RMSE on training set: 29256.08729644668
    RMSE on testing set: 30382.098034646577  

Overall observation: While the train/test scores of models 2, 4 and 5 are all very similar, we will use the Root Mean Square Error (RMSE; i.e. standard deviation of the residuals (prediction errors)) for our model selection. As such, we will go with the ridge regression model (model 4) which has the lowest RMSE for our test score (i.e. lowest standard deviation for our residuals from the test set).

In terms of the predictive powers of our ridge model, it should work decently well on new data (i.e. generalization) considering that its ridge model test score is 0.88. Based on our graphical representation (see plot in jupyter notebook), it can also be seen that our predicted sale prices are quite close to those of the actual sale prices (note: points on red line means 100% accurate prediction).

### Conclusion and Recommendations

**Conclusion**

Based on our ridge coefficients, the top 10 features that would most affect sale prices are:
- gr_liv_area
- overall_qual
- total_bsmt_sf
- mas_vnr_area
- mas_vnr_type_None
- neighborhood_NridgHt
- garage_area
- mas_vnr_type_Stone
- year_built
- neighborhood_StoneBr


In terms of the interpretation of coefficients, they are as such: 

- for continuous/discrete/ordinal variables, a one unit increase in [variable] will result in a beta increase/decrease in the sale price (holding everything else constant). For e.g. increasing above grade living area (grv_liv_area) by one square feet increases sale price by ~$27,000.
- for dummy variables, beta represents the addition increase/decrease in sale price over and above the base dummy (category that is dropped as part of drop_first= True; holding everything else constant). For example, a property in Northridge Heights (neighborhood_nridgHt) will see its sale price to be ~$$8,500 higher than that of a property in Bloomington Heights (base category for our neighborhood dummies). 

**Recommendations**

Re recommendations, our real estate company should strive to ensure that we get at least the top 10 features right (for future property pricing decisions) so that our model is able to provide a decent estimate for the starting sale price we should set. This estimate can then be used in tandem with that from our realtors (based on their opinions and expertise) to come up with an informed decision.

**Further studies**

As an extension to this project, we can consider supplementing the existing dataset with factors that we know are likely to affect house prices such as:

Data re proximity to schools, basic amenities and facilities
More transactional details of past sales (e.g. demographic of buyers/sellers) to predict if certain segments of the population are more/less likely to buy a specific type of property.


### Data Source and Dictionary

Data Sets: https://www.kaggle.com/c/dsir-28-project-2-regression-challenge/data

Data Dictionary: http://jse.amstat.org/v19n3/decock/DataDocumentation.txt
