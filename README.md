## recipes_and_ratings_project
**Name(s)**: Ethan Hu, Zhihan Li

### Introduction

### Data Cleaning and Exploratory Data Analysis

| name                             | recipe_id | minutes | contributor_id | submitted  | tags | n_steps | steps | description | ingredients | ... | carbohydrates | hours | days | months | vegetables | beginner | diabetic | beverages | fruit | popularity |
|----------------------------------|-----------|---------|----------------|------------|------|---------|-------|-------------|-------------|-----|---------------|-------|------|--------|------------|----------|----------|-----------|-------|------------|
| 1 brownies in the world best ever| 333281    | 40      | 985201         | 2008-10-27 | ...  | 10      | ...   | [description]| [ingredients]| ... | 6.0           | 1.0   | 0.0  | 0.0    | False      | False    | False    | False     | False | 1          |
| 1 in canada chocolate chip cookies| 453467    | 45      | 1848091        | 2011-04-11 | ...  | 12      | ...   | [description]| [ingredients]| ... | 26.0          | 1.0   | 0.0  | 0.0    | False      | False    | False    | False     | False | 0          |

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
  height="600"
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