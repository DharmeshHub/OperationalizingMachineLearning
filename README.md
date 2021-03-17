# Operationalizing Machine Learning

## Table of contents
   * [Overview](#Overview)
   * [Architectural Diagram](#Architectural-Diagram)
   * [Key Steps](#Key-Steps)
   * [Screen Recording](#Screen-Recording)
   * [Comments and future improvements](#Comments-and-future-improvements)
  
***


## Overview

Purpose of this project to operationalizing Machine learning through two different approach. The data used here is of direct marketing campaigns through phone calls for banking institution. The classification goal is to predict if customer will subscribe to term deposit (yes/no).
In first method we are creating model using Automated ML run in Azure Machine learning studio. Based on different accuracy best model is selected and deployed. Deployed model is consume with the help of Swagger documentation using REST API endpoint and key produced by deployed best model.
In Second part we are creating pipeline for model creation using same bank marketing dataset. Using Azure Python SDK we create, train and publish pipeline. Later we schedule pipeline to run every 20 minutes.
Please go through details mentioned below and in the screencast video.


***
## Architectural Diagram

Below is the architectural diagram showing the flow of operations:

![Architectural Diagram](images/Architectural_Diagram.png?raw=true "Architectural Diagram") 
  

***
## Key Steps

Below are key steps of the project:

- **Authentication:**
This step was skipped as we are using lab provided by Udacity and we are not authorize to create a security pricipal. Service Principal is a user role with controlled permission to access specific resources. With Service Principal we can allow authentication with specific scope of permission. This is best for security in terms of access.

- **Data Set:**
We are using Udacity Provided lab, Bank-marketing data set was already registered as shown below.

![Registered Datasets](images/AutoML_RegisteredDataset.png?raw=true "Registered Datasets")



- **Automated ML Experiment:**
In this step create Automated ML run using Bank-Marketing dataset and with following compute cluster configuration to run experiment.

	- Task: _Classification_
	- Primary metric: _Accuracy_
	- Checked: _Explain best model_
	- Exit criterion: 1 hour in _Job training time (hours)
	- Max concurrent iterations: 5

![Automated ML Experiment](images/AutoML_Exp_Running.png?raw=true "Automated ML Experiment")

	- Compute Cluster: Cluster node allocation as experiment runs

![Compute Cluster](images/AutoML_ComputeCluster.png?raw=true "Compute Cluster")

	- Experiment Completed

![Experiment Completed](images/AutoML_Exp_Completed.png?raw=true "Experiment Completed")

	- AutoML Completed

![AutoML Completed](images/AutoML_Completed.png?raw=true "AutoML Completed")



- **Deploy the Best Model:**
Once experiment completed, Best model appears in Details tab of Experiment Run. VotingEnsemble is the best model with Accuracy 91.98%

![AutoML BestModel](images/AutoML_BestModel.png?raw=true "AutoML BestModel")

	- Best Model with Explanation

![AutoML BestModel Explanation](images/AutoML_BestModel_Explanation.png?raw=true "AutoML BestModel Explanation")

	- Deployed best model with Authentication enable and with compute type Azure Container Instance (ACI). Deployed model generate Rest endpoint to interact with model with HTTP API service

![AutoML BestModel Explanation](images/AutoML_BestModel_Deployed.png?raw=true "AutoML BestModel Explanation")

	- Best Model metrics
![AutoML BestModel Explanation](images/AutoML_BestModel_Metrics.png?raw=true "AutoML BestModel Explanation")



- **Enable Logging:**
While deploying model we can enable logging but here we have enable logging through logs.py script. Once logging is enabled you can retrieve logs.

	- Executing logs.py to enable Application Insights logging.

![AutoML logs](images/AutoML_logs.png?raw=true "AutoML logs")

	- Application Insights is enabled.

![AutoML Application Insights](images/AutoML_Endpoint_ApplicationInsights_Enabled.png?raw=true "AutoML ApplicationInsights")

	- Application Insights logging.

![AutoML Application Insights logs](images/AutoML_Endpoint_ApplicationInsights.png?raw=true "AutoML ApplicationInsights logs")



- **Swagger Documentation:**

Swagger is a set of open-source tools built around the OpenAPI Specification that can help you design, build, document and consume REST APIs. We have used Swagger UI tool to generate documentation to interact with API.
Azure provide swagger.json file URL for deployed models. We downloaded this file in swagger folder.

	- Using swagger.sh script downloaded the latest swagger container and run swagger UI. I have used port 9000 on my local machine as port 80 was already allocated. Swagger is running on local host at port 9000. See below Swagger default UI.

![Swagger UI](images/Swagger_Default.png?raw=true "Swagger UI")

	- Using serve.py started the Python server on port 8000.

![Swagger](images/Swagger.png?raw=true "Swagger")


- **Consume Model Endpoints:**
Once the model is deployed, use the endpoint.py script to interact with the trained model. scoring_uri is REST endpoint point URL and Key is the authentication key generated while deploying the model using Enable Authentication.

![Consume Model](images/AutoML_Endpoint_Consume.png?raw=true "Consume Model")



- **Create and Publish a Pipeline:**
In this part of the project, I am using the Jupyter Notebook with the same keys, URI, dataset, cluster, and model names already created.

In this step we created, published and consume a Pipeline using Jupter notebook provided by Udacity - aml-pipelines-with-automated-machine-learning-step.ipynb. We update all required key values with this project specific.

	- Created Pipeline using same Bank-Marketing data set to train the model.

![Pipeline Created](images/Pipeline_Created.png?raw=true "Pipeline Created")

	- Pipeline Endpoint

![Pipeline Endpoint](images/Pipeline_Endpoint.png?raw=true "Pipeline Endpoint")

	- Bankmarketing dataset with AutoML module

![Pipeline Dataset](images/Pipeline_BankMarketing_Dataset.png?raw=true "Pipeline Dataset")
![Pipeline AutoMLModule](images/Pipeline_AutoMLModule.png?raw=true "Pipeline AutoMLModule")


	- Pipeline Run Completed

![Pipeline Completed](images/Pipeline_Completed.png?raw=true "Pipeline Completed")


	- Published Pipeline showing a REST endpoint and a Status of ACTIVE

![Pipeline Published](images/Pipeline_Published_Endpoint.png?raw=true "Pipeline Published")


	- Jupyter Notebook showing RunDetail Widget

![Pipeline RunDetails](images/Pipeline_RunDetails.png?raw=true "Pipeline RunDetails")


	- Pipeline Schedule Run every 20 minutes

![Pipeline RunDetails](images/Pipeline_ScheduleRun.png?raw=true "Pipeline RunDetails")


## Screen Recording

The screen recording can be found [here](https://youtu.be/jfnwvncqIfE) and it shows the project in action. More specifically, the screencast demonstrates:



## Comments and future improvements

Mostly AutoML covers all possible ways to predict best accuracy but in this case (rather all) data plays important role.

Here data is highly imbalanced. 
![Data Imbalancing](images/DataImbalancing.png?raw=true "Data Imbalancing")

- We can explore more on using customize featurization settings to ensure that the data and features that are used to train your ML model result in relevant predictions.
- Try different metrics using different algorithms for accurate prediction.
- We can vectorize data and try deep learning.
- Maximize processing time.


