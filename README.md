# Project-Analysis
This is a project of DSC 80 at University of California San Diego that analyzes the relationship among average ratings of recipes and times required of recipes.

---
## Introduction

The raw dataset `RAW_interactions.csv` contains information on the ratings and reviews commented by different users based on the recipe they followed while the `RAW_recipes.csv` contains information on the details such as the working time, nutrition, and number of steps of each recipe.

In this project, we focus on finding **the relationship between the cooking time and average rating of recipes**, which will provide more **personalized suggestions** to people while they are finding suitable recipes. The users will be able to make more informed choices on the recipes they prefer and the expected time they need to spend. Moreover, this is also helpful to recipe makers who will gain a more **comprehensive understanding** of which type of recipes is able to provide them high ratings and popularity.

There are **731927 rows** in the `interactions` dataset, and we will focus on the columns: 
> `user_id` : the user ID on the website \
> `recipe_id` : the ID of the recipe \
> `date` : the date of the interaction \
> `rating` : the rating given on the recipe columns

At the same time, there are **83782 rows** in the `recipes` dataset, and we will focus on the columns: 
> `name` : the name of the recipe \
> `id` : the id of the recipe \
> `minutes` : how long does it take to work on the recipe \
> `contributor_id` : the ID of the user who submitted the recipe \
> `submitted` : the date when the recipe was submitted

---
## Cleaning and EDA (Exploratory Data Analysis)

### Data Cleaning

- First, we left merge the `recipes` and `interactions` datasets together on `recipe_id` to clean the data because the analysis would be easier to conduct and understand if the **cooking time** and **ratings** are on the **same** dataframe. We fill all the ratings of 0 with `np.nan` since the rating should be in the range of 1 to 5 (inclusive), and a 0 means the user did not fill out the rate.
- Then, we add an additional column of the average rating `avg_rating` of each recipe on the `merged` dataframe by grouping it by the `recipe_id` and drop the unrelated columns. The head of the `merged` dataframe is shown below:

| name                               |   recipe_id |   minutes |   contributor_id |    submitted |   avg_rating |       user_id |   recipe_id |         date |   rating |
|:----------------------------------:|------------:|----------:|-----------------:|-------------:|-------------:|--------------:|------------:|-------------:|---------:|
| 1 brownies in the world best ever  |      333281 |        40 |           985201 |   2008-10-27 |            4 |        386585 |      333281 |   2008-11-19 |        4 |
| 1 in canada chocolate chip cookies |      453467 |        45 |          1848091 |   2011-04-11 |            5 |        424680 |      453467 |   2012-01-26 |        5 |
| 412 broccoli casserole             |      306168 |        40 |            50969 |   2008-05-30 |            5 |         29782 |      306168 |   2008-12-31 |        5 |
| 412 broccoli casserole             |      306168 |        40 |            50969 |   2008-05-30 |            5 |   1.19628e+06 |      306168 |   2009-04-13 |        5 |
| 412 broccoli casserole             |      306168 |        40 |            50969 |   2008-05-30 |            5 |        768828 |      306168 |   2013-08-02 |        5 |


- Finally, we clean the `interactions` dataset by dropping the `review` column, which we will not focus on. The head of the cleaned `interactions` dataframe is shown below: 

|    user_id |   recipe_id |         date |   rating |
|-----------:|------------:|-------------:|---------:|
|    1293707 |       40893 |   2011-12-21 |        5 |
|     126440 |       85009 |   2010-02-27 |        5 |
|      57222 |       85009 |   2011-10-01 |        5 |
|     124416 |      120345 |   2011-08-06 |      nan |
| 2000192946 |      120345 |   2015-05-10 |        2 |


- We also keep the columns we are using, which are `name`, `recipe_id`, `minutes`, `contributor_id`, `submitted`, and `avg_rating`, in the rest of our analysis in the `recipes` dataset and add the `avg_rating` column to it for our future analysis on the relationship between the cooking time and average rating of recipes. The head of the cleaned `recipes` dataframe is shown below:

| name                               |   recipe_id |   minutes |   contributor_id |   submitted | avg_rating |
|:----------------------------------:|------------:|----------:|-----------------:|------------:|-----------:|
| 1 brownies in the world best ever  |      333281 |        40 |           985201 |  2008-10-27 |          4 |
| 1 in canada chocolate chip cookies |      453467 |        45 |          1848091 |  2011-04-11 |          5 |
| 412 broccoli casserole             |      306168 |        40 |            50969 |  2008-05-30 |          5 |
| millionaire pound cake             |      286009 |       120 |           461724 |  2008-02-12 |          5 |
| 2000 meatloaf                      |      475785 |        90 |          2202916 |  2012-03-06 |          5 |

---

### Univariate Analysis

