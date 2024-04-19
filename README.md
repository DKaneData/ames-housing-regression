# Project 2 - Ames Housing Data and Kaggle Challenge

## Author: David Kanevsky

## Looking at the impact of the Great Recession on Ames Housing prices

### Reading Notebooks
The notebooks should be read in the following order

1. Reading-EDA-Cleaning.ipynb:
-- This was where much of the initial exploratory analysis and recoding was done. This was where I read in the original train.csv data set, explored it, cleaned it, and exported the celaned version as clean_recoded_ames.csv.
   
2. Feature_selection_modeling.ipynb
-- This was where I took the cleaned test set and ran multiple models on it (including linear regression, lasso, and ridge) to determine which models perform the best. Several models were built:
   --- A linear regression model featuring all the numeric data. This model was called 'lr_numeric'.
   --- A linear regression model featuring all the numeric data where the numeric data had a correlation above 0.3 with sale price. This model was called 'lr'.
   --- A linear regression model featuring all the numeric data where the numeric data had a correlation above 0.3 with a standard scaler applied. This model was called 'lr_scaled'.
   --- A linear regression model featuring just categorical data (removing categories where more than 90% of houses shared a single feature). This model was called 'lr_dum'
   --- A linear regression model that reduced the categorical data to just 7 features. This was called 'lr_fewer_dum'.
   --- A linear regression model that combined the numerical data with a correlation above 0.3 with sale price and the 7 categorical variables. This model was called 'lr_comb'.
   --- A ridge model on the combined numerical data with a correlation above 0.3 with sale price and the 7 categorical variables. This model was called 'ridge'.
   --- A lasso model on the combined numerical data with a correlation above 0.3 with sale price and the 7 categorical variables. This model was called 'lasso'.
   --- A linear regression model on all the numeric features and the 7 categorical features. This model was called 'lr_filt_num_cat'.
   --- A linear regresion model on all the numeric features and the 7 categorical features that used a standard scaler. This was called 'ols'.
   --- A linear regression model on all the numeric features, minus "sold in crisis," and the 7 categorical features. This model was called 'lr_drop_crisis'.   

3. Cleaning-Test-Data
-- This was where I read in the test.csv data set, and performed the same cleaning on the test.csv data as I did on the train.csv data set. The final cleaned test set was exported as 'clean_recoded_test_ames.csv'

4. Score-Test-Data
--This was where I loaded both the 'clean_recoded_test_ames.csv' test data set and the 'clean_recoded_ames.csv' (that was built off the training models). I then ran the linear regression model on just the numeric features (lr_numeric) and used that to build the predictions for the sale price on test data set. The final predictions were exported as 'DK-submission.csv' with the identifier and predicted sale price for the houses in the testing data set.

5. Score-Attempt-2
-- This is another worksheet where I am loading the cleaned test data, and attempting to run the Linear Regression model where I include all the numeric data (except for "sold in crisis") and the 7 categorical variables to see if I can improve my Kaggle score. I wasn't able to finish this in time since the number of columns in the training and test data set didn't line up, since when I dummified the test data set, some categories that are in the training data set, don't appear in the test data set.

### Data Modifications
-- The original data dictionary can be referenced, with some modications as noted below.

-- The original data dictionary can be referenced, with some modifications as noted below.

-- Two houses that were above 4,000 square feet were dropped from the data set since they are outliers.

-- The variable names in the original data dictionary were changed to lower case and snake case to make it easier to code. Ex: 'MS Zoning' becomes 'ms_zoning'

-- A variable called 'age_at_sale' was calculated by taking the year the house was sold ('yr_sold') and subtracting the year the house was built ('year_built').

-- A variable called 'age_at_remodel' was calculated by taking the year the house was sold ('yr_sold') and subtracting the year the houise was remodeled ('year_remod/add').

-- A variable called 'bsmt_fin_sf', referring to the finished square feet in the basement, was calculated by taking the total basement square feet ('total_bsmt_sf') and subtracting the basement unfinished square feet ('bsmt_unf_sf').

-- Since according to Wikipedia (https://en.wikipedia.org/wiki/2000s_United_States_housing_market_correction), the Great Recession is seen as starting in December 2007, a variable called 'sold_in_crisis' was created to identify if the house was sold during the Great Recession. If houses were sold in 2008 or later based on the 'yr_sold' column, data was coded as a 1 under 'sold_in_crisis' and a 0 if it was sold before 2008. Since the data does not indicate what day the the house was sold, I made the choice to include all 18 houses sold in December 2007 as being sold before the financial crisis. The houses sold in December 2007 represent less than 1% of all records in the data set and less than 2% of all records marked as being sold before the financial crisis.

-- The missing data for the square feet of Mason Vaneer Area ('mas_vnr_area') was filled as 0 since all 22 rows where there was missing data were also rows were Mason Vaneer Type ('mas_vnr_type') was missing.

-- The missing data around basement and garage variables ('bsmt_qual', 'bsmt_cond', 'bsmt_exposure', 'bsmtfin_type_1', 'bsmt_sf_1', 'bsmtfintype_2', bsmt_fin_sf_2', 'bsmt_unf_sf', 'total_bsmt_sf', 'bsmnt_fin_sf', 'bsmt_full_bath', 'bsmt_half_bath') were filled in as 0 under the assumption that no data meant there was no basement or basement bathroom.

-- Several categorical variables were recoded as ordinal variables based on the initial data dictionary that indicated the order of the variables. if the variable was missing data due to a feature non-extsent, it was labeled as 0, while otherwise the lowest rated feature would be coded as 1. The categorical variables recoded as ordinal variables include 'lot_shape', 'land_slope', 'exter_qual', 'exter_cond', 'bsmt_qual', 'bsmt_cond', 'bsmt_exposure', 'bsmt_fin_type_1', 'bsmt_fin_type_2', 'heat_ing_qc', 'central_air', 'electrical', 'kitchen_qual', 'functional', 'fireplace_qu', 'garage_finish', 'garage_qual', 'garage_cond', 'paved_drive' and 'fence'.

-- Mo sold was recoded as a categorical variable.

-- Since less than 0.5% of the training data had a pool, 'pool_area' was recoded into 'has_pool' with values for 1 for having a pool and 0 for not. Similarly, a variable was added for 'has_wood_deck' to indicate the presence of a wood deck.

## Conclusion

-- Being sold during the financial crisis in 2008 did not have a significant impact on sales price, though some of this is limited by the data set we have only running from 2006 to 2010. After that initial exploratory analysis, most of the focus was on producing the best performing model for predicting price.
