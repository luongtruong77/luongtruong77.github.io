---
title: 'Home Credit Loan Default Risk Classification'
subtitle: 'Can we get accepted to the loan that we want when applying?'
featured_image: '/images/loan-default/background.jpg'
---

![](\images\loan-default\money1.jpg)

## Abstract
---
In this project, I will build classification model to predict if potential clients are capable of paying their loans if accepted to help [Home Credit](http://www.homecredit.net/) ensure to not reject people with potentially good credit, also not to mistakenly accept clients who essentially have bad credit scores. I will build several models (Logistic Regression, KNN, Random Forest, XGBoost, etc) and choose the final model to optimize and improve. The final product should help the bank to optimize their process of screening and monitoring their loan applications.

## Design
---
- I will build the model to answer to question of the bank: Is this applicant eligible for loan?
- The model will predict **Yes/No** based on historical credit data and appropriate algorithm.
- I will do **exploratory analysis** on my dataset to seek insights and do **features engineer/selection** to ensure the model has the optimized input.
- I will few baseline models (Logistic Regression, KNN, SGD) and move on in to more advanced models (AdaBoost, XGBoost, LightGBM), compare them and choose the best model to optimize.
- XGBoost is the chosen model based on dedicated metrics (ROC AUC score and F2 score) to be optimized and deployment.

## Data
---
- The dataset is provided by [Home Credit](http://www.homecredit.net/) and can be downloaded via [Kaggle](https://www.kaggle.com/c/home-credit-default-risk/data). 
- The original dataset has over *300,000* data points and *122* features to work with. After doing EDA and features engineering, I have arrived with the final dataset consists of over *300,000* data points and *250* features to build the models.
- **Here is the first few rows of our dataset**:

![](\images\loan-default\sample-data.png)

## Tools
---
- Python
- Pandas
- Numpy
- Matplotlib
- Seaborn
- Scikit-learn
- XGBoost
- LightGBM
- Google Sheets

## Methodology
---
### Exploratory Data Analysis (EDA)

![](\images\loan-default\imbalance-eda.png)

As we can see from the figure above, this is a highly imbalance class problem where the ratio is roughly 11:1. <br/>
So we have over 122 columns, let's see how much of the missing data we have.

![](\images\loan-default\null1.png)

Some of them even have almost **70%** of the records are null. Let's set the threshold to be **20%** and see how many of them are met the threshold:

![](\images\loan-default\null2.png)

So we have 50 features that have more than 20% of rows contain null values and we have to figure out the way to deal with them (except for our buddy XGBoost). Let's explore the distributions of few features before we actually start building our models.

- Number of days employed:

![](\images\loan-default\days-employed.png)

- Distribution of customers' ages:

![](\images\loan-default\ages.png)

- Failure to repay by age group (intuitively, age and income could be the major impact on the application results.)

![](\images\loan-default\failure-to-repay-by-age.png)

- Correlation between sources of income and the target variable:

![](\images\loan-default\corr-income-target.png)

Insights:
- We can see the distribution of days employed is left-skewed, which is easy to understand since in general, people that have stable jobs (higher in number of days employed) are less likely to apply for loans.
- The distribution of ages is just all over the place, which indicates that a job's stability does not necessarily align with people's age.
- We easily see that the younger customers are more likely to fail to repay their loans.
- There are correlations between incomes and target, but not very strong nor weak.

### Features Engineering
- Construct polynomial features between several income sources.

![](\images\loan-default\poly-features.png)

- Construct domain features (such as **percentage of credit vs income, percentage of annuity income vs total income,** etc.). This step is inspired by a Kaggler.

![](\images\loan-default\domain-knowlegde.png)

- Imputting missing values, convert categorial features to binary dummy variables.
- Apply oversampling to deal with imbalanced dataset.

### Building Models
I've tried quite a few algorithms, then choose the best one to optimize:
- Logistic Regression
- K-Nearest Neighbors (KNN)
- Random Forest
- Naive Bayes
- Stochastic Gradient Descent (SGD)
- AdaBoost
- XGBoost (notebook on how I train and tune for this algorithm can be found [HERE](https://github.com/luongtruong77/loan_risk_classification/blob/main/notebooks/XGBoost_train_tune.ipynb))
- LightGBM (notebook on how I train and GridSearch for this algorithm can be found [HERE](https://github.com/luongtruong77/loan_risk_classification/blob/main/notebooks/lightgbm_params_search.ipynb))
- Multilayer perceptron (MLP)

#### Baseline model: Logistic Regression

![](\images\loan-default\log-reg.png)

Since this is our baseline model, its performance is not very good. There are so much more room to improve. <br>
In this particular problem, I want to focus on optimizing **F-2 score (F-2 measure)** instead of either **F1** or **precision/recall** since I would want to put more attention on minimizing **false negatives** (we falsely predict that an applicant is able to pay back when in fact, they are not). <br>
**Note:** We usually choose to optimize **recall** when it comes to default loan risk since when we maximize **recall**, the bank will easily default applicants based on some merits when in fact, if we look closely, there are other factors that might help their cases. This way the bank is not at risk of losing money.<br>
However, I chose **F2** because even though I don't want my clients (the bank) to lose money, I also want to give applicants a higher chance to get approved (focus more on a bit of precision instead of just recall alone). This is where **F2** comes in. <br>

The F2-measure is calculated as follows:

```
- F2-Measure = ((1 + 2^2) * Precision * Recall) / (2^2 * Precision + Recall)
- F2-Measure = (5 * Precision * Recall) / (4 * Precision + Recall)
```

#### Models Comparison

- ROC Curve Comparison:

![](\images\loan-default\roc_curve_comparison_plot.png)

- F2-score Comparison:

![](\images\loan-default\f2_comparison.png)

As we can see, in both cases, XGBoost outperforms its competitors. [XGBoost](https://xgboost.readthedocs.io/en/latest/) is an optimized distributed gradient boosting library designed to be highly efficient, flexible and portable. It implements machine learning algorithms under the [Gradient Boosting](https://en.wikipedia.org/wiki/Gradient_boosting) framework. XGBoost provides a parallel tree boosting (also known as GBDT, GBM) that solve many data science problems in a fast and accurate way. The same code runs on major distributed environment (Hadoop, SGE, MPI) and can solve problems beyond billions of examples. ([XGBoost Documentation](https://xgboost.readthedocs.io/en/latest/)) <br>
It is one of the most optimized state-of-the-art (SOTA) algorithms that wins many data science and machine learning challenges and Kaggle competition. It is highly used to solve tabular data and it is very good at its job. <br>
Now we have our chosen model (XGBoost), let's dig deeper and explore its hypeparameters and optimize it. At the moment, the metrics we have from un-tuned model is:

| ROC AUC       | F-2              |
|---------------|------------------|
| 0.766         | 0.3921           |

#### Fine Tuning XGBoost

After having the algorithm running with different set of hyperparameters, the best set is:

![](\images\loan-default\best-params.png)

| ROC AUC       | F-2              |
|---------------|------------------|
| 0.768         | 0.4320           |

The full notebook on fine-tuning process can be found [here](https://github.com/luongtruong77/classification-loan-default-risk-detection/blob/main/notebooks/XGBoost_train_tune.ipynb)

### Confusion Matrix and Precision/Recall Curve

![](\images\loan-default\confusion-matrix-precision-recall-curve.png)

### Feature Importance

![](\images\loan-default\feature-important.png)

---
**Thank you for your time!** If you're interested in this project, you can see the source code [here](https://github.com/luongtruong77/classification-loan-default-risk-detection) on Github, or you can reach out to me [here](https://www.linkedin.com/in/luongtruong77/) on LinkedIn.

