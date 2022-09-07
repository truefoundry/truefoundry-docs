# <img height="25px" src="https://raw.githubusercontent.com/FortAwesome/Font-Awesome/6.x/svgs/solid/bolt.svg" width="50" height="50"> Service And Usecases

A service is a continuously running process which usually has some APIs to interact with it. Since a service is continuosly running, we incur the cost of the service based on the resource usage. Services can be scaled up or down depending on the incoming traffic or resource usage. Good examples of some services can be:

1. Realtime Model Inference Hosting (Flask, FastAPI)
2. Model Inference on incoming stream like Kafka messages. 
3. A backend to power dynamic websites.
4. Model demos like Streamlit, Gradio.

### Key things to note while building a service

A service will usually expose certain APIs at some port. This port can be mapped to a domain which will allow others to call this service. There can be multiple ports exposed by a service and each of them can be mapped to a different domain.

Its usually a good idea to have the API documentation while building a service since it makes it quite easy for the consumers
of your service to understand API inputs and outputs. 
If you are writing your service in FastAPI, the swagger UI is automatically rendered in the /docs link. 

### Deploying a model using Flask

Let us see an example of deploying a model using [Flask](https://flask.palletsprojects.com/). We will use Flask to create a web API and deploy the [T5 Small](https://huggingface.co/t5-small) model.

**File Structure:**

```
.
├── inference.py
├── main.py
└── requirements.txt
```

**`inference.py`**
```python
from transformers import AutoTokenizer, AutoModelForSeq2SeqLM

tokenizer = AutoTokenizer.from_pretrained("t5-small")
model = AutoModelForSeq2SeqLM.from_pretrained("t5-small")


def t5_model_inference(input_text: str) -> str:
    input_ids = tokenizer(input_text, return_tensors="pt").input_ids
    outputs = model.generate(input_ids)
    return tokenizer.decode(outputs[0], skip_special_tokens=True)
```

**`main.py`**
```python
from flask import Flask, request, abort, jsonify
from inference import t5_model_inference

app = Flask(__name__)


@app.get("/infer")
def infer():
    input_text = request.args.get("input_text")
    if input_text is None:
        abort(400, "'input_text' is a required query param.")
    output_text = t5_model_inference(input_text=input_text)
    return jsonify({"output": output_text})
```

**`requirements.txt`**
```
flask==2.2.2
gunicorn==20.1.0
transformers==4.17.0
torch==1.12.1
```

To run this on your local machine,

1. Install the requirements first,
```shell
python3 -m venv ~/.venv
source ~/.venv/bin/activate
pip install -r requirements.txt
```
2. Run using gunicorn.
```shell
gunicorn -b 0.0.0.0:8000 main:app
```


In the next section, we will learn how to deploy this service via Truefoundry. 