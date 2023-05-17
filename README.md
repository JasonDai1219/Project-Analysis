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
## Cleaning and EDA (Exploratory Data Analysis)

### Data Cleaning

First, we left merge the `recipes` and `interactions` datasets together to clean the data as the analysis would be easier to conduct and understand if the cooking time and ratings are on the same dataset. We fill all the ratings of 0 with `np.nan` since the rating should be in the range of 1 to 5 (inclusive), and a 0 means the user did not fill out the rate.

Then we add an additional column of the average rating `avg_rating` of each recipe on the `merged` dataset by grouping it by the `recipe_id`.
| name                                 |   recipe_id |   minutes |   contributor_id | submitted   | tags                                                                                                                                                                                                                        | nutrition                                    |   n_steps | steps                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | description                                                                                                                                                                                                                                                                                                                                                                       | ingredients                                                                                                                                                                    |   n_ingredients |          user_id | date       |   rating |   avg_rating |
|:-------------------------------------|------------:|----------:|-----------------:|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------|----------:|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------:|-----------------:|:-----------|---------:|-------------:|
| 1 brownies in the world    best ever |      333281 |        40 |           985201 | 2008-10-27  | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'for-large-groups', 'desserts', 'lunch', 'snacks', 'cookies-and-brownies', 'chocolate', 'bar-cookies', 'brownies', 'number-of-servings'] | [138.4, 10.0, 50.0, 3.0, 3.0, 19.0, 6.0]     |        10 | ['heat the oven to 350f and arrange the rack in the middle', 'line an 8-by-8-inch glass baking dish with aluminum foil', 'combine chocolate and butter in a medium saucepan and cook over medium-low heat , stirring frequently , until evenly melted', 'remove from heat and let cool to room temperature', 'combine eggs , sugar , cocoa powder , vanilla extract , espresso , and salt in a large bowl and briefly stir until just evenly incorporated', 'add cooled chocolate and mix until uniform in color', 'add flour and stir until just incorporated', 'transfer batter to the prepared baking dish', 'bake until a tester inserted in the center of the brownies comes out clean , about 25 to 30 minutes', 'remove from the oven and cool completely before cutting']                                                  | these are the most; chocolatey, moist, rich, dense, fudgy, delicious brownies that you'll ever make.....sereiously! there's no doubt that these will be your fav brownies ever for you can add things to them or make them plain.....either way they're pure heaven!                                                                                                              | ['bittersweet chocolate', 'unsalted butter', 'eggs', 'granulated sugar', 'unsweetened cocoa powder', 'vanilla extract', 'brewed espresso', 'kosher salt', 'all-purpose flour'] |               9 | 386585           | 2008-11-19 |        4 |            4 |
| 1 in canada chocolate chip cookies   |      453467 |        45 |          1848091 | 2011-04-11  | ['60-minutes-or-less', 'time-to-make', 'cuisine', 'preparation', 'north-american', 'for-large-groups', 'canadian', 'british-columbian', 'number-of-servings']                                                               | [595.1, 46.0, 211.0, 22.0, 13.0, 51.0, 26.0] |        12 | ['pre-heat oven the 350 degrees f', 'in a mixing bowl , sift together the flours and baking powder', 'set aside', 'in another mixing bowl , blend together the sugars , margarine , and salt until light and fluffy', 'add the eggs , water , and vanilla to the margarine / sugar mixture and mix together until well combined', 'add in the flour mixture to the wet ingredients and blend until combined', 'scrape down the sides of the bowl and add the chocolate chips', 'mix until combined', 'scrape down the sides to the bowl again', 'using an ice cream scoop , scoop evenly rounded balls of dough and place of cookie sheet about 1 - 2 inches apart to allow for spreading during baking', 'bake for 10 - 15 minutes or until golden brown on the outside and soft & chewy in the center', 'serve hot and enjoy !'] | this is the recipe that we use at my school cafeteria for chocolate chip cookies. they must be the best chocolate chip cookies i have ever had! if you don't have margarine or don't like it, then just use butter (softened) instead.                                                                                                                                            | ['white sugar', 'brown sugar', 'salt', 'margarine', 'eggs', 'vanilla', 'water', 'all-purpose flour', 'whole wheat flour', 'baking soda', 'chocolate chips']                    |              11 | 424680           | 2012-01-26 |        5 |            5 |
| 412 broccoli casserole               |      306168 |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                                        | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]    |         6 | ['preheat oven to 350 degrees', 'spray a 2 quart baking dish with cooking spray , set aside', 'in a large bowl mix together broccoli , soup , one cup of cheese , garlic powder , pepper , salt , milk , 1 cup of french onions , and soy sauce', 'pour into baking dish , sprinkle remaining cheese over top', 'bake for 25 minutes or until cheese is lightly browned', 'sprinkle with rest of french fried onions and bake until onions are browned and cheese is bubbly , about 10 more minutes']                                                                                                                                                                                                                                                                                                                              | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 | ['frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions']          |               9 |  29782           | 2008-12-31 |        5 |            5 |
| 412 broccoli casserole               |      306168 |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                                        | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]    |         6 | ['preheat oven to 350 degrees', 'spray a 2 quart baking dish with cooking spray , set aside', 'in a large bowl mix together broccoli , soup , one cup of cheese , garlic powder , pepper , salt , milk , 1 cup of french onions , and soy sauce', 'pour into baking dish , sprinkle remaining cheese over top', 'bake for 25 minutes or until cheese is lightly browned', 'sprinkle with rest of french fried onions and bake until onions are browned and cheese is bubbly , about 10 more minutes']                                                                                                                                                                                                                                                                                                                              | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 | ['frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions']          |               9 |      1.19628e+06 | 2009-04-13 |        5 |            5 |
| 412 broccoli casserole               |      306168 |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                                        | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]    |         6 | ['preheat oven to 350 degrees', 'spray a 2 quart baking dish with cooking spray , set aside', 'in a large bowl mix together broccoli , soup , one cup of cheese , garlic powder , pepper , salt , milk , 1 cup of french onions , and soy sauce', 'pour into baking dish , sprinkle remaining cheese over top', 'bake for 25 minutes or until cheese is lightly browned', 'sprinkle with rest of french fried onions and bake until onions are browned and cheese is bubbly , about 10 more minutes']                                                                                                                                                                                                                                                                                                                              | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 | ['frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions']          |               9 | 768828           | 2013-08-02 |        5 |            5 |


