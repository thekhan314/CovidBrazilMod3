

# Overview

The aim of this project is to develop a model that can accurately classify the kind of hospitalization required for patients suffering from COVID Sars 2, based on their clinical data and test results. 

The final model can determine whether a patient needs to be admitted to the regular ward, the intensive care ward or if they do not require any admission whatsoever. 

# Dataset

The dataset used in this project comes from the Hospital Israelita Albert Einstein in Sao Paulo Brazil, and can be found [here](https://www.kaggle.com/einsteindata4u/covid19).

The data set contains around 5000 records and consists of over 100 features. The biggest challenge with this dataset was the enormous amount of missing data. The chart below is a histogram of row counts and number of empty features. We see that a majority of rows are missing data for almost 90% of the features. 

<img src='Images/row_cull.png'>

This absence of data points is partly due to the nature of the problem itself. Most of the records are for patients that were never admitted at all, and labs were only performed on patients who were. Since most of the features relate to lab test results, we have many features empty for many records on account of the fact that the labs were never performed. 

The following chart ranks the features by most data contained to most empty. The fill levels are shown seperatelyfor admitted patients and not admitted patients. 

<img src='Images/feature_fills.png'>

Additionaly, there is strong class imbalance between not hospitalized, regular ward,semi_intensive and intensive care patients. 

After fitting a preliminary Logistic Regression model on the dataset, I saw that the "semi_intensive" class was perfoming very poorly. 

<img src='Images/baseline_conf_mat.png'>

<img src='Images/roc_curves.png'>


I again used regression to reclassify those data points as either regular ward or intensive. his was in the hopes that while by itself the semi intensive data was hard to predict since it was on the margin of two other classes, it might be more usefull to use those data sets to strengthen the other two classes. 


# Methodology

To address the challenge of missing data, I have employed an iterative regression technique that takes data that IS present as a training set to predict the values missing for a given feature, and then adding the now complete feature to the training set for the next feature. 



As for developing the actual model, I fit a variety of models to the data nd selected the best one. The following dataframe shows the performance metrics for each class with each model variant. 

<img src='Images/model_results.png'>

To verify the coefficients computed by the best model, I used hypothesis testing to compare the means of features between admited and not admitted patients to see if there was a statistically significant difference those two populations for that feature. The idea is, that is a significant difference exists, then those features do in fact bear heavily on determing which category a patient will fall into. 

# Conclusions

A Logistic Regression model using SMOTE to address the imbalance issue appears to be the best peforming model. 

The following chart shows the most important features identified by the model. 

<img src='Images/feature_importance.png'>

The following dataframe shows the results of conducting relevant hypothesis tests on the means of the top features for the not admitted and admitted populations. We see that in most cases, the null hypothesis that the means are not statistically significant between the two populations has been rejected. 

<img src='Images/hyp_tests.png'>

# Recommendations

Additional data would be very helpful. There are some easy to obtain data points that are missing, things like BMI data, pre-existing conditions, smoking history etc. Ofcourse, patient privacy might be a concern. 
