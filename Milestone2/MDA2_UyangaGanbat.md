Mini Data Analysis Milestone 2
================

*To complete this milestone, you can edit [this `.rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are commented out with
`<!--- start your work here--->`. When you are done, make sure to knit
to an `.md` file by changing the output in the YAML header to
`github_document`, before submitting a tagged release on canvas.*

# Welcome to your second (and last) milestone in your mini data analysis project!

In Milestone 1, you explored your data, came up with research questions,
and obtained some results by making summary tables and graphs. This
time, we will first explore more in depth the concept of *tidy data.*
Then, you’ll be sharpening some of the results you obtained from your
previous milestone by:

-   Manipulating special data types in R: factors and/or dates and
    times.
-   Fitting a model object to your data, and extract a result.
-   Reading and writing data as separate files.

**NOTE**: The main purpose of the mini data analysis is to integrate
what you learn in class in an analysis. Although each milestone provides
a framework for you to conduct your analysis, it’s possible that you
might find the instructions too rigid for your data set. If this is the
case, you may deviate from the instructions – just make sure you’re
demonstrating a wide range of tools and techniques taught in this class.

# Instructions

**To complete this milestone**, edit [this very `.Rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are tagged with
`<!--- start your work here--->`.

**To submit this milestone**, make sure to knit this `.Rmd` file to an
`.md` file by changing the YAML output settings from
`output: html_document` to `output: github_document`. Commit and push
all of your work to your mini-analysis GitHub repository, and tag a
release on GitHub. Then, submit a link to your tagged release on canvas.

**Points**: This milestone is worth 55 points (compared to the 45 points
of the Milestone 1): 45 for your analysis, and 10 for your entire
mini-analysis GitHub repository. Details follow.

**Research Questions**: In Milestone 1, you chose two research questions
to focus on. Wherever realistic, your work in this milestone should
relate to these research questions whenever we ask for justification
behind your work. In the case that some tasks in this milestone don’t
align well with one of your research questions, feel free to discuss
your results in the context of a different research question.

# Learning Objectives

By the end of this milestone, you should:

-   Understand what *tidy* data is, and how to create it using `tidyr`.
-   Generate a reproducible and clear report using R Markdown.
-   Manipulating special data types in R: factors and/or dates and
    times.
-   Fitting a model object to your data, and extract a result.
-   Reading and writing data as separate files.

# Setup

Begin by loading your data and the tidyverse package below:

``` r
library(datateachr) # <- might contain the data you picked!
library(tidyverse)
library(broom)
library(here)
```

# Task 1: Tidy your data (15 points)

In this task, we will do several exercises to reshape our data. The goal
here is to understand how to do this reshaping with the `tidyr` package.

A reminder of the definition of *tidy* data:

-   Each row is an **observation**
-   Each column is a **variable**
-   Each cell is a **value**

*Tidy’ing* data is sometimes necessary because it can simplify
computation. Other times it can be nice to organize data so that it can
be easier to understand when read manually.

### 2.1 (2.5 points)

Based on the definition above, can you identify if your data is tidy or
untidy? Go through all your columns, or if you have \>8 variables, just
pick 8, and explain whether the data is untidy or tidy.

<!--------------------------- Start your work below --------------------------->

``` r
glimpse(apt_buildings)
```

    ## Rows: 3,455
    ## Columns: 37
    ## $ id                               <dbl> 10359, 10360, 10361, 10362, 10363, 10…
    ## $ air_conditioning                 <chr> "NONE", "NONE", "NONE", "NONE", "NONE…
    ## $ amenities                        <chr> "Outdoor rec facilities", "Outdoor po…
    ## $ balconies                        <chr> "YES", "YES", "YES", "YES", "NO", "NO…
    ## $ barrier_free_accessibilty_entr   <chr> "YES", "NO", "NO", "YES", "NO", "NO",…
    ## $ bike_parking                     <chr> "0 indoor parking spots and 10 outdoo…
    ## $ exterior_fire_escape             <chr> "NO", "NO", "NO", "YES", "NO", NA, "N…
    ## $ fire_alarm                       <chr> "YES", "YES", "YES", "YES", "YES", "Y…
    ## $ garbage_chutes                   <chr> "YES", "YES", "NO", "NO", "NO", "NO",…
    ## $ heating_type                     <chr> "HOT WATER", "HOT WATER", "HOT WATER"…
    ## $ intercom                         <chr> "YES", "YES", "YES", "YES", "YES", "Y…
    ## $ laundry_room                     <chr> "YES", "YES", "YES", "YES", "YES", "Y…
    ## $ locker_or_storage_room           <chr> "NO", "YES", "YES", "YES", "NO", "YES…
    ## $ no_of_elevators                  <dbl> 3, 3, 0, 1, 0, 0, 0, 2, 4, 2, 0, 2, 2…
    ## $ parking_type                     <chr> "Underground Garage , Garage accessib…
    ## $ pets_allowed                     <chr> "YES", "YES", "YES", "YES", "YES", "Y…
    ## $ prop_management_company_name     <chr> NA, "SCHICKEDANZ BROS. PROPERTIES", N…
    ## $ property_type                    <chr> "PRIVATE", "PRIVATE", "PRIVATE", "PRI…
    ## $ rsn                              <dbl> 4154812, 4154815, 4155295, 4155309, 4…
    ## $ separate_gas_meters              <chr> "NO", "NO", "NO", "NO", "NO", "NO", "…
    ## $ separate_hydro_meters            <chr> "YES", "YES", "YES", "YES", "YES", "Y…
    ## $ separate_water_meters            <chr> "NO", "NO", "NO", "NO", "NO", "NO", "…
    ## $ site_address                     <chr> "65  FOREST MANOR RD", "70  CLIPPER R…
    ## $ sprinkler_system                 <chr> "YES", "YES", "NO", "YES", "NO", "NO"…
    ## $ visitor_parking                  <chr> "PAID", "FREE", "UNAVAILABLE", "UNAVA…
    ## $ ward                             <chr> "17", "17", "03", "03", "02", "02", "…
    ## $ window_type                      <chr> "DOUBLE PANE", "DOUBLE PANE", "DOUBLE…
    ## $ year_built                       <dbl> 1967, 1970, 1927, 1959, 1943, 1952, 1…
    ## $ year_registered                  <dbl> 2017, 2017, 2017, 2017, 2017, NA, 201…
    ## $ no_of_storeys                    <dbl> 17, 14, 4, 5, 4, 4, 4, 7, 32, 4, 4, 7…
    ## $ emergency_power                  <chr> "NO", "YES", "NO", "NO", "NO", "NO", …
    ## $ `non-smoking_building`           <chr> "YES", "NO", "YES", "YES", "YES", "NO…
    ## $ no_of_units                      <dbl> 218, 206, 34, 42, 25, 34, 14, 105, 57…
    ## $ no_of_accessible_parking_spaces  <dbl> 8, 10, 20, 42, 12, 0, 5, 1, 1, 6, 12,…
    ## $ facilities_available             <chr> "Recycling bins", "Green Bin / Organi…
    ## $ cooling_room                     <chr> "NO", "NO", "NO", "NO", "NO", "NO", "…
    ## $ no_barrier_free_accessible_units <dbl> 2, 0, 0, 42, 0, NA, 14, 0, 0, 1, 25, …

``` r
print(apt_buildings, n=10)
```

    ## # A tibble: 3,455 × 37
    ##       id air_c…¹ ameni…² balco…³ barri…⁴ bike_…⁵ exter…⁶ fire_…⁷ garba…⁸ heati…⁹
    ##    <dbl> <chr>   <chr>   <chr>   <chr>   <chr>   <chr>   <chr>   <chr>   <chr>  
    ##  1 10359 NONE    Outdoo… YES     YES     0 indo… NO      YES     YES     HOT WA…
    ##  2 10360 NONE    Outdoo… YES     NO      0 indo… NO      YES     YES     HOT WA…
    ##  3 10361 NONE    <NA>    YES     NO      Not Av… NO      YES     NO      HOT WA…
    ##  4 10362 NONE    <NA>    YES     YES     Not Av… YES     YES     NO      HOT WA…
    ##  5 10363 NONE    <NA>    NO      NO      12 ind… NO      YES     NO      HOT WA…
    ##  6 10364 NONE    <NA>    NO      NO      Not Av… <NA>    YES     NO      HOT WA…
    ##  7 10365 NONE    <NA>    NO      YES     Not Av… NO      YES     NO      HOT WA…
    ##  8 10366 CENTRA… Indoor… YES     NO      Not Av… NO      YES     YES     HOT WA…
    ##  9 10367 NONE    <NA>    YES     YES     0 indo… NO      YES     YES     ELECTR…
    ## 10 10368 NONE    Indoor… YES     YES     Not Av… NO      YES     NO      HOT WA…
    ## # … with 3,445 more rows, 27 more variables: intercom <chr>,
    ## #   laundry_room <chr>, locker_or_storage_room <chr>, no_of_elevators <dbl>,
    ## #   parking_type <chr>, pets_allowed <chr>, prop_management_company_name <chr>,
    ## #   property_type <chr>, rsn <dbl>, separate_gas_meters <chr>,
    ## #   separate_hydro_meters <chr>, separate_water_meters <chr>,
    ## #   site_address <chr>, sprinkler_system <chr>, visitor_parking <chr>,
    ## #   ward <chr>, window_type <chr>, year_built <dbl>, year_registered <dbl>, …

*Columns* air_conditioning: looks good. amenities: Not tidy. Because
there are many items in one row and it’s not very organized.
bike_parking: doesn’t look tidy. Maybe it should be divided into two
columns with one indoor and one outdoor parking spots.
exterior_fire_escape: looks good. visitor_parking: looks tidy.
window_type: looks tidy. emergency_power: looks tidy. visitor_parking:
Maybe I can make the “Both” category into “Paid, Free”. Generally, yes
or no columns were tidy. However other columns, such as amenities,
bike_parking etc. don’t look tidy.

<!----------------------------------------------------------------------------->

### 2.2 (5 points)

Now, if your data is tidy, untidy it! Then, tidy it back to it’s
original state.

If your data is untidy, then tidy it! Then, untidy it back to it’s
original state.

Be sure to explain your reasoning for this task. Show us the “before”
and “after”.

<!--------------------------- Start your work below --------------------------->

``` r
apt_buildings1 <- apt_buildings %>% 
    unite(col = "years_built_registered", c(year_built, year_registered), sep = ", ")

apt_buildings2 <- apt_buildings1 %>% 
    separate(col = "years_built_registered", c("year_built", "year_registered"), sep = ", ")

#Before
glimpse(apt_buildings$year_built)
```

    ##  num [1:3455] 1967 1970 1927 1959 1943 ...

``` r
#After
glimpse(apt_buildings1$years_built_registered)
```

    ##  chr [1:3455] "1967, 2017" "1970, 2017" "1927, 2017" "1959, 2017" ...

Reasoning: I couldn’t do what I really wanted to do. What I want to do
was separate bike_parking column into Indoor and outdoor bike_parking
columns with the numbers in it. I didn’t know how to do it. If you know,
send me some instructions, please. So instead, I used unite and separate
function.

<!----------------------------------------------------------------------------->

### 2.3 (7.5 points)

Now, you should be more familiar with your data, and also have made
progress in answering your research questions. Based on your interest,
and your analyses, pick 2 of the 4 research questions to continue your
analysis in the next four tasks:

<!-------------------------- Start your work below ---------------------------->

1.  I want to know if the number of units is correlated with the number
    of storeys.  
    Updated research question: If the number of units is correlated with
    the number of storeys and other variables.
2.  Previous research question: After creating a new variable that
    categorizes the number of units, I want to know how which category
    has more exterior fire escapes. Updated research question: if there
    is statistically significant difference in the presence of exterior
    fire escapes between buildings with more than 50 units and buildings
    with less than 50 units.
    <!----------------------------------------------------------------------------->

Explain your decision for choosing the above two research questions.

<!--------------------------- Start your work below --------------------------->

Those questions asked correlations between two variables and hypothesis
testing between two categories.

<!----------------------------------------------------------------------------->

Now, try to choose a version of your data that you think will be
appropriate to answer these 2 questions. Use between 4 and 8 functions
that we’ve covered so far (i.e. by filtering, cleaning, tidy’ing,
dropping irrelevant columns, etc.).

<!--------------------------- Start your work below --------------------------->

I am going to use the original apt_building version.

Research question 1. If the number of units is correlated with the
number of storeys and other variables.

``` r
summary(apt_buildings$no_of_units)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##    0.00   25.00   52.00   91.09  124.00 4111.00

``` r
summary(apt_buildings$no_of_storeys)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   0.000   3.000   5.000   7.738  10.000  51.000

``` r
apt_buildings3 <- apt_buildings %>% select(no_of_units, no_of_storeys, no_of_elevators, no_of_accessible_parking_spaces)
head(apt_buildings3)
```

    ## # A tibble: 6 × 4
    ##   no_of_units no_of_storeys no_of_elevators no_of_accessible_parking_spaces
    ##         <dbl>         <dbl>           <dbl>                           <dbl>
    ## 1         218            17               3                               8
    ## 2         206            14               3                              10
    ## 3          34             4               0                              20
    ## 4          42             5               1                              42
    ## 5          25             4               0                              12
    ## 6          34             4               0                               0

``` r
filter(apt_buildings3, no_of_elevators >= 1) %>% arrange(no_of_elevators)
```

    ## # A tibble: 2,113 × 4
    ##    no_of_units no_of_storeys no_of_elevators no_of_accessible_parking_spaces
    ##          <dbl>         <dbl>           <dbl>                           <dbl>
    ##  1          42             5               1                              42
    ##  2          79             6               1                              NA
    ##  3          44             4               1                               0
    ##  4          39             4               1                               0
    ##  5          25             4               1                               0
    ##  6          40             4               1                               1
    ##  7          61             5               1                              53
    ##  8          49             7               1                               0
    ##  9          59            10               1                               0
    ## 10          40             4               1                               6
    ## # … with 2,103 more rows

``` r
model1 <- lm(no_of_units~no_of_storeys, apt_buildings3)
summary(model1)
```

    ## 
    ## Call:
    ## lm(formula = no_of_units ~ no_of_storeys, data = apt_buildings3)
    ## 
    ## Residuals:
    ##    Min     1Q Median     3Q    Max 
    ## -273.6  -16.7   -7.7   11.7 3812.7 
    ## 
    ## Coefficients:
    ##               Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)   -13.9966     2.1897  -6.392 1.85e-10 ***
    ## no_of_storeys  13.5801     0.2203  61.657  < 2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 80.8 on 3453 degrees of freedom
    ## Multiple R-squared:  0.524,  Adjusted R-squared:  0.5239 
    ## F-statistic:  3802 on 1 and 3453 DF,  p-value: < 2.2e-16

``` r
glance(model1)
```

    ## # A tibble: 1 × 12
    ##   r.squared adj.r.sq…¹ sigma stati…² p.value    df  logLik    AIC    BIC devia…³
    ##       <dbl>      <dbl> <dbl>   <dbl>   <dbl> <dbl>   <dbl>  <dbl>  <dbl>   <dbl>
    ## 1     0.524      0.524  80.8   3802.       0     1 -20076. 40158. 40176.  2.25e7
    ## # … with 2 more variables: df.residual <int>, nobs <int>, and abbreviated
    ## #   variable names ¹​adj.r.squared, ²​statistic, ³​deviance

``` r
model2 <- lm(no_of_units~no_of_storeys+no_of_elevators, apt_buildings3)
summary(model2)
```

    ## 
    ## Call:
    ## lm(formula = no_of_units ~ no_of_storeys + no_of_elevators, data = apt_buildings3)
    ## 
    ## Residuals:
    ##    Min     1Q Median     3Q    Max 
    ## -633.2  -16.6   -3.8   10.5 3824.8 
    ## 
    ## Coefficients:
    ##                 Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)     -13.9851     2.1415  -6.531 7.51e-11 ***
    ## no_of_storeys    10.3912     0.3229  32.183  < 2e-16 ***
    ## no_of_elevators  20.4023     1.5322  13.315  < 2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 78.86 on 3447 degrees of freedom
    ##   (5 observations deleted due to missingness)
    ## Multiple R-squared:  0.5465, Adjusted R-squared:  0.5462 
    ## F-statistic:  2077 on 2 and 3447 DF,  p-value: < 2.2e-16

``` r
model3 <- lm(no_of_units~no_of_storeys+no_of_elevators+no_of_accessible_parking_spaces, apt_buildings3)
summary(model3)
```

    ## 
    ## Call:
    ## lm(formula = no_of_units ~ no_of_storeys + no_of_elevators + 
    ##     no_of_accessible_parking_spaces, data = apt_buildings3)
    ## 
    ## Residuals:
    ##    Min     1Q Median     3Q    Max 
    ## -692.2  -17.4   -3.5   11.0 3825.9 
    ## 
    ## Coefficients:
    ##                                  Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)                     -13.18704    2.29179  -5.754  9.5e-09 ***
    ## no_of_storeys                    10.06458    0.33809  29.769  < 2e-16 ***
    ## no_of_elevators                  22.38513    1.63767  13.669  < 2e-16 ***
    ## no_of_accessible_parking_spaces  -0.05348    0.08361  -0.640    0.522    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 79.99 on 3325 degrees of freedom
    ##   (126 observations deleted due to missingness)
    ## Multiple R-squared:  0.544,  Adjusted R-squared:  0.5436 
    ## F-statistic:  1322 on 3 and 3325 DF,  p-value: < 2.2e-16

Conclusion: It looks like the number of units is significantly
associated with number of storeys and number of elevators but not with
the number of accessible parking spaces.

Research question 2. if there is a statistically significant difference
in the presence of exterior fire escapes between buildings with more
than 50 units and buildings with less than 50 units.

``` r
apt_buildings$no_of_units_cat <- 
       ifelse(apt_buildings$no_of_units < 50,"1. 0-50 units", "2. Above_50_units")
table(apt_buildings$no_of_units_cat)
```

    ## 
    ##     1. 0-50 units 2. Above_50_units 
    ##              1681              1774

``` r
rename1 <- rename (apt_buildings, units_number=no_of_units)
head(rename1)
```

    ## # A tibble: 6 × 38
    ##      id air_co…¹ ameni…² balco…³ barri…⁴ bike_…⁵ exter…⁶ fire_…⁷ garba…⁸ heati…⁹
    ##   <dbl> <chr>    <chr>   <chr>   <chr>   <chr>   <chr>   <chr>   <chr>   <chr>  
    ## 1 10359 NONE     Outdoo… YES     YES     0 indo… NO      YES     YES     HOT WA…
    ## 2 10360 NONE     Outdoo… YES     NO      0 indo… NO      YES     YES     HOT WA…
    ## 3 10361 NONE     <NA>    YES     NO      Not Av… NO      YES     NO      HOT WA…
    ## 4 10362 NONE     <NA>    YES     YES     Not Av… YES     YES     NO      HOT WA…
    ## 5 10363 NONE     <NA>    NO      NO      12 ind… NO      YES     NO      HOT WA…
    ## 6 10364 NONE     <NA>    NO      NO      Not Av… <NA>    YES     NO      HOT WA…
    ## # … with 28 more variables: intercom <chr>, laundry_room <chr>,
    ## #   locker_or_storage_room <chr>, no_of_elevators <dbl>, parking_type <chr>,
    ## #   pets_allowed <chr>, prop_management_company_name <chr>,
    ## #   property_type <chr>, rsn <dbl>, separate_gas_meters <chr>,
    ## #   separate_hydro_meters <chr>, separate_water_meters <chr>,
    ## #   site_address <chr>, sprinkler_system <chr>, visitor_parking <chr>,
    ## #   ward <chr>, window_type <chr>, year_built <dbl>, year_registered <dbl>, …

``` r
buildings_with_exterior_fire_escape <- apt_buildings %>% filter(exterior_fire_escape=="YES") %>% arrange(year_built)
head(buildings_with_exterior_fire_escape) 
```

    ## # A tibble: 6 × 38
    ##      id air_co…¹ ameni…² balco…³ barri…⁴ bike_…⁵ exter…⁶ fire_…⁷ garba…⁸ heati…⁹
    ##   <dbl> <chr>    <chr>   <chr>   <chr>   <chr>   <chr>   <chr>   <chr>   <chr>  
    ## 1 10424 NONE     <NA>    NO      NO      Not Av… YES     YES     NO      HOT WA…
    ## 2 10906 NONE     <NA>    YES     YES     Not Av… YES     YES     NO      ELECTR…
    ## 3 12869 NONE     <NA>    NO      NO      Not Av… YES     YES     NO      HOT WA…
    ## 4 10899 NONE     <NA>    NO      YES     0 indo… YES     YES     YES     HOT WA…
    ## 5 10437 INDIVID… <NA>    NO      NO      8 indo… YES     YES     NO      HOT WA…
    ## 6 12662 NONE     <NA>    NO      NO      Not Av… YES     YES     NO      HOT WA…
    ## # … with 28 more variables: intercom <chr>, laundry_room <chr>,
    ## #   locker_or_storage_room <chr>, no_of_elevators <dbl>, parking_type <chr>,
    ## #   pets_allowed <chr>, prop_management_company_name <chr>,
    ## #   property_type <chr>, rsn <dbl>, separate_gas_meters <chr>,
    ## #   separate_hydro_meters <chr>, separate_water_meters <chr>,
    ## #   site_address <chr>, sprinkler_system <chr>, visitor_parking <chr>,
    ## #   ward <chr>, window_type <chr>, year_built <dbl>, year_registered <dbl>, …

``` r
apt_buildings4 <- apt_buildings %>% select(no_of_units_cat, exterior_fire_escape)

chisq.test (apt_buildings4$no_of_units_cat, apt_buildings4$exterior_fire_escape)
```

    ## 
    ##  Pearson's Chi-squared test with Yates' continuity correction
    ## 
    ## data:  apt_buildings4$no_of_units_cat and apt_buildings4$exterior_fire_escape
    ## X-squared = 71.73, df = 1, p-value < 2.2e-16

Conclusion: There was a statistically significant difference in the
presence of exterior fire escapes between buildings with more than 50
units and buildings with less than 50 units.
<!----------------------------------------------------------------------------->

# Task 2: Special Data Types (10)

For this exercise, you’ll be choosing two of the three tasks below –
both tasks that you choose are worth 5 points each.

But first, tasks 1 and 2 below ask you to modify a plot you made in a
previous milestone. The plot you choose should involve plotting across
at least three groups (whether by facetting, or using an aesthetic like
colour). Place this plot below (you’re allowed to modify the plot if
you’d like). If you don’t have such a plot, you’ll need to make one.
Place the code for your plot below.

<!-------------------------- Start your work below ---------------------------->

``` r
apt_buildings$no_of_storeys_cat <- 
  ifelse(apt_buildings$no_of_storeys < 5,"Less_than_5_storeys",
    ifelse(apt_buildings$no_of_storeys < 10,"5-10_storeys", 
       ifelse(apt_buildings$no_of_storeys < 20,"10-20_storeys", "Above_20_storeys")))


ggplot(apt_buildings, aes(no_of_storeys_cat, year_built)) + 
    geom_jitter(width=0.1, alpha = 0.3)
```

    ## Warning: Removed 13 rows containing missing values (geom_point).

![](MDA2_UyangaGanbat_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

``` r
#I used this plot for both tasks. 
```

<!----------------------------------------------------------------------------->

Now, choose two of the following tasks.

1.  Produce a new plot that reorders a factor in your original plot,
    using the `forcats` package (3 points). Then, in a sentence or two,
    briefly explain why you chose this ordering (1 point here for
    demonstrating understanding of the reordering, and 1 point for
    demonstrating some justification for the reordering, which could be
    subtle or speculative.)

2.  Produce a new plot that groups some factor levels together into an
    “other” category (or something similar), using the `forcats` package
    (3 points). Then, in a sentence or two, briefly explain why you
    chose this grouping (1 point here for demonstrating understanding of
    the grouping, and 1 point for demonstrating some justification for
    the grouping, which could be subtle or speculative.)

3.  If your data has some sort of time-based column like a date (but
    something more granular than just a year):

    1.  Make a new column that uses a function from the `lubridate` or
        `tsibble` package to modify your original time-based column. (3
        points)

        -   Note that you might first have to *make* a time-based column
            using a function like `ymd()`, but this doesn’t count.
        -   Examples of something you might do here: extract the day of
            the year from a date, or extract the weekday, or let 24
            hours elapse on your dates.

    2.  Then, in a sentence or two, explain how your new column might be
        useful in exploring a research question. (1 point for
        demonstrating understanding of the function you used, and 1
        point for your justification, which could be subtle or
        speculative).

        -   For example, you could say something like “Investigating the
            day of the week might be insightful because penguins don’t
            work on weekends, and so may respond differently”.

<!-------------------------- Start your work below ---------------------------->

**Task Number 1**

``` r
apt_buildings_reorder <- apt_buildings %>% 
   mutate(no_of_storeys_cat_new = factor(case_when(no_of_storeys < 5 ~ "Less_than_5_storeys",
                                              no_of_storeys < 10 ~ "5-10_storeys",
                                              no_of_storeys < 20 ~ "10-20_storeys",
                                              TRUE ~ "Above_20_storeys"),
                                              levels = c('Less_than_5_storeys', '5-10_storeys', '10-20_storeys', 'Above_20_storeys')))

ggplot(apt_buildings_reorder, aes(no_of_storeys_cat_new, year_built)) + 
    geom_jitter(width=0.1, alpha = 0.3)
```

    ## Warning: Removed 13 rows containing missing values (geom_point).

![](MDA2_UyangaGanbat_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

<!----------------------------------------------------------------------------->
<!-------------------------- Start your work below ---------------------------->

**Task Number 2**

``` r
apt_buildings_reorder1 <- apt_buildings %>% 
   mutate(no_of_storeys_cat_new1 = factor(case_when(no_of_storeys < 5 ~ "Less_than_5_storeys",
                                              no_of_storeys < 10 ~ "5-10_storeys",
                                              no_of_storeys < 15 ~ "10-15_storeys",
                                              no_of_storeys < 20 ~ "15-20_storeys",
                                              no_of_storeys < 25 ~ "20-25_storeys",
                                              TRUE ~ "Above_25_storeys"),
                                              levels = c('Less_than_5_storeys', '5-10_storeys', '10-15_storeys', '15-20_storeys','20-25_storeys','Above_25_storeys')))

apt_buildings_lump <- apt_buildings_reorder1 %>% mutate(no_of_storeys_cat_new2 = fct_lump(no_of_storeys_cat_new1))

ggplot(apt_buildings_lump, aes(no_of_storeys_cat_new2, year_built)) + 
    geom_jitter(width=0.1, alpha = 0.3)                                                      
```

    ## Warning: Removed 13 rows containing missing values (geom_point).

![](MDA2_UyangaGanbat_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

``` r
#Created 6 categories and lumped the least two into "Other" category. 
```

<!----------------------------------------------------------------------------->

# Task 3: Modelling

## 2.0 (no points)

Pick a research question, and pick a variable of interest (we’ll call it
“Y”) that’s relevant to the research question. Indicate these.

<!-------------------------- Start your work below ---------------------------->

**Research Question**: If the number of units is correlated with the
number of storeys and other variables.

**Variable of interest**: Variable of interest is the number of units.

<!----------------------------------------------------------------------------->

## 2.1 (5 points)

Fit a model or run a hypothesis test that provides insight on this
variable with respect to the research question. Store the model object
as a variable, and print its output to screen. We’ll omit having to
justify your choice, because we don’t expect you to know about model
specifics in STAT 545.

-   **Note**: It’s OK if you don’t know how these models/tests work.
    Here are some examples of things you can do here, but the sky’s the
    limit.

    -   You could fit a model that makes predictions on Y using another
        variable, by using the `lm()` function.
    -   You could test whether the mean of Y equals 0 using `t.test()`,
        or maybe the mean across two groups are different using
        `t.test()`, or maybe the mean across multiple groups are
        different using `anova()` (you may have to pivot your data for
        the latter two).
    -   You could use `lm()` to test for significance of regression.

<!-------------------------- Start your work below ---------------------------->

``` r
model1 <- lm(no_of_units~no_of_storeys, apt_buildings3)
summary(model1)
```

    ## 
    ## Call:
    ## lm(formula = no_of_units ~ no_of_storeys, data = apt_buildings3)
    ## 
    ## Residuals:
    ##    Min     1Q Median     3Q    Max 
    ## -273.6  -16.7   -7.7   11.7 3812.7 
    ## 
    ## Coefficients:
    ##               Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)   -13.9966     2.1897  -6.392 1.85e-10 ***
    ## no_of_storeys  13.5801     0.2203  61.657  < 2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 80.8 on 3453 degrees of freedom
    ## Multiple R-squared:  0.524,  Adjusted R-squared:  0.5239 
    ## F-statistic:  3802 on 1 and 3453 DF,  p-value: < 2.2e-16

``` r
glance(model1)
```

    ## # A tibble: 1 × 12
    ##   r.squared adj.r.sq…¹ sigma stati…² p.value    df  logLik    AIC    BIC devia…³
    ##       <dbl>      <dbl> <dbl>   <dbl>   <dbl> <dbl>   <dbl>  <dbl>  <dbl>   <dbl>
    ## 1     0.524      0.524  80.8   3802.       0     1 -20076. 40158. 40176.  2.25e7
    ## # … with 2 more variables: df.residual <int>, nobs <int>, and abbreviated
    ## #   variable names ¹​adj.r.squared, ²​statistic, ³​deviance

``` r
model2 <- lm(no_of_units~no_of_storeys+no_of_elevators, apt_buildings3)
summary(model2)
```

    ## 
    ## Call:
    ## lm(formula = no_of_units ~ no_of_storeys + no_of_elevators, data = apt_buildings3)
    ## 
    ## Residuals:
    ##    Min     1Q Median     3Q    Max 
    ## -633.2  -16.6   -3.8   10.5 3824.8 
    ## 
    ## Coefficients:
    ##                 Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)     -13.9851     2.1415  -6.531 7.51e-11 ***
    ## no_of_storeys    10.3912     0.3229  32.183  < 2e-16 ***
    ## no_of_elevators  20.4023     1.5322  13.315  < 2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 78.86 on 3447 degrees of freedom
    ##   (5 observations deleted due to missingness)
    ## Multiple R-squared:  0.5465, Adjusted R-squared:  0.5462 
    ## F-statistic:  2077 on 2 and 3447 DF,  p-value: < 2.2e-16

``` r
model3 <- lm(no_of_units~no_of_storeys+no_of_elevators+no_of_accessible_parking_spaces, apt_buildings3)
summary(model3)
```

    ## 
    ## Call:
    ## lm(formula = no_of_units ~ no_of_storeys + no_of_elevators + 
    ##     no_of_accessible_parking_spaces, data = apt_buildings3)
    ## 
    ## Residuals:
    ##    Min     1Q Median     3Q    Max 
    ## -692.2  -17.4   -3.5   11.0 3825.9 
    ## 
    ## Coefficients:
    ##                                  Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)                     -13.18704    2.29179  -5.754  9.5e-09 ***
    ## no_of_storeys                    10.06458    0.33809  29.769  < 2e-16 ***
    ## no_of_elevators                  22.38513    1.63767  13.669  < 2e-16 ***
    ## no_of_accessible_parking_spaces  -0.05348    0.08361  -0.640    0.522    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 79.99 on 3325 degrees of freedom
    ##   (126 observations deleted due to missingness)
    ## Multiple R-squared:  0.544,  Adjusted R-squared:  0.5436 
    ## F-statistic:  1322 on 3 and 3325 DF,  p-value: < 2.2e-16

<!----------------------------------------------------------------------------->

## 2.2 (5 points)

Produce something relevant from your fitted model: either predictions on
Y, or a single value like a regression coefficient or a p-value.

-   Be sure to indicate in writing what you chose to produce.
-   Your code should either output a tibble (in which case you should
    indicate the column that contains the thing you’re looking for), or
    the thing you’re looking for itself.
-   Obtain your results using the `broom` package if possible. If your
    model is not compatible with the broom function you’re needing, then
    you can obtain your results by some other means, but first indicate
    which broom function is not compatible.

<!-------------------------- Start your work below ---------------------------->

``` r
model1 <- lm(no_of_units~no_of_storeys, apt_buildings3)
summary(model1)
```

    ## 
    ## Call:
    ## lm(formula = no_of_units ~ no_of_storeys, data = apt_buildings3)
    ## 
    ## Residuals:
    ##    Min     1Q Median     3Q    Max 
    ## -273.6  -16.7   -7.7   11.7 3812.7 
    ## 
    ## Coefficients:
    ##               Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)   -13.9966     2.1897  -6.392 1.85e-10 ***
    ## no_of_storeys  13.5801     0.2203  61.657  < 2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 80.8 on 3453 degrees of freedom
    ## Multiple R-squared:  0.524,  Adjusted R-squared:  0.5239 
    ## F-statistic:  3802 on 1 and 3453 DF,  p-value: < 2.2e-16

``` r
glance(model1)
```

    ## # A tibble: 1 × 12
    ##   r.squared adj.r.sq…¹ sigma stati…² p.value    df  logLik    AIC    BIC devia…³
    ##       <dbl>      <dbl> <dbl>   <dbl>   <dbl> <dbl>   <dbl>  <dbl>  <dbl>   <dbl>
    ## 1     0.524      0.524  80.8   3802.       0     1 -20076. 40158. 40176.  2.25e7
    ## # … with 2 more variables: df.residual <int>, nobs <int>, and abbreviated
    ## #   variable names ¹​adj.r.squared, ²​statistic, ³​deviance

``` r
tidy(model1)
```

    ## # A tibble: 2 × 5
    ##   term          estimate std.error statistic  p.value
    ##   <chr>            <dbl>     <dbl>     <dbl>    <dbl>
    ## 1 (Intercept)      -14.0     2.19      -6.39 1.85e-10
    ## 2 no_of_storeys     13.6     0.220     61.7  0

``` r
#I used summary function to show beta, standard error, t value, and p value. 

model2 <- lm(no_of_units~no_of_storeys+no_of_elevators, apt_buildings3)
summary(model2)
```

    ## 
    ## Call:
    ## lm(formula = no_of_units ~ no_of_storeys + no_of_elevators, data = apt_buildings3)
    ## 
    ## Residuals:
    ##    Min     1Q Median     3Q    Max 
    ## -633.2  -16.6   -3.8   10.5 3824.8 
    ## 
    ## Coefficients:
    ##                 Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)     -13.9851     2.1415  -6.531 7.51e-11 ***
    ## no_of_storeys    10.3912     0.3229  32.183  < 2e-16 ***
    ## no_of_elevators  20.4023     1.5322  13.315  < 2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 78.86 on 3447 degrees of freedom
    ##   (5 observations deleted due to missingness)
    ## Multiple R-squared:  0.5465, Adjusted R-squared:  0.5462 
    ## F-statistic:  2077 on 2 and 3447 DF,  p-value: < 2.2e-16

``` r
model3 <- lm(no_of_units~no_of_storeys+no_of_elevators+no_of_accessible_parking_spaces, apt_buildings3)
summary(model3)
```

    ## 
    ## Call:
    ## lm(formula = no_of_units ~ no_of_storeys + no_of_elevators + 
    ##     no_of_accessible_parking_spaces, data = apt_buildings3)
    ## 
    ## Residuals:
    ##    Min     1Q Median     3Q    Max 
    ## -692.2  -17.4   -3.5   11.0 3825.9 
    ## 
    ## Coefficients:
    ##                                  Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)                     -13.18704    2.29179  -5.754  9.5e-09 ***
    ## no_of_storeys                    10.06458    0.33809  29.769  < 2e-16 ***
    ## no_of_elevators                  22.38513    1.63767  13.669  < 2e-16 ***
    ## no_of_accessible_parking_spaces  -0.05348    0.08361  -0.640    0.522    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 79.99 on 3325 degrees of freedom
    ##   (126 observations deleted due to missingness)
    ## Multiple R-squared:  0.544,  Adjusted R-squared:  0.5436 
    ## F-statistic:  1322 on 3 and 3325 DF,  p-value: < 2.2e-16

<!----------------------------------------------------------------------------->

# Task 4: Reading and writing data

Get set up for this exercise by making a folder called `output` in the
top level of your project folder / repository. You’ll be saving things
there.

## 3.1 (5 points)

Take a summary table that you made from Milestone 1 (Task 4.2), and
write it as a csv file in your `output` folder. Use the `here::here()`
function.

-   **Robustness criteria**: You should be able to move your Mini
    Project repository / project folder to some other location on your
    computer, or move this very Rmd file to another location within your
    project repository / folder, and your code should still work.
-   **Reproducibility criteria**: You should be able to delete the csv
    file, and remake it simply by knitting this Rmd file.

<!-------------------------- Start your work below ---------------------------->

``` r
apt_buildings$no_of_storeys_cat <- 
  ifelse(apt_buildings$no_of_storeys < 5,"1. Less_than_5_stories",
    ifelse(apt_buildings$no_of_storeys < 10,"2. 5-10_stories", 
       ifelse(apt_buildings$no_of_storeys < 20,"3. 10-20_stories", "4. Above_30_stories")))

table1 <- table(apt_buildings$no_of_storeys_cat)
table1
```

    ## 
    ## 1. Less_than_5_stories        2. 5-10_stories       3. 10-20_stories 
    ##                   1692                    824                    720 
    ##    4. Above_30_stories 
    ##                    219

``` r
##working directory 

dir.create(here::here("MDA2"))
```

    ## Warning in dir.create(here::here("MDA2")): '/Users/uyangaganbat/Desktop/
    ## STAT545A/MDA2/MDA2' already exists

``` r
write_csv(as.data.frame(table1), here::here("MDA2", "exported_table1.csv"))
```

<!----------------------------------------------------------------------------->

## 3.2 (5 points)

Write your model object from Task 3 to an R binary file (an RDS), and
load it again. Be sure to save the binary file in your `output` folder.
Use the functions `saveRDS()` and `readRDS()`.

-   The same robustness and reproducibility criteria as in 3.1 apply
    here.

<!-------------------------- Start your work below ---------------------------->

``` r
saveRDS(model1, file="~/Desktop/STAT545A/MDA2/mda2_model1.RDS")

model1a <- readRDS(here("~/Desktop/STAT545A/MDA2/mda2_model1.RDS"))
```

<!----------------------------------------------------------------------------->

# Tidy Repository

Now that this is your last milestone, your entire project repository
should be organized. Here are the criteria we’re looking for.

## Main README (3 points)

There should be a file named `README.md` at the top level of your
repository. Its contents should automatically appear when you visit the
repository on GitHub.

Minimum contents of the README file:

-   In a sentence or two, explains what this repository is, so that
    future-you or someone else stumbling on your repository can be
    oriented to the repository.
-   In a sentence or two (or more??), briefly explains how to engage
    with the repository. You can assume the person reading knows the
    material from STAT 545A. Basically, if a visitor to your repository
    wants to explore your project, what should they know?

Once you get in the habit of making README files, and seeing more README
files in other projects, you’ll wonder how you ever got by without them!
They are tremendously helpful.

## File and Folder structure (3 points)

You should have at least four folders in the top level of your
repository: one for each milestone, and one output folder. If there are
any other folders, these are explained in the main README.

Each milestone document is contained in its respective folder, and
nowhere else.

Every level-1 folder (that is, the ones stored in the top level, like
“Milestone1” and “output”) has a `README` file, explaining in a sentence
or two what is in the folder, in plain language (it’s enough to say
something like “This folder contains the source for Milestone 1”).

## Output (2 points)

All output is recent and relevant:

-   All Rmd files have been `knit`ted to their output, and all data
    files saved from Task 4 above appear in the `output` folder.
-   All of these output files are up-to-date – that is, they haven’t
    fallen behind after the source (Rmd) files have been updated.
-   There should be no relic output files. For example, if you were
    knitting an Rmd to html, but then changed the output to be only a
    markdown file, then the html file is a relic and should be deleted.

Our recommendation: delete all output files, and re-knit each
milestone’s Rmd file, so that everything is up to date and relevant.

PS: there’s a way where you can run all project code using a single
command, instead of clicking “knit” three times. More on this in STAT
545B!

## Error-free code (1 point)

This Milestone 1 document knits error-free, and the Milestone 2 document
knits error-free.

## Tagged release (1 point)

You’ve tagged a release for Milestone 1, and you’ve tagged a release for
Milestone 2.

### Attribution

Thanks to Victor Yuan for mostly putting this together.
