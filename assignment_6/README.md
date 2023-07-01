# Home Mortgage Disclosure Act (HMDA) Model Card

### Basic Information

* **Person or organization developing model**: Mariana Lungu, `marianalungu@gwu.edu`
* **Model date**: June 30, 2023
* **Model version**: 1.0
* **License**: MIT
* **Model implementation code**: [DNSC_6301_HMDA_Project.ipynb](https://github.com/marlungu/gwu_rml/blob/main/assignment_6/assignment_6.ipynb)

### Intended Use

* **Primary intended uses**: This model is not intended for business use; it's been developed for educational purposes. Its primary goal is to facilitate understanding and learning.
* **Primary intended users**: Students in GWU.
* **Out-of-scope use cases**: Any use beyond an educational example is out-of-scope.


### Training Data

* Data:


<img src="https://github.com/marlungu/gwu_rml/blob/main/assignment_6/data/data.png">

* **Dataset Columns**:
        'row_id', 'black', 'asian', 'white', 'amind', 'hipac', 'hispanic',
        'non_hispanic', 'male', 'female', 'agegte62', 'agelt62', 'term_360',
        'conforming', 'debt_to_income_ratio_missing', 'loan_amount_std',
        'loan_to_value_ratio_std', 'no_intro_rate_period_std',
        'intro_rate_period_std', 'property_value_std', 'income_std',
        'debt_to_income_ratio_std', 'high_priced'
* **Source of training data**: GWU Blackboard, email `jphall@gwu.edu` for more information
* **How training data was divided into training and validation data**: 70% training, 30% validation
* **Number of rows in training and validation data**:
  * Training rows: 112,253
  * Validation rows: 48,085


### Test Data

* **Source of test data**: GWU Blackboard, email `jphall@gwu.edu` for more information
* **State any differences in columns between training and test data**: None


### Model details

* **Columns used as inputs in the final model**: 
       'term_360', 'conforming', 'debt_to_income_ratio_missing', 'loan_amount_std', 'loan_to_value_ratio_std', 'no_intro_rate_period_std',
           'intro_rate_period_std', 'property_value_std', 'income_std', 'debt_to_income_ratio_std'
* **Column(s) used as target(s) in the final model**: 'high_priced'
* **Type of model**: Decision Tree
* **Software used to implement the model**: Python, scikit-learn
* **Version of the modeling software**: 1.0.2
* **Hyperparameters or other settings of your model**:

```
DecisionTreeClassifier(ntrees=1,
                        sample_rate=1,  # use all rows in that tree
                        mtries=-2,  # use all columns in that tree's split search
                        max_depth=4,  # shallow trees are easier to understand
                        seed=seed_,  # set random seed for reproducibility
                        nfolds=3,  # cross-validation for stability and ...
                        # only way to get metrics for 1 tree in h2o
                        model_id=model_id
                       )
```

### Quantitative Analysis

* Models were assessed primarily with AUC and AIR. See details below:

| Train AUC | Validation AUC 
| --------- | -------------- 
| 0.970011  | 0.623467

| Group              | Validation AIR |
|--------------------|----------------|
| Black vs. White    |         0.816 |
| Hispanic vs. White |         0.8765 |
| Asian vs. White    |          1.208 |
| Female vs. Male    |          0.948 |


#### Correlation Heatmap

![Correlation Heatmap](correletion_map.png)

### Explainable Boosting Machine

```
# dictionary of hyperparameter value lists for grid search
gs_params = {'max_bins': [128, 256, 512],
             'max_interaction_bins': [16, 32, 64],
             'interactions': [5, 10, 15],
             'outer_bags': [4, 8, 12],
             'inner_bags': [0, 4],
             'learning_rate': [0.001, 0.01, 0.05],
             'validation_size': [0.1, 0.25, 0.5],
             'min_samples_leaf': [1, 2, 5, 10],
             'max_leaves': [1, 3, 5]}
```
> gs_params dictionary contains different hyperparameters for the EBM model as keys and lists of values for those hyperparameters as values. These parameters include:
max_bins: The maximum number of bins to use for numeric features.
max_interaction_bins: The maximum number of bins to use for interaction features.
interactions: The number of interaction terms to use in the model.
outer_bags: The number of outer bagging iterations to use.
inner_bags: The number of inner bagging iterations to use.
learning_rate: The learning rate for the boosting process.
validation_size The proportion of the dataset to include in the validation split.
min_samples_leaf: The minimum number of samples required at a leaf node.
max_leaves: The maximum number of leaves in any tree.

### Grid Search AIR vs. AUC for EBMs

Below, we have a scatter plot of the Adverse Impact Ratio (AIR) against the Area Under the Receiver Operating Characteristic Curve (AUC) for the Explainable Boosting Machine (EBM) model. This plot enables us to examine the trade-off between fairness (as measured by AIR) and model performance (as measured by AUC).
The red vertical line drawn at AIR = 0.8 represents a fairness threshold. Any model falling to the right of this line would be fair according to the Four-Fifths Rule, commonly used in employment practices to measure disparate impact.

![Grid Search AIR vs. AUC for EBMs](grid.png)
### Visualize simulated data

Here we have histograms for each feature in the random_frame DataFrame, where each histogram displays the distribution of values for one feature.
<img src="https://github.com/marlungu/gwu_rml/blob/main/assignment_6/data/vis1.png">
<img src="https://github.com/marlungu/gwu_rml/blob/main/assignment_6/data/vis2.png">

### Sensitivity Analysis: Stress Testing
#### Simulate recession conditions in validation da

<img src="https://github.com/marlungu/gwu_rml/blob/main/assignment_6/data/stress1.png">
<img src="https://github.com/marlungu/gwu_rml/blob/main/assignment_6/data/stress2.png">
<img src="https://github.com/marlungu/gwu_rml/blob/main/assignment_6/data/stress3.png">


### Bias (Investigate the model for Discrimination)

|  | cut  | f1       |  acc     | air     |
|--|-----:|---------:|---------:|--------:|
|22| 0.22 | 0.356794 | 0.832942 | 0.816260|
|23| 0.23 | 0.350179 | 0.841156 | 0.843019|
|24| 0.24 | 0.341709 | 0.850161 | 0.864101|
|25| 0.25 | 0.330154 | 0.858313 | 0.878680|
|26| 0.26 | 0.316466 | 0.865155 | 0.887407|

##### Cutoffs in the 0.22-0.26 range provide increased accuracy and less bias toward others


### Residual Analysis Plot

> Below, we can see that residuals are very unbalanced. This model struggles to predict when customers will receive a high-priced loan correctly. It does much better when predicting customers will NOT receive a high-priced loan. There are also some very noticeable outliers.

<img src="https://github.com/marlungu/gwu_rml/blob/main/assignment_6/data/residuals.png">

### Plot tree depth vs. training and validation AUC

Decision Tree Classifier for various depths (1 to 20) computes the Area Under the ROC Curve (AUC) for the training and validation sets.

<img src="https://github.com/marlungu/gwu_rml/blob/main/assignment_6/data/tr_val.png">


### View results as a table 
<!-- , using pandas iloc to remove first column of table -->

|      | Training AUC  | Validation AUC |
|------|:-------------:|---------------:|
| 0    | 0.697141      | 0.692440       |
| 1    | 0.747007      | 0.742198       |
| 3    | 0.787190      | 0.786166       |
| 4    | 0.804219      | 0.801483       |
| 5    | 0.812611      | 0.806100       |
| 6    | 0.820184      | 0.807270       |
| 7    | 0.828544      | 0.806489       |
| 8    | 0.836905      | 0.802915       |
| 9    | 0.846124      | 0.794189       |
| 10   | 0.856340      | 0.783777       |
| 11   | 0.868401      | 0.769902       |
| 12   | 0.882286      | 0.752769       |
| 13   | 0.895562      | 0.737576       |
| 14   | 0.909777      | 0.718061       |
| 15   | 0.924238      | 0.702308       |
| 16   | 0.937470      | 0.683528       |
| 17   | 0.949437      | 0.663831       |
| 18   | 0.960672      | 0.642775       |
| 19   | 0.970011      | 0.623467       |

<!-- <img src="https://github.com/marlungu/gwu_rml/blob/main/assignment_6/data/results.png"> -->

> Training and validation AUC are relatively close at lower depths (closer to 0 index), indicating the model is neither overfitting nor underfitting. We achieve the maximum AUC for the validation set at a tree depth of 6, with an AUC score of approximately 0.807270. After this point, while the training AUC increases, the validation AUC decreases as the tree depth increases. This is a typical sign of overfitting, as the model is becoming too complex and is starting to fit the noise in the training data.


### Create a DataFrame for visualization & Sort by importance

* This extracts the feature importance scores from the model. Feature importance scores indicate how useful or valuable each feature was in constructing the boosted decision trees within the model. The higher the score, the more important the feature.

<img src="https://github.com/marlungu/gwu_rml/blob/main/assignment_6/data/feut_imp.png">

From the resulting plot, you can observe which features impact the model's predictions most.
