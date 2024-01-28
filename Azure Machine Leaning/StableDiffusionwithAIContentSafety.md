# DesignBuddy: A Image Generation Web App Powered by Stability AI, Hosted on Azure with AI Content Safety #

DesignBuddy is a Gradio UI application that uses Stability AI's Stable Diffusion Model to generate images from text inputs. Hosted on Azure, it ensures reliable performance and scalability. It integrates with Azure AI Content Safety, preventing the generation of inappropriate content. DesignBuddy is a user-friendly tool for anyone looking to leverage AI in their creative process.

## High-level Architecture

<img width="631" alt="image" src="https://github.com/mahes-a/2024/assets/120069348/f0e8671b-89f2-4bf4-bece-e95d29dba550">

## UI Screen Grabs

![image](https://github.com/mahes-a/2024/assets/120069348/497a6208-ab7e-4f21-ba8c-25a15a7ce11c)

![image](https://github.com/mahes-a/2024/assets/120069348/fb46e65f-a4fe-4e33-a30e-859e18c0dce8)

![image](https://github.com/mahes-a/2024/assets/120069348/3917a84f-393a-4543-bb4d-b9a1005822e4)


## Prerequisites

- Azure subscription with Azure Machine Learning resource
- Deploying stable-diffusion-2-1 requires GPU compute of V100 / A100 SKUs. You can view and request AzureML compute quota [here](https://ml.azure.com/quota).

  ## Technical Flow

- Gradio UI depoyed as a Azure web app via App Service , Gradio UI sends user inputs to  Azure Machine learning real-time inference endpoints
  
- Azure Machine learning real-time inference endpoints hosts the stable-diffusion-2-1 model with Azure AI Content Safety monitoring
  
- The User prompts are monitored , validated and filtered by  Azure AI Content Safety resource
  
- Safe Prompts are sent to stable-diffusion-2-1 model and response from  model is also monitored and validated by the Azure AI Content Safety resource
  
- Safe Image Responses are sent back to UI via the Azure Machine learning real-time inference endpoints

