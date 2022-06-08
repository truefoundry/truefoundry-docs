# HuggingFace Trainer Integration



## Introduction

🪄 MLFoundry can automatically log metrics and model from HuggingFace Transformers 🤗 Trainer
**Just attach the callback and let it do the rest!**

```python
from transformers import TrainingArguments, Trainer
from mlfoundry.integrations.transformers import MlFoundryTrainerCallback

mlf_cb = MlFoundryTrainerCallback(
    project_name="huggingface",
    run_name="my-hf-run",
    flatten_params=True,
    log_model=True,
)

args = TrainingArguments(..., report_to=[])
trainer = Trainer(..., args=args, callbacks=[mlf_cb])
trainer.train()
```

> Make sure to login or set the API Key in the environment. See [Setting up MLFoundry](guides/experiment_tracking/setup.md)

**You can also create the callback from a pre initialised run**

```python
from transformers import TrainingArguments, Trainer
import mlfoundry as mlf
from mlfoundry.integrations.transformers import MlFoundryTrainerCallback

client = mlf.get_client(api_key="...")
run = client.create_run(project_name="huggingface", run_name="my-hf-run")

mlf_cb = MlFoundryTrainerCallback.from_run(
    run=run,
    auto_end_run=False,
    flatten_params=True,
    log_model=True,
)

args = TrainingArguments(..., report_to=[])
trainer = Trainer(..., args=args, callbacks=[mlf_cb])
trainer.train()

run.end()
```

> If you are using Tensorflow, just swap the PyTorch `Trainer` for the TensorFlow `TFTrainer`.



## Example Notebook:

[Training a Text Classifier with HuggingFace Transformers Trainer](https://github.com/truefoundry/mlfoundry-examples/blob/main/examples/huggingface_transformers/tweet_eval_emotion_text_classification.ipynb)



## API Reference

### MlFoundryTrainerCallback

```python
def __init__(
	project_name: str,
	run_name: Optional[str] = None,
	flatten_params: bool = False,
	log_model: bool = False,
	**kwargs
)
```

Huggingface Transformers Trainer Callback for tracking training run on MLFoundry

**Args:**

---

- **project_name** (str): name of the project to create the run under
- **run_name** (Optional[str], *optional*): name of the run. When not provided a run name is automatically generated
- **flatten_params** (bool, *optional*): if to flatten the args and model config dictionaries before logging them, By default, this is `False` E.g. When set to True,`config = {"id2label": {"0": "hello", "1": "bye"}}` will be logged as two parameters as `{"config/id2label.0": "hello", "config/id2label.1": "bye"}`. When set to False, `config = {"id2label": {"0": "hello"}}` will be logged as a single parameter as `{"config/id2label": '{"0": "hello", "1": "bye"}'}`
- **log_model** (bool, *optional*): if to log the generated model artifacts at the end of training. By default, this is `False`. Note this logs the model at the end of training. This model will be the best model according to configured metrics if `load_best_model_at_end` is set to `True` in [`transformers.training_args.TrainingArguments`](https://huggingface.co/docs/transformers/main/en/main_classes/trainer#transformers.TrainingArguments) passed to the [`Trainer`](https://huggingface.co/docs/transformers/main/en/main_classes/trainer#transformers.Trainer) instance, otherwise this model will be the latest model obtained after running all epochs (or steps). This model will be logged at path `mlf/huggingface_models/model/` under the run artifacts

#### Methods

---

```python
@classmethod
def from_run(
	cls,
	run: mlfoundry.mlfoundry_run.MlFoundryRun,
	auto_end_run: bool = False,
	flatten_params: bool = False,
	log_model: bool = False,
	**kwargs
) -> mlfoundry.integrations.transformers.MlFoundryTrainerCallback:
```

Create a MLFoundry Huggingface Transformers Trainer callback from an existing MLFoundry run instance

**Args:**

---

- **run** (MlFoundryRun): `MlFoundry` run instance
- **auto_end_run** (bool, *optional*): if to end the run when training finishes. By default, this is `False`
- **flatten_params** (bool, *optional*): if to flatten the args and model config dictionaries before logging them, By default, this is `False` E.g. When set to True,`config = {"id2label": {"0": "hello", "1": "bye"}}` will be logged as two parameters as `{"config/id2label.0": "hello", "config/id2label.1": "bye"}`. When set to False, `config = {"id2label": {"0": "hello"}}` will be logged as a single parameter as `{"config/id2label": '{"0": "hello", "1": "bye"}'}`
- **log_model** (bool, *optional*): if to log the generated model artifacts at the end of training. By default, this is `False`. Note this logs the model at the end of training. This model will be the best model according to configured metrics if `load_best_model_at_end` is set to `True` in [`transformers.training_args.TrainingArguments`](https://huggingface.co/docs/transformers/main/en/main_classes/trainer#transformers.TrainingArguments) passed to the [`Trainer`](https://huggingface.co/docs/transformers/main/en/main_classes/trainer#transformers.Trainer) instance, otherwise this model will be the latest model obtained after running all epochs (or steps). This model will be logged at path `mlf/huggingface_models/model/` under the run artifacts

**Returns:**

---

**MlFoundryTrainerCallback:** an instance of `MlFoundryTrainerCallback`