Finally, we clean the `interactions` dataset by dropping the `review` column, which we will not focus on. 
`print(interactions_new.to_markdown(index = False))`


We also keep the columns we are using, whcih are `name`,`recipe_id`,`minutes`,`contributor_id`,`submitted`, and `avg_rating`, in the rest of our analysis in the `recipes` dataset, and add the `avg_rating` column to it for our future analysis on the relationship between the cooking time and average rating of recipes.
`print(recipes_new.to_markdown(index = False))`

---

### Univariate Analysis

1. Here is the distribution of minutes(excluded extreme outliers) in our adjusted recipes dtataset: *recipes_new*
<iframe src="assets/minutes_distribution.html" width=800 height=600 frameBorder=0></iframe>

As we can see most recipes takes less than 3 hours(180 minutes) to make. Moreover, we excluded some extremely large outliers that could potentially make our estimation biasedly high, and there are only a few of them, thus, excluding those values would not introduce significant influence to our analysis.

---

2. Here is the distribution of ratings, in our adjusted interactions dataset: *interactions_new*:
<iframe src="assets/ratings_distribution.html" width=800 height=600 frameBorder=0></iframe>

In this distribution of ratings across various recipes, we can observe that most recipes have the highest rating score, 5. Then we may wonder that is there a relationship among the timing of a recipe and the rating of a recipe?
Later on, we will explore this question, and now, let us take a look at bivariate analysis of our dataset.

---

### Bivariate Analysis

1. Here, we present the scatter plot of timing vs. rating of recipes that takes *less than 180 minutes to work on*:
<iframe src="assets/minutes_less_than_180_vs._avg_rating.html" width=800 height=600 frameBorder=0></iframe>

In the first look of this scatter plot, you may think that it is kind of strange. However, we can take a closer look at the *UPPER LEFT* part of this graph, which has a *MUCH MORE* dense concentration of points than other parts in the graph. This graph also tells us that the x-axis represents *minutes* that a recipe requires, and the y-axis represents the *rating* of a recipe. Therefore, we can extract an information in this graph that it shows a trend that *the less time a recipe takes, it has higher possibility of getting higher rating*. We will explore this trend in later parts.

---

2. Here, we present the scatter plot of timing vs. rating of recipes that takes *more than 180 minutes to work on*:
<iframe src="assets/minutes_greater_than_180_vs._avg_rating.html" width=800 height=600 frameBorder=0></iframe>

In the graph of recipes that takes more than 180 minutes(3 hours), we can still see the pattern that more 5 ratings are those recipes that has less time, however, since there are not many recipes that takes time longer than 3 hours, so our analysis would not focus on these recipes.

---
### Interesting Aggregate

`print(interesting_aggregates.head().to_markdown(index = False))`

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
    
    <iframe src="assets/raing_vs._has_wheat.html" width=800 height=600 frameBorder=0></iframe>

---

## Hypothesis Testing

Our null hypothesis is 
> There is no significant relationship between cooking time and average rating of recipes.

The alternative hypothesis is 
> There is a significant relationship between cooking time and average rating of recipes. The less cooking time (less than 180 min) required, the higher the rating is.

Since the typical preparation of a meal would be under 3 hours, we divide the dataset into two groups, which are the recipes with cooking time more than or equal to 180 min and those are less than 180 min. The test statistics chosen is the difference between the means of the ratings of the two groups. We will conduct the permutation test under the significance level of 5%. 

The resulting p-value is 0.0, which is lower than 0.05. Therefore, we reject the null hypothesis.

---

