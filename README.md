# WHO BMI & Child Growth Calculator (All Ages)

A responsive, zero-dependency web application that automatically calculates **Body Mass Index (BMI)**, **BMI-for-Age**, and **Height-for-Age** Z-scores and percentiles across all age groups:
- **0–5 Years (0–60 months):** WHO Child Growth Standards (WHO CGS)
- **5–19 Years (61–228 months):** WHO 2007 Growth Reference
- **19+ Years (Adults):** WHO Adult BMI Classification

## Features & Functions

### 1. Live Auto-Computing Inputs
- **Sex Selection (`setSex`)**: Switch between **Boy** and **Girl** WHO CGS reference tables.
- **Age / Height-in-Age (`setAgeUnit`, `syncAgeSlider`)**: Enter age in **Months** (`0–60`) or **Years** (`0–5`) via numeric input or range slider. Automatically indicates whether the **recumbent length standard** (`< 24` months) or **standing height standard** (`>= 24` months) applies.
- **Height / Length (`setHeightUnit`)**: Input in **cm** or **m** with automatic unit conversion.
- **Weight (`setWeightUnit`)**: Input in **kg** or **lbs** with automatic unit conversion.
- **Quick Clinical Presets (`applyPreset`)**: One-click test scenarios (`24m Boy · Median`, `36m Girl · Overweight`, `12m Boy · Thinness`, `48m Girl · Stunted`).

### 2. WHO CGS Calculations (`compute`)
- **BMI Calculation**: Computes $\text{BMI} = \frac{\text{weight (kg)}}{\text{height (m)}^2}$ live on every input change.
- **LMS Interpolation (`interpolateLMS`)**: Linearly interpolates WHO $L$ (Box-Cox power), $M$ (median), and $S$ (coefficient of variation) parameters across `0–60` months for both BMI-for-age (`WHO_BMI_LMS`) and Length/Height-for-age (`WHO_HEIGHT_MS`).
- **WHO Z-Score Calculation (`calcWhoZ`)**: Applies the Box-Cox LMS transformation, including WHO's restricted LMS tail adjustment for $|Z| > 3$.
- **Percentile Conversion (`normCdf`, `formatPercentile`)**: Converts Z-scores to percentiles using a standard normal CDF approximation.
- **Clinical Classifications (`classifyBmiCgs`, `classifyHeightCgs`)**:
  - **BMI-for-Age (`0–60` months)**: `Severe Thinness (< -3 SD)`, `Wasted / Thinness (-3 to -2 SD)`, `Normal Weight (-2 to +2 SD)` (with a `> +1 SD` watch flag), `Overweight (> +2 to +3 SD)`, and `Obesity (> +3 SD)`.
  - **Height-for-Age (`0–60` months)**: `Severely Stunted (< -3 SD)`, `Stunted (-3 to -2 SD)`, `Normal Stature (-2 to +2 SD)`, and `Tall for Age (> +2 SD)`.

### 3. Visual Feedback & Growth Curves
- **Z-Score Position Bar**: Plots the child's BMI-for-Age Z-score across `-4 SD` to `+4 SD`.
- **Interactive SVG Growth Chart (`drawChart`, `setChartMode`)**: Renders WHO CGS `-3 SD`, `-2 SD`, `0 SD (Median)`, `+2 SD`, and `+3 SD` curves from `0–60` months with a live marker for the child's measurement. Toggleable between **BMI-for-Age** and **Height-for-Age**.
- **Mobile-Friendly Design**: Responsive layout, `inputmode="decimal"` numeric keypads, `16px` mobile inputs to prevent iOS zoom, and `44px+` touch targets.

## Running Locally

```bash
python -m http.server 8080
```

Then open `http://localhost:8080` in your browser.

## Free Hosting on Render

This repository includes a [`render.yaml`](render.yaml) configured for a **Render Static Site** (`runtime: static`, `staticPublishPath: .`), which deploys free of cost on Render:

1. Connect this GitHub repository in the Render dashboard.
2. Choose **New → Blueprint** (or **New → Static Site** with Publish Directory `.`).
3. Deploy.
