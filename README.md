
# Bank TeleMarketing Prediction





## Problem Statement & Goals
From May 2008 to June 2013, a Portuguese retail bank conducted a direct telemarketing campaign to sell long term deposits to its exisiting clientele. Given the binary successful or unsuccessful contact, our goal is to model the success of subscribing a long-term deposit using attributes that were known before the telemarketing call was executed . For the best given predictive
model, an additional goal is to achieve an ROC AUC score greater than or equal to 0.80. This criteria is inspired by the following [research paper](https://github.com/Jleung1996/Bank-TeleMarketing-Prediction/blob/main/Relevant%20Information/Research_paper.pdf) where the researcher used the same dataset and achieved their best ROC AUC score of 0.80.
 

## Dataset Information
The dataset was taken from [UCI Machine Learning Repository](http://archive.ics.uci.edu/ml/datasets/Bank+Marketing). There are 4 variants of this dataset, of which we selected the most completed one ('bank-additional-full.csv') which contained 41188 entries with 20 features. 
The dataset was gathered in the period May 2008 to June 2013 where the call agent recorded manually each successful or unsuccesful calls. The researcher than further enriched the data with social and economical influences taken from the central bank of Portuguese Republic statistical website.More information on each features are laid out in the appendix.

## Performance Metric Information
The dataset we are using is inbalanced(89% failure, 11% success). Because of this, metrics such as accuracy is misleading and inapporiate for our problem(ie: if we use model that always predict failure, model will have an accuracy of 89%).
There were two metrics that we considered: F1 score & Receiver Operating Characteristic Area Under Curve Score (AUROC). To be aligned with the given research paper, AUROC was selected. 
ROC Curve is a probablitity curve that plots True Positive Rate against False Positive Rate in various different thresholds. The area under the ROC curve (AUC) is the measure of the ability of the model to differentiate between the two binary classes, and is used to summarized the ROC curve.
The AUC score is in the range  0 to 1,where AUC=1 means the model is able to distinguished the two binary classes perfectly. AUC=0.5 is used as a baseline for all predictive models because when AUC=0.5, it means the model is predicting at random for all data points. Typically, AUROC is
use to compare various models and models with AUC score > 0.8 is considered excellent discrimination.

## Methods
We began the project by cleaning the data. The stock csv file required a custom parser to format the data into traditional tabular format
form. Once that was done, we performed Elementary Data Analysis(EDA) for both numerical and catergoical features using various techniques such as correlation analysis,kernal density, and summary statistics.
From EDA and problem statement, we then built several binary classification models like Logistic Regression, RandomForest, XGBoost, and CatBoost. For each model, we fine tune the parameters using feature selection and hyperparameter tuning(GridSearch). In addition, each model is
cross-validated using the train/test split (90%/10% split) & 5-Fold cross-validation. We then picked the model with best cross-validation AUC score, and fit the testing data into it. In the final step, we did feature analysis on our best model to help us draw real world conclusions. For more details regarding the procedures, please check Jupiter notebook.


## Best Model Results
Among the models, XGBoost after Hyperparameter tuning is the best model,achieving a cross-validated ROC ACU score of 0.80. We then tested
our best model with the testing dataset and achieving an AUC score of 0.83. The slight increase in score is most likely due to the size and data variation in the
training and testing set. Our model has a relatively high precision score(0.76) to recall score (0.31) ratio. This means that our model returns slightly fewer positive results(subscribing to a long term loan),but each of the predicted positive results are labeled correctly.

In addition to creating a good model, interpretability of our model is very important(ie: feature importances).
Since XGBoost algorithm is a blackbox, we employed SHAP values. For our model, nr.employed and euribor3m contributed the most when predicting a successful/unsuccessful contact.

<sub>Models with Training and CV AUC</sub>

![](Relevant%20Information/TrainingCVAUC.png)


<sub>XGBoost with Hyperparameter on testing data: AUROC & confusion matrix</sub>
![](Relevant%20Information/BestModel.png)






## Conclusion
After the 2008 Global Financial Crisis, many European banks had to drastically change their debt to captial ratio. Particularly Portugese banks were
pressured to increase their captial requirements(e.g. capturing more long-term deposits). Similarly in 2022 
with the COVID-19 pandemic, rapid increase of inflation, and the uncertainty of the European conflicts, Portugese banks like their European and American counterparts
are taking a more cautionary approach(e.g. increasing captial holdings and reducing operational cost). With this in mind, deploying a decision support system using
machine learning algorithms to predict the results of a telemarketing call for long-term deposits is a valuable tool for bank campaign managers. In
this project, we were able to show algorithms such as XGBoost is able to discriminate successful/unsuccesful calls quite distinctively. Futhermore, using SHAP values and various differently graphs, we were able to increase model interpretability even for black box algorithms such as XGBoost. 
We were able to see that both national number of employee and the three month Euribor rate were the most releveant model feature and their effects on our predictions.
For example, with a high three month Euribor rate(short-term interest), it reduces the chance a call is successful in the XGBoost model. This bears some similarity with economic behaviors since attractive short term interest rates
make long-term rates unattractive. 

For our verdict, we suggest the Portugese bank manager to investigate the social and economic conditions when embarking a telemarketing campaign for long-term deposits, and to further invest into a DSS based one data driven models to reduce operational cost and increase telemarketing efficiency.




## Appendix


<sub>Shap Value Beeswarm Graph</sub>

![](Relevant%20Information/Shap.png)


### Attribute Information(Taken from UCI Machine Learning Repository):

Input variables:
#### bank client data:
1 - age (numeric)\
2 - job : type of job (categorical: 'admin.','blue-collar','entrepreneur','housemaid','management','retired','self-\
employed','services','student','technician','unemployed','unknown')
3 - marital : marital status (categorical: 'divorced','married','single','unknown'; note: 'divorced' means divorced or widowed)\
4 - education (categorical: 'basic.4y','basic.6y','basic.9y','high.school','illiterate','professional.course','university.degree','unknown')\
5 - default: has credit in default? (categorical: 'no','yes','unknown')\
6 - housing: has housing loan? (categorical: 'no','yes','unknown')\
7 - loan: has personal loan? (categorical: 'no','yes','unknown')
#### related with the last contact of the current campaign:
8 - contact: contact communication type (categorical: 'cellular','telephone')\
9 - month: last contact month of year (categorical: 'jan', 'feb', 'mar', ..., 'nov', 'dec')\
10 - day_of_week: last contact day of the week (categorical: 'mon','tue','wed','thu','fri')\
11 - duration: last contact duration, in seconds (numeric). Important note: this attribute highly affects the output target (e.g., if duration=0 then y='no'). Yet, the duration is not known before a call is performed. Also, after the end of the call y is obviously known. Thus, this input should only be included for benchmark purposes and should be discarded if the intention is to have a realistic predictive model.
#### other attributes:
12 - campaign: number of contacts performed during this campaign and for this client (numeric, includes last contact)\
13 - pdays: number of days that passed by after the client was last contacted from a previous campaign (numeric; 999 means client was not previously contacted)\
14 - previous: number of contacts performed before this campaign and for this client (numeric)\
15 - poutcome: outcome of the previous marketing campaign (categorical: 'failure','nonexistent','success')
#### social and economic context attributes
16 - emp.var.rate: employment variation rate - quarterly indicator (numeric)\
17 - cons.price.idx: consumer price index - monthly indicator (numeric)\
18 - cons.conf.idx: consumer confidence index - monthly indicator (numeric)\
19 - euribor3m: euribor 3 month rate - daily indicator (numeric)\
20 - nr.employed: number of employees - quarterly indicator (numeric)

#### Output variable (desired target):
21 - y - has the client subscribed a term deposit? (binary: 'yes','no')



