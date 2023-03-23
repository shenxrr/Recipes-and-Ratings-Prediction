# Recipes-and-Ratings-Prediction
**Names:** Xinran Shen, Yi Zhang

Our exploratory data analysis on this dataset can be found [here](https://shenxrr.github.io/Recipes-and-Ratings/)

## Framing the Problem

Our predition preblem is to **predict the cooking time(minutes) of the receipes using classification**. 

We used a multiclass classification to classify cooking time(response variable) into three categories: 0-30 mins, 30-60 mins, 60+ mins. We decided to use cooking time as our response variable because the purpose of our project is to investigate the relationship of healthiness and cooking time of a recipes, and classifying cooking time into three categories gives us an idea of how long it takes for the recipe to be cooked. We use features such as nutritions(calories, protein, sugar,etc), n_steps, n_ingredients, to predict the cooking time of a recipe. The metric we used to evaluate our model is accuracy, because it measures the proportion of correctly classified cases in the dataset. At the time of prediction, we would have all the information we mentioned before as features to predict cooking time. 

## Baseline Model

Since our model is used to predict cooking time into three categories: 0-30 mins, 30-60 mins, 60+ mins, we used the following features in our baseline model:

  1) n_ingredients: a quantative variable that contains the information of how many ingredients are needed for the recipe. We have standard scale this variable in our model to ensure the accuracy of the model. We used this feature in our baseline model because the cooking time can be affected by the time it takes to process the ingredients. If a recipe has more ingredients, it tends to have a longer cooking time.
  
  2) calories: a quantative variable that contains the calories in the recipes. We have binarized it according to our interpretation of calories in a recipe. We used a threshold of 400 as we believe a recipe of 400 calories or higher are considered high in calories.

We use the Decision Tree Classifier to perform muliclass classification on the cooking time. We used train-test split on our data set. The current baseline model has an accuracy around 0.45 for both the training set and the testing set, which is a reasonable model but still needs some further improvements. In our final model, we plan to include more features and hyperparameters to improve model performance.

## Final Model

In addition to our baseline model, we decided to add additional features to maximize model performance. We find the best features to include through comparing the accuracy of different pipelines. 

  1) protein, a quantative feature including the amount of protein in percent daily value of the recipe. We have binarized it according to our interpretation of protain in a recipe. We used a threshold of 25 as we believe a recipe of 25 protein or higher are considered high in protein. We added this feature in our final model because recipes with higher protein tends to contain ingredients such as meat, seafood, etc that requires longer cooking time.
  
  2) n_steps, a quantative feature including the number of steps the recipe requires. We have standard scale this feature for better accuracy of the model. We added this feature in our final model because this feature tells us the number of procedures involved in making the recipe. Recipes with more steps to make tend to result in a longer cooking time.

We want to determine the best hyperparameters for our final model. We want to get the most appropriate max_depth to prevent the model from underfitting (low max_depth) or overfitting (high max_depth). Also, the min_sample_split determines the number of samples needed to split, which can limit the complexity of our decision tree. We want to choose the optimal criterion, which is the technique for qualifying the uncertainty in questions. We found the best hyperparameters using a GridSearchCV, that is criterion: 'entropy', max_depth: 7, and min_samples_split: 100. Our final model has an accuracy of 0.53 on the training set and 0.52 on the testing set, which was slightly improved from the baseline model. Since we are doing a multiclass classification model of three classes, we found it difficult to improve accuracy further. 

## Fairness Analysis

To test the fairness of our final model, we evaluate on the year of the submission time of the recipe. The evaluation metric we used here is accuracy. We have divided our data into two groups according to year, the old recipe (2008-2013), and the new recipe (2014-2018). We noticed that the model's accuracy was slightly higher for the old recipes, so we run a permutation test to see if the difference in accuracy is significant.

- Null Hypothesis: Our classifier's accuracy is the same for both new recipe and old recipe, and any differences are due to chance.
- Alternative Hypothesis: The classifier's accuracy is higher for old recipe.
- Test statistic: Difference in accuracy (old minus new).
- Significance level: 0.05.

After running 1000 permutation, we arrive at the p-value of 0.001. Since our p-value 0.001 > 0.05, we reject the null hypothesis that our classifier's accuracy is the same for both new recipe and old recipe.
