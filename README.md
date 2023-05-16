# Project-Analysis
This is a project of DSC 80 at University of California San Diego


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


2. Here, we present the scatter plot of timing vs. rating of recipes that takes *more than 180 minutes to work on*:
<iframe src="assets/minutes_greater_than_180_vs._avg_rating.html" width=800 height=600 frameBorder=0></iframe>