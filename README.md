
<!-- README.md is generated from README.Rmd. Please edit that file -->

# PRDAbeta: Prospective and Retrospective Design Analysis

<!-- badges: start -->

[![Travis build
status](https://travis-ci.org/ClaudioZandonella/PRDA_beta.svg?branch=develop)](https://travis-ci.org/ClaudioZandonella/PRDA_beta)
[![Project Status: Active – The project has reached a stable, usable
state and is being actively
developed.](https://www.repostatus.org/badges/latest/active.svg)](https://www.repostatus.org/#active)
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.3630733.svg)](https://doi.org/10.5281/zenodo.3630733)

<hr>

<!-- badges: end -->

Given a plausible value of effect size, PRDAbeta performs a prospective
or retrospective design analysis to evaluate the inferential risks
(i.e., power, Type M error, and Type S error) related to the study
design.

For a general overview of the package see
`vignettes("PRDAbeta_overview")`. Examples for retrospective design
analysis and prospective design analysis are provided in
`vignettes("retrospective")` and `vignettes("prospective")`
respectively.

## Installation

<!-- You can install the released version of PRDAbeta from [CRAN](https://CRAN.R-project.org) with: -->

<!-- ``` r -->

<!-- install.packages("PRDAbeta") -->

<!-- ``` -->

<!-- And the development version from [GitHub](https://github.com/) with: -->

You can install the development version from
[GitHub](https://github.com/ClaudioZandonella/PRDAbeta/tree/develop)
with:

``` r
# install.packages("devtools")
devtools::install_github("ClaudioZandonella/PRDAbeta",
                         ref = "develop",
                         build_vignettes = TRUE)
```

## Some Background Information

The term *Design Analysis* was introduced by Gelman and Carlin (2014) as
a broader definition of Power Analysis. Traditional power analysis has a
narrow focus on statistical significance. Design analysis, instead,
evaluates together with power levels also other inferential risks (i.e.,
Type M error and Type S error), to assess estimates uncertainty under
hypothetical replications of a study.

Given a *plausible effect size* and study characteristics (i.e., sample
size, statistical test directionality, \(\alpha\) level), design
analysis evaluates:

  - **Power**: the probability of the test rejecting the null
    hypothesis.
  - **Type M error** (Magnitude): the factor by which a statistically
    significant effect is on average exaggerated, also known as
    *Exaggeration Ratio*.
  - **Type S error** (Sign): the probability of finding a statistically
    significant result in the opposite direction to the plausible one.

Although Type M error and Type S error depends directly on power level,
they allow enhancing researchers awareness about the inferential risks
related to their studies. For example, obtaining a significant result in
a study with 40% of power could sound a promising finding to the
researchers. However, knowing that this is associated with a Type M
error of almost 1.60 (i.e., on average significant results are an
overestimation of 60%) would warn researchers to interpret their results
with caution.

Moreover, Gelman and Carlin (2014) distinguished between two types of
design analysis according to the study phase:

  - **Prospective Design Analysis**: if the analysis is performed in the
    planning stage of the study to define the sample size needed to
    obtain a required level of power.
  - **Retrospective Design Analysis**: if the analysis is performed in a
    later stage when the data have already been collected. This is still
    useful to evaluate the inferential risks associated with the study.

It is important to do not mistake a retrospective design analysis for
post-hoc power analysis. The former defines the plausible effect size
according to previous results in the literature or experts’ indications,
whereas the latter defines the plausible effect size based on the same
study results and it is a widely-deprecated practice (Goodman 1994;
Lenth 2007; Senn 2002).

To know more about design analysis consider Gelman and Carlin (2014).
While, for an introduction about design analysis considering examples in
psychology see Altoè et al. (2020) and Bertoldo, Zandonella Callegher,
and Altoè (2020).

## The Package

PRDAbeta package can be used for Pearson’s correlation between two
variables or mean comparisons (one-sample, paired, two-samples, and
Welch’s t-test) considering a plausible value of *ρ* or Cohen’s *d*
respectively. See `vignette("retrospective")` to know how to set
function arguments for the different effect types.

### Functions

In PRDAbeta there are two main functions `retrospective()` and
`prospective()`.

#### • `retrospective()`

Given the hypothetical population effect size and the study sample size,
the function `retrospective()` performs a retrospective design analysis
for Cohen’s *d* (t-test comparing group means) or Pearson’s correlation
test between two variables. According to the defined alternative
hypothesis and the significance level, the inferential risks (i.e.,
Power level, Type M error, and Type S error) are computed together with
the critical effect value (i.e., the minimum absolute effect size value
that would result significant).

Considering a study that evaluated the correlation between two variables
with a sample of 30 subjects. Suppose that according to the literature
the hypothesized effect is *ρ* = .25. To evaluate the inferential risks
related to the study we use the function `retrospective()`.

``` r
retrospective(effect_size = .25, sample_n1 = 30,
              effect_type = "correlation", test_method = "pearson",
              seed = 2020)
#> 
#>  Design Analysis
#> 
#> Hypothesized effect:  rho = 0.25 
#> 
#> Study characteristics:
#>    test_method   sample_n1   sample_n2   alternative   sig_level   df
#>    pearson       30          30          two_sided     0.05        28
#> 
#> Inferential risks:
#>    power   typeM   typeS
#>    0.27    1.826   0.003
#> 
#> Critical value(s): rho  =  ± 0.361
```

In this case, the statistical power is almost 30% and the associated
Type M error and Type S errer are respectively around 1.80 and 0.003.
That means, statistical significant results are on average an
overestimation of 80% of the hypothesized population effect and there is
a .3% of probability to obtain a statistically significant result in the
opposite direction.

To know more about function arguments and examples see the function
documentation `?retrospective()` and `vignette("retrospective")`.

#### • `prospective()`

Given the hypothetical population effect size and the required power
level, the function `prospective()` performs a prospective design
analysis for Cohen’s *d* (t-test comparing group means) or Pearson’s
correlation test between two variables. According to the defined
alternative hypothesis and the significance level, the required sample
size is computed together with the associated Type M error, Type S
error, and the critical correlation value (i.e., the minimum absolute
effect size value that would result significant).

Considering a study that will evaluate the correlation between two
variables. Knowing from the literature that we expect an effect size of
*ρ* = .25, the function `prospective()` can be used to know the required
sample size to obtain a power of 80%.

``` r
prospective(effect_size = .25, power = .80, 
            effect_type = "correlation", test_method = "pearson",
            display_message = FALSE, seed = 2020)
#> 
#>  Design Analysis
#> 
#> Hypothesized effect:  rho = 0.25 
#> 
#> Study characteristics:
#>    test_method   sample_n1   sample_n2   alternative   sig_level   df 
#>    pearson       126         126         two_sided     0.05        124
#> 
#> Inferential risks:
#>    power   typeM   typeS
#>    0.807   1.107   0    
#> 
#> Critical value(s): rho  =  ± 0.175
```

The required sample size is \(n=126\), the associated Type M error is
around 1.10 and the Type S error is approximately 0.

To know more about function arguments and examples see the function
documentation `?prospective()` and `vignette("prospective")`.

### Hypothetical effect size

The hypothetical population effect size can be defined as a single value
or as a probability distribution. This is a useful feature of PRDAbeta,
as it allows users to define hypothetical effect size according to a
distribution that represents their expectations or literature
indications. For an example of application see
`vignette("retrospective")`.

## Citation Information

## References

Altoè, Gianmarco, Giulia Bertoldo, Claudio Zandonella Callegher, Enrico
Toffalini, Antonio Calcagnì, Livio Finos, and Massimiliano Pastore.
2020. “Enhancing Statistical Inference in Psychological Research via
Prospective and Retrospective Design Analysis.” <em>Frontiers in
Psychology</em> 10.
<a href="https://doi.org/10.3389/fpsyg.2019.02893">https://doi.org/10.3389/fpsyg.2019.02893</a>.

Bertoldo, Giulia, Claudio Zandonella Callegher, and Gianmarco Altoè.
2020. “Designing Studies and Evaluating Research Results: Type M and
Type S Errors for Pearson Correlation Coefficient.” Preprint. PsyArXiv.
<a href="https://doi.org/10.31234/osf.io/q9f86">https://doi.org/10.31234/osf.io/q9f86</a>.

Gelman, Andrew, and John Carlin. 2014. “Beyond Power Calculations:
Assessing Type S (Sign) and Type M (Magnitude) Errors.” <em>Perspectives
on Psychological Science</em> 9 (6): 641–51.
<a href="https://doi.org/10.1177/1745691614551642">https://doi.org/10.1177/1745691614551642</a>.

</p>

Goodman, Steven N. 1994. “The Use of Predicted Confidence Intervals When
Planning Experiments and the Misuse of Power When Interpreting Results.”
<em>Annals of Internal Medicine</em> 121 (3): 200.
<a href="https://doi.org/10.7326/0003-4819-121-3-199408010-00008">https://doi.org/10.7326/0003-4819-121-3-199408010-00008</a>.

Lenth, R. V. 2007. “Statistical Power Calculations1.” <em>Journal of
Animal Science</em> 85 (suppl\_13): E24–E29.
<a href="https://doi.org/10.2527/jas.2006-449">https://doi.org/10.2527/jas.2006-449</a>.

Senn, Stephen J. 2002. “Power Is Indeed Irrelevant in Interpreting
Completed Studies.” <em>BMJ: British Medical Journal</em> 325 (7375):
1304.
<a href="https://doi.org/10.1136/bmj.325.7375.1304">https://doi.org/10.1136/bmj.325.7375.1304</a>.
