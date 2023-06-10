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


