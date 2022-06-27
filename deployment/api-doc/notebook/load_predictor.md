# load_predictor

`load_predictor` loads python file with defines a predict function. This API returns a predictor object after syntax verification.

### Arguments
| Parameter    | Type | Default | Description                                   |
|--------------|------|---------|-----------------------------------------------|
| predict_file | str  | NA      | File location of file with predict function   |

### Returns
[Predictor](Predictor.md)

### Example:
predict.py
```python
import logging
logger = logging.getLogger(__name__)


def predict(a: float, b: float, c: float, d: float):
    logger.info(f"Loaded sklearn model")
    return a + b + c + d
```

Load the predictor from predict.py

```python
import servicefoundry.core as sfy

predictor = sfy.load_predictor("predict.py")
```

Use the predictor object to test your predict function.
```python
predictor.predict(a=1.1, b=2.4, c=3.5, d=7.8)
```
