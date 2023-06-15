# Predicting Ratings of Recipes

## Problem
For our project 5 for DSC 80, we decided to predict the average rating of each recipe by using text data and numerical data to try to fit a model that can predict the average rating. For this particular model, this would be a regression problem. In this model, our response variable is the average rating because being able to predict the average rating based on other confounding factors instead of just the review per user / total number of reviews allows for a user to be able to predict the rating of recipe from this model by looking at the description of the recipe, time to cook, ingredients, etc. To be able to predict the average rating, we will use a RandomForestRegressor to explore more about this regression problem. We will be using the Pipeline .score function to assess the accuracy of our model since it is on a simple scale of 0 to 1.

---
## Baseline Model

For our baseline model we used 4 simple feautures, minutes, n_steps, n_ingredients, and sug > carb to attempt to predict the average rating. Since sug > carb is a boolean value, we run a OneHotEncoder transformation on it. Since the values are just boolean values, we do not need to specify drop="First" since there will be no multicolinearity with binary values. In this simple pipeline, we decided to use a linear regressor to fit this data to make a prediction. 

After fitting on the training data set and seeing the scores of the training and testing set, we can see how accurate our model is. When .score is called on the training data, we get a R-squared value of 0.0016634202670422482 and on the testing data, a R-Square value of 0.0020559755355559206. After construction and fitting of this model, we can see that this is not a good fit to predict the average rating, as the R-Squared values of both the testing and training set is extremely low. In the final model, we will attempt to vectorize the description and use a RandomForestRegressor to better fine-tune our model. From the score values we notice that these column values have little to no correlation with the average rating. 

---
## Final Model

From the baseline model, we see that there is no correlation between the columns we have chosen. In the final model, we will improve our model by using a CountVectorizer on the description of the recipe. Additionally, we will use another column, num_reviews to better fine tune 
