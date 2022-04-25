---
layout: single
title: "Stroke Prediction"
category: Projects
tags: [Classification, Class Imbalance, Deployment]
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
#classes: wide
tagline: "A probabilistic model to assess the risk for stroke onset"
header:
  overlay_image: /assets/images/Stroke/brain.jpg
  overlay_filter: 0.5
  caption: "Photo credit: [**Unsplash**](https://www.unsplash.com/)"
  teaser: /assets/images/Stroke/brain.jpg
  og_image: /assets/images/Stroke/brain.jpg
---

## Abstract
A stroke is a medical condition that cuts off the oxygen supply to the brain. In 2015, stroke was the second most frequent cause of death after coronary artery disease. Due to its deadly and sudden nature, it is important to be able to predict the onset of strokes. Here I propose a classification model to predict the onset of strokes using features that can easily be collected from patients.

See the presentation slides here([.pdf](https://github.com/JoshJingtianWang/Stroke_Prediction/blob/main/presentation_josh_wang.pdf)/[.ppt](https://github.com/JoshJingtianWang/Stroke_Prediction/blob/main/presentation_josh_wang.pptx)).

## Design
My work allows customers to input simple body metrics such as height, weight, health history, smoking habits, etc. and outputs the probability of the onset of stroke. This work will allow for self-diagnosis by patients and assist in the medical diagnosis by doctors.

## The Data
The dataset was downloaded from here: https://www.kaggle.com/fedesoriano/stroke-prediction-dataset.
The data contains 11 features and 5110 rows, with 249 positive rows and 4861 negative rows.
Input variables include physical metrics of patients such as age, BMI, health history.
The output variable is whether the patient had a stroke (1 or 0).

## Tools
•	The dataset was downloaded from here: https://www.kaggle.com/fedesoriano/stroke-prediction-dataset
•	Categorical variables were one-hot encoded. Missing values were filled in by kNNimputing. Interaction features were generated
•	Undersampling, oversampling, and class weights were used to deal with class imbalance of the dataset.
•	For probabilistic output, KNN, RandomForest, XGBoost and SVC were calibrated with CalibratedClassifierCV().
•	RandomSearchCV and GridSearchCV were used to tune the hyperparameters of Logistic Regression, KNN, RandomForest, XGBoost and SVC.

## Findings
Because I wanted to use the probabilistic output from my models to assess the stroke risk, I decided to use Brier Skill Score (BSS) as the evaluation metric for my models. BSS is based on Brier Score (BS), which measures the residuals of the probabilistic output of classification models, similar to mean squared error (MSE) in linear regression:
*Brier Score*
![Figure 1](/assets/images/Stroke/brier.png "Figure 1")
#### (source: magoo.medium.com)

BSS is BS normalized to the baseline:
*Brier Skill Score*
![Figure 2](/assets/images/Stroke/bss.jpg "Figure 2")
#### (source: wikimedia.org)
If BSS > 0, the model performs better than the baseline; if BSS = 0, the model performs equally to the baseline; if BSS < 0, the model performs worse than the baseline.

Histogram shows the data has severe class imbalance, with only 5% of the data being of the positive class:
*Class Imbalance*
![Figure 3](/assets/images/Stroke/hist.png "Figure 3")

Consequently, I used imblearn's over/under sampling and the built-in class weights hyperparameters of some algorithms to account for the class imbalance.

The pipeline was implemented as such:
~~~
#getting categorical and numerical columns
cat_cols = X_train.select_dtypes('object').columns.tolist()
cont_cols = X_train.select_dtypes('number').columns.tolist()

#One-hot encoding categorical features
cat_pipeline = Pipeline([
            ('encode',OneHotEncoder(handle_unknown='ignore', sparse=False))
])
#scaling numerical features
cont_pipeline = Pipeline([ 
            ('scale',StandardScaler())
])

preprocessor = ColumnTransformer([
    ('cat', cat_pipeline, cat_cols),
    ('num', cont_pipeline, cont_cols)
])
#the pipeline. Using logistic regression in this instance
lr_pipe = Pipeline([
        ('preprocessor', preprocessor),
        ('poly', polynomial),
        ('oversampling', over_sampling.SMOTE()),
        #('undersampling', under_sampling.NeighbourhoodCleaningRule()),
        ('lr', LogisticRegression(solver='liblinear'))
])
#Tuning parameters with RandomizedSearchCV/GridSearchCV
gs_lr = GridSearchCV(lr_pipe,
                    param_grid=params,
                    scoring=BSS_scorer,
                    refit=True,
                    cv=5,
                    n_jobs=-1,
                    error_score='raise',
                    verbose=3)
~~~

I trained and compared 5 models:
*Results*
![Figure 4](/assets/images/Stroke/results.png "Figure 4")
SupportVectorClassifier won out.

## Deployment
WIP

Thank you for reading. The code for my project can be found [here](https://github.com/JoshJingtianWang/Stroke_Prediction). 