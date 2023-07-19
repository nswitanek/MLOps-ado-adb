
# Part 2:  Preparing jobs and workflows for remote triggering and tracking

## Pre-requisites
- Complete [Part 0](part_0.md), [Part 1](part_1.md)
- Run each notebook feature_engineering, ml_training and evaluating successfully via the Databricks notebook UI
- [TODO address whether this part needs to use different compute - possibly job cluster?]Have Azure ML workspace setup with a Compute Cluster named ```cpu-cluster```

## Summary 
After successfully restructuring the monolithic Databricks notebook into task-focused, modular notebooks, and running those modules via the Databricks UI, your team wants to prepare to run the notebooks automatically in response to code changes.
They also want to take advantage of experiment tracking and model management capabilities of [MLflow](https://mlflow.org/) to keep track of the experiment, model metrics, and model assets. 
Finally, the team wants to deploy the model for batch inferencing.
To accomplish these goals, you will perform the following:
- Run the feature_engineering notebook as a job in Azure Databricks 
- Run the ml_training notebook as a job in Azure Databricks and observe the experiment metrics 
- Run the evaluating notebook as a job in Azure Databricks and observe how the model can be registered to MLflow 
- Run the three jobs together as a workflow
- Deploy and test the resulting model [originally: "as an API using Azure Managed Online Endpoint" TODO: determine the deployment pattern and then replace the description as appropriate]


## Steps
1. Go to the workshop folder.
   > Action Item: Run the following code snippet.
    ```bash 
    cd src/workshop
    ```
2. Set defaults values to configure your resource group and workspace.
   > Action Item: Run the following code snippet.
    ```bash 
    az configure --defaults group=YOUR_RESOURCE_GROUP workspace=YOUR_WORKSPACE
    ```

3. Run the ```feature_engineering.py``` module under the ```data_engineering``` folder by following the steps below:
   > Action Items:
   > - Run the following code snippet:
      ```bash 
        az ml job create -f core/data_engineering/feature_engineering.yml 
      ```
   > - Go to Azure ML Studio and locate the run detail for this experiment.

4. Run the ```ml_training.py``` module under the ```training``` folder by following the steps below:
   > Action Items:
   > - Run the following code snippet:
      ```bash 
        az ml job create -f core/training/ml_training.yml 
      ```
   > - Go to Azure ML Studio and locate the run detail for this experiment.

5. Run the ```ml_evaluating.py``` module under the ```evaluating``` folder by following the steps below:
   > Action Items: 
   > - Run the following code snippet:

      ```bash 
        az ml job create -f core/evaluating/ml_evaluating.yml 
      ```
   > - Go to Azure ML Studio and locate the run detail for this experiment. Observe the ML metrics and how the model was logged to Azure ML's model registry.

6. Create a pipeline that runs the feature_engineering, training and evaluation in one workflow.
   > Action Items: Run the pipeline, by running the following code snippet.
   
      ```bash 
        az ml job create -f core/pipelines/training_pipeline.yml 
      ```
   > - Go to the run detail at Azure ML studio and observe the relationship graph among the modules. (See chart below as well.)

7. Discuss this question: Why should we run the modules both individually and together in a pipeline? 

8. Deploy to Azure ML Managed Online Endpoint by following the steps below:
   > Action Items:
   > - Update the ```endpoint.yml``` file and ```deployment.yml``` by updating the name of the endpoint (should be a unique name)
   > - Create your endpoint
      ```bash 
        az ml online-endpoint create --file core/scoring/endpoint.yml 
      ```
   > - Create a green deployment 
      ```bash 
        az ml online-deployment create --file core/scoring/deployment.yml 
      ```
   > - Test the deployed service with mock-up data from scoring_test_request.json
      ```bash 
        az ml online-endpoint invoke -n YOUR_ENDPOINT_NAME --deployment green --request-file core/scoring/scoring_test_request.json 
      ``` 
   > - Observe the returned scores from the endpoint evaluation.

### The entire training pipeline is illustrated with this diagram
![training_pipeline](images/training_pipeline.png)

## Success criteria
- Run the modules individually in Azure 
- Capture metrics and models in ml_training and ml_evaluating modules
- Run three modules together in a pipeline
- Model is deployed successfully to managed endpoint. 
- Testing is successful

## Reference materials
- [Azure ML CLI v2 tutorial](https://docs.microsoft.com/en-us/learn/paths/train-models-azure-machine-learning-cli-v2/)
- [Azure ML CLI single job examples](https://github.com/Azure/azureml-examples/tree/main/cli/jobs/single-step)
- [Azure ML CLI pipeline examples](https://github.com/Azure/azureml-examples/tree/main/cli/jobs/pipelines)
- [Deploy to managed online endpoint](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-deploy-managed-online-endpoints)
- [Deploy to batch endpoint](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-use-batch-endpoint)

## [Go to Part 3](part_3.md)
