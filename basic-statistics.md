---
title: "Basic Statistics: describing, modelling and reporting"
teaching: 60
exercises: 20
source: Rmd
---




::::::::::::::::::::::::::::::::::::::: objectives

- To be able to describe the different types of data
- To be able to do basic data exploration of a dataset
- To be able to calculate descriptive statistics
- To be able to perform statistical inference on a dataset

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How can I detect the type of data I have?
- How can I make meaningful summaries of my data?

::::::::::::::::::::::::::::::::::::::::::::::::::

## Content

-   Types of Data
-   Exploring your dataset
-   Descriptive Statistics
-   Inferential Statistics

## Data


``` r
# We will need these libraries and this data later.
library(tidyverse)
library(ggplot2)

Up until this point, we have been working with the complete_old dataframe contained in the ratdat package. However, you typically won’t access data from an R package; it is much more common to access data files stored somewhere on your computer. We are now going to start using the data in our cleaned directory inside the data directory that you downloaded at the beginning of the lesson (instructions here). This file is called surveys_complete_77_89.csv.

# loading data
surveys <- read_csv("data/cleaned/surveys_complete_77_89.csv")
```

``` error
Error in parse(text = input): <text>:5:4: unexpected symbol
4: 
5: Up until
      ^
```

## The big picture

-   Research often seeks to answer a question about a larger population by collecting data on a small sample
-   Data collection:
    -   Many variables
    -   For each person/unit.
-   This procedure, *sampling*, must be controlled so as to ensure **representative** data.

## Descriptive and inferential statistics

::: callout
Just as data in general are of different types - for example numeric vs text data - statistical data are assigned to different *levels of measure*. The level of measure determines how we can describe and model the data.
:::

# Describing data

-   Continuous variables
-   Discrete variables

::: callout
How do we convey information on what your data looks like, using numbers or figures?
:::

### Describing continuous data.

First establish the distribution of the data. You can visualise this with a histogram.


``` r
ggplot(surveys, aes(x = weight)) +
  geom_histogram()
```

``` error
Error in `ggplot()`:
! could not find function "ggplot"
```
What is the distribution of this data?

### What is the distribution of population?

If the raw values are difficult to visualise, so we can take the log of the values and log those.  Try this command


``` r
ggplot(surveys, aes(x = log(weight))) +
  geom_histogram()
```

``` error
Error in `ggplot()`:
! could not find function "ggplot"
```
What is the distribution of this data?

## Parametric vs non-parametric analysis

-   Parametric analysis assumes that
    -   The data follows a known distribution
    -   It can be described using *parameters*
    -   Examples of distributions include, normal, Poisson, exponential.
-   Non parametric data
    -   The data can't be said to follow a known distribution

::::::::::::::::::::::::::::::::::::: instructor
Emphasise that parametric is not equal to normal.
::::::::::::::::::::::::::::::::::::::::::::::::

### Describing parametric and non-parametric data

How do you use numbers to convey what your data looks like.

-   Parametric data
    -   Use the parameters that describe the distribution.
    -   For a Gaussian (normal) distribution - use mean and standard deviation
    -   For a Poisson distribution - use average event rate
    -   etc.
-   Non Parametric data
    -   Use the median (the middle number when they are ranked from lowest to highest) and the interquartile range (the number 75% of the way up the list when ranked minus the number 25% of the way)
-   You can use the command `summary(data_frame_name)` to get these numbers for each variable.

## Mean versus standard deviation

-   What does standard deviation mean?
-   Both graphs have the same mean (center), but the second one has data which is more spread out.


``` r
# small standard deviation
dummy_1 <- rnorm(1000, mean = 10, sd = 0.5)
dummy_1 <- as.data.frame(dummy_1)
ggplot(dummy_1, aes(x = dummy_1)) +
  geom_histogram()
```

``` error
Error in `ggplot()`:
! could not find function "ggplot"
```

``` r
# larger standard deviation
dummy_2 <- rnorm(1000, mean = 10, sd = 200)
dummy_2 <- as.data.frame(dummy_2)
ggplot(dummy_2, aes(x = dummy_2)) +
  geom_histogram()
```

``` error
Error in `ggplot()`:
! could not find function "ggplot"
```

::::::::::::::::::::::::::::::::::::: instructor
Get them to plot the graphs. Explain that we are generating random data from different distributions and plotting them.
::::::::::::::::::::::::::::::::::::::::::::::::

