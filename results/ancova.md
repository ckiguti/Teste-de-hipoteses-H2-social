ANCOVA test for `fss`\~`dfs`+`scenario`\*`social`
================
Geiser C. Challco <geiser@alumni.usp.br>

-   [Initial Variables and Data](#initial-variables-and-data)
    -   [Descriptive statistics of initial
        data](#descriptive-statistics-of-initial-data)
-   [Checking of Assumptions](#checking-of-assumptions)
    -   [Assumption: Symmetry and treatment of
        outliers](#assumption-symmetry-and-treatment-of-outliers)
    -   [Assumption: Normality distribution of
        data](#assumption-normality-distribution-of-data)
    -   [Assumption: Linearity of dependent variables and covariate
        variable](#assumption-linearity-of-dependent-variables-and-covariate-variable)
    -   [Assumption: Homogeneity of data
        distribution](#assumption-homogeneity-of-data-distribution)
-   [Saving the Data with Normal Distribution Used for Performing ANCOVA
    test](#saving-the-data-with-normal-distribution-used-for-performing-ancova-test)
-   [Computation of ANCOVA test and Pairwise
    Comparison](#computation-of-ancova-test-and-pairwise-comparison)
    -   [ANCOVA test](#ancova-test)
    -   [Pairwise comparison](#pairwise-comparison)
    -   [Descriptive Statistic of Estimated Marginal
        Means](#descriptive-statistic-of-estimated-marginal-means)
    -   [Ancova plots for the dependent variable
        “fss”](#ancova-plots-for-the-dependent-variable-fss)
    -   [Textual Report](#textual-report)
-   [Tips and References](#tips-and-references)

## Initial Variables and Data

-   R-script file: [../code/ancova.R](../code/ancova.R)
-   Initial table file:
    [../data/initial-table.csv](../data/initial-table.csv)
-   Data for fss [../data/table-for-fss.csv](../data/table-for-fss.csv)
-   Table without outliers and normal distribution of data:
    [../data/table-with-normal-distribution.csv](../data/table-with-normal-distribution.csv)
-   Other data files: [../data/](../data/)
-   Files related to the presented results: [../results/](../results/)

### Descriptive statistics of initial data

| scenario     | social | variable |   n |  mean | median |   min |   max |    sd |    se |    ci |   iqr | symmetry | skewness | kurtosis |
|:-------------|:-------|:---------|----:|------:|-------:|------:|------:|------:|------:|------:|------:|:---------|---------:|---------:|
| gamified     | lower  | fss      |   8 | 3.889 |  4.222 | 2.667 | 4.556 | 0.779 | 0.275 | 0.651 | 1.306 | YES      |   -0.433 |   -1.764 |
| gamified     | upper  | fss      |   8 | 3.167 |  3.000 | 2.778 | 3.889 | 0.389 | 0.138 | 0.326 | 0.556 | NO       |    0.627 |   -1.236 |
| non.gamified | lower  | fss      |   8 | 3.292 |  3.278 | 2.556 | 3.667 | 0.346 | 0.122 | 0.289 | 0.333 | NO       |   -0.937 |   -0.181 |
| non.gamified | upper  | fss      |   8 | 3.764 |  3.889 | 2.667 | 4.556 | 0.630 | 0.223 | 0.526 | 0.722 | YES      |   -0.342 |   -1.286 |
| NA           | NA     | fss      |  32 | 3.528 |  3.444 | 2.556 | 4.556 | 0.620 | 0.110 | 0.223 | 0.833 | YES      |    0.327 |   -1.096 |

![](ancova_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

    ## [1] "P19"

## Checking of Assumptions

### Assumption: Symmetry and treatment of outliers

#### Applying transformation for skewness data when normality is not achieved

#### Dealing with outliers (performing treatment of outliers)

### Assumption: Normality distribution of data

#### Removing data that affect normality (extreme values)

``` r
non.normal <- list(

)
sdat <- removeFromDataTable(rdat, non.normal, wid)
```

#### Result of normality test in the residual model

|     | var |   n | skewness | kurtosis | symmetry | statistic | method       |     p | p.signif | normality |
|:----|:----|----:|---------:|---------:|:---------|----------:|:-------------|------:|:---------|:----------|
| fss | fss |  32 |   -0.504 |    -0.41 | NO       |     0.965 | Shapiro-Wilk | 0.367 | ns       | YES       |

#### Result of normality test in each group

This is an optional validation and only valid for groups with number
greater than 30 observations

| scenario     | social | variable |   n |  mean | median |   min |   max |    sd |    se |    ci |   iqr | normality | method       | statistic |     p | p.signif |
|:-------------|:-------|:---------|----:|------:|-------:|------:|------:|------:|------:|------:|------:|:----------|:-------------|----------:|------:|:---------|
| gamified     | lower  | fss      |   8 | 3.889 |  4.222 | 2.667 | 4.556 | 0.779 | 0.275 | 0.651 | 1.306 | YES       | Shapiro-Wilk |     0.827 | 0.055 | ns       |
| gamified     | upper  | fss      |   8 | 3.167 |  3.000 | 2.778 | 3.889 | 0.389 | 0.138 | 0.326 | 0.556 | YES       | Shapiro-Wilk |     0.864 | 0.132 | ns       |
| non.gamified | lower  | fss      |   8 | 3.292 |  3.278 | 2.556 | 3.667 | 0.346 | 0.122 | 0.289 | 0.333 | YES       | Shapiro-Wilk |     0.847 | 0.088 | ns       |
| non.gamified | upper  | fss      |   8 | 3.764 |  3.889 | 2.667 | 4.556 | 0.630 | 0.223 | 0.526 | 0.722 | YES       | Shapiro-Wilk |     0.953 | 0.744 | ns       |

**Observation**:

As sample sizes increase, parametric tests remain valid even with the
violation of normality \[[1](#references)\]. According to the central
limit theorem, the sampling distribution tends to be normal if the
sample is large, more than (`n > 30`) observations. Therefore, we
performed parametric tests with large samples as described as follows:

-   In cases with the sample size greater than 100 (`n > 100`), we
    adopted a significance level of `p < 0.01`

-   For samples with `n > 50` observation, we adopted D’Agostino-Pearson
    test that offers better accuracy for larger samples
    \[[2](#references)\].

-   For samples’ size between `n > 100` and `n <= 200`, we ignored the
    normality test, and our decision of validating normality was based
    only in the interpretation of QQ-plots and histograms because the
    Shapiro-Wilk and D’Agostino-Pearson tests tend to be too sensitive
    with values greater than 200 observation \[[3](#references)\].

-   For samples with `n > 200` observation, we ignore the normality
    assumption based on the central theorem limit.

### Assumption: Linearity of dependent variables and covariate variable

``` r
ggscatter(sdat[["fss"]], x=covar, y="fss", facet.by=between, short.panel.labs = F) + 
 stat_smooth(method = "lm", span = 0.9)
```

    ## `geom_smooth()` using formula 'y ~ x'

![](ancova_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

### Assumption: Homogeneity of data distribution

|       | var | method         | formula                      |   n | DFn.df1 | DFd.df2 | statistic |     p | p.signif |
|:------|:----|:---------------|:-----------------------------|----:|--------:|--------:|----------:|------:|:---------|
| fss.1 | fss | Levene’s test  | `.res`\~`scenario`\*`social` |  32 |       3 |      28 |     1.534 | 0.227 | ns       |
| fss.2 | fss | Anova’s slopes | `.res`\~`scenario`\*`social` |  32 |       3 |      24 |     2.665 | 0.071 | ns       |

## Saving the Data with Normal Distribution Used for Performing ANCOVA test

``` r
ndat <- sdat[[1]]
for (dv in names(sdat)[-1]) ndat <- merge(ndat, sdat[[dv]])
write.csv(ndat, paste0("../data/table-with-normal-distribution.csv"))
```

Descriptive statistics of data with normal distribution

|       | scenario     | social | variable |   n |  mean | median |   min |   max |    sd |    se |    ci |   iqr |
|:------|:-------------|:-------|:---------|----:|------:|-------:|------:|------:|------:|------:|------:|------:|
| fss.1 | gamified     | lower  | fss      |   8 | 3.889 |  4.222 | 2.667 | 4.556 | 0.779 | 0.275 | 0.651 | 1.306 |
| fss.2 | gamified     | upper  | fss      |   8 | 3.167 |  3.000 | 2.778 | 3.889 | 0.389 | 0.138 | 0.326 | 0.556 |
| fss.3 | non.gamified | lower  | fss      |   8 | 3.292 |  3.278 | 2.556 | 3.667 | 0.346 | 0.122 | 0.289 | 0.333 |
| fss.4 | non.gamified | upper  | fss      |   8 | 3.764 |  3.889 | 2.667 | 4.556 | 0.630 | 0.223 | 0.526 | 0.722 |

![](ancova_files/figure-gfm/unnamed-chunk-17-1.png)<!-- -->

## Computation of ANCOVA test and Pairwise Comparison

### ANCOVA test

| var | Effect          | DFn | DFd |   SSn |   SSd |      F |     p |   ges | p.signif |
|:----|:----------------|----:|----:|------:|------:|-------:|------:|------:|:---------|
| fss | dfs             |   1 |  27 | 0.484 | 8.439 |  1.549 | 0.224 | 0.054 | ns       |
| fss | scenario        |   1 |  27 | 0.003 | 8.439 |  0.009 | 0.925 | 0.000 | ns       |
| fss | social          |   1 |  27 | 0.148 | 8.439 |  0.474 | 0.497 | 0.017 | ns       |
| fss | scenario:social |   1 |  27 | 3.289 | 8.439 | 10.525 | 0.003 | 0.280 | \*\*     |

### Pairwise comparison

| var | scenario     | social | group1   | group2       | estimate | conf.low | conf.high |    se | statistic |     p | p.adj | p.adj.signif |
|:----|:-------------|:-------|:---------|:-------------|---------:|---------:|----------:|------:|----------:|------:|------:|:-------------|
| fss | NA           | lower  | gamified | non.gamified |    0.729 |    0.116 |     1.343 | 0.299 |     2.439 | 0.022 | 0.022 | \*           |
| fss | NA           | upper  | gamified | non.gamified |   -0.601 |   -1.175 |    -0.028 | 0.280 |    -2.152 | 0.041 | 0.041 | \*           |
| fss | gamified     | NA     | lower    | upper        |    0.833 |    0.231 |     1.435 | 0.293 |     2.840 | 0.008 | 0.008 | \*\*         |
| fss | non.gamified | NA     | lower    | upper        |   -0.498 |   -1.073 |     0.077 | 0.280 |    -1.776 | 0.087 | 0.087 | ns           |

### Descriptive Statistic of Estimated Marginal Means

| var | scenario     | social |   n | emmean |  mean | conf.low | conf.high |    sd | sd.emms | se.emms |
|:----|:-------------|:-------|----:|-------:|------:|---------:|----------:|------:|--------:|--------:|
| fss | gamified     | lower  |   8 |  3.976 | 3.889 |    3.546 |     4.407 | 0.779 |   0.593 |   0.210 |
| fss | gamified     | upper  |   8 |  3.143 | 3.167 |    2.736 |     3.551 | 0.389 |   0.562 |   0.199 |
| fss | non.gamified | lower  |   8 |  3.247 | 3.292 |    2.835 |     3.659 | 0.346 |   0.568 |   0.201 |
| fss | non.gamified | upper  |   8 |  3.745 | 3.764 |    3.338 |     4.151 | 0.630 |   0.561 |   0.198 |

### Ancova plots for the dependent variable “fss”

``` r
plots <- twoWayAncovaPlots(sdat[["fss"]], "fss", between
, aov[["fss"]], pwc[["fss"]], addParam = c("jitter"), font.label.size=16, step.increase=0.25)
```

#### Plot for: `fss` \~ `scenario`

``` r
plots[["scenario"]]
```

![](ancova_files/figure-gfm/unnamed-chunk-25-1.png)<!-- -->

#### Plot for: `fss` \~ `social`

``` r
plots[["social"]]
```

![](ancova_files/figure-gfm/unnamed-chunk-26-1.png)<!-- -->

### Textual Report

After controlling the linearity of covariance “dfs”, ANCOVA tests with
independent between-subjects variables “scenario” (gamified,
non.gamified) and “social” (upper, lower) were performed to determine
statistically significant difference on the dependent varibles “fss”.
For the dependent variable “fss”, there was statistically significant
effects in the interaction of factors “scenario:social” with
F(1,27)=10.525, p=0.003 and ges=0.28 (effect size).

Pairwise comparisons using the Estimated Marginal Means (EMMs) were
computed to find statistically significant diferences among the groups
defined by the independent variables, and with the p-values ajusted by
the method “bonferroni”. For the dependent variable “fss”, the mean in
the scenario=“gamified” (adj M=3.976 and SD=0.779) was significantly
different than the mean in the scenario=“non.gamified” (adj M=3.247 and
SD=0.346) with p-adj=0.022; the mean in the scenario=“gamified” (adj
M=3.143 and SD=0.389) was significantly different than the mean in the
scenario=“non.gamified” (adj M=3.745 and SD=0.63) with p-adj=0.041; the
mean in the social=“lower” (adj M=3.976 and SD=0.779) was significantly
different than the mean in the social=“upper” (adj M=3.143 and SD=0.389)
with p-adj=0.008.

## Tips and References

-   Use the site <https://www.tablesgenerator.com> to convert the HTML
    tables into Latex format

-   \[2\]: Miot, H. A. (2017). Assessing normality of data in clinical
    and experimental trials. J Vasc Bras, 16(2), 88-91.

-   \[3\]: Bárány, Imre; Vu, Van (2007). “Central limit theorems for
    Gaussian polytopes”. Annals of Probability. Institute of
    Mathematical Statistics. 35 (4): 1593–1621.