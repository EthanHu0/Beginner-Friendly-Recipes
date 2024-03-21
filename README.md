## recipes_and_ratings_project
**Name(s)**: Ethan Hu, Zhihan Li

### Introduction

### Data Cleaning and Exploratory Data Analysis

name
recipe_id
minutes
contributor_id
submitted
tags
n_steps
steps
description
ingredients
...
days
months
vegetables
beginner
diabetic
beverages
fruit
popularity_x
beginner_name
popularity_y


Some columns, like 'nutrition', contain values that look like lists, but are actually strings that look like lists. You may want to turn the strings into actual lists, or create columns for every unique value in those lists. For instance, per the data dictionary, each value in the 'nutrition' column contains information in the form "[calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), and carbohydrates (PDV)]"; you could create individual columns in your dataset titled 'calories', 'total fat', etc.


<iframe
  src="assets/distribution-of-cooking-steps.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

This graph displays the distribution of the number of cooking steps for recipes.

We try to find out what relates to the rating of a recipe, how long it tekes, how complicated it is or how fattening it is?

<iframe
  src="assets/scatter-plot-cooking-time-vs-average-rating.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/scatter-plot-n-steps-vs-average-rating.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/scatter-plot-fat-content-vs-average-rating.html"
  width="800"
  height="200"
  frameborder="0"
></iframe>

It appears that fat content has the strongest correlation with average rating among these three variables.

## Interesting Aggregates

<iframe
  src="assets/pivot-table.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

This suggests that recipes containing fruit and beverages generally have shorter cooking times compared to recipes without these ingredients.

### Assessment of Missingness

In our data analysis journey, understanding and addressing missing data is paramount to ensure the integrity of our insights. This step involves a meticulous examination of missingness patterns within our dataset to make informed decisions on how to handle them effectively.

NMAR Analysis

We delve into the concept of Not Missing at Random (NMAR), where the absence of data is dependent on the column itself. In our case, the 'rating' column exhibits NMAR characteristics, primarily due to our data cleaning process. By replacing zero ratings with null values, we ensure the integrity of our analysis, as a zero rating often signifies no ratings rather than low scores. Similarly, the 'avg_rating' column showcases NMAR properties, as the mean rating calculation process carries forward these null values.

Missingness Dependency

We conduct permutation tests to discern dependencies between missingness and other columns. For instance, we investigate whether the missingness of the 'rating' column depends on whether a recipe is labeled as diabetic. Our hypothesis testing, employing a significance level of 0.05, reveals no significant association between diabetic recipes and missing ratings, implying that the absence of ratings is unrelated to their diabetic categorization.

Graphical Analysis

To visually grasp these dependencies, we utilize histograms to illustrate the relationship between missingness and other factors. The histogram "Number of Cooking Steps by Missingness of Recipe Rating" portrays how the presence or absence of ratings correlates with the number of cooking steps. Additionally, the histogram "Whether the Recipe is Diabetic by Missingness of Recipe Rating" provides insights into how diabetic classification relates to missing ratings.

Through this comprehensive assessment, we gain a deeper understanding of missingness patterns in our dataset, enabling us to make informed decisions in subsequent analyses and data handling processes.

<iframe
  src="assets/number-of-cooking-steps-by-missingness-of-recipe-rating.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/whether-recipe-is-diabetic-by-missingness-of-recipe-rating.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Hypothesis Testing

Is there a difference in the number of ingredients between beginner and non-beginner recipes?

H0: There is no significant difference in the number of ingredients used (n_ingredients) between recipes labeled as beginner and recipes not labeled as beginner.
H1: Recipies labeled as beginner tend to have less ingredients used (n_ingredients) compared to recipies that are not labeled as beginner.

Test of Test: Permutation Test
Test Statistic: Given the similarity in the distributions of number of cooking steps across recipes labeled as 'beginner' and those not labeled as such, we select difference in mean (mean_diff) as our test statistic to assess whether there is a significant difference in the number of cooking stepsbetween the two groups.
Significance Level (α): 0.05

<iframe
  src="assets/number-of-ingredients-required-for-recipes.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/number-of-ingredients-histogram.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


