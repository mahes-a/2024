# DesignBuddy: A Image Generation Web App Powered by Stability AI stable-diffusion, Hosted on Azure with AI Content Safety #

DesignBuddy is a Gradio UI application that uses Stability AI's Stable Diffusion Model to generate images from text inputs. Hosted on Azure, it ensures reliable performance and scalability. It integrates with Azure AI Content Safety, preventing the generation of inappropriate content. DesignBuddy is a user-friendly tool for anyone looking to leverage AI in their creative process.

## High-level Architecture

<img width="631" alt="image" src="https://github.com/mahes-a/2024/assets/120069348/f0e8671b-89f2-4bf4-bece-e95d29dba550">

## UI Screen Grabs

![image](https://github.com/mahes-a/2024/assets/120069348/497a6208-ab7e-4f21-ba8c-25a15a7ce11c)

![image](https://github.com/mahes-a/2024/assets/120069348/fb46e65f-a4fe-4e33-a30e-859e18c0dce8)

![image](https://github.com/mahes-a/2024/assets/120069348/3917a84f-393a-4543-bb4d-b9a1005822e4)


## Prerequisites

- Azure subscription with Azure Machine Learning resource
- Deploying stabilityai-stable-diffusion-2-1 requires GPU compute of V100 / A100 SKUs. You can view and request AzureML compute quota [here](https://ml.azure.com/quota).

  ## Technical Flow

- Gradio UI depoyed as a Azure web app via App Service , Gradio UI sends user inputs to  Azure Machine learning real-time inference endpoints
  
- Azure Machine learning real-time inference endpoints hosts the stabilityai-stable-diffusion-2-1 model with Azure AI Content Safety monitoring
  
- The User prompts are monitored , validated and filtered by  Azure AI Content Safety resource
  
- Safe Prompts are sent to stabilityai-stable-diffusion-2-1 model and response from  model is also monitored and validated by the Azure AI Content Safety resource
  
- Safe Image Responses are sent back to UI via the Azure Machine learning real-time inference endpoints

##### Deploying stabilityai-stable-diffusion-2-1 model with Azure AI Content Safety

- Login into your ML Studio [here](https://ml.azure.com/)

   <img width="998" alt="image" src="https://github.com/mahes-a/StagingBuild/assets/120069348/b905bda5-79df-41cf-b444-28beb0543953">

- Go to Notebooks , Open a Terminal and connect to a Compute Instance
  
- Git clone the repo from https://github.com/Azure/azureml-examples/blob/main/sdk/python/foundation-models/system/inference/text-to-image/safe-text-to-image-online-deployment.ipynb
  
- Execute the cells in the notebook by entering the required details like the Subscription ID , Resource group names
  
- Once notebook execution completes the stabilityai-stable-diffusion-2-1 model with Azure AI Content Safety resource is deployed  open up the endpoints->Real-time-endpoints-> The stable diffusion endpoint

  ![image](https://github.com/mahes-a/2024/assets/120069348/73ebc904-0cdb-4805-acea-85a12afc2629)

- Test the endpoint

  ![image](https://github.com/mahes-a/2024/assets/120069348/a592f34b-c7dc-47d0-ae3a-fdd930e5a214)


- select the Consume tab , Note down the Rest endpoint , deployment name and Authentication key these will be used to make the request from the UI
 
    ![image](https://github.com/mahes-a/2024/assets/120069348/39b306a9-bdd3-4fb7-8896-d268bbb0fbcc)


* To avoid costs , If you aren't going use the real time deployment, Please delete the endpoint by following the article [here](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-deploy-online-endpoints?view=azureml-api-2&tabs=azure-cli#delete-the-endpoint-and-the-deployment)*
