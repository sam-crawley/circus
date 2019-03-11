
# circus

***The Circus of Monsters\!***

`circus` contains a variety fitted models to help the systematic testing
of other packages.

## Installation

Run the following:

``` r
install.packages("devtools")
devtools::install_github("easystats/circus")
```

``` r
library("circus")
```

## How to use it

You can use the package in your `testthat` block by directly calling the
models. However, for it to work on **travis**, it is easier to directly
download them from github:

``` r
test_that("my_function_works", {
  library(circus)

  # model <- circus::lmerMod_1  # Local solution
  model <- circus::download_model("lmerMod_1")
  testthat::expect_equal(myFunction(model), 0.333)
})
```

## Contribute

Feel free to add any model you find missing\! Any scary creature for the
depth of your mind has its place here\!

## List of Models

### Base

``` r
# h-tests
htest_1 <- cor.test(iris$Sepal.Width, iris$Sepal.Length, method = "spearman")
htest_2 <- cor.test(iris$Sepal.Width, iris$Sepal.Length, method = "pearson")
htest_3 <- cor.test(iris$Sepal.Width, iris$Sepal.Length, method = "kendall")
htest_4 <- t.test(iris$Sepal.Width, iris$Sepal.Length)
htest_5 <- t.test(iris$Sepal.Width, iris$Sepal.Length, var.equal = TRUE)
htest_6 <- t.test(iris$Sepal.Width, iris$Sepal.Length)
htest_7 <- t.test(mtcars$mpg ~ mtcars$vs)
htest_8 <- t.test(iris$Sepal.Width, mu = 1)

# ANOVAs
anova_1 <- anova(lm(Sepal.Width ~ Species, data=iris))
aov_1 <- aov(Sepal.Width ~ Species, data=iris)
aovlist_1 <- aov(wt ~ cyl + Error(gear), data=mtcars)

data <- iris
data$Cat1 <- rep(c("X", "X", "Y"), length.out = nrow(data))
data$Cat2 <- rep(c("A", "B"), length.out = nrow(data))

aov_2 <- aov(Sepal.Length ~ Species * Cat1 * Cat2, data=data)
anova_2 <- anova(lm(Sepal.Length ~ Species * Cat1 * Cat2, data=data))
aovlist_2 <- aov(Sepal.Length ~ Species * Cat1+ Error(Cat2), data=data)

aov_3 <- aov(Sepal.Length ~ Species / Cat1 * Cat2, data=data)
anova_3 <- anova(lm(Sepal.Length ~ Species / Cat1 * Cat2, data=data))
aovlist_3 <- aov(Sepal.Length ~ Species / Cat1 + Error(Cat2), data=data)


# GLMs
lm_0 <- lm(mpg ~ 1, data = mtcars)
lm_1 <- lm(mpg ~ wt, data = mtcars)
lm_2 <- lm(mpg ~ wt + cyl, data = mtcars)
lm_3 <- lm(mpg ~ wt * cyl, data = mtcars)
lm_4 <- lm(mpg ~ wt + poly(cyl, 2), data = mtcars)
lm_5 <- lm(mpg ~ wt + poly(cyl, 2, raw=TRUE), data = mtcars)
lm_6 <- lm(mpg ~ wt * as.factor(gear), data = mtcars)
lm_7 <- lm(mpg ~ as.factor(gear) / wt, data = mtcars)

glm_0 <- glm(vs ~ 1, data = mtcars, family="binomial")
glm_1 <- glm(vs ~ wt, data = mtcars, family="binomial")
glm_2 <- glm(vs ~ wt + cyl, data = mtcars, family="binomial")
glm_3 <- glm(vs ~ wt * cyl, data = mtcars, family="binomial")
glm_4 <- glm(vs ~ wt + cyl, data = mtcars, family=binomial(link="probit"))
glm_5 <- glm(vs ~ wt + cyl, data = mtcars, family="poisson")

anova_4 <- anova(lm_0, lm_1, lm_2)
```

### lme4

