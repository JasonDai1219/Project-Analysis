# Project-Analysis
This is a project of DSC 80 at University of California San Diego

---
## Introduction

The file `food_data/RAW_interactions.csv` contains information on the ratings and reviews commented by different users based on the recipe they followed:

The file `food_data/RAW_recipes.csv` contains information on the details such as the work time, nutrition, and number of steps of each recipe:

In this project, we focus on finding the relationship between the cooking time and average rating of recipes, which will provide more personalized suggestions to people while they are finding suitable recipes. The users will be able to make more informed choices on the recipes they prefer and the expected time they need to spend.
There are 731927 rows in the `interactions` dataset, and we will focus on the 
`user_id`, `recipe_id`, `date`, `rating` columns. At the same time, there are 83782 rows in the `recipes` dataset, and we will focus on the `name`, `id`, `minutes`, `contributor_id`, `submitted` columns.

---

## Univariate Analysis

1. Here is the distribution of minutes(excluded extreme outliers) in our adjusted recipes dtataset: *recipes_new*
<iframe src="assets/minutes_distribution.html" width=800 height=600 frameBorder=0></iframe>

As we can see most recipes takes less than 3 hours(180 minutes) to make. Moreover, we excluded some extremely large outliers that could potentially make our estimation biasedly high, and there are only a few of them, thus, excluding those values would not introduce significant influence to our analysis.

---

2. Here is the distribution of ratings, in our adjusted interactions dataset: *interactions_new*:
<iframe src="assets/ratings_distribution.html" width=800 height=600 frameBorder=0></iframe>

In this distribution of ratings across various recipes, we can observe that most recipes have the highest rating score, 5. Then we may wonder that is there a relationship among the timing of a recipe and the rating of a recipe?
Later on, we will explore this question, and now, let us take a look at bivariate analysis of our dataset.

---

## Bivariate Analysis

1. Here, we present the scatter plot of timing vs. rating of recipes that takes *less than 180 minutes to work on*:
<iframe src="assets/minutes_less_than_180_vs._avg_rating.html" width=800 height=600 frameBorder=0></iframe>

In the first look of this scatter plot, you may think that it is kind of strange. However, we can take a closer look at the *UPPER LEFT* part of this graph, which has a *MUCH MORE* dense concentration of points than other parts in the graph. This graph also tells us that the x-axis represents *minutes* that a recipe requires, and the y-axis represents the *rating* of a recipe. Therefore, we can extract an information in this graph that it shows a trend that *the less time a recipe takes, it has higher possibility of getting higher rating*. We will explore this trend in later parts.

---

2. Here, we present the scatter plot of timing vs. rating of recipes that takes *more than 180 minutes to work on*:
<iframe src="assets/minutes_greater_than_180_vs._avg_rating.html" width=800 height=600 frameBorder=0></iframe>

In the graph of recipes that takes more than 180 minutes(3 hours), we can still see the pattern that more 5 ratings are those recipes that has less time, however, since there are not many recipes that takes time longer than 3 hours, so our analysis would not focus on these recipes.

---
## Interesting Aggregate

`print(interesting_aggregates.to_markdown(index = False))`

As shown in the above pivot table, we used the merged dataframe, which has the information of the number of ratings in each recipes. By separating these recipes according to their minutes used, we are able to observe that most people would actually try or make ratings on recipes that take relatively shorter time as compared to recipes that take longer time. Starting from here, we would want to observe that whether there is a relationship among minutes and rating columns in our dataset.

---
## Assessment of Missingness

### NMAR Analysis
First of all, for the only missing value in the name column, we conclude it as Missing Completely At Random (MCAR). Since all the values in recipe_id, minutes, contributor_id, and submitted are filled, the probability of a missing name is unrelated to any other columns, and also it is unlikely that the name is missing because of some properties of itself. Moreover, the missingness of avg_rating relates to the missingness of the rating column, so it is Missing At Random(MAR), and not NMAR. Thus, through observing the missingness of the rating column, as permutation test shown in the later part, the missingess of the rating column has a relationship with the n_step column, so we also conclude that the missingness of rating column is also MAR, not NMAR. All in all, we do not observe NMAR missingness in our dataset's three columns that has missing values.

---

### Missingness Dependency

1. In our analysis, we tried to explore whether the missingness of the *rating* column depends on *the number of steps*.

    - Null hypothesis:
        - The observed difference of means among recipes that have ratings and recipes that do not have ratings is because of random chance.

    - Alternative hypothesis:
        - The observed difference is not due to random chance of those two groups.

    - The significance level is at 5%
    - The 
    - Accoridng to the permutation test we made, the p-value is `0.0`, which is much smaller than our significant level, thus we reject the null hypothesis. 
    - Here is the graph showing where the observation lies in the distribution of  *the number of steps* when *rating* is missing and when *rating* is not missing.
    <iframe src="assets/n_steps_by_missingness_of_ratings.html" width=800 height=600 frameBorder=0></iframe>
        - As we can see in the above graph, the distribution of step numbers when rating is missing has a bimodal distribution and more outliers, which is different from that of the distribution of *step numbers* when rating is not missing.
    - And here, we have the empirical dustribution of our permutation test, and we can see that the observation lies almost outside the whole empirical distribution, therefore, we should reject the null hypothesis of this permutation test.
    <iframe src="assets/rating_vs._nsteps.html" width=800 height=600 frameBorder=0></iframe>

2. In our analysis, we also tried to explore whether the missingness of the *rating* column depends on the recipe has `wheat` in its *ingredients* or not.
    - Null hypothesis:
        - The missingness of rating of recipes has no relationship with whether a recipe's ingredients has wheat.
    - Alternative hypothesis:
        - There is a relationship among the missingness of rating and whether a recipe's ingredients has wheat.
    - The significance level is at 5%
    - According to our permutation test, the p-value is `0.301`, which is greater than our significance level: `0.05`, thus we fail to reject the null hypothesis.
    - Here is the graph showing the number of recipes have `wheat` in their `ingredients`, and that of recipes do not have `wheat` in their ingredients. And we can see that there is no apparent differences among groups that have `wheat` in their `ingredients, and groups that does not have `wheat` in their `ingredients`.
    <iframe src="assets/has_wheat_and_rating_missingness.html" width=800 height=600 frameBorder=0></iframe>
    - Moreover, we have the empirical distribution of our permutation test, and we can see that the observation lies in the position that is close to the middle, which supports out assertion that we fail to reject the null hypothesis.
    
    <iframe src="assets/rating_vs._has_wheat.html" width=800 height=600 frameBorder=0></iframe>

---