Based on the permutation test with 500 repetitions, the obtained p-value of 0.0 indicates strong evidence to reject the null hypothesis. This suggests a significant difference in the mean number of ingredients required for recipes between beginner and non-beginner tags.

### Framing a Prediction Problem
In the hypothesis testing section, we conducted tests to determine whether the distribution of various recipe features was influenced by the 'beginner' tag. Our analysis revealed that the 'beginner' tag does impact the values of several features. Consequently, we believe that the presence of the 'beginner' tag on a recipe can be predicted based on the features it possesses. Developing such a predictive model could prove valuable for efficiently tagging new recipe entries by features they have.

### Baseline Model
We want to create a model that predicts whether a recipe should be tagged as beginner based on the features that recipe has. 

According to the outcomes of our hypothesis tests, it has been established that certain recipe features exhibit significant predictive capabilities regarding whether a recipe should be labeled as beginner:

n_steps
avg_rating
n_ingredients
popularity

In addition to the recipe features mentioned earlier, there could be other factors that contribute to predicting beginner tags. While these factors might not have been as prominent in our hypothesis tests or may not have been addressed, we believe they could still play a role in predicting beginner tags. Some of these additional features, based on our intuition, include:

minutes, as it makes sense for beginner recipes to take a shorter time to cook since they are easier.
the presense of the word 'beginner' in description
the presense of the word 'beginner' in name, beginner_name
contributor_id, as some contributors might have a tendency to publish beginner recipes

We noticed an interesting column, calories, which contains values that are equal to zero. This is quite counterintuitive because everything except water will have some calories. For fun, in our modeling, we will also utilize the calories column.

However, since we are attempting to build a model that assists in determining whether a newly entered recipe should be tagged as beginner, it makes sense to avoid using recipe features related to interaction, as interactions won't occur until someone has used it. Based on this consideration, we will be using the following features to train our model:

n_steps
n_ingredients
minutes
beginner_name
contributor_id
calories (for fun)

For efficiency, we will create a DataFrame consisting of a subset of features from the original food DataFrame that are relevant to our model creation. Additionally, we will retain only unique rows based on the 'recipe_id' column, as we will not be dealing with interaction data.

<iframe
  src="assets/model_data.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

After using Pipeline to create a Linear Regression model that uses multiple recipe features to predict thebeginner tag. Our baseline model achieved an accuracy score of 0.8609 in training, indicating that approximately 86.09% of the data points in the training set were correctly labeled by our model. Additionally, the baseline model achieved an accuracy score of 0.8602 on testing, suggesting that about 86.02% of the unseen data points were correctly labeled. Both the training and testing scores are high and similar in value, indicating that our model performs equally well on both seen and unseen data.

### Final Model
In an effort to enhance model performance beyond the feature engineering conducted during baseline model training, we aim to further improve the features by transforming. Thoughtfully tuning the hyperparameters of our decision tree could also aid in enhancing our model performance. The hyperparameters in the baseline model were largely selected arbitrarily, so careful consideration of these parameters may lead to improvements.

<iframe
  src="assets/heatmap_data.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Fairness Analysis
One feature we utilized in our model to predict the beginner tag is the cooking time of the recipe in minutes. It's intuitive that beginner recipes typically have shorter cooking times compared to non-beginner recipes due to their lower complexity. After arbitrarily setting 30 minutes as the threshold to distinguish between recipes with shorter cook times (30 minutes or less) and those with longer cooking times (above 30 minutes), we observed that approximately 58% of recipes in the beginner group fell into the short cook time category, while about 42% did so in the non-beginner group. While these percentages align with our intuition, we are concerned that they are not significantly different. We worry that relying on cooking time as a predictive criterion might increase the chance of non-beginner recipes being falsely labeled as beginner recipes. Given our concern about the high false positive rate, we will focus on precision as the error metric to evaluate the fairness of our model.

cook_time
long     0.819396
short    0.842481
dtype: float64

<iframe
  src="assets/fair_df.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
Observed precision: 0.12131751227495902
P-value: 0.022
successfully reject the null hypothesis

We obtained a p-value that is greater than the significance level. Therefore, we fail to find enough evidence to reject the null hypothesis. Conducting this test leads us to conclude that our model fairly labels recipes with both long and short cook times.