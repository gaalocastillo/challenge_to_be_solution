
Data analysis and classification model proposed as solution of the **Challenge To Be or Not To Be**, presented on the Hands on Machine Learning course 2021-2022 at Université Paris-Saclay.

# Group members
* Galo Castillo
* Maria Guaranda
* Abdurrahman Shahid

# Presentation of the challenge

## Main question of this challenge

* How to predict the survival of a patient according to their medical files and physiological data?
* Specifically, you will need to predict it during their hospital stay
 
Every day, the nursing staff collects information about the patients by asking questions and using measurement tools (stethoscope, blood test, sensors, etc.). These data are very useful for monitoring the state of health, diagnosing, and choosing treatments.

## Data

The dataset contains information on 80,000 patients, represented by categorical, binary and, numeric variables (features). These variables are, e.g., age, sex, ethnicity, marital status, as well as medical data such as blood pressure or glucose level. There are a total of 342 variables.

The class to predict is a binary variable indicating whether the patient died or not during his stay in the hospital. Fortunately, most survive.

# Note on the dataset

The dataset does not contain actual medical data. It is not allowed to share these **confidential data** from ICU patients. To avoid this problem, the data have been replaced with **artificial data**.
To have credible data, they were generated using a generative adversarial network (**GAN**) Wasserstein.

For more information:
Andrew Yale, Saloni Dash, Ritik Dutta, Isabelle Guyon, Adrien Pavao, et al. [Privacy Preserving Synthetic Health Data](https://hal.inria.fr/hal-02160496/document). ESANN 2019 - European Symposium on Artificial Neural Networks, Computational Intelligence and Machine Learning, Apr 2019, Bruges, Belgium.

# Explanation of what we achieved, conclusion and perspectives
Our best model is LightGBM, which got a 0.764 (+/-) 0.006 balanced accuracy score when doing a 5-fold cross-validation. In the Codalab platform, we got a score of 0.76 too, which is similar to what we got with our validation sets. To get to that model, we:  
1) Performed exploratory data analysis: 
- Checked types of variables present, deleted variables that only had one value (constant variables)
- Handled missing values (imputation using most frequent values). We performed this because only categorical and binary variables had missing values.
- Grouped underrepesented values of categorical features under a single value.
- Performed data scaling
- We also tried to handle outliers by using the interquantile range rule and replacing, per each feature, the outlier values with the lower and upper boundaries obtained by the IQR computation (i.e. values above an upper boundary were replaced by the upper boundary, values below the lower boundary were replaced by the lower boundary). However this did not improve the results, so we left it  out as it only increased the computation time (and it would have been like trash code).  

2) Tested several ML models: LightGBM, XGBoost, SVM, Perceptron, Linear and Logistic Regression and Decision Trees.
Out of them, during the training phase, without any aditional tuning, LightGBM gave us the best score so we kept it for further improvements.  
3) We found that experiments where we used col_sample_by_tree and subsample hyperparameters (randomly selecting a fraction of features and observations, respectively) obtained better results when performing K-fold cross validation, since they work as regularization.

Also, we found that the max_depth and scale_pos_weight hyperparameters were the most important for obtaining good results on the tree-based models (LightGBM and XGBoost). For this classification problem, using properly the scale_pos_weight hyperparameter was key since it helps to handle imbalanced labels problems.

Moreover there was no significant difference between the training time of the Logistic Regression and our best LightGBM model (2.2s and 4.5s on average, respectively). However, the XGBoost model took around 14s to be trained.

If we had had more time:
- We would have given more time to ensamble our best models using an Stacking approach.
- We would have explored more in detail other resampling techniques to handle the labels imbalance issue.
- We would have read more about the meaning of the features (domain knowledge) to perform better feature engineering steps (i.e. eliminating features or creating new ones).
