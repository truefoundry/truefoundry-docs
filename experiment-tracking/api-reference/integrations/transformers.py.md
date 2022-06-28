<!-- markdownlint-disable -->

<a href="../../../../mlfoundry/integrations/transformers.py#L0"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

# <kbd>module</kbd> `transformers.py`




**Global Variables**
---------------
- **HF_ARTIFACTS_PATH**
- **HF_MODELS_PATH**
- **HF_MODEL_PATH**


---

## <kbd>class</kbd> `LogModelStrategy`
An enumeration. 





---

## <kbd>class</kbd> `MlFoundryTrainerCallback`
Huggingface Transformers Trainer Callback for tracking training run on MLFoundry 



**Examples:**
 

```
     import mlfoundry as mlf
     mlf.login()
     # or set API key via `MLF_API_KEY` env variable

     from transformers import TrainingArguments, Trainer
     from mlfoundry.integrations.transformers import MlFoundryTrainerCallback, LogModelStrategy

     mlf_cb = MlFoundryTrainerCallback(
         project_name="huggingface",
         run_name="my-hf-run",
         flatten_params=True,
         log_model_strategy=LogModelStrategy.BEST_PLUS_LATEST,
     )

     args = TrainingArguments(..., report_to=[])
     trainer = Trainer(..., args=args, callbacks=[mlf_cb])
     trainer.train()
    ``` 

 Callback can also be created from an existing run 

