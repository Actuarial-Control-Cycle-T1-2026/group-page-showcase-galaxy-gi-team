# SOA Case Study 2026: Actuaries in Space

### Project Team
Michelle Chen, Ianish Ketaruth, Ellen Lim, Alec Peng, Swetha Ramesh

## INSTRUCTIONS (from ellen)
The website is just to showcase our work, no requirements other than:
- the page must be working and properly deployed
- minimum 3 people in the group must contribute (different members must edit/commit)


Step 1: Upload images of plots we want to include

Step 2: Update the README.md file (basically this file dictates what the website shows)


## Project Outline

This project was completed as part of the 2026 SOA Research Institute Student Research Case Study Challenge. Our team developed insurance product recommendations for Cosmic Quarry Mining Corporation, a space mining company operating across three solar systems. Using historical claims data across four hazard areas including equipment failure, cargo loss, workers’ compensation, and business interruption, we applied actuarial modelling techniques to assess risk and estimate costs.

Our analysis focused on evaluating loss distributions and extreme scenarios to inform pricing and capital considerations. Based on these insights, we proposed tailored insurance product designs that account for the unique characteristics of each solar system. 

## Data Preparation

The primary datasets used in this analysis were provided in the Project Data section of the case study. These datasets included historical claims information for the hazard areas, macroeconomic data and prospective data. Qualitative data from the Online Encyclopedia Entries was also utilised to create assumptions for prospective data, based on risk hazards in each solar system. 

Significant data cleaning was required before modelling to ensure the datasets were suitable for statistical analysis. Data cleaning performed was the removal of negative values and missing claim information and the cleaning of data outside the ranges defined in the Data Dictionary. The missing claim information was removed to maintain data accuracy and sufficient data observations remain. Hence, through these procedures, we have ensured accurate, appropriate and reliable data is inputted into our models.  

See example code to ensure data is appropriate as per the Data Dictionary

``` r
cargo_freq_clean <- cargo_freq_clean %>%
  filter(
    cargo_value >= 50000 & cargo_value <= 680000000,
    weight >= 1500 & weight <= 250000,
    route_risk %in% c(1,2,3,4,5),
    distance >= 1 & distance <= 100,
    transit_duration >= 1 & transit_duration <= 60,
    pilot_experience >= 1 & pilot_experience <= 30,
    vessel_age >= 1 & vessel_age <= 50,
    solar_radiation >= 0 & solar_radiation <= 1,
    debris_density >= 0 & debris_density <= 1,
    exposure >= 0 & exposure <= 1,
    claim_count >= 0 & claim_count <= 5,
    claim_count == floor(claim_count)
  )
```

## Exploratory Data Analysis
To better understand the severity and frequency data from the datasets, exploratory data analysis was undertaken. In this step, we looked at summary statistics, histograms and plots. 

<img width="666" height="484" alt="Screenshot 2026-04-14 at 10 25 56 PM" src="https://github.com/user-attachments/assets/930cf15d-179f-47b6-8a1c-caa446a575df" />


Example for Cargo Loss

The above histogram demonstrates the bimodal nature of cargo loss severity. This is due to a higher number of small or catastrophic cargo accidents. Hence, through this exploratory data analysis, we can better understand the nature of the cargo loss claims and therefore the best way to price them. 

## Modelling Approach and Methodology

We developed our modelling framework by first preparing historical claims data across all four hazard areas, ensuring consistency in both frequency and severity datasets. Using this cleaned data, we constructed Generalised Linear Models (GLMs) to capture the underlying relationships between risk drivers and claim outcomes.

To project future performance, we combined the GLM outputs with prospective exposure data provided in the RFP, allowing us to generate predictions of claim frequency and severity for Cosmic Quarry Mining Corporation’s operations. We then applied a stochastic simulation approach, running 10,000 simulations to model the aggregate loss distribution for each hazard area. This enabled us to quantify the expected losses, variability, and tail risk, providing a solid foundation for pricing and product design.

### Cargo Loss
For cargo loss frequency, a Poisson model was initially fit. However, after finding the dispersion coefficient of 4.506735, it was clear that the model was too dispersed to fit the data. Then a negative binomial was fit to cargo loss frequency. After testing the AIC, it was clear that the negative binomial model was a better fit for the dataset. Then, testing was performed to reduce the complexity of the model by removing insignificant and unnecessary predictors. AIC was also used to test the goodness of fit of the model, with results showing a reduced model was less complex and a better fit. 

For cargo loss severity, from the EDA above it was clear that severity was split into two distinct groups. After testing a Gamma model and the Pearson residuals, it was clear that Gamma was not a good fit for the data. 

