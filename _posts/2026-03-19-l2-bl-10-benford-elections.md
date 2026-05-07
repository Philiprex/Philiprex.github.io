---
layout: post
title:  "L2-BL 10: A Novel Benford-Based Approach for Detecting Statistical Anomalies in Precinct-Level Election Data"
date:   2026-03-19 12:00:00 -0400
categories: research
---

This is the companion code repository for a paper I co-authored with Steven J. Miller and Kevin Dayaratna, titled "A Novel Benford-Based Approach for Detecting Statistical Anomalies in Precinct-Level Election Data." The paper is currently in proofs at *Statistical Modelling* (the journal of the Statistical Modelling Society, published by SAGE) and is expected in the next quarterly issue. The code lives at [Philiprex/L2-BL-10](https://github.com/Philiprex/L2-BL-10), and depends on my [`dWeibullAlt`]({% post_url 2026-01-20-dweibullalt-stats-package %}) package for the discrete Weibull distribution.

This post is a high-level walkthrough of what the framework is and how the code is organized. It deliberately does not discuss the empirical results or any specific jurisdictions; that's what the paper is for.

## What "L2-BL 10" stands for

The traditional Benford's Law test looks at the distribution of *first* digits of a dataset and asks whether they follow the logarithmic distribution Benford predicts. That works well when the data spans many orders of magnitude, but precinct-level vote counts typically don't — they cluster in a relatively narrow range, which is exactly the situation where first-digit Benford analysis is known to be unreliable.

L2-BL 10 ("last two digits, base 10") instead examines the joint distribution of the *final two digits* of the counts. The last two digits don't depend on the order of magnitude of the underlying number, so the test applies cleanly to data that lives within a single order of magnitude. The framework works out the expected joint distribution of those digit pairs under a stated distributional assumption and tests how well observed precinct data conforms to it.

## The pipeline

For a given election, the analysis proceeds in four steps:

1. **Fit a discrete Weibull distribution** to each county's precinct-level vote counts via maximum likelihood. This is where `dWeibullAlt` comes in — the (μ, σ) parameterization is the natural one for this fitting step.
2. 2. **Assess goodness of fit** of the Weibull using a parametric bootstrap discrete Kolmogorov–Smirnov test (the discrete, one-sample, two-tail bootstrap KS test). This tells us whether the Weibull is a defensible distributional model for the precincts in that county before we go further.
   3. 3. **Assess Benford conformity of the *fitted* distribution** via a Monte Carlo chi-squared test on samples drawn from the fitted Weibull. This step asks: "if the data really were Weibull with these parameters, would we expect L2-BL 10 conformity?"
      4. 4. **Assess Benford conformity of the *observed* data** via chi-squared tests on the matched-pair frequencies of last-two-digit counts.
        
         5. A county is flagged for further investigation only when *all four* of those checks line up in a specific way — the Weibull fit is good, the fitted distribution is itself Benford-conforming, but the observed data is not. Bonferroni correction is applied across counties within a state, so the flagging threshold accounts for multiple comparisons. The framework is conservative by design: any one of those steps failing in the other direction is enough to *not* flag.
        
         6. ## The empirical application
        
         7. The repository applies the pipeline to nine states across the 2016 and 2020 U.S. presidential elections: FL, GA, MD, MO, NE, NY, PA, TN, and WA. For some of those states the analysis is restricted to in-person votes because of how absentee ballot data was reported in the underlying source. Precinct-level data comes from the MIT Election Data and Science Lab (MEDSL).
        
         8. The paper itself is the place to read about the results, the limitations, and the appropriate interpretation of any flagging the framework produces. I'm deliberately not summarizing or paraphrasing those parts here.
        
         9. ## Repository layout
        
         10. The code is split into roughly four areas:
        
         11. - `utilities/` — the core statistical functions: the d/p/q/r discrete Weibull family (which mirrors and depends on `dWeibullAlt`), the MLE fitter, the bootstrap d-KS test, the Monte Carlo chi-squared test, and the Benford probability calculations for digit positions and digit pairs.
             - - `president_2016/` and `president_2020/` — election-specific directories containing the raw data, a data-refining script, the pipeline runner, and a results formatter that compiles state-level results and applies Bonferroni correction.
               - - `weibullSimulations/` — a separate study that maps L2-BL 10 conformity of the discrete Weibull itself across a grid of (μ, σ) values at several sample sizes. This is what justifies the assumption-checking step in the main pipeline.
                 - - `graphing/` — generators for the seven figures in the paper.
                  
                   - The pipeline is computationally heavy (1,000 bootstrap d-KS samples and 100 Monte Carlo chi-squared simulations per county-candidate) but is designed to be re-run end-to-end from the data file forward.
                  
                   - ## Reproducibility
                  
                   - Everything needed to reproduce the analysis is in the repository: the cleaned and raw input data, the utility functions, the per-state pipeline scripts, and the figure generators. The only external dependencies are R (≥ 4.0), the tidyverse, `KSgeneral` for exact discrete KS test statistics, and `dWeibullAlt`.
                  
                   - *More to come — once the paper is out I'll link the published version and add a short non-technical writeup of the framework's motivation and the limits of what it can and cannot tell you.*
                   - 
