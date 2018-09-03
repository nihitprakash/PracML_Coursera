# PracML_Coursera
For the Practical Machine Learning Coursera Course
Practical Maching Learning-Course Project
Nihit Prakash
09/02/2018
Background
Using devices such as Jawbone Up, Nike FuelBand, and Fitbit it is now possible to collect a large amount of data about personal activity relatively inexpensively. These type of devices are part of the quantified self movement - a group of enthusiasts who take measurements about themselves regularly to improve their health, to find patterns in their behavior, or because they are tech geeks. One thing that people regularly do is quantify how much of a particular activity they do, but they rarely quantify how well they do it. In this project, your goal will be to use data from accelerometers on the belt, forearm, arm, and dumbell of 6 participants. They were asked to perform barbell lifts correctly and incorrectly in 5 different ways

Analysis Walk Through
The training and testing data was imported and converted to dataframe variables. The first point explored was the distribution of our output variable. The distribution seems a bit skewed towards “A”, but generally uniform. There is no strong class imbalance. As stated before, we have 5 different levels of outputs here: A through E. For multi-output classfication problems, tree based models will generally work well Hence, the analysis from this point on will be performed keeping that in mind.

The classe column(the output variable) was then separated from the training dataset.

From further exploration of the dataset, it was seen that there were some columns unnecessary for our analysis like, names, the timestamp and v1 columns. These were removed. It was also straighaway observed that multiple predictors had NA’s. Therefore, full NA columns were removed, and then any columns which had >95% missing values. NearZeroVariance predictors were also removed.

A corelation plot was built at this point to observe the correlations between our predictors. There were some predictors that were highly correlated with each other (postively and negatively). Principal Component Analysis could be performed at this point, to remove correlations and reduce number of predictors. However, since we have decided to use Tree-based models anyway, we don’t need to perform this step, since Tree-based models usually take care of collinearity.

Now, with our data munging complete, at this point we recombine the classe column with the training dataset. We then split out the training dataset into two sets: training_new and validation (80:20). We will use the training_new dataset for training our models, and the validation dataset to test our model to determine our out-of-sample accuracy. The model with the best accuracy here will be used to predict on the Test Dataset.

Our first model will be a single decision tree, the CART Model. We use 10 fold cross validation to avoid any overfitting. From predicting on our validation set, we get an out-of-sample accuracy on the Validation set of 49.3%

Our second model will be a Random Forest Model. With the same 10 fold cross validation, we observe a significant increase in out-of-sample accuracy to 99.8%

Our third model will be a Gradient Boosing Model. Here, we get an accuracy of 98.93%. Pretty good, but our Random Forest Model still perfomed better.

Thus, the Random Forest Model was selected for predicting on the Test Set.

Importing Packages

## 
## Attaching package: 'dplyr'
## The following objects are masked from 'package:stats':
## 
##     filter, lag
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
## Type 'citation("pROC")' for a citation.
## 
## Attaching package: 'pROC'
## The following objects are masked from 'package:stats':
## 
##     cov, smooth, var
## Loading required package: lattice
## 
## Attaching package: 'data.table'
## The following objects are masked from 'package:dplyr':
## 
##     between, first, last
## Loading required package: grid
## corrplot 0.84 loaded
## Rattle: A free graphical interface for data science with R.
## Version 5.1.0 Copyright (c) 2006-2017 Togaware Pty Ltd.
## Type 'rattle()' to shake, rattle, and roll your data.
Correlation Plot 

set.seed(1243)
intrain <- createDataPartition(training$classe, p=0.80, list=FALSE)

training_new <- training[intrain,]
validation <- training[-intrain,]
CART MODEL


## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A 1007  305  309  293  119
##          B   16  257   16  103   76
##          C   90  197  359  247  215
##          D    0    0    0    0    0
##          E    3    0    0    0  311
## 
## Overall Statistics
##                                           
##                Accuracy : 0.493           
##                  95% CI : (0.4772, 0.5088)
##     No Information Rate : 0.2845          
##     P-Value [Acc > NIR] : < 2.2e-16       
##                                           
##                   Kappa : 0.3377          
##  Mcnemar's Test P-Value : NA              
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity            0.9023  0.33860  0.52485   0.0000  0.43135
## Specificity            0.6345  0.93331  0.76876   1.0000  0.99906
## Pos Pred Value         0.4953  0.54915  0.32401      NaN  0.99045
## Neg Pred Value         0.9423  0.85470  0.88455   0.8361  0.88640
## Prevalence             0.2845  0.19347  0.17436   0.1639  0.18379
## Detection Rate         0.2567  0.06551  0.09151   0.0000  0.07928
## Detection Prevalence   0.5182  0.11930  0.28244   0.0000  0.08004
## Balanced Accuracy      0.7684  0.63596  0.64680   0.5000  0.71520
Random Forest Model
## Random Forest 
## 
## 15699 samples
##    53 predictor
##     5 classes: 'A', 'B', 'C', 'D', 'E' 
## 
## No pre-processing
## Resampling: Cross-Validated (10 fold) 
## Summary of sample sizes: 14129, 14129, 14131, 14129, 14129, 14129, ... 
## Resampling results across tuning parameters:
## 
##   mtry  Accuracy   Kappa    
##    2    0.9948407  0.9934735
##   27    0.9983439  0.9979052
##   53    0.9963054  0.9953263
## 
## Accuracy was used to select the optimal model using the largest value.
## The final value used for the model was mtry = 27.
## 
## Call:
##  randomForest(x = x, y = y, mtry = param$mtry) 
##                Type of random forest: classification
##                      Number of trees: 500
## No. of variables tried at each split: 27
## 
##         OOB estimate of  error rate: 0.13%
## Confusion matrix:
##      A    B    C    D    E  class.error
## A 4463    1    0    0    0 0.0002240143
## B    4 3033    1    0    0 0.0016458196
## C    0    5 2733    0    0 0.0018261505
## D    0    0    4 2568    1 0.0019432569
## E    0    0    0    4 2882 0.0013860014
## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A 1115    1    0    0    0
##          B    0  755    1    0    0
##          C    0    3  683    2    0
##          D    0    0    0  641    0
##          E    1    0    0    0  721
## 
## Overall Statistics
##                                          
##                Accuracy : 0.998          
##                  95% CI : (0.996, 0.9991)
##     No Information Rate : 0.2845         
##     P-Value [Acc > NIR] : < 2.2e-16      
##                                          
##                   Kappa : 0.9974         
##  Mcnemar's Test P-Value : NA             
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity            0.9991   0.9947   0.9985   0.9969   1.0000
## Specificity            0.9996   0.9997   0.9985   1.0000   0.9997
## Pos Pred Value         0.9991   0.9987   0.9927   1.0000   0.9986
## Neg Pred Value         0.9996   0.9987   0.9997   0.9994   1.0000
## Prevalence             0.2845   0.1935   0.1744   0.1639   0.1838
## Detection Rate         0.2842   0.1925   0.1741   0.1634   0.1838
## Detection Prevalence   0.2845   0.1927   0.1754   0.1634   0.1840
## Balanced Accuracy      0.9994   0.9972   0.9985   0.9984   0.9998
Gradient Boosting Model
## Stochastic Gradient Boosting 
## 
## 15699 samples
##    53 predictor
##     5 classes: 'A', 'B', 'C', 'D', 'E' 
## 
## No pre-processing
## Resampling: Cross-Validated (5 fold) 
## Summary of sample sizes: 12557, 12559, 12558, 12560, 12562 
## Resampling results across tuning parameters:
## 
##   interaction.depth  n.trees  Accuracy   Kappa    
##   1                   50      0.7634258  0.7000048
##   1                  100      0.8310733  0.7861893
##   1                  150      0.8705663  0.8362228
##   2                   50      0.8827317  0.8515435
##   2                  100      0.9413333  0.9257630
##   2                  150      0.9640111  0.9544658
##   3                   50      0.9347726  0.9174364
##   3                  100      0.9718437  0.9643774
##   3                  150      0.9872601  0.9838841
## 
## Tuning parameter 'shrinkage' was held constant at a value of 0.1
## 
## Tuning parameter 'n.minobsinnode' was held constant at a value of 10
## Accuracy was used to select the optimal model using the largest value.
## The final values used for the model were n.trees = 150,
##  interaction.depth = 3, shrinkage = 0.1 and n.minobsinnode = 10.
## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A 1113    5    0    1    0
##          B    3  737    3    7    1
##          C    0   16  679    6    0
##          D    0    1    0  629    3
##          E    0    0    2    0  717
## 
## Overall Statistics
##                                          
##                Accuracy : 0.9878         
##                  95% CI : (0.9838, 0.991)
##     No Information Rate : 0.2845         
##     P-Value [Acc > NIR] : < 2.2e-16      
##                                          
##                   Kappa : 0.9845         
##  Mcnemar's Test P-Value : NA             
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity            0.9973   0.9710   0.9927   0.9782   0.9945
## Specificity            0.9979   0.9956   0.9932   0.9988   0.9994
## Pos Pred Value         0.9946   0.9814   0.9686   0.9937   0.9972
## Neg Pred Value         0.9989   0.9931   0.9984   0.9957   0.9988
## Prevalence             0.2845   0.1935   0.1744   0.1639   0.1838
## Detection Rate         0.2837   0.1879   0.1731   0.1603   0.1828
## Detection Prevalence   0.2852   0.1914   0.1787   0.1614   0.1833
## Balanced Accuracy      0.9976   0.9833   0.9929   0.9885   0.9969
Final Results
##    problem_id predres_rf_test
## 1           1               B
## 2           2               A
## 3           3               B
## 4           4               A
## 5           5               A
## 6           6               E
## 7           7               D
## 8           8               B
## 9           9               A
## 10         10               A
## 11         11               B
## 12         12               C
## 13         13               B
## 14         14               A
## 15         15               E
## 16         16               E
## 17         17               A
## 18         18               B
## 19         19               B
## 20         20               B
