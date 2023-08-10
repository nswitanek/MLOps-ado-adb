
# Part 1: Structure code for fast iterative development
## Pre-requisites
- Complete [Part 0](part_0.md) to set up the required resources and permissions in Azure. 


## Summary 
Your team has been working on a new ML problem. The team has done initial exploratory work preparing the data and fitting models and has now come to a state that the solution direction is mostly solidified. Now, it is time to structure the work so that the team can systematically and quickly iterate towards an improved and deployable solution.   

So far, team members have been working mostly independently in Azure Databricks notebooks that handle their end-to-end model development workflow. To enable more effective collaboration for continuous improvement and easier maintenance of the workflow, they will benefit from breaking the workflow into separately maintainable but linked parts.

As a first step towards MLOps, the team needs to accomplish the following:  

- Modularization: A monolithic Databricks notebook is refactored into smaller, "module" notebooks that focus on a particular stage in the overall model development lifecycle and can be developed and tested independently and in parallel by multiple members.
- Parameterization: The modular notebooks are parameterized so that they be rerun with different parameter values.

To illustrate how the process works, the monolithic notebook was refactored into a feature engineering notebook, a model training notebook, and an evaluation notebook. You will run these modules individually to see how they work.

 ![monolithic to modular](./images/monolithic_modular.png)

## Steps

> Note: You can review notebooks and run the following tasks in the Databricks Repo in your Azure Databricks workspace. 

0. Navigate to `Repos/{your Databricks user account}/MLOps-ado-adb/src/workshop/notebooks`.

![Databricks Repo file explorer](part_1_db_repo_file_explorer.png)


1. Familiarize yourself with the steps in the
  notebook in the Databricks Repo at `/notebooks/mlflow-end-to-end-example.ipynb`. The notebook, developed by Databricks and available in the [Azure Databricks documentation](https://learn.microsoft.com/en-us/azure/databricks/mlflow/end-to-end-example), shows the end-to-end data preparation and model building workflow in a single notebook. **There is no need to run this as part of this workshop.**
   
2. Ask yourself, and discuss with your team, why putting the entire workflow into a single notebook is a challenge to scalable and repeatable ML development.
    > Note: Now observe how the monolithic notebook was refactored into a data prep or feature engineering module, a model training module, and a model evaluation module so that each step in the overall process can be developed and run independently.

3. The basic version control and git branching strategy we'll use is as follows:
- the `main` branch contains all the code used to develop the model in production 
- the `integration` branch starts as a complete copy of `main`
- data scientists create feature branches off of `integration` with names like `dev-{yourname}` to experiment with changes to some part of the workflow, in the hopes of finding an improvement in the models produced by the workflow
- if results are promising, the work done in `dev-{yourname}` is merged into `integration`
- if the new work results in a model that outperforms the production model in `main`, then the new code in `integration` becomes the new `main`, and the model is updated to reflect the new workflow.

4. In your Databricks repo, create your own development branch off of the `integration` branch where you can make and track changes. This branch will be your development area to create and test new code or pipelines before committing or merging the code back into a common branch, such as `integration`.

To do this, right-click the `/MLOps-ado-adb` folder in your Databricks Repos section of your Workspace, and select the "Git..." option from the drop-down menu.

![Git options from Databricks Repo](part_1_git_options_from_adb_repo.png)

In the next screen, make sure the `integration` branch is selected from the drop-down menu.
![Databricks Repo branch UI with integration default](part_1_branch_ui_integration.png)

Select "Create Branch." In the next screen, type "dev-{yourname}" in the "Branch name" field and "Create" the branch based on "Branch: integration".
![Databricks Repo create dev branch based on integration branch](part_1_adb_create_branch.png)

After you've created the branch, close the branch window and confirm that `dev-{yourname}` appears in the filepath at the top of the Repos view in Azure Databrcks:
![Databricks Repo file explorer with dev branch selected](image-8.png)

While your dev branch is selected, you'll be looking at version-controlled copies of the files from the integration branch. 

Next let's review those task-focused notebooks that were refactored from the end-to-end monolithic notebook.

5. Review the refactored data preparation logic in the notebook at `/notebooks/part_1_1_data_prep.ipynb`.

This modular notebook focused on data prep does the following:

- Loads the raw data from dbfs.
- Checks for missing values.
- Does some basic data visualizations.
- Creates a new, binary outcome variable.
- Saves the prepared data to dbfs.

Run this notebook.

6. Review the refactored model training logic in the `/notebooks/part_1_2_training.ipynb` notebook. 

This modular notebook focused on model training does the following:

- Loads the data prepared in the data prep notebook.
- Splits the data for training and validation.
- Builds a baseline model and 
- Registers the model to the Model Registry and labels it as "Staging"

Run this notebook.

7. Review the refactored model evaluation logic in the `part_1_3_evaluating.ipynb`

This modular notebook focused on model evaluation does the following:

- Load the test data.
- Load the model registered to staging in the training step.
- Use the trained model to predict on the test data and generate model evaluation metrics.
- If no prior trained model exists, the model will be registered as a baseline model in production.
- If a production model is found, the evaluation metrics for that model will be compared against the newly trained model and if they surpass production, model will be registered to production. If they do not, the notebook exits and raises an exception.

Run this notebook.

8. Navigate to the Models section of Azure Databricks to see the a model is produced and labeled as Production. This will be our baseline model that future iterative development of parts of the ML workflow will aim to beat.

![Databricks Registered Models View](part_1_model_registry.png)

## Success criteria
- Feature engineering notebook runs and writes prepared data to dbfs.
- Model training notebook creates and registers a model.
- Model evaluation notebook either promotes a model to production or exits.


## Reference materials
- [Databricks Repos]()
- [Parameterizing Databricks Jobs]()

## [Go to Part 2](part_2.md)