- Here is the distribution of `minutes` (excluded extreme outliers) in our adjusted recipes dataset `recipes_new`:
<iframe src="assets/minutes_distribution.html" width=800 height=600 frameBorder=0></iframe>

As we can see **most** recipes takes less than 3 hours (180 minutes) to complete. Moreover, we excluded some extremely large outliers that could potentially make our estimation biasedly high. Since there are only **a few** of them, excluding those values would not introduce significant influence to our analysis.


- Here is the distribution of `ratings`, in our adjusted interactions dataset `interactions_new`:
<iframe src="assets/ratings_distribution.html" width=800 height=600 frameBorder=0></iframe>

In this distribution of ratings across various recipes, we can observe that **most** recipes have the **highest rating score, 5**, which inspired us to explore  that whether there is a relationship between the timing and the rating of the recipe.

Later on, we will study on this question. Now, let us take a look at bivariate analysis of our dataset.

---

### Bivariate Analysis

- Here, we present the scatter plot of **timing vs. rating** of the recipes that takes *less than 180 minutes* to work on:
<iframe src="assets/minutes_less_than_180_vs._avg_rating.html" width=800 height=600 frameBorder=0></iframe>

At the first glance, you may think the scatter plot looks kind of strange. However, we can take a closer look at the **upper left** part of this graph, which has a **much more** dense concentration of points than other parts in the graph. This graph also tells us that the `x-axis` represents `minutes` that a recipe requires, and the `y-axis` represents the `average rating` of a recipe. Therefore, we can extract a message from this graph that it shows a trend that **the less time a recipe takes, the higher possibility it has to get a higher rating**. We will explore this trend in later parts.


- Here, we present the scatter plot of **timing vs. rating** of recipes that takes *more than 180 minutes* to work on:
<iframe src="assets/minutes_greater_than_180_vs._avg_rating.html" width=800 height=600 frameBorder=0></iframe>

In the graph of recipes that takes more than 180 minutes (3 hours), we can still see the pattern that the rating of 5 is **more likely** to appear on those recipes that take **less time**. 

---
### Interesting Aggregate

| Minutes_Groups   |   1.0 |   2.0 |   3.0 |   4.0 |    5.0 |
|:-----------------|------:|------:|------:|------:|-------:|
| 0 - 180          |  2548 |  2119 |  6549 | 34575 | 158181 |
| 181 - 720        |   293 |   218 |   565 |  2466 |   9901 |
| 721 - 1440       |     6 |     7 |    23 |   101 |    490 |
| 1441 - 10080     |    22 |    23 |    32 |   147 |    928 |
| 10081 - 20160    |     0 |     1 |     2 |     6 |     36 |
| 20160 -          |     1 |     0 |     1 |    12 |    137 |

As shown in the above pivot table, we used the `merged` dataframe, which has the information of the number of ratings in each recipes. By separating these recipes according to their minutes used, we can observe that **most people** would actually try or make ratings on recipes that take **relatively shorter time** as compared to recipes that take longer time. Starting from here, we would want to observe that whether there is a relationship among `minutes` and `avg_rating` columns in our dataset.

---
## Assessment of Missingness

For the `merged` dataset, there is one row that has missing `user_id` and `date`, which means that this recipe was not reviewed, and it is not in the `interactions` dataset so we drop the row. Therefore, for the revised `merged` dataset, there are three columns contain missing values, `name`, `avg_rating`, and `rating`. 

### NMAR Analysis
First of all, for the only missing value in the `name` column, we conclude it as **Missing Completely At Random (MCAR)**. Since all the values in `recipe_id`, `minutes`, `contributor_id`, and `submitted` are filled, the probability of a missing name is unrelated to any other columns, and also it is unlikely that the name is missing because of some properties of itself. 

Moreover, the missingness of `avg_rating` relates to the missingness of the `rating` column, so it is **Missing At Random(MAR)**, not NMAR. Thus, through observing the missingness of the `rating` column, as permutation test shown in the later part, the missingess of the `rating` column has a relationship with the `n_step` column, so we also conclude that the missingness of `rating` column is also **MAR**, not NMAR. All in all, we do not observe NMAR missingness in our dataset's three columns that has missing values.

---
### Missingness Dependency

- We see that the 'rating` column is the one that has most missing values, therefore, we would like to investigate on the missingness of that column.

