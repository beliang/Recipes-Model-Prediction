# Predicting Ratings of Recipes

## Problem
For our project 5 for DSC 80, we decided to predict the average rating of each recipe by using text data and numerical data to try to fit a model that can predict the average rating. For this particular model, this would be a regression problem. In this model, our response variable is the average rating because being able to predict the average rating based on other confounding factors instead of just the review per user / total number of reviews allows for a user to be able to predict the rating of recipe from this model by looking at the description of the recipe, time to cook, ingredients, etc. To be able to predict the average rating, we will use a RandomForestRegressor to explore more about this regression problem. We will be using the Pipeline .score function to assess the accuracy of our model since it is on a simple scale of 0 to 1.

---
## Baseline Model

For our baseline model, we used 4 simple features, minutes, n_steps, n_ingredients, and sug > carb to attempt to predict the average rating. Since sug > carb is a boolean value, we run a OneHotEncoder transformation on it. Since the values are just boolean values, we do not need to specify drop="First" since there will be no multicollinearity with binary values. In this simple pipeline, we decided to use a linear regressor to fit this data to make a prediction. 

After fitting the training data set and seeing the scores of the training and testing set, we can see how accurate our model is. When .score is called on the training data, we get a R-squared value of 0.002001623990236845 and on the testing data, a R-Square value of 0.0013986603724497337. After the construction and fitting of this model, we can see that this is not a good fit to predict the average rating, as the R-Squared values of both the testing and training set is extremely low. In the final model, we will attempt to vectorize the description and use a RandomForestRegressor to better fine-tune our model. From the score values, we notice that these column values have little to no correlation with the average rating. 

---
## Final Model

From the baseline model, we see that there is no correlation between the columns we have chosen. In the final model, we will improve our model by using a CountVectorizer on the description of the recipe. Additionally, we will use another column, num_reviews to better fine-tune the model. Since the average would depend on the number of reviews, we decided to add this column in our hopes of increasing our scores. For our final model, instead of deciding to use a Linear Regression model, we chose to use the Random Forest Regressor to better model our data. We were deciding between a Decision Tree Regressor and a Random Forest Regressor, but decided to go with the Random Forest Regressor because it is just a forest collection of Decision Tree Regressors. We decided to use a CountVectorizer on the description of the recipe in the hopes that the model would be able to predict the average rating of a certain recipe based on the information from the description. 

In our Random Forest Regressor, we used 2 parameters, max_depth and n_estimators. To find the best hyperparameters, we used GridSearchCV with a cv of 6 to look through all combinations of max_depth and n_estimators to find the optimal values for them. In the end, we achieved a max_dept of 50 and a n_estimators of 70. With a max_depth of 70 of n_estimators of 70, we were able to improve our score to 0.19033842363037878 for the training set and 0.028421564739377292 for the testing set. Since the training score is significantly higher than the testing score, it means that we are slightly overfitting our data. To fix this, we would need to increase the k-fold for our fitting but doing so would take too long to run. 

---
## Fairness Criteria
To see if our model is fair or not we decided to do a permutation test on two groups sugary foods vs non sugary foods. We deemed a food to be sugary if any sugar (PDV) value was above the mean of the 97th percentile. We did this to account for any outliers in the dataset that might be inputted due to mistake. Our evalutation metric will be the R-squared values because we are doing a regression problem and cannot run any accuracy tests since we have no classifier. 
> Null hypothesis: The model is fair 

> Alternative hypothesis: The model is not fair. The R-squared value for not sugary foods is higher than the R-sqaured value for sugary foods

>p-value: 0.96

>Significance level: 0.05

>Test statistic: Difference in means

>Conclusion: Fail to reject the null

Since our p-value comes out to be 0.96, we fail to reject the null. This suggests that our model is fair and there is no unfair rating based on whether the food was considered sugary or not. 
<iframe src="assets/fairness.html" width=800 height=600 frameBorder=0></iframe>

---
## Conclusion
In conclusion, we can see that the average rating is something that is very hard to predict especially in this dataset. In our baseline model, we attempted to use the columns minutes, n_steps, n_ingredients, and sug > carb to make a prediction. We made the assumption that the longer and more ingredients a recipe was needed to cook led to a lower rating due to frustration or time needed. This was apparently not the case as we can from the correlation plot of the dataset, most of the columns have a correlation close to 0 to the average rating. In our final model, we hoped to increase the score of our baseline model by adding text features to predict. We also changed the Linear Regressor to a Random Forest Regressor to better model our data since our data is not linear to average rating. We can see that doing so led to a big increase in the training set and a slight increase in our testing set. This suggests that our model does well on the training data which means that we have overfitted it. Even with a k-fold of 6 we are still overfitting which means we should increase the k-fold size. Doing so, will cause our grid.fit to timeout so we chose to set the k-fold to 6.
