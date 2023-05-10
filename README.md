
# Operationalizing-Machine-Learning by Ebbin Daniel

This is an end-to-end machine learning project on Microsoft Azure where I have configured an **Automated Machine Learning Model** to run and select the best model and then to deploy and consume it. Based on the best model a pipeline was created to publish and run from a REST endpooint. 

**Dataset**

The <a href='https://archive.ics.uci.edu/ml/datasets/Bank+Marketing'>Bank Marketing Dataset</a> was used to train and test the AutoML models. The data is related with direct marketing campaigns of a Portuguese banking institution. The marketing campaigns were based on phone calls. The marketing data consists of various attributes like age,job, education, etc to access if a bank term deposit for a client would be subscribed or not. 

Here we try to train our data on the attributes and we seek to Classify if a bank term deposit for a client would be subscribed or not. 


## Architectural Diagram
![Architecture](screenshots/Architecture.jpg?raw=true "Architecture")

**Authentication :** In this step, we need to create a Service Principal (SP) account to interact with the Azure Workspace.

A “Service Principal” is a user role with controlled permissions to access specific resources. Using a service principal is a great way to allow authentication while reducing the scope of permissions, which enhances security.

**Automated ML Experiment :** In this step, we create an experiment using Automated ML and provide the required configuration to perform regression/classification jobs based on the requirements.  The Auto ML runs various models and keeps a track of the metrics to find the best model.

**Deploy the best model :**  Once we have the best model we deploy it as a web service on a ACI(Azure Container Instance).Once the Model is deployed it will generate a REST endpoint with an authentication token to interact with the deployed model. Deploying the Best Model will allow us to interact with the HTTP API service and interact with the model by sending data over POST requests.

**Enable logging :** Logging helps monitor our deployed model. It helps us know the number of requests it gets, the time each request takes, etc.Once the model is deployed we can enable the application insights from the python sdk as well. 

**Swagger Documentation :**  Swagger is a tool that helps build, document, and consume RESTful web services in Azure ML Studio. It further explains what types of HTTP requests that an API can consume, like POST and GET.Azure provides a swagger.json that is used to create a web site that documents the HTTP endpoint for a deployed model.In this step, we consume the deployed model using Swagger.

**Consume model endpoints :**  We can consume a deployed service via an HTTP API. An HTTP API is a URL that is exposed over the network so that interaction with a trained model can happen via HTTP requests.
Users can initiate an input request, usually via an HTTP POST request. HTTP POST is a request method that is used to submit data. The HTTP GET is another commonly used request method. 
HTTP GET is used to retrieve information from a URL. The allowed requests methods and the different URLs exposed by Azure create a bi-directional flow of information.
The APIs exposed by Azure ML will use JSON (JavaScript Object Notation) to accept data and submit responses. It served as a bridge language among different environments.

In this Step we provide with test data to receive a predicted result back. 

**Create and publish a pipeline :** In this step, we automate this workflow by creating a pipeline with the Python SDK. we create an AutoML step where we pass the AutoML config details and then pass the step to run in the pipeline. 


## Key Steps

The Key Steps for this project are as follows:

**Step 1: Authentication**

In this step, we will need to install the Azure Machine Learning Extension which allows to interact with Azure Machine Learning Studio, part of the az command. After having the Azure machine Learning Extension, we will create a Service Principal account and associate it with our specific workspace.

Since this step was optional, this was skipped.

**Step 2: Automated ML Experiment**

**Dataset**

Here we upload the <a href='https://archive.ics.uci.edu/ml/datasets/Bank+Marketing'>Bank Marketing Dataset</a> to the azure datasets as a Tabular format. 

![data](screenshots/Step2_Registered_Dataset.png?raw=true "data")

Once the Data is uploaded we create a new AutoML Experiment. We first connect our uploaded data asset.

![AutoML_data](screenshots/Step2_Registered_Dataset2.png?raw=true "AutoML_data")

**Compute**

We configure a new compute cluster and assign atleast 1 as the min number of nodes.

**AutoML**

We run the autoML for classification and wait for it to complete. the completed run can be seen as below with the details for the best model and its respective summary.  

![AutoML](screenshots/Step2_AutoML_completed.png?raw=true "AutoML")

