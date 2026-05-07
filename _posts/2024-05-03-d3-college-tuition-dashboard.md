---
layout: post
title:  "(Relative) Triumph in D3: Visual Analytic Tools Regarding College Tuition"
date:   2024-05-03 12:00:00 -0400
categories: school
---

This was a visual dashboard I built with a friend as a final project for a data visualization class. The goal was to give a prospective student (or anyone curious) a few different lenses through which to look at U.S. college tuition: where it is, how it's distributed, how it changes over time, and how it differs across public and private institutions. The data comes from the U.S. Department of Education's College Affordability and Transparency List, filtered and processed for the dashboard.

The dashboard lives on Observable: [College Tuition Exploratory Dashboard](https://observablehq.com/d/4546a20283e5d150).

## What's in it

There are four linked visualizations, each focused on a different question.

**A choropleth map of the U.S.** colored by average tuition per state. Hovering a state surfaces its name and average cost of attendance. This is the orientation view — useful for a quick "where does my state sit" read before drilling in.

**A boxplot with controls** for variable (tuition vs. percent change), year range (2011–2021), and sector (all / public / private). The boxplot is the distributional view: it shows the spread, the quartiles, and how the middle of the market shifts as you slide the year range or flip between public and private.

**A scatterplot** with tuition on the x-axis and year-over-year percent change on the y-axis, filtered by year and sector. Hovering a point reveals the institution name, its tuition, and its percent change. This is where individual schools become legible — you can spot outliers in either dimension.

**A multi-line chart** showing each school's tuition trajectory over time, filterable by sector and by state. Some lines drop out where data is missing, but the overall shape of how tuition has moved is the point.

## Notes

The "(Relative) Triumph" in the title is honest. Some of what I'd do differently with another pass: tooltip styling on the scatterplot is rough, the boxplot's brush doesn't actually drive any downstream filtering, and the multi-line chart could use color encoding for sector or state rather than a single steel-blue. But for a course final it did what it set out to do, and putting it on Observable instead of stapling screenshots to a PDF made the difference between "a project" and "a thing you can actually use."

*More to come — I'll add screenshots and a more detailed retrospective on the design choices when I get a chance.*
