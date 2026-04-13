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
INSERT TEXT

## Data Preparation
INSERT TEXT

## Modelling Approach and Methodology
INSERT TEXT

## Assumptions
INSERT TEXT

## Key Results
INSERT TEXT

## Recommendations
Product design, future scalability, etc?
