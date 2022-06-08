# Enums

MLFoundry has some set of enums for FileFormat, ModelFramework, DataSlice, ModelType. All enums mentioned below can be acceessed under mlfloundry.

#### Example

```python
import mlfoundry as mlf

format = mlf.FileFormat.CSV # fileformat
framework = mlf.ModelFramework.SKLEARN # Framework
slice = mlf.DataSlice.TRAI # dataslice
type = mlf.ModelType.BINARY_CLASSIFICATION
```

### FileFormat

| Key                                          | Value     |
| -------------------------------------------- | --------- |
| <mark style="color:blue;">**CSV**</mark>     | 'csv'     |
| <mark style="color:blue;">**PARQUET**</mark> | 'parquet' |

### ModelFramework

| Key                                             | Value        |
| ----------------------------------------------- | ------------ |
| <mark style="color:blue;">**SKLEARN**</mark>    | 'sklearn'    |
| <mark style="color:blue;">**TENSORFLOW**</mark> | 'tensorflow' |
| <mark style="color:blue;">**PYTORCH**</mark>    | 'pytorch'    |
| <mark style="color:blue;">**KERAS**</mark>      | 'keras'      |
| <mark style="color:blue;">**XGBOOST**</mark>    | 'xgboost'    |
| <mark style="color:blue;">**LIGHTGBM**</mark> | 'lightgbm' |
| <mark style="color:blue;">**FASTAI**</mark>    | 'fastai'    |
| <mark style="color:blue;">**H2O**</mark>      | 'h2o'      |
| <mark style="color:blue;">**ONNX**</mark>      | 'onnx'      |
| <mark style="color:blue;">**SPACY**</mark>    | 'spacy'    |
| <mark style="color:blue;">**STATSMODELS**</mark> | 'statsmodels' |
| <mark style="color:blue;">**GLUON**</mark>    | 'gluon'    |
| <mark style="color:blue;">**PADDLE**</mark>      | 'paddle'      |


### DataSlice

|                                                 |              |
| ----------------------------------------------- | ------------ |
| <mark style="color:blue;">**TRAIN**</mark>      | 'train'      |
| <mark style="color:blue;">**VALIDATE**</mark>   | 'validate'   |
| <mark style="color:blue;">**TEST**</mark>       | 'test'       |
| <mark style="color:blue;">**PREDICTION**</mark> | 'prediction' |

### ModelType

| Key                                                             | Value                        |
| --------------------------------------------------------------- | ---------------------------- |
| <mark style="color:blue;">**BINARY\_CLASSIFICATION**</mark>     | 'binary\_classification'     |
| <mark style="color:blue;">**MULTICLASS\_CLASSIFICATION**</mark> | 'multiclass\_classification' |
| <mark style="color:blue;">**REGRESSION**</mark>                 | 'regression'
| <mark style="color:blue;">**TIMESERIES**</mark>                 | 'timeseries'            |

### FieldType

| Key                                                             | Value                        |
| --------------------------------------------------------------- | ---------------------------- |
| <mark style="color:blue;">**NUMERICAL**</mark>     | 'numerical'     |
| <mark style="color:blue;">**CATEGORICAL**</mark> | 'categorical' |
