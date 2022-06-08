# log\_model

Logs a model, given a model object along with the framework. We can only log one model per run. If attempt to log model more than once an error is thrown out.

| Parameters                                     | Description                                                                                                                                                   |
| ---------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <mark style="color:blue;">**framework**</mark> | <p>(str or mlf.ModelFrameWork) Framework of the model. example: sklearn, pytorch, tensorflow.</p><p>List of available frameworks: <a href="../enums.md">Link</a></p> |
| <mark style="color:blue;">**model**</mark>     | model object in particular                                                                                                                                    |

#### Example

```python
mlf_run.log_model(clf, "sklearn")
```

#### Tensorflow

While saving tensorflow models, we may need to also register the model inference function via `signatures` argument.
More on `signatures` can be found [here](https://www.tensorflow.org/api_docs/python/tf/saved_model/save).

1. The model already has only one concrete (input signature specified) `tf.function` decorated method.
```python
class Model(tf.keras.Model):
    def __init__(self):
        super().__init__()
        self.dense1 = tf.keras.layers.Dense(1, activation=tf.nn.tanh)

    @tf.function(input_signature=[tf.TensorSpec([None, 1], tf.float32, name="inputs")])
    def call(self, inputs):
        return self.dense1(inputs)

model = Model()
run.log_model(
    model,
    framework="tensorflow",
)
```

2. The model already has multiple concrete `tf.function` decorated method. In this case, we need to
manually specify the signature we want to use for inference.

```python
class Model(tf.keras.Model):
    def __init__(self):
        super().__init__()
        self.dense1 = tf.keras.layers.Dense(1, activation=tf.nn.tanh)

    @tf.function(input_signature=[tf.TensorSpec([None, 1], tf.float32, name="inputs")])
    def call(self, inputs):
        return self.dense1(inputs)

    @tf.function(input_signature=[tf.TensorSpec([None, 1], tf.float32, name="inputs")])
    def call_and_add_one(self, inputs):
        return self.dense1(inputs) + 1

model = Model()
run.log_model(
    model,
    signatures={"call": model.call, "call_and_add_one": model.call_and_add_one},
    tf_signature_def_key="call_add_one",
    framework="tensorflow",
)
# or we can just log one specific signature,
run.log_model(
    model,
    signatures=model.call_and_add_one,
    framework="tensorflow",
)
```

3. The model does not have a concrete `tf.function` decorated method.
```python
class Model(tf.keras.Model):
    def __init__(self):
        super().__init__()
        self.dense1 = tf.keras.layers.Dense(1, activation=tf.nn.tanh)

    def call(self, inputs):
        return self.dense1(inputs)

model = Model()
run.log_model(
    model,
    signatures=tf.function(model.call).get_concrete_function(tf.TensorSpec([None, 1], tf.float32)),
    framework="tensorflow",
)
```


The `get_model` function will return the registered inference function.

```python
model = mlf_run.get_model()
output = model(inputs=tf.ones((5, 1)))
```