<img width="669" height="488" alt="Screenshot 2026-04-14 at 10 33 17 PM" src="https://github.com/user-attachments/assets/8638a0a5-f79a-4d95-a1ea-a7eae710921f" />

After seeing the spread of claim amount by cargo type (see above image), the severity data was split depending on cargo group. Then two Gamma generalised linear models were fit to each dataset. After comparing the AIC after this change, it was determined that this was a better fit for the data. 

## Assumptions
### 5a. Business Interruption

**Model**
- Negative Binomial frequency (dispersion: 1.97)
- Gamma severity (dispersion: 2.82)
- Claims assumed independent

**Key Variables**
- Supply chain index (p = 0.022)
- Energy backup score (p = 0.036)  
→ Both significant for frequency only

**Product Features**
- Deductible: 10th percentile ($462,800)
- Per-claim cap: 95th percentile ($15.2M)
- Aggregate cap: 99.5th percentile ($1.105B)
- Minimum requirements:
  - Energy backup ≥ 2/5
  - Safety compliance ≥ 2/5

**Pricing Inputs**
- Expense ratio: 15%
- Risk loading: 10%
- Profit margin: 5%
- Inflation: 4.23%
- Discount rate: 5.10%

---

### 5b. Workers Compensation

**Model**
- Poisson frequency (dispersion: 0.994)
- Gamma severity (dispersion: 5.50)
- Claims assumed independent

**Key Assumptions**
- Gravity factors:
  - Bayesia: 1.375
  - Oryn Delta: 0.875
  - Helionis: 1.125
- Workforce distribution: 54.5% / 27.3% / 18.2%
- 23 roles mapped to 11 occupation categories
- Salary cap: $130,000
- Missing worker characteristics simulated

**Key Variables**
- Frequency:
  - Occupation
  - Accident history
  - Psychological stress
  - Gravity (p = 0.003)
  - Safety training
- Severity:
  - Psychological stress
  - Base salary
- Gravity not significant for severity (p = 0.895)

**Product Features**
- Excess: 10th percentile ($555)
- Per-claim cap: 95th percentile ($48,000)
- Aggregate cap: ~99.5th percentile ($24M)
- Exclusion:
  - Safety training < level 2 (43% higher claim rate)
  - No exclusion for gear (not significant)

**Pricing Inputs**
- Expense ratio: 15%
- Risk loading: 10%
- Profit margin: 5%
- Inflation: 4.23%
- Discount rate: 5.10%
- Workforce growth: 2.1% p.a.

---

### 5c. Equipment Failure

**Model**
- Poisson frequency (dispersion ≈ 1)
- Gamma severity (dispersion ≈ 0.73)

**Key Variables**
- Maintenance frequency (p < 2e-16)  
→ Significant for frequency only

**Product Features**
- Deductible: 10th percentile ($34,740)
- Per-claim cap: 95th percentile ($196,169)
- Aggregate cap: 99.5th percentile ($60M)
- Maintenance requirement:
  - ≤ 2,500 hours between servicing

**Pricing Inputs**
- Expense ratio: 15%
- Risk loading: 10%
- Profit margin: 5%
- Inflation: 4.23%
- Discount rate: 5.10%

---

### 5d. Cargo Loss

**Model**
- Poisson frequency (dispersion ≈ 1)
- Gamma severity (dispersion ≈ 0.73)

**Key Variables**
- Maintenance (p < 2e-16)  
→ Highly significant for frequency, limited impact on severity

**Product Features**
- Deductible: 10th percentile ($34,740)
- Per-claim cap: 95th percentile ($196,169)
- Aggregate cap: 99.5th percentile ($60M)
- Maintenance threshold:
  - ≤ 2,500 hours between servicing

**Pricing Inputs**
- Risk margin: 15%
- Expense ratio: 10%
- Profit margin: 5%
- Inflation: 2.46% p.a.
- Trend factor: 1.22 (over 8 years)
- Discount rate: 4.74%
- Claim settlement lag: 0.5 years

---

## Key Results
We could include a table containing expected net revenue for each of the systems for each hazard area?

### Expected Net Revenue For Each Product

|Solar System|Business Interruption|Cargo|Equipment Failure|Workers Compensation|
|:---|:---|:---|:---|:---|
|Helionis Cluster|insert value|$3.6B|$3.5M|insert value|
|Bayesia System|insert value|$3.1B|$1M|insert value|
|Oryn Delta|insert value|$3.2B|$363K|insert value|

## Risk Assessment
Go over risk profile of each solar system, no need to do separately for each hazard area I reckon

## Recommendations
Product design, future scalability, etc?
