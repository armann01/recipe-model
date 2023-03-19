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

## Baseline Model
 Our model is a multiple feature linear regression model made to predict the response variable "calories" in our original dataset. Our model contains 11 quantitative features, with the n_steps, n_ingredients, mean_rating, and dessert variables all being discrete. In our original dataset, dessert was a categorical nominal variable but it became a quantitative discrete variable in our model because we one-hot encoded it. It is now a quantitative variable because we can perform mathematical operations on it, such as adding up an entire one-hot encoded column to get the total number of desserts from the entire dataset. As stated above, we one-hot encoded the one categorical column we had using the OneHotEncoder() from scikit learn. We put the OneHotEncoder within a column transformer in case we wanted to add any more transformations, which ended up not happening. The column transformer then was put into a pipeline with a LinearRegression object from scikit lear and that is our entire model! A single OneHotEncoder transformer with a LinearRegression model.

The performance of our model is very good, in our opinion. The metric we used to calculate our model performance is RMSE, as we are using a regression model. Our avg RMSE after running the model around 50 times was 31. That means that on average our model was off by 31 calories while predicting. That's pretty good, we expected that the model would have an error of around 100 - 200 calories while we were making it. For most people, 31 calories barely makes a difference, so for all general purposes, our model is doing very well. We made sure to perform scikit learn's train test split so that we could evaluate our model's performance on unseen data, and it continued to perform exceptionally! 
