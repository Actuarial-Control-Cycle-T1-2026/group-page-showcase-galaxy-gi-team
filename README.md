# SOA Case Study 2026: Actuaries in Space

### Project Team
Michelle Chen, Ianish Ketaruth, Ellen Lim, Alec Peng, Swetha Ramesh

## INSTRUCTIONS (from ellen)
The website is just to showcase our work, no requirements other than:
- the page must be working and properly deployed
- minimum 3 people in the group must contribute (different members must edit/commit)


Step 1: Upload images of plots we want to include

Step 2: Update the README.md file (basically this file dictates what the website shows)

I have already added the headings (below).

Notes:

Can insert R code like this (check README.md file):
``` r
freq_model <- glm(claim_count ~ equipment_type + equipment_age_group +
                    risk_factor + maintenance_int + usage_int,
                  family = poisson(),
                  data = data_freq_EF)
summary(freq_model)
```

Can insert image (plots, etc) like this (check README.md file):
![Figure 1](ACC.png)

## Project Outline

This project was completed as part of the 2026 SOA Research Institute Student Research Case Study Challenge. Our team developed insurance product recommendations for Cosmic Quarry Mining Corporation, a space mining company operating across three solar systems. Using historical claims data across four hazard areas including equipment failure, cargo loss, workers’ compensation, and business interruption, we applied actuarial modelling techniques to assess risk and estimate costs.

Our analysis focused on evaluating loss distributions and extreme scenarios to inform pricing and capital considerations. Based on these insights, we proposed tailored insurance product designs that account for the unique characteristics of each solar system. 

## Data Preparation

The primary datasets used in this analysis were provided in the Project Data section of the case study. These datasets included historical claims information for the hazard areas, macroeconomic data and prospective data. Qualitative data from the Online Encyclopedia Entries was also utilised to create assumptions for prospective data, based on risk hazards in each solar system. 

Significant data cleaning was required before modelling to ensure the datasets were suitable for statistical analysis. Data cleaning performed was the removal of negative values and missing claim information and the cleaning of data outside the ranges defined in the Data Dictionary. The missing claim information was removed to maintain data accuracy and sufficient data observations remain. Hence, through these procedures we have ensured accurate, appropriate and reliable data is inputted into our models.  

## Modelling Approach and Methodology
Describe general modelling appraoch used (i.e. just 1-2 paragraphs rather than including a modelling appraoch for each hazard area), and then include outputs from each of the hazard areas maybe

## Assumptions
INSERT TEXT

## Key Results
We could include a table containing expected net revenue for each of the systems for each hazard area?

## Risk Assessment
Go over risk profile of each solar system, no need to do separately for each hazard area I reckon

## Recommendations
Product design, future scalability, etc?