``` r
library(lme4)

lmerMod_0 <- lme4::lmer(wt ~ 1 + (1|gear), data = mtcars)
lmerMod_1 <- lme4::lmer(wt ~ cyl + (1|gear), data = mtcars)
lmerMod_2 <- lme4::lmer(wt ~ drat + cyl + (1|gear), data = mtcars)
lmerMod_3 <- lme4::lmer(wt ~ drat * cyl + (1|gear), data = mtcars)
lmerMod_4 <- lme4::lmer(wt ~ drat / cyl + (1|gear), data = mtcars)
lmerMod_5 <- lme4::lmer(Petal.Width ~ Cat1 + (1+Cat1|Species), data=data)

merMod_0 <- lme4::glmer(vs ~ 1 + (1|gear), data = mtcars, family="binomial")
merMod_1 <- lme4::glmer(vs ~ cyl + (1|gear), data = mtcars, family="binomial")
merMod_2 <- lme4::glmer(vs ~ drat + cyl + (1|gear), data = mtcars, family="binomial")
merMod_3 <- lme4::glmer(vs ~ drat * cyl + (1|gear), data = mtcars, family="binomial")
merMod_4 <- lme4::glmer(vs ~ cyl + (1|gear), data = mtcars, family=binomial(link="probit"))

anova_lmerMod_0 <- anova(lmerMod_0)
anova_lmerMod_1 <- anova(lmerMod_1)
anova_lmerMod_2 <- anova(lmerMod_2)
anova_lmerMod_3 <- anova(lmerMod_3)
anova_lmerMod_4 <- anova(lmerMod_4)
anova_lmerMod_5 <- anova(lmerMod_5)

anova_lmerMod_6 <- anova(lmerMod_0, lmerMod_1, lmerMod_2)
```

### Rstanarm

``` r
set.seed(333)

library(rstanarm)

stanreg_lm_0 <- rstanarm::stan_glm(mpg ~ 1, data = mtcars)
stanreg_lm_1 <- rstanarm::stan_glm(mpg ~ wt, data = mtcars)
stanreg_lm_2 <- rstanarm::stan_glm(mpg ~ wt + cyl, data = mtcars)
stanreg_lm_3 <- rstanarm::stan_glm(mpg ~ wt * cyl, data = mtcars)
stanreg_lm_4 <- rstanarm::stan_glm(mpg ~ wt + poly(cyl, 2), data = mtcars)
stanreg_lm_5 <- rstanarm::stan_glm(mpg ~ wt + poly(cyl, 2, raw=TRUE), data = mtcars)
stanreg_lm_6 <- rstanarm::stan_glm(mpg ~ wt * as.factor(gear), data = mtcars)
stanreg_lm_7 <- rstanarm::stan_glm(mpg ~ as.factor(gear) / wt, data = mtcars)

stanreg_glm_0 <- rstanarm::stan_glm(vs ~ 1, data = mtcars, family="binomial")
stanreg_glm_1 <- rstanarm::stan_glm(vs ~ wt, data = mtcars, family="binomial")
stanreg_glm_2 <- rstanarm::stan_glm(vs ~ wt + cyl, data = mtcars, family="binomial")
stanreg_glm_3 <- rstanarm::stan_glm(vs ~ wt * cyl, data = mtcars, family="binomial")
stanreg_glm_4 <- rstanarm::stan_glm(vs ~ wt + cyl, data = mtcars, family=binomial(link="probit"))
stanreg_glm_5 <- rstanarm::stan_glm(vs ~ wt + cyl, data = mtcars, family="poisson")

stanreg_lmerMod_0 <- rstanarm::stan_lmer(wt ~ 1 + (1|gear), data = mtcars)
stanreg_lmerMod_1 <- rstanarm::stan_lmer(wt ~ cyl + (1|gear), data = mtcars)
stanreg_lmerMod_2 <- rstanarm::stan_lmer(wt ~ drat + cyl + (1|gear), data = mtcars)
stanreg_lmerMod_3 <- rstanarm::stan_lmer(wt ~ drat * cyl + (1|gear), data = mtcars)
stanreg_lmerMod_4 <- rstanarm::stan_lmer(wt ~ drat / cyl + (1|gear), data = mtcars)
stanreg_lmerMod_5 <- rstanarm::stan_lmer(Petal.Width ~ Cat1 + (1+Cat1|Species), data=data)

stanreg_merMod_0 <- rstanarm::stan_glmer(vs ~ 1 + (1|gear), data = mtcars, family="binomial")
stanreg_merMod_1 <- rstanarm::stan_glmer(vs ~ cyl + (1|gear), data = mtcars, family="binomial")
stanreg_merMod_2 <- rstanarm::stan_glmer(vs ~ drat + cyl + (1|gear), data = mtcars, family="binomial")
stanreg_merMod_3 <- rstanarm::stan_glmer(vs ~ drat * cyl + (1|gear), data = mtcars, family="binomial")
stanreg_merMod_4 <- rstanarm::stan_glmer(vs ~ cyl + (1|gear), data = mtcars, family=binomial(link="probit"))


stanreg_meanfield_lm_1 <- update(stanreg_lm_1, algorithm="meanfield")
stanreg_fullrank_lm_1 <- update(stanreg_lm_1, algorithm="fullrank")


stanreg_gamm4_1 <- stan_gamm4(Sepal.Width ~ s(Petal.Length), data=iris)
stanreg_gamm4_2 <- stan_gamm4(Sepal.Width ~ Species + s(Petal.Length), data=iris)
stanreg_gamm4_3 <- stan_gamm4(Sepal.Width ~ Species + s(Petal.Length), random = ~ (1 | Cat1), data=data)
```

