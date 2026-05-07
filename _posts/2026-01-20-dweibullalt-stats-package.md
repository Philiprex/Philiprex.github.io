---
layout: post
title:  "dWeibullAlt: a {stats}-like Package for the Discrete Weibull Distribution"
date:   2026-01-20 12:00:00 -0500
categories: research
---

[`dWeibullAlt`](https://github.com/Philiprex/dWeibullAlt) is a small R package that provides `stats`-style support for the discrete Weibull distribution. "`stats`-style" here means the familiar `d`/`p`/`q`/`r` family — density, cumulative distribution, quantile, and random sampling — plus a maximum-likelihood parameter fitter, all written so that working with the discrete Weibull feels the same as working with any of the built-in distributions in base R.

A package for the discrete Weibull already exists on CRAN. The reason for writing this one was that I needed first-class support for the **location–scale (Stute) parameterization** — the one you'll see on the [Wikipedia page for the discrete Weibull](https://en.wikipedia.org/wiki/Discrete_Weibull_distribution) reparameterized in terms of μ and σ — for a paper to appear in *Statistical Modelling* (the journal of the Statistical Modelling Society, published by SAGE; the paper is currently in proofs and is expected in the next quarterly issue). The package also supports the original parameterization first introduced for the discrete Weibull on Wikipedia, so you can move between the two without leaving the package.

## Why a second parameterization matters

The original discrete Weibull is parameterized in terms of a shape parameter and a probability — fine for many purposes, but awkward when you want to reason about a location and a spread on the same scale as the data. The location–scale parameterization makes the distribution behave more like its continuous cousin: μ shifts the bulk of the mass, σ stretches it. For the precinct-level vote-count modeling in the [L2-BL 10 paper]({% post_url 2026-03-19-l2-bl-10-benford-elections %}), this matters because we needed a fitting routine and a goodness-of-fit pipeline that operated naturally in (μ, σ) space.

## What's in the package

The core surface is small and follows base R conventions:

- `ddweibull(x, ...)` — the probability mass function
- - `pdweibull(q, ...)` — the cumulative distribution function
  - - `qdweibull(p, ...)` — the quantile function
    - - `rdweibull(n, ...)` — random sampling
      - - a parameter-fitter that returns MLE estimates for either parameterization
       
        - Each of the d/p/q/r functions takes a flag selecting which parameterization you want, so you can fit in (μ, σ), simulate in (μ, σ), and convert when you need to.
       
        - ## Status
       
        - The package is intentionally lean — it exists to support the L2-BL 10 analysis and to make the (μ, σ) parameterization easy to reach for. The README is currently one sentence; expanding it with usage examples and a small vignette is on my list.
       
        - *More to come — I'll add a worked example (fitting to a small precinct-level dataset, running the bootstrap d-KS test from L2-BL 10) and a writeup of the numerical considerations behind the MLE fitter.*
        - 