In this case the best model is the voting ensamble. The details are below:

![best_model](screenshots/Step2_Best_model.png?raw=true "best_model")



**Step 3: Deploy the Best Model**

Here we deploy the best model as a ACI (Azure Container Instance)

![deploy1](screenshots/Step3_autoML_deploy1.png?raw=true "deploy1")

![deploy2](screenshots/Step3_autoML_deploy2.png?raw=true "deploy2")

Once the Model is successfully deployed it shows that the deployment state is healthy

![deploy2](screenshots/Step3_autoML_deployed.png?raw=true "deploy2")


**Step 4: Enable Application Insights**

Here we use the Logs.py script to enable the application insights.

![app_insights1](screenshots/Step4_Enable_Insights.png?raw=true "app_insights1")

Once enables we can navigate to the UI and can see the application insights been turned on and has a TRUE value. 

![app_insights2](screenshots/Step4_Enable_Insights_UI.png?raw=true "app_insights2")


**Step 5: Swagger Documentation**

The deployed model provides the swagger.json an we use the swagger.sh and serve.py script to view the swagger UI for documentation on the localhost. 

![swagger1](screenshots/Step5_swagger1.png?raw=true "swagger1")

![swagger2](screenshots/Step5_swagger2_POST.png?raw=true "swagger2")

**Step 6: Consume Model Endpoints**

Here we copy the REST endpoint URL and the Authentication key from the UI and update the endpoints.py file.

Once the code runs its takes the JSON input we provided and gives us a result in a JSON format. 

![endpoints3](screenshots/Step6_Endpoints3.png?raw=true "endpoints3")

![endpoints2](screenshots/Step6_Endpoints2.png?raw=true "endpoints2")

![endpoints1](screenshots/Step6_Endpoints.png?raw=true "endpoints1")

**Step 7: Create, Publish and Consume a Pipeline**

Here we import the data, create, publish and consume the pipeline for the AutoML Step we created. 

**Dataset**
![pipeline_data](screenshots/Step7_Dataset.png?raw=true "pipeline_data")

**Configuring AutoML step and starting a pipeline**
![pipeline_data1](screenshots/Step7_pipeline1.png?raw=true "pipeline_data1")

**UI View of the Pipeline**
![pipeline_data2](screenshots/Step7_pipeline2.png?raw=true "pipeline_data2")

**Run Details from Azure SDK**
![pipeline_data3](screenshots/Step7_pipeline3.png?raw=true "pipeline_data3")

![pipeline_data4](screenshots/Step7_pipeline4.png?raw=true "pipeline_data4")

Once the Run is Completed the green check appears in the UI and the Status in the Run details says **completed**
![pipeline_data5](screenshots/Step7_pipeline5.png?raw=true "pipeline_data5")

![pipeline_data6](screenshots/Step7_pipeline6.png?raw=true "pipeline_data6")


The Published Pipeline shows the Status as active with a REST Endpoint link.

![pipeline_data8](screenshots/Step7_pipeline_Overview.png?raw=true "pipeline_data8")

This Can also be viewed from the Azure ML Studio UI, 

Completed Pipeline Jobs from the Jobs section with Job type as Pipeline and status completed

![pipeline_job](screenshots/Step7_pipeline_job_completed.png?raw=true "pipeline_job")

Under Pipelines > Pipeline Jobs: The Various pipelines and the respective completed status can be viewed. 

![pipeline_job_done](screenshots/Step7_pipeline1_created.png?raw=true "pipeline_job_done")

We can also View Status Under Pipeline > Pipeline Endpoints for the pipeline and ACTIVE Status. 

![pipeline_status](screenshots/Step7_deployed_pipeline1.png?raw=true "pipeline_status")

Once we click the on the pipeline and go to the Published Pipeline Overview Page we can see the Status as ACTIVE and the REST endpoint.

![pipeline_view](screenshots/Step7_pipeline_overview_1.png?raw=true "pipeline_view")




## Future Improvements:

This project can be further improved on by:

* To perform an Exploratory Data Analysis to find Bias and Correlation in data
* Feature Importance of the factors to remove features that do not contribute to the model improvement
* Enabling Deep Learning for a better prediction
* Using the Apache Benchmark to set a measure for optimal performance.