**Assigment 2**


## Installation<a name="Install"></a>  

```python
pip install PiML  
```

### Stage 1:  Initialize an experiment

```python
from piml import Experiment
exp = Experiment()
```

```from google.colab import drive
drive.mount('/content/drive')
```

### Import pandas and and load HMDA dataset from the drive

```
import pandas as pd

data = pd.read_csv('/content/drive/MyDrive/ResponsibleMachineLearning/data/hmda_train_preprocessed.csv')
exp.data_loader(data=data)
test = pd.read_csv('/content/drive/MyDrive/ResponsibleMachineLearning/data/hmda_test_preprocessed.csv')

```

<img src="https://github.com/marlungu/gwu_rml/blob/main/assigment_2/data/data.png">

```
exp.data_summary()
```
<img src="https://github.com/marlungu/gwu_rml/blob/main/assigment_2/data/data_summary.png">


### Remove Unnecessary Features

```
exp.data_summary(feature_exclude = [
    "row_id", "black","asian","white","amind","hipac",
    "hispanic","non_hispanic","male", "female", "agegte62", 
    "agelt62"
    ]
  )
```
<img src="https://github.com/marlungu/gwu_rml/blob/main/assigment_2/data/data_with_needed_fut.png">


### Data Preparation

```
exp.data_prepare()

```
<img src="https://github.com/marlungu/gwu_rml/blob/main/assigment_2/data/data_pre.png">

### Exploratory analysis

```
exp.eda()
```
<img src="https://github.com/marlungu/gwu_rml/blob/main/assigment_2/data/exp_eda.png">

```
exp.feature_select()
```
<img src="https://github.com/marlungu/gwu_rml/blob/main/assigment_2/data/data_select.png">


```
exp.feature_select(method="cor", corr_algorithm="spearman", threshold=0.1, figsize=(5, 4))
```
<img src="https://github.com/marlungu/gwu_rml/blob/main/assigment_2/data/spearman.png">

### Train intepretable models

```
exp.model_train()
```
<img src="https://github.com/marlungu/gwu_rml/blob/main/assigment_2/data/trained_model.png">
<img src="https://github.com/marlungu/gwu_rml/blob/main/assigment_2/data/leaderboard.png">
<img src="https://github.com/marlungu/gwu_rml/blob/main/assigment_2/data/regis_model.png">

### Post-hoc Explaination: Global and Local Methods

```
exp.model_explain()
```
<img src="https://github.com/marlungu/gwu_rml/blob/main/assigment_2/data/glm_local.png">
<img src="https://github.com/marlungu/gwu_rml/blob/main/assigment_2/data/xgb_local.png">
<img src="https://github.com/marlungu/gwu_rml/blob/main/assigment_2/data/ebm_local.png">
<img src="https://github.com/marlungu/gwu_rml/blob/main/assigment_2/data/ebm2_local.png">
<img src="https://github.com/marlungu/gwu_rml/blob/main/assigment_2/data/ebm3_local.png">
