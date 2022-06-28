# HuggingFace Trainer Integration

## Introduction

🪄 MLFoundry can automatically log metrics and model from HuggingFace Transformers 🤗 Trainer
**Just attach the callback and let it do the rest!**

```python
from transformers import TrainingArguments, Trainer
import mlfoundry as mlf
from mlfoundry.integrations.transformers import MlFoundryTrainerCallback

mlf.login()
# or set API key via `MLF_API_KEY` env variable

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
from mlfoundry.integrations.transformers import MlFoundryTrainerCallback, LogModelStrategy

mlf.login()
# or set API key via `MLF_API_KEY` env variable

client = mlf.get_client()
run = client.create_run(project_name="huggingface", run_name="my-hf-run",)

mlf_cb = MlFoundryTrainerCallback.from_run(
    run=run,
    auto_end_run=False,
    flatten_params=True,
    log_model_strategy=LogModelStrategy.BEST_PLUS_LATEST,
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

### `class` MlFoundryTrainerCallback

```python
def __init__(
	project_name: str,
	run_name: Optional[str] = None,
	flatten_params: bool = False,
	log_model_strategy: Union[str, LogModelStrategy] = LogModelStrategy.NO,
	**kwargs
)
```

Huggingface Transformers Trainer Callback for tracking training run on MLFoundry

#### Args:

- **project_name** (str): name of the project to create the run under
  - **run_name** (Optional[str], *optional*): name of the run. When not provided a run name is automatically generated
  - **flatten_params** (bool, *optional*): if to flatten the args and model config dictionaries before logging them, By default, this is `False` E.g. When set to True,`config = {"id2label": {"0": "hello", "1": "bye"}}` will be logged as two parameters as `{"config/id2label.0": "hello", "config/id2label.1": "bye"}`. When set to False, `config = {"id2label": {"0": "hello"}}` will be logged as a single parameter as `{"config/id2label": '{"0": "hello", "1": "bye"}'}`
  - **log_model_strategy** (`mlfoundry.integrations.transformers.LogModelStrategy`, *optional*): The strategy to use for logging models
    - `LogModelStrategy.NO` (default): Do not log any models
    - `LogModelStrategy.BEST_ONLY`: Log only the best model checkpoint. Note that `args.metric_for_best_model` and `args.args.save_strategy` needs to be set correctly for this to work
    - `LogModelStrategy.BEST_PLUS_LATEST`: Log both the latest checkpoint and the best checkpoint (if available) and different from the latest checkpoint
    - `LogModelStrategy.CHECKPOINTS_ON_TRAIN_END`: Log all available checkpoints in `args.output_dir` at the end of training.

    In addition to checkpoints a `metadata.json` file is logged which stores references to the best checkpoint and the latest checkpoint. The files will be logged under `mlf/huggingface_models/model/` under the run artifacts.

---

#### `method` from_run
```python
@classmethod
def from_run(
	cls,
	run: mlfoundry.mlfoundry_run.MlFoundryRun,
	auto_end_run: bool = False,
	flatten_params: bool = False,
	log_model_strategy: Union[str, LogModelStrategy] = LogModelStrategy.NO,
	**kwargs
) -> mlfoundry.integrations.transformers.MlFoundryTrainerCallback:
```

Create a MLFoundry Huggingface Transformers Trainer callback from an existing MLFoundry run instance

#### Args:

- **run** (MlFoundryRun): `MlFoundry` run instance
  - **auto_end_run** (bool, *optional*): if to end the run when training finishes. By default, this is `False`
  - **flatten_params** (bool, *optional*): if to flatten the args and model config dictionaries before logging them, By default, this is `False` E.g. When set to True,`config = {"id2label": {"0": "hello", "1": "bye"}}` will be logged as two parameters as `{"config/id2label.0": "hello", "config/id2label.1": "bye"}`. When set to False, `config = {"id2label": {"0": "hello"}}` will be logged as a single parameter as `{"config/id2label": '{"0": "hello", "1": "bye"}'}`
  - **log_model_strategy** (`mlfoundry.integrations.transformers.LogModelStrategy`, *optional*): The strategy to use for logging models
    - `LogModelStrategy.NO` (default): Do not log any models
    - `LogModelStrategy.BEST_ONLY`: Log only the best model checkpoint. Note that `args.metric_for_best_model` and `args.args.save_strategy` needs to be set correctly for this to work
    - `LogModelStrategy.BEST_PLUS_LATEST`: Log both the latest checkpoint and the best checkpoint (if available) and different from the latest checkpoint
    - `LogModelStrategy.CHECKPOINTS_ON_TRAIN_END`: Log all available checkpoints in `args.output_dir` at the end of training.

    In addition to checkpoints a `metadata.json` file is logged which stores references to the best checkpoint and the latest checkpoint. The files will be logged under `mlf/huggingface_models/model/` under the run artifacts.

#### Returns:

**MlFoundryTrainerCallback:** an instance of `MlFoundryTrainerCallback`

