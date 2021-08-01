---
title: 'Cars Price Predictor with XGBoost'
subtitle: 'How much is your car worth?'
featured_image: '/images/cars-price/background.jpg'
---

![](/images/cars-price/car1.jpg)

---
## Abstract
In this project, I will use the data scraped from [cars.com](https://www.cars.com/) and build predictive model for people who want to predict the car's price. Buying and selling used cars is always a big decision if we have little-to-no knowledge about the market or automotive industry. After gathering data, I will analyze and seek insights from it before start modeling. In order to optimize my results, I engineer features from the original dataset and try multiple algorithms and present the outcomes. Furthermore, I also build the interactive web app to predict the car's price based on user's input features.

Please try out the [web app](https://car-predictor-regression.herokuapp.com/) using the XGBoost model that I've trained in this project.

---
## Data
The dataset is scraped from [cars.com](https://www.cars.com/) using `BeautifulSoup` and being analyzed in `Jupyter notebook` with `Python` and its libraries. The original dataset has `187168` rows and `15` columns. I engineer more features and drop irrelevant features to achieve the ready-to-work-with data set that has `122351` rows and `18` columns, `8` of which are categorical features.

![](/images/cars-price/car-detail.png)

Also, here is the 10 random samples from the full dataset where `Make == 'Tesla'`:

![](/images/cars-price/tesla-sample.png)

---
## Tools
- BeautifulSoup, Numpy and Pandas for data scraping, data cleaning and manipulation.
- Matplotlib, yellowbrick and Seaborn for plotting and visualizing.
- Scikit-learn and xgboost for modeling.
- Streamlit and Heroku for building interactice app and deploying model to the cloud.

---
## Methodology
### Exploratory Data Analysis (EDA)

- Correlation between `price` and other features:

![](/images/cars-price/corr1.png)

We can easily see that `Model Year` and `Num_ent_features (Number of entertainment features)` are positively correlated with the `price`, whereas `MPG` and `Mileage` are negatively correlated. This is understandable and expected.

- Correlation between `price` and `Model year`:

![](/images/cars-price/corr2.png)

In general, we would expect that the newer the car, the more expensive it is. However, there is a slight difference between 2001-2010: the price does not change very much. One of the most exciting aspects of data science is to find out the interesting insights that we generally do not think of. Apparently, the value of the cars seems to decrease drastically within the first 5-10 years since being bought, but if appropriately maintained, they would not lose much of their value afterward.

### Feature Engineering
I observe that some features of the dataset contains different information, some of which can be indepent feature, I decide to extract them from the original features.
- Extract the `Make, Car Model, Model Year` from `Name`.
- Construct polynomial features.
- Convert categorical features to binary dummy variables to be suitable for buidling models.
- Try to build the models on different combination of numerical and categorical features.

### Models Building
- I use linear regression, polynomial regression, random forest regressor, gradient boosted regressor, and extreme gradient boosting (XGBoost). Linear model performs very good, but the XGBoost has the best performance. Feature importance ranking is extracted from the XGBoost model to refine the models.
- I choose R^2 score and RMSE as my metrics. The entire dataset is split into 60-20-20 train/validation/test to train and test models. I also use L1 and L2 regularization to fine-tune my models. Later after narrowing down to 2 best candidates (Linear and XGBoost), I again split the data into 80-20 and using K-fold cross validation to conclude the results.
- Final performance results:

| Algorithms           | R^2           | RMSE             |
|----------------------|---------------|------------------|
| Linear Regression    | 0.869         | 4787.90          |
| Lasso                | 0.866         | N/A              |
| Ridge                | 0.865         | N/A              |
| Random Fores         | 0.860         | N/A              |
| **XGBoost**              | **0.955**         | **2788.11**          |

### Models Selection
Even though we clearly have the winner, I still also want to test out my two best models (Linear Regression and XGBoost) to see how they performce against each other.

- Case 1: 2017 Chevrolet Camaro 2SS

![](\images\cars-price\predict1.png)

- Case 2: 2018 Infinity Q60 3.0t LUXE

![](\images\cars-price\predict2.png)

In both cases, the XGBoost model outperforms the Linear Regression models with the predicted values closer to the true values.

#### Feature importance of XGBoost model

![](\images\cars-price\feature-importance.png)

Looking the chart, we can see that the `Mileage`, `Year` and `MPG` are the most important features to predict the car's price.

---
## Model Deployment
So we have our final model; what's our next step? No matter how accurate our model is; if we can not deploy it into the production environment, there would be no use. Luckily for me, [streamlit](https://streamlit.io/) and [heroku](https://www.heroku.com/) provide an easy way to put my data science model into production as an web application with minimal afford. For more detail on how to use streamlit and heroku, please visit their docs [here](https://streamlit.io/) and [here](https://www.heroku.com/).

![](\images\cars-price\app1.png)

![](\images\cars-price\app2.png)

---
**Thank you for your time!** If you're interested in this project, you can see the source code [here](https://github.com/luongtruong77/regression-car-price-predictor) on Github, or you can reach out to me [here](https://www.linkedin.com/in/luongtruong77/) on LinkedIn.
