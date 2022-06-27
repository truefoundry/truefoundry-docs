# sfn_load_predict

`sfn_load_predict` is jupyter cell magic which loads
python file with defines a predict function.
To access the predictor use `get_predict`API.

### Example:
Import module.

```python
import servicefoundry.core as sfy
```

Load predict definition
```python
%%sfy_load_predict
import logging

def predict(a: float, b: float, c: float, d: float):
    res = a + b + c + d
    print(f"Predict called with {a}, {b}, {c}, {d}, output={res}")
    return res
```

Get the predictor object like below:
```python
predictor = sfy.get_predictor()
```

Use the predictor object to test your predict function.
```python
predictor.predict(a=1.1, b=2.4, c=3.5, d=7.8)
```