## Save

``` r
usethis::use_data(htest_1,
                  htest_2,
                  htest_3,
                  htest_4,
                  htest_5,
                  htest_6,
                  htest_7,
                  htest_8,
                  
                  anova_1,
                  aov_1,
                  aovlist_1,
                  anova_2,
                  aov_2,
                  aovlist_2,
                  anova_3,
                  aov_3,
                  aovlist_3,
                  anova_4,
                  
                  lm_0,
                  lm_1,
                  lm_2,
                  lm_3,
                  lm_4,
                  lm_5,
                  lm_6,
                  lm_7,
                  glm_0,
                  glm_1,
                  glm_2,
                  glm_3,
                  glm_4,
                  glm_5,
                  
                  lmerMod_0,
                  lmerMod_1,
                  lmerMod_2,
                  lmerMod_3,
                  lmerMod_4,
                  lmerMod_5,
                  merMod_0,
                  merMod_1,
                  merMod_2,
                  merMod_3,
                  merMod_4,
                  
                  anova_lmerMod_0,
                  anova_lmerMod_1,
                  anova_lmerMod_2,
                  anova_lmerMod_3,
                  anova_lmerMod_4,
                  anova_lmerMod_5,
                  anova_lmerMod_6,
                  
                  stanreg_lm_0,
                  stanreg_lm_1,
                  stanreg_lm_2,
                  stanreg_lm_3,
                  stanreg_lm_4,
                  stanreg_lm_5,
                  stanreg_lm_6,
                  stanreg_lm_7,
                  stanreg_glm_0,
                  stanreg_glm_1,
                  stanreg_glm_2,
                  stanreg_glm_3,
                  stanreg_glm_4,
                  stanreg_glm_5,
                  stanreg_lmerMod_0,
                  stanreg_lmerMod_1,
                  stanreg_lmerMod_2,
                  stanreg_lmerMod_3,
                  stanreg_lmerMod_4,
                  stanreg_lmerMod_5,
                  stanreg_merMod_0,
                  stanreg_merMod_1,
                  stanreg_merMod_2,
                  stanreg_merMod_3,
                  stanreg_merMod_4,
                  stanreg_meanfield_lm_1,
                  stanreg_fullrank_lm_1,
                  
                  stanreg_gamm4_1,
                  stanreg_gamm4_2,
                  stanreg_gamm4_3,
                  
                  overwrite=TRUE)
```
