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
If you are writing your service in FastAPI, the swagger UI is automatically rendered in the `/docs` link. 

The code below shows a sample service code in Flask

// TODO: Write a simple model serving code in Flask
// Explain a bit of the code

In the next section, we will learn how to deploy this service via Truefoundry. 