# STAT 5010 - Final Project: Modeling Calorie Content in Starbucks Beverages

**Team Members:**
* Jayan Agarwal
* Abhiram M V
* Tuan Nguyen

---

## Introduction

Starbucks' diverse menu exhibits considerable nutritional variability, particularly in calories. Understanding calorie determinants is relevant for consumer information and public health contexts. This study analyzes nutritional data from 242 Starbucks beverages (Kaggle dataset) to identify significant calorie predictors. Multiple linear regression (MLR) is employed to model calories based on nutritional components, evaluate model performance, and interpret predictor influence.

## Methods

Data preprocessing was performed using R (`tidyverse`, `janitor`). After loading and standardizing variable names, key nutritional metrics (`Calories`, `Fat`, `Sodium`, `Carbohydrates`, `Cholesterol`, `DietaryFibre`, `Sugars`, `Protein`, `VitaminA`, `VitaminC`, `Calcium`, `Iron`) were selected. Percentage Daily Values (%DV) were converted to absolute units (mg/mcg) using NIH reference values. Notably, due to model instability and diagnostic issues in initial analyses, an iterative 1.5\*IQR outlier removal procedure was applied across all selected variables, reducing the dataset to 162 observations for modeling. The implications of this data reduction are addressed later. Exploratory Data Analysis (EDA) on this reduced dataset utilized histograms, scatter plots, correlation heatmaps, and pairs plots to assess distributions and relationships.

The core analysis employed **Multiple Linear Regression (MLR)** on an 80%/20% train/test split (n_train=129, n_test=33). **Model Selection** via backward stepwise AIC identified a parsimonious predictor set from an initial model including all nutritional variables. The final selected model underwent **Model Diagnostics**: multicollinearity (VIF), residual analysis (standard diagnostic plots), and formal **Hypothesis Testing** for normality (Shapiro-Wilk), homoscedasticity (Breusch-Pagan), and independence (Durbin-Watson). **Confidence Intervals** (95%) for coefficients were computed, and predictive accuracy (MSPE/RMSE) was evaluated on the test set. Key statistical concepts employed include MLR, model selection, hypothesis testing (F-test, t-tests), model diagnostics, and confidence intervals.

## Results

EDA revealed strong positive correlations between `Calories` and `Carbohydrates` (r=0.96), `Sugars` (r=0.94), and `Fat` (r=0.47) in the outlier-removed data. A perfect correlation (r=1.00) between `Sugars` and `Carbohydrates` indicated substantial multicollinearity in the initial predictor set.

Stepwise AIC yielded a final model predicting `Calories` using `Fat`, `Sodium`, `Carbohydrates`, `Cholesterol`, `DietaryFibre`, and `Calcium`. This model achieved excellent explanatory power (Adjusted R² = 0.9971; F-test p < 2.2e-16). T-tests indicated `Fat`, `Carbohydrates`, `Cholesterol`, `DietaryFibre`, and `Calcium` were significant predictors (p < 0.01); `Sodium` was not (p ≈ 0.11). The coefficient for `Fat` (~9.66) indicates that each additional gram of fat is associated with an average increase of approximately 9.66 calories, holding other model variables constant. The coefficient for `DietaryFibre` was negative, suggesting a slight decrease in predicted calories per gram of fiber. Confidence intervals provided precise estimates for these effects.

Diagnostics confirmed no problematic multicollinearity in the final model (VIFs < 1.3) and supported error independence (Durbin-Watson p > 0.05) and homoscedasticity (Breusch-Pagan p > 0.05). However, residuals were not normally distributed (Shapiro-Wilk p < 0.001). Predictive accuracy on the test set remained high (MSPE = 16.75; RMSE ≈ 4.1 calories), indicating predictions are typically within about 4 calories of the actual value.

## Discussion and Conclusion

The MLR model effectively identified significant nutritional drivers of calorie content in the analyzed Starbucks beverages. `Carbohydrates` and `Fat` were confirmed as primary positive contributors, aligning with established energy values (approx. 4 kcal/g for carbs, 9 kcal/g for fat). The estimated coefficient for fat (~9.66) closely matched expectations. The small coefficient for `Carbohydrates` likely results from the removal of the highly collinear `Sugars` variable during model selection. The significance of `Cholesterol` and `Calcium` may reflect associations with calorie-contributing ingredients like dairy.

The primary limitation is the non-normality of residuals, potentially impacting the precision of inferential statistics. However, given the model's robustness, adequate sample size, and satisfaction of other assumptions, the findings are considered reliable for interpreting key relationships. The outlier removal strategy, while necessary for model stabilization in this case, restricts the model's applicability primarily to beverages with typical nutritional profiles and may limit direct comparison to analyses using the full dataset.

In conclusion, MLR effectively modeled calorie content based on core nutritional data. The model provides a statistically sound and accurate predictive tool for beverages within the studied range, identifying carbohydrates and fat as the most influential factors contributing to caloric load.

---

*This analysis was performed using R within a Jupyter Notebook environment. The notebook (`Starbucks-Nutritional-Analysis.ipynb`) and data (`starbucks.csv`) are included in this repository.*