### Calculating mean and standard deviation


``` r
mean(surveys$weight, na.rm = TRUE)
```

``` error
Error:
! object 'surveys' not found
```
Calculate the standard deviation and confirm that it is the square root of the variance:


``` r
sdweight <- sd(surveys$weight, na.rm = TRUE)
```

``` error
Error:
! object 'surveys' not found
```

``` r
print(sdweight)
```

``` error
Error:
! object 'sdweight' not found
```

``` r
varweight <- var(surveys$weight, na.rm = TRUE)
```

``` error
Error:
! object 'surveys' not found
```

``` r
print(varweight)
```

``` error
Error:
! object 'varweight' not found
```

``` r
sqrt(varweight) == sdweight
```

``` error
Error:
! object 'varweight' not found
```
Calculate the standard deviation and confirm that it is the square root of the variance:


``` r
sdweight <- sd(surveys$weight, na.rm = TRUE)
```

``` error
Error:
! object 'surveys' not found
```

``` r
print(sdweight)
```

``` error
Error:
! object 'sdweight' not found
```

``` r
varweight <- var(surveys$weight, na.rm = TRUE)
```

``` error
Error:
! object 'surveys' not found
```

``` r
print(varweight)
```

``` error
Error:
! object 'varweight' not found
```

``` r
sqrt(varweight) == sdweight
```

``` error
Error:
! object 'varweight' not found
```
The `na.rm` argument tells R to ignore missing values in the variable.

### Calculating median and interquartile range


``` r
median(surveys$weight, na.rm = TRUE)
```

``` error
Error:
! object 'surveys' not found
```

``` r
IQR(surveys$weight, na.rm = TRUE)
```

``` error
Error:
! object 'surveys' not found
```
Again, we ignore the missing values.

## Describing discrete data

In our data set there is a variable `sex`.What is the proportion of males and females in this data set?

-   Frequencies


``` r
table(surveys$sex)
```

``` error
Error:
! object 'surveys' not found
```
-   Proportions


``` r
gendertable <- table(surveys$sex)
```

``` error
Error:
! object 'surveys' not found
```

``` r
prop.table(gendertable)
```

``` error
Error:
! object 'gendertable' not found
```

Contingency tables of frequencies can also be tabulated with **table()**. For example:


``` r
table(
  surveys$plot_id,
  surveys$species_id
)
```

``` error
Error:
! object 'surveys' not found
```
Which leads quite naturally to the consideration of any association between the observed frequencies.

# Inferential statistics

## Meaningful analysis

-   What is your hypothesis - what is your null hypothesis?

::: callout
Always: the level of the independent variable has no effect on the level of the dependent variable.
:::

-   What type of variables (data type) do you have?

-   What are the assumptions of the test you are using?

-   Interpreting the result

## Testing significance

-   p-value

-   \<0.05

-   0.03-0.049

    -   Would benefit from further testing.

**0.05** is not a magic number.

## Comparing means

It all starts with a hypothesis

-   Null hypothesis
    -   "There is no difference in mean hindfoot length between males and females" $$mean\_height\_men - mean\_height\_women = 0$$
-   Alternate hypothesis
    -   "There is a difference in mean hindfoot length between males and females"

## More on hypothesis testing

-   The null hypothesis (H0) assumes that the true mean difference (μd) is equal to zero.

-   The two-tailed alternative hypothesis (H1) assumes that μd is not equal to zero.

-   The upper-tailed alternative hypothesis (H1) assumes that μd is greater than zero.

-   The lower-tailed alternative hypothesis (H1) assumes that μd is less than zero.

-   Remember: hypotheses are never about data, they are about the processes which produce the data. The value of μd is unknown. The goal of hypothesis testing is to determine the hypothesis (null or alternative) with which the data are more consistent.

## Comparing means

Let's use the hypothesis introduced above: is there is a difference in hind foot length between species?


``` r
surveys %>%
  group_by(sex) %>%
  summarise(mean = mean(hindfoot_length, na.rm=TRUE), n = n())
```

``` error
Error in `surveys %>% group_by(sex) %>% summarise(mean = mean(hindfoot_length, na.rm = TRUE),
  n = n())`:
! could not find function "%>%"
```
Is the difference between the sexes statistically significant?

## t-test

