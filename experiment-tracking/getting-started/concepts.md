# Concepts

The entities defined in MLFoundry can be understood from the diagram below.

![Mlfoundry Entities](../../assets/mlf-concept.png)

* **Run** : A run represents a single experiment which in the context of Machine Learning is one specific model (say Logistic Regression), with a fixed set of hyper-parameters. Models, artifacts, metrics, and parameters (details below) are all logged under a specific run.
* **Project**:  Multiple runs are organized under a single project which represents a high-level business problem. Access controls and collaboration with teammates happen on a project level.
* **Artifacts**: An artifact can be any file or directory.
* **Parameters**: Change this to -> Parameters or HyperParameters that define your experiment and Machine Learning model. For example, `learning_rate`. 
* **Metric**: Metrics are values that help you to evaluate and compare different runs.
* **Dataset**: The piece of data on which machine learning models are trained.
