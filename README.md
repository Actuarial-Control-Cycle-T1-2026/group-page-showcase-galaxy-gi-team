# SOA Case Study 2026: Actuaries in Space

### Project Team
Michelle Chen, Ianish Ketaruth, Ellen Lim, Alec Peng, Swetha Ramesh

## Project Outline
INSERT TEXT

## Data Preparation
INSERT TEXT

## Modelling Approach and Methodology
INSERT TEXT

Notes:
Can insert R code like this (check raw version):
``` r
freq_model <- glm(claim_count ~ equipment_type + equipment_age_group +
                    risk_factor + maintenance_int + usage_int,
                  family = poisson(),
                  data = data_freq_EF)
summary(freq_model)
```

Can insert image (plots, etc) like this (check raw version):
![Figure 1](ACC.png)

## Assumptions
INSERT TEXT

## Key Results
INSERT TEXT

## Recommendations
Product design, future scalability, etc