### Assumptions of a t-test

-   One independent categorical variable with 2 groups and one dependent continuous variable

-   The dependent variable is approximately normally distributed in each group

-   The observations are independent of each other

-   For students' original t-statistic, that the variances in both groups are more or less equal. This constraint should probably be abandoned in favour of always using a conservative test.

## Doing a t-test


``` r
t.test(hindfoot_length. ~ sex, data = surveys)
```

``` error
Error:
! object 'surveys' not found
```

``` r
# we can also specify specific components
t.test(hindfoot_length. ~ sex, data = surveys)$statistic
```

``` error
Error:
! object 'surveys' not found
```

``` r
t.test(hindfoot_length. ~ sex, data = surveys)$parameter
```

``` error
Error:
! object 'surveys' not found
```

Notice that the summary()** of the test contains more data than is output by default.

### t-test result

Testing supported the rejection of the null hypothesis that there is no difference between the hind foot length of males and females in this data set. 

(Can you get p to display to four places?  Cf *format()*.)

## More than two levels of IV

While the t-test is sufficient where there are two levels of the IV, for situations where there are more than two, we use the **ANOVA** family of procedures. To show this, we will compare the `hindfoot_length` between `species_id` . If the ANOVA result is statistically significant, we will use a post-hoc test method to do pairwise comparisons (here Tukey's Honest Significant Differences.)


``` r
anovamodel <- aov(surveys$hindfoot_length ~ surveys$species_id)
```

``` error
Error:
! object 'surveys' not found
```

``` r
summary(anovamodel)
```

``` error
Error:
! object 'anovamodel' not found
```

``` r
TukeyHSD(anovamodel)
```

``` error
Error:
! object 'anovamodel' not found
```

## Regression Modelling

The most common use of regression modelling is to explore the relationship between two continuous variables, for example between `weight` and `hindfoot_length` in our data. We can first determine whether there is any significant correlation between the values, and if there is, plot the relationship.


``` r
cor.test(surveys$weight, surveys$hindfoot_length)
```

``` error
Error:
! object 'surveys' not found
```

``` r
ggplot(surveys, aes(weight, hindfoot_length)) +
  geom_point() +
  geom_smooth()
```

``` error
Error in `ggplot()`:
! could not find function "ggplot"
```

Having decided that a further investigation of this relationship is worthwhile, we can create a linear model with the function `lm()`.


``` r
linear_model <- lm(surveys$weight ~ surveys$hindfoot_length)
```

``` error
Error:
! object 'surveys' not found
```

``` r
summary(linear_model)
```

``` error
Error:
! object 'linear_model' not found
```

### Regression with a categorical IV (the t-test)

Run the following code chunk and compare the results to the t-test conducted earlier.


``` r
surveys %>%
  mutate(sex = factor(sex))
```

``` error
Error in `surveys %>% mutate(sex = factor(sex))`:
! could not find function "%>%"
```

``` r
cat_test <- lm(surveys$hindfoot_length ~ surveys$sex)
```

``` error
Error:
! object 'surveys' not found
```

``` r
summary(cat_test)
```

``` error
Error:
! object 'cat_test' not found
```
:::::::::::::::::::::::::::::::::::::::  challenge

### Challenge: Regression with a categorical IV (ANOVA)

Use the `lm()` function to model the relationship between `surveys$hindfoot_length`
and `surveys$species_id`. 

Compare the results with the ANOVA carried out earlier.

:::::::::::::::  solution

### Solution

First we need to convert `species_id` to a factor, then we can create our model.
If we compare the p-values for the Anova (<2e-16 *** and the lm we have just created (< 2.2e-16)
we can see that the outcome is the same.


``` r
surveys %>%
  mutate(species_id = factor(species_id))
```

``` error
Error in `surveys %>% mutate(species_id = factor(species_id))`:
! could not find function "%>%"
```

``` r
model_anova <- lm(surveys$hindfoot_length ~ surveys$species_id)
```

``` error
Error:
! object 'surveys' not found
```

``` r
summary(model_anova)
```

``` error
Error:
! object 'model_anova' not found
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::




:::::::::::::::::::::::::::::::::::::::: keypoints

- R has a range of in-built functions to enable initial data exploration.
- Linear models (lm) can be used with continuous and categorical variables.

::::::::::::::::::::::::::::::::::::::::::::::::::