1. In our analysis, we tried to explore whether the missingness of the `rating` column depends on the number of steps, `n_steps`.

    Null hypothesis:
    > The observed difference of distributions among recipes that have ratings and recipes that do not have ratings is because of random chance.

    Alternative hypothesis:
    > The observed difference is not due to random chance of those two groups.

    - The significance level is at 5%.

    Here is the graph showing where the observation lies in the distribution of  `n_steps` when `rating` is missing and when `rating` is not missing.
    <iframe src="assets/n_steps_by_missingness_of_ratings.html" width=800 height=600 frameBorder=0></iframe>
    
    - As we can see in the above graph, the distribution of step numbers when `rating` is missing has a **bimodal** distribution, which is different from that of the distribution of *step numbers* when `rating` is not missing. Thus, we think an appropriate test statistic in this situation is the **Kolmogorov-Smirnov** test statistic. Without looking **in such a detailed scope** we may say that these two distribution looks similar, but if we compare the **cdf**s of these two distribution, we would realize that the difference is apparent.

    - Analysis process: \
        The missingness of the `rating` column may depend on `n_steps` a recipe requires. As there are a proportion around **0.064135** of data in the `rating` column are missing, we would like to make a **permutation test** to see if the missingness of the `rating` of a recipe actually depends on how many steps it has.

        Here, we have the **empirical distribution** of our permutation test, and we can see that the **observation** lies almost **outside** the whole empirical distribution, which implies that we should **reject** the null hypothesis of this permutation test.
    <iframe src="assets/rating_vs._nsteps1.html" width=800 height=600 frameBorder=0></iframe>

    - Result: \
        Accoridng to the permutation test we conducted, the **p-value** is **0.0**, which is much **smaller** than our significant level of **0.05**. Thus, we **reject** the null hypothesis. 

2. In our analysis, we also tried to explore whether the missingness of the `rating` column depends on the recipe has *wheat* in its `ingredients` or not.

    Null hypothesis:
    > The missingness of rating of recipes has no relationship with whether a recipe's ingredients has wheat.

    Alternative hypothesis:
    > There is a relationship among the missingness of rating and whether a recipe's ingredients has wheat.

    - The significance level is at 5%.

    Here is the graph showing the number of recipes have *wheat* in their `ingredients`, and that of recipes do not have *wheat* in their `ingredients`. We can see that there is **no apparent differences** among the two groups.
    <iframe src="assets/has_wheat_and_rating_missingness.html" width=800 height=600 frameBorder=0></iframe>

    - Analysis Process: \
        As we think the missingness of the `rating` column, which has a proportion around **0.064135** missing, may relate to whether it has *wheat* in its `ingredients` or not. Thus we performed a **permutation test** again to investigate this question.

        Moreover, we have the **empirical distribution** of our permutation test, and we can see that the **observation** lies in the position that is **close to the middle**, which supports out assertion that we **fail to reject** the null hypothesis.
    <iframe src="assets/rating_vs._has_wheat1.html" width=800 height=600 frameBorder=0></iframe>

    - Result: \
        According to our permutation test, the **p-value** is **0.301**, which is **greater** than our significance level of **0.05**. Thus, we **fail to reject** the null hypothesis.

---
## Hypothesis Testing

So far, we have analyzed the information contained in the dataset, and we are finally getting back to our central question: \
**What is the relationship between the cooking time and average rating of recipes?**
We will conduct a **permutation test** to explore.

Our `null hypothesis` is 
> There is no significant relationship between cooking time and average rating of recipes.

The `alternative hypothesis` is 
> There is a significant relationship between cooking time and average rating of recipes. The less cooking time (less than 180 min) required, the higher the rating is.

- Justification on choices of **null** and **alternative** hypothesis: 
    In our earlier observation, we see that there seems to exist a relationship among `minutes` a recipes requires , and `rating` the recipe has. Here, we use these hypotheses to investigate.


Since the typical preparation of a meal would be under 3 hours, we divide the dataset into two groups, which are the recipes with cooking time more than or equal to 180 min and those are less than 180 min. The `test statistics` chosen is the difference between the means of the ratings of the two groups. We will conduct the permutation test under the `significance level` of 5%.

- Justification on the choice of the **test statistic**: 
    Since we are investigating numerical values, we use the difference between the means of ratings of the group of recipes that take less than 3 hours (180 minutes) and those take more than 3 hours (180 minutes).

- Justification on **significance level**:
    We think 0.05 is strong enough in our analysis.

Here, we have the **empirical distribution** of our permutation test, and we can see that the **observation** lies almost **outside** the whole empirical distribution, which implies that we should **reject** the null hypothesis of this permutation test.
<iframe src="assets/hypothesis_test.html" width=800 height=600 frameBorder=0></iframe>


The resulting **p-value** is **0.0**, which is significantly **lower** than 0.05. Therefore, we **reject** the null hypothesis.

In conclusion, based on the statistical analysis performed, we reject the null hypothesis and find that there **is a significant relationship** between **cooking time** and the **average rating** of recipes. The data suggests that cooking time has a notable impact on the rating of recipes. However, it is important to acknowledge that the significant association **does not** imply a definitive **cause-and-effect** relationship between **cooking time** and recipe **ratings**.

---