```
     import mlfoundry as mlf
     mlf.login()
     # or set API key via `MLF_API_KEY` env variable

     from transformers import TrainingArguments, Trainer
     import mlfoundry as mlf
     from mlfoundry.integrations.transformers import MlFoundryTrainerCallback, LogModelStrategy

     client = mlf.get_client()
     run = client.create_run(project_name="huggingface", run_name="my-hf-run")

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

<a href="../../../../mlfoundry/integrations/transformers.py#L98"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

### <kbd>function</kbd> `__init__`

```python
__init__(
    project_name: str,
    run_name: Optional[str] = None,
    flatten_params: bool = False,
    log_model_strategy: Union[str, LogModelStrategy] = <LogModelStrategy.NO: 'no'>,
    **kwargs
)
```



**Args:**
 
 - <b>`project_name`</b> (str):  name of the project to create the run under 
 - <b>`run_name`</b> (Optional[str], optional):  name of the run. When not provided a run name is automatically generated 
 - <b>`flatten_params`</b> (bool, optional):  if to flatten the args and model config dictionaries before logging them,  By default, this is `False` 

 For e.g. 

 when set to True, 
 - <b>``config = {"id2label"`</b>:  {"0": "hello", "1": "bye"}}` will be logged as two parameters as 
 - <b>``{"config/id2label.0"`</b>:  "hello", "config/id2label.1": "bye"}` 

when set to False, 
 - <b>``config = {"id2label"`</b>:  {"0": "hello"}}` will be logged as a single parameter as 
 - <b>``{"config/id2label"`</b>:  '{"0": "hello", "1": "bye"}'}` 


 - <b>`log_model_strategy`</b> (LogModelStrategy, optional):  The strategy to use for logging models 
        - LogModelStrategy.NO (default): Do not log any models 
        - LogModelStrategy.BEST_ONLY: Log only the best model checkpoint.  Note that `args.metric_for_best_model` and `args.args.save_strategy` needs to be set  correctly for this to work 
        - LogModelStrategy.BEST_PLUS_LATEST: Log both the latest checkpoint and the best checkpoint (if  available) and different from the latest checkpoint 
        - LogModelStrategy.CHECKPOINTS_ON_TRAIN_END: Log all available checkpoints in `args.output_dir` at the  end of training. 

 In addition to checkpoints a metadata.json file is logged which stores references to the best checkpoint  and the latest checkpoint 

 The files will be logged under `mlf/huggingface_models/model/` under the run artifacts. 
 - <b>`log_model`</b> (bool, optional):  **DEPRECATED**, use `log_model_strategy` instead.  If to log the generated model artifacts at the end of training. By default, this is `False`.  If set to True this will set `log_model_strategy` to `LogModelStrategy.BEST_PLUS_LATEST`. Please  see the description for `log_model_strategy` 




---

<a href="../../../../mlfoundry/integrations/transformers.py#L169"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

### <kbd>classmethod</kbd> `from_run`

```python
from_run(
    run: MlFoundryRun,
    auto_end_run: bool = False,
    flatten_params: bool = False,
    log_model_strategy: Union[str, LogModelStrategy] = <LogModelStrategy.NO: 'no'>,
    **kwargs
) → MlFoundryTrainerCallback
```

Create a MLFoundry Huggingface Transformers Trainer callback from an existing MLFoundry run instance 



**Args:**
 
 - <b>`run`</b> (MlFoundryRun):  `MlFoundry` run instance 
 - <b>`auto_end_run`</b> (bool, optional):  if to end the run when training finishes. By default, this is `False` 
 - <b>`flatten_params`</b> (bool, optional):  if to flatten the args and model config dictionaries before logging them,  By default, this is `False` 

 For e.g. 

 when set to True, 
 - <b>``config = {"id2label"`</b>:  {"0": "hello", "1": "bye"}}` will be logged as two parameters as 
 - <b>``{"config/id2label.0"`</b>:  "hello", "config/id2label.1": "bye"}` 

when set to False, 
 - <b>``config = {"id2label"`</b>:  {"0": "hello"}}` will be logged as a single parameter as 
 - <b>``{"config/id2label"`</b>:  '{"0": "hello", "1": "bye"}'}` 


 - <b>`log_model_strategy`</b> (LogModelStrategy, optional):  The strategy to use for logging models 
        - LogModelStrategy.NO (default): Do not log any models 
        - LogModelStrategy.BEST_ONLY: Log only the best model checkpoint.  Note that `args.metric_for_best_model` and `args.args.save_strategy` needs to be set  correctly for this to work 
        - LogModelStrategy.BEST_PLUS_LATEST: Log both the latest checkpoint and the best checkpoint (if  available) and different from the latest checkpoint 
        - LogModelStrategy.CHECKPOINTS_ON_TRAIN_END: Log all available checkpoints in `args.output_dir` at the  end of training. 

 In addition to checkpoints a metadata.json file is logged which stores references to the best checkpoint  and the latest checkpoint 

 The files will be logged under `mlf/huggingface_models/model/` under the run artifacts. 
 - <b>`log_model`</b> (bool, optional):  **DEPRECATED**, use `log_model_strategy` instead.  If to log the generated model artifacts at the end of training. By default, this is `False`.  If set to True this will set `log_model_strategy` to `LogModelStrategy.BEST_PLUS_LATEST`. Please  see the description for `log_model_strategy` 
 - <b>`**kwargs`</b>:  Additional keyword arguments to pass to init 



**Returns:**
 
 - <b>`MlFoundryTrainerCallback`</b>:  an instance of `MlFoundryTrainerCallback` 

---

<a href="../../../../mlfoundry/integrations/transformers.py#L231"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

### <kbd>function</kbd> `on_init_end`

```python
on_init_end(args, state, control, **kwargs)
```

Event called at the end of the initialization of the [`Trainer`]. 

---

<a href="../../../../mlfoundry/integrations/transformers.py#L495"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

### <kbd>function</kbd> `on_log`

```python
on_log(args, state, control, model=None, logs=None, **kwargs)
```

Event called after logging the last logs. 

---

<a href="../../../../mlfoundry/integrations/transformers.py#L515"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

### <kbd>function</kbd> `on_save`

```python
on_save(args, state, control, **kwargs)
```





---

<a href="../../../../mlfoundry/integrations/transformers.py#L314"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

### <kbd>function</kbd> `on_train_begin`

```python
on_train_begin(args, state, control, model=None, **kwargs)
```

Event called at the beginning of training. 

---

<a href="../../../../mlfoundry/integrations/transformers.py#L480"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

### <kbd>function</kbd> `on_train_end`

```python
on_train_end(args, state, control, **kwargs)
```

Event called at the end of training. 

---

<a href="../../../../mlfoundry/integrations/transformers.py#L267"><img align="right" style="float:right;" src="https://img.shields.io/badge/-source-cccccc?style=flat-square"></a>

### <kbd>function</kbd> `setup`

```python
setup(args, state, model, **kwargs)
```






