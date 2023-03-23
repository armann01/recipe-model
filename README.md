# Deriving Calorie Counts from Recipe Information

Our exploratory data analysis on this dataset can be found [Here](https://armann01.github.io/recipe-analysis/)

### created by Dean Solano and Arman Rahman

## Problem Identification
 Our goal is to build a model based on the recipe and rating information from food.com, predicting the number of calories.

 We are trying to predict the calorie count of a meal based on all of the variables (mostly quantitative) within our data. Some of our features include the number of steps in the recipe, how long it took to make the recipe (in minutes), the average rating of the recipe, and the amount of sugar in the recipe (in Percent Daily Value).

 We will be using a Decision Tree Regressor to predict the reponse variables which is calorie count. We choose a regressior model because most of our features were quantitative (our original data only having one categorical variable).

An example of our working dataset is presented below. 

|    |   minutes |   n_steps |   n_ingredients |   mean_rating |   calories |   total fat (PDV) |   sugar (PDV) |   sodium (PDV) |   protein (PDV) |   saturated fat (PDV) |   carbohydrates (PDV) |
|---:|----------:|----------:|----------------:|--------------:|-----------:|------------------:|--------------:|---------------:|----------------:|----------------------:|----------------------:|
|  0 |        40 |        10 |               9 |             4 |      138.4 |                10 |            50 |              3 |               3 |                    19 |                     6 |
|  1 |        45 |        12 |              11 |             5 |      595.1 |                46 |           211 |             22 |              13 |                    51 |                    26 |
|  2 |        40 |         6 |               9 |             5 |      194.8 |                20 |             6 |             32 |              22 |                    36 |                     3 |
|  3 |       120 |         7 |               7 |             5 |      878.3 |                63 |           326 |             13 |              20 |                   123 |                    39 |
|  4 |        90 |        17 |              13 |             5 |      267   |                30 |            12 |             12 |              29 |                    48 |                     2 |


We choose the calorie count as our response variable because we believed it has the most correlation 
with the other quantitative features and would produce the most accurate model. We believed it had the most correlation as we made graphs plotting different combinations of variables and calories had high correlations with the other features in the dataset. 

RMSE will be the metric we use to evaluate our linear regression model. We choose RMSE over MSE because the MSE values sometimes can end up becoming too big and make evaluating our model more tiresome. RMSE felt like the right choice because it gives us smaller numbers which are easier to deal with. We also didn't use the R squared metric as we wanted to know in numbers how much our model was off by, instead of just evaluating the correlation. 

All of the quantitative features in the dataset are present at "the time of prediction". So there is no need to exclude any variables due to data leakage.

## Baseline Model
 Our model is a multiple feature Decision Tree regression model made to predict the response variable "calories" in our original dataset. Our model contains 11 quantitative features, with the n_steps, n_ingredients, mean_rating, and dessert variables all being discrete. The other seven quantitative variables which include: Total Fat (PDV), Sugar (PDV), Sodium (PDV), Protein (PDV), Saturated Fat (PDV) and Carbohydrates (PDV) are all continuous. In our original dataset, dessert was a categorical nominal variable but it became a quantitative discrete variable in our model because we one-hot encoded it. It is now a quantitative variable because we can perform mathematical operations on it, such as adding up an entire one-hot encoded column to get the total number of desserts from the entire dataset. As stated above, we one-hot encoded the one categorical column we had using the OneHotEncoder() from scikit learn. We put the OneHotEncoder within a column transformer in case we wanted to add any more transformations, which ended up not happening. The column transformer then was put into a pipeline with a Decision Tree Regressor object from scikit learn and that is our entire model! A single OneHotEncoder transformer with a Decision Tree Regressor model. No hyperparameters were tuned in this part of the process, we ended using all default parameters for the model.

The performance of our model is very good, in our opinion. The metric we used to calculate our model performance is RMSE, as we are using a regression model. Our avg RMSE after running the model 20 times was 50. That means that on average our model was off the real calorie value by 50 calories while predicting. That's pretty good, we expected that the model would have an error of around 100 - 200 calories while we were making it. For most people, 50 calories barely makes a difference, so for all general purposes, our model is doing very well. We made sure to perform scikit learn's train test split so that we could evaluate our model's performance on unseen data, and it continued to perform exceptionally! 

## Final Model
For our final model, we decided to add a few more features to increase our model performance. One of our features that we added was a Binarizer feature, which binarized the "minutes" column using a threshold of 60. This feature was added because we saw a difference in the mean amount of calories from recipes taking less than 60 minutes and recipes taking more than 60 minutes. Recipes that take more than 60 minutes had a 110 calorie greater average than recipes that take less than 60 minutes. We felt that adding an identifier to show whether a recipe took more than 60 minutes would help model performance because greater minutes correlated to greater amounts of calories. As shown in the visualization below, there is a difference between mean calories of recipes that take longer versus shorter recipes.

|                           |   Mean Calories |
|:--------------------------|----------------:|
| Recpie Minutes Below Mean |         351.749 |
| Recpie Minutes Above Mean |         469.721 |

 Another feature that was added was a quantile transform of the "mean_rating" column. As shown in the visualization below, a lot of the mean_ratings values are clustered around the 4 to 5 range. 

| mean_rating   |   # of Recipes in Rating Value |
|:--------------|-------------------------------:|
| (0, 1]        |                            571 |
| (1, 2]        |                            619 |
| (2, 3]        |                           2636 |
| (3, 4]        |                          13428 |
| (4, 5]        |                          62370 |

We quantile transformed this feature so that we could have a more normal distrubution of mean ratings by spreading out these clustered values to help us better predict the calorie count. We also added a few more features in the form of one-hot encoded columns. In the original dataframe, there exists a column that holds multiple tags for the recipes. We exctracted a few tags that we believed would help us predict the calorie count. These tags include: "breakfast", "lunch", "brunch", "dinner-party", "desserts" (present in the baseline model), "appetizers", "snacks", "weeknight". These tags would likely help determine rougly how many calories a recipe had, for example, a recipe with the "snacks" tag would probably have less calories than a recipe with a "dinner-party" tag. All of these tags were one-hot encoded as features into our data. As shown in the visualization below, there are differences between the mean of calories whether or not a recipe has a 'breakfast' tag or not, 'lunch' tag or not, etc.

|              |   Difference of Mean Calories When Tag is Present |
|:-------------|--------------------------------------------------:|
| breakfast    |                                         -36.87    |
| lunch        |                                          -4.09743 |
| brunch       |                                         -30.8179  |
| dinner-party |                                           4.5584  |
| desserts     |                                          -8.07256 |
| appetizers   |                                         -69.4886  |
| snacks       |                                         -73.2803  |
| weeknight    |                                          15.6091  |

For our final model, we pivoted away from the Decision Tree Regressor model and to something with a little more accuracy. Our final model used the XGBoost Regressor, which is similar to the Decision Tree Regressor because it also uses decision trees to make predictions. XGBoost Regressor models use gradient boosted decision trees which penalize the leaves of trees that do not improve the model performance. Using XGB Regressor gave our model the boost it needed to perform much better on our data. Tuning hyperparameters for our XGB Regressor model was done by utilizing the GridSearchCV package from scikit learn. We ran grid search on five XGB Regressor hyperparameters, including min_child_weight, gamma, subsample, colsample_bytree, and max_depth. The best hyperparameters for our model ended being 
0.1, 5, 1.0, 1.0, 5, following the order of the hyperparameters in the last sentence. GridSearch was done on a list of three to four potential values for each hyperparameter. Our final model's performance compared to our baseline model's increased significantly. Running the final model 20 times over, while performing sklearn train test split for every iteration, we got an average RMSE of 33. That's over a 50% (51.51% exactly) increase in model accuracy, for a model that was already very good in the first place! We added a lot more features in our final model, and we even changed the type of regressor we used. All of those things definitely contributed to our increase in accuracy. With our new final model, we can predict how many calories a recipe has within a 30 calorie range instead of a 50 calorie range that we had in our baseline model.

## Fairness Analysis 
To evaluate the fairness of our model, we compared its performance on our entire dataset and assessed whether it gave accurate predictions based on a certain subgroup of the dataset. This was achieved by creating a group that was not used in transforming the dataset of our model, or used as a feature. A test was then conducted on a quantity classified version of the dataset dependent on the n_ingredients, and whether a recipe had ‘few’ or ‘many’ recipes. The quantity classifier was done by comparing if a recipe’s n_ingredients value was less than or greater than the mean (which was found to be 9), and adding a {True:’few’,False:’many’} column onto model_data. These were the two groups used in the analysis, Groups ‘few’ and ‘many’. The prediction of the calories was then compared:

| many_ing   |   prediction |
|:-----------|-------------:|
| few        |      332.217 |
| many       |      432.581 |

and it was found that the model predicts a higher amount of calories for recipes with many ingredients. A comparison was also made using the Root Mean Squared Error of the predictions against the actual calorie count of the dataset:

| many_ing   |    rmse |
|:-----------|--------:|
| few        | 31.4325 |
| many       | 19.738  |

and it was found that the model has a lower RMSE for recipes with a higher n_ingredients count, with a difference of approximately -11.70. To ensure that this finding was significant and not due to random chance, we conducted a permutation test to check if the differences  in simulated RMSE was less than or equal to the observed RMSE.
Null Hypothesis: The model is fair, and the RMSE for recipes with less ingredients and more ingredients are mostly the same, and the observed difference was due to random chance.
Alternative Hypothesis: The model is unfair, and the RMSE of the recipes with less ingredients is indeed lower than the RMSE of the recipes with more ingredients. 
After the permutation test of 100 samples and a significance level of 0.05, the p-value was found to be zero. This means that there is statistical support that the model is unfair towards recipes with fewer n_ingredients, and that the model performs much better on recipes with ‘many’ n_ingredients. This can be due to the fact that there are more ‘many’ recipes than ‘few’ recipes, and that recipes with more ingredients generally have a higher chance to contain more calories due to each ingredient only positive calories added to the recipe.
