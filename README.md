# Vectice-Examples
A set of examples for using Vectice.com software. The notebooks are seperated into two categories, being either Vanilla or MLflow. Vanilla notebooks use the Vectice App and SDK without any third party integrations. Whereas, MLflow uses the MLflow integration offered by Vectice. In the future, more integration examples will be added.

The Vectice SDK documentation can be found [here](https://doc.vectice.com/)

- [Getting Started](#getting-started)
- [Integrations](#integrations) 
  - [MLflow](#mlflow)

## Getting Started

```
pip install vectice
```
The following code is just an example to test that the Vectice SDK is working as it should be. You can use an IDE or a notebook to execute this code. It's intializing a vectice object that connects to vectice. If everything is working as it should be you'll recieve no errors. 
```python3
from vectice import Vectice
vectice = Vectice(project_token="PROJECT_TOKEN")
```
The Vectice SDK leverages runs as the terminology used when capturing metadata from the work you do. Thus, if you want to clean data, for example, and capture what you've done, you would create the inputs of the data that will be cleaned, create a run and then start it. Then you'd perform the data cleaning. 

```python3
ds_version = [vectice.create_dataset_version().with_parent_name("DATASET_NAME_IN_VECTICE_APP")]
run = vectice.create_run("RUN_NAME")
vectice.start_run(run, inputs = ds_version)
```
Once you've performed the data cleaning or any other actions you end the run by simple creating outputs and then calling the end_run method.
```python3
outputs = [vectice.create_dataset_version().with_parent_name("DATASET_NAME_IN_VECTICE_APP")]
vectice.end_run(outputs)
```
## Integrations

### MLflow

The Vectice API has MLflow integration and the possibility to either capture metadata after a run or in a fully integrated manner. This can be achieved by using the Vectice API at a high level. 

```python3
inputs = [Vectice.create_dataset_version().with_parent_name("standalone").with_tag("a_tag", "a tag value")]
# MLflow run
Vectice.save_after_run(PROJECT_TOKEN, run, "MLflow", inputs)
```

The fully integrated use of MLflow with Vectice uses the Python context manager to easily leverage MLflow with the Vectice API. The MLflow metadata is leveraged by the Vectice API and autolog allows all the metadata to be captured. Furthermore, more parameters and metrics can be captured by using MLflow methods. 

```python3
mlflow.autolog()
vectice = Vectice(project_token=PROJECT_TOKEN, lib="MLflow")
vectice.create_run(MLFLOW_EXPERIMENT_NAME)

with vectice.start_run(inputs=inputs):
    mlflow.log_param("algorithm", "linear regression")
    mlflow.log_metric("MAE", MAE)
```
