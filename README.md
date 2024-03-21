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