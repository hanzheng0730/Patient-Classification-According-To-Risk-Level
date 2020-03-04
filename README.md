# Patient-Classification-According-To-Risk-Level
## Dataset
The dataset includes the information of patients who went through different surgical procedures. In order to properly use the hospital resources, the length-of-stay (LOS) of each patient needs to be predicted based on the known information like surgical area, age, all blood test results and etc. LOS of >5 is labeled as 1 while LOS of <=5 is labeled as 0.
## Models selected
LogisticRegression, RandomForest, XGBoost, LightGBM
## Accuracy
LightGBM has the highest AUC for both train set and test set. And the model is neither overfit nor underfit. So we focus on this model and proceed with the deployment analysis.
## Deployment Stratery
Below is the histogram showing the distribution of the Predicted Probability which is labeled as “PredProb” in this study.
The patients were sorted by the predited probability. Then we splited the patients into three bins using pd.qcut(df['PredProb'], 3, labels=["low", "medium", "high"]) so that each bin has the same number of people (~26666). We labeled the bins as "low", "medium", "high".
The first bin is labled "low" that indicates this group of people have low risk to have "status=1". The predited probability is in the range of 0.0055 to 0.3899. The true-positive rate for this bin is 2.72% which is low. So we would like to say our model predicts very well for the low risk people.
The middle bin is labled "medium" that indicates this group of people are not very sure of being positive or negative. The predited probability is in the range of 0.3899 to 0.7144. The true-positive rate for this bin is 15.2%. That indicates the people in this bin is mostly negative.
The last bin is labled "high" that indicates this group of people mostly likely have "STATUS=1". The predited probability is in the range of 0.7144 to 0.9890. The true-positive rate for this bin is 46.9% which is less than 50%. That indicates our model does not perform very well in predicting high risk people. The total true positive rate for the whole dataset is 21.6%. The percentage of the people having LOS>5 in the "high" risk bin is about double that percentage of the whole sample set.
In summary, we would like to strongly suggest deploying our model for predicting low risk people. The result for predicting high risk people could be used as a reference for initial filtering of the patients.
## Parameter Tuning
In the update notebook, parameter tuning using Hyperopt, GridSearchCV and RandomizedSearchCV have been performedused. It was found that Hyperopt (0.8453/0.8189) performs the best, and GridSeachCV result (0.8404/0.8194) is better than RandomizedSearchCV (0.8244/0.8157) in this case. In addition, SHAP has been used to explain the variable importance.
