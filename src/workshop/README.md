# MLOps Workshop

## Introduction
The MLOps workshop is an instructor-led workshop that provides guidance on an MLOps
implementation in Azure. MLOps is a pattern of practices rather than a technology, and there are various ways of implementing MLOps on Azure. This workshop leverages [Azure Databricks](https://learn.microsoft.com/en-us/azure/databricks/introduction/)
and [Azure DevOps](https://learn.microsoft.com/en-us/azure/devops/user-guide/what-is-azure-devops?view=azure-devops)
to implement a robust set of workflows to support machine learning models in production. For a workshop using Azure Machine Learning and GitHub Actions, see a similar set of materials [here](https://github.com/microsoft/MLOpsTemplate/).

The core capability deployed in this scenario is a prediction of wine quality using a set of empirical measures. This is based on a [UCI Dataset](https://archive.ics.uci.edu/dataset/186/wine+quality). This is treated as a classification scenario, which occurs frequently for many enterprises. For the purpose of this workshop, the key stages of exploring the data,
engineering predictive features (data engineering) and model building (training, hyperparameter tuning,
algorithm selection, etc.) will be assumed to be done and already codified in this [Databricks
notebook](https://learn.microsoft.com/en-us/azure/databricks/mlflow/end-to-end-example).
The core focus of the workshop will then be how to refactor this notebook for easier maintenance and iterative development, lay the DevOps foundations for the ML lifecycle, for continuous delivery of the best predictive capabilities in production even as data science team members experiment with new techniques to improve model performance.

## Audience
- Customer data scientists
- ML engineers
- ML platform architects and managers
- ... and any other roles that require hands-on experience to support ML models in Azure

## Goals
- Understand key elements of modern MLOps and how it helps improve and accelerate ML practices.
- Design experiments and MLOps pipelines in Azure Databricks.
- Get hands-on experience in building continuous integration and continuous deployment pipelines with Azure DevOps.


[TODO: update the Structure and Repo sections, once the parts have been updated]
## Structure
- [Pre-Workshop Checklist](documents/part_tips.md)
- [Part 0: MLOps overview and environment setup](documents/part_0.md)
- [Part 1: Structure code for fast iterative development](documents/part_1.md)
- [Part 2: Use cloud scale compute and monitor experiment with Azure ML](documents/part_2.md)
- [Part 3: Use github for version control and automation](documents/part_3.md)
- [Part 4: Continuous integration (CI)](documents/part_4.md)
- [Part 5: Continuous deployment (CD)](documents/part_5.md) 

## Repo Structure
> Note: This is the repository file structure from the repository root.
- `README.md` > Core README for the repository
- `.github`
	- `actions` > YAML files for Github Actions relating to AML job creation, and endpoint deployment
	- `workflows` > YAML files for Github Actions relating to unit tests and CI/CD workflows
- `src`
	- `workshop`
		- `README.md` > README file highlighting the workshop goals, steps and key audience
		- `conda-local.yml` > Third-party python dependencies for managing the conda virtual environment
		- `core`
			- ``data_engineering`` > Python and YAML files to support feature engineering
			- ``evaluating`` > Python and YAML files to support model evaluation based on specific model metrics
			- ``pipelines`` > YAML files to support creation of ML pipelines
			- ``scoring`` > Python and YAML files to support model deployment and scoring
			- ``training`` > Python and YAML files to support model training
		- ``data`` > Base datasets in parquet, with a Python file to load the data into the default datastore
		- ``documents`` > Setup scripts, and markdown files to support a hands-on workshop
		- ``infra`` > Setup scripts to support initial creation of the Azure Machine Learning infrastructure and resources
		- ``notebooks`` > Jupyter notebook containing all the code related to data exploration, cleansing, feature engineering and model
		  creation

## Workshop Scenario
> **Note: Read before starting the workshop!**

Your team has been working on a new machine learning (ML) problem (predicting whether a wine is high quality or not). The
team has been performing exploratory work on the data and has come to a state where the model is solidified.
Now, it is time to put a structure into the work so that the team can iterate faster toward building better and better models. So far, team members have been working mostly with Azure Databricks notebooks on various clusters. 

To re-engineer this to align with MLOps processes, the following steps will be taken:
1. The notebook will be modularized (refactored into separate notebooks) and parameterized (configured so they
   can be re-run with different values). This will lay the foundation for good software practices and allow
   multiple data scientists/engineers to work collaboratively on the code. (Later, we will reinforce DevOps
   practices around continuous integration and continuous delivery with specific workflows to support model
   training and evaluation. MLOps builds off a strong foundation in DevOps and looks to additionally manage
   the ML model and data lifecycle to support the continuous delivery to production of the best model.)
2. After successfully restructuring the Databricks notebook and running the notebooks in a smaller cluster on a subset of the data, your team will run the ML experiment at scale. They will take advantage of experiment tracking
   and model management capabilities in MLflow to keep track of experiments. The team will then register the best model and deploy the
   model as a UDF for batch inferencing on a Delta table.
4. They will then set up centralized version control in GitHub to keep track of project code and manage different
   feature development tracks and releases. They will need to understand how to automate and orchestrate
   common tasks such as data prep, training, and testing. 
5. After setting up Azure DevOps for MLOps, your team will start automating the model training and evaluation
   process with a Continuous Integration (CI) pipeline.
6. After a successful run of the CI pipeline, your team will complete the process with a Continuous
   Delivery (CD) pipeline that will handle the deployment of the model without introducing any downtime in
   production (hot swap).
7. Now, head to [Workshop Environment Setup: Part 0](https://github.com/microsoft/MLOpsTemplate/blob/main/src/workshop/documents/part_0.md#part-0-workshop-environment-setup)


## Contributing
This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.opensource.microsoft.com.

When you submit a pull request, a CLA bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., status check, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

## Trademarks
This project may contain trademarks or logos for projects, products, or services. Authorized use of Microsoft 
trademarks or logos is subject to and must follow 
[Microsoft's Trademark & Brand Guidelines](https://www.microsoft.com/en-us/legal/intellectualproperty/trademarks/usage/general).
Use of Microsoft trademarks or logos in modified versions of this project must not cause confusion or imply Microsoft sponsorship.
Any use of third-party trademarks or logos are subject to those third-party's policies.
