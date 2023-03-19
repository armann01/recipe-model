# Deriving Calorie Counts from Recipe Information

### created by Dean Solano and Arman Rahman

## Framing the Problem

Our goal is to build a model based on the recipe and rating information from food.com, predicting the number of calories.

We are trying to predict the calorie count of a meal based on all of the quantitative variables within our data. Some of our features include (n_steps, minutes, mean_rating, n_ingredients, sugar (PDV)).

We will be using a linear regression model to predict the reponse variables which is calorie count. We choose a linear regression model because most of our features were quantitative (our original data only having one categorical variable).

We choose the calorie count as our response variable because we believed it has the most correlation 
with the other quantitative features and would produce the most accurate model. We believed it had the most correlation as we made graphs plotting different combinations of variables and calories had high correlations with the other features in the dataset. 

RMSE will be the metric we use to evaluate our linear regression model. We choose RMSE over MSE because the MSE values sometimes can end up becoming too big and make evaluating our model more tiresome. We felt like RMSE is better because it gives us smaller numbers which are easier to deal with.

All of the quantitative features in the dataset are present at "the time of prediction". So there is no need to exclude any variables due to data leakage.

