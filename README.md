# e-car Loan Pricing Dashboard

An interactive web dashboard that estimates the profit-maximizing APR for a car loan at **e-car**. Adjust the applicant and market inputs, and the chart shows expected profit across APRs from 2% to 10%, with the best APR highlighted.

This is a browser version of the original Python notebook (`ipywidgets` `interact` around `pricing_helper`). It is a single self-contained HTML file with no dependencies, build step or server.

## Features

- Expected profit vs. APR chart with the optimal APR marked by red dashed guide lines
- Summary cards: best APR, expected profit at that APR, and acceptance probability
- Live updates as any control changes
- Light and dark mode support
- Works on desktop and mobile

## Inputs

| Input | Type | Range / Options | Default |
|---|---|---|---|
| Tier | Dropdown | 1, 2, 3, 4 | 3 |
| FICO | Slider | 585 – 860 | 697 |
| Term (months) | Dropdown | 36, 48, 60, 66, 72 | 60 |
| Amount ($) | Slider | 5 – 100,000 | 21,400 |
| Car Type | Dropdown | N (new), U (used), R (refinance) | U |
| Competition rate (%) | Slider | 2.99 – 6.45 | 4.8 |
| Cost of Funds (%) | Slider | 1.02 – 2.127 | 1.3 |
| Partner Bin | Dropdown | 1, 2, 3 | 3 |

## How it works

For each APR on a 2%–10% grid, the dashboard computes:

```
expected profit = P(accept) × margin × loan amount × average balance factor
margin          = APR − cost of funds − expected loss rate
```

The best APR is the grid point with the highest expected profit.

## Important note: placeholder model

The pricing logic in `pricingModel()` is a **simplified stand-in**, not the production `pricing_helper` model:

- Acceptance probability is a logistic function of APR relative to the competition rate, shifted by partner bin and tier.
- Expected loss rate rises as FICO falls, and also varies with tier, car type and term.
- Profit is shown in dollars, so the y-axis scale will differ from the original notebook.

To use the real model, replace the `pricingModel(p, apr)` function in the `<script>` block. It must return `{prob, profit}` for the given inputs and APR. If the real model can't run in a browser (for example, it depends on a trained model or data files), export a precomputed grid from Python and have the page look values up from it.

## Run locally

Open `index.html` (or `ecar_pricing_dashboard.html`) in any modern browser.

## Project structure

```
.
├── index.html     # the dashboard (HTML, CSS and JS in one file)
└── README.md
```
