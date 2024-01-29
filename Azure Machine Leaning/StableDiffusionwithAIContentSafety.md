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

## Deploying stabilityai-stable-diffusion-2-1 model with Azure AI Content Safety

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

## Validating Safe generation with Azure AI Content Safety

- Test the Rel-time end-points with and un-safe prompts
  
- Unsafe prompts are blocked by blocked by the Azure AI Content Safety (AACS) service.
  
- From the Aure Portal open the deployed  Azure AI Content Safety (AACS) service resource and using the Monitor online activity validate the content filtering for unsafe prompts

  ![image](https://github.com/mahes-a/2024/assets/120069348/e1e2b68d-6ec2-4210-9062-fa07c2cb183e)

     

## Deploying the sample Gradio web UI

- We use Gradio UI to execute the stabilityai-stable-diffusion-2-1 AML endpoint
  
- stabilityai-stable-diffusion-2-1 AML endpoint returns image content in base64 format.
  
- We can Decode the base64 string and render as PIL Image , The entire sample code as below

          #pip install gradio
          import gradio as gr
          import numpy as np
          from PIL import Image
          import io
          import base64
          import requests
          import json
          import urllib.request
          import os
          import ssl
          
          def allowSelfSignedHttps(allowed):
              # bypass the server certificate verification on client side
              if allowed and not os.environ.get('PYTHONHTTPSVERIFY', '') and getattr(ssl, '_create_unverified_context', None):
                  ssl._create_default_https_context = ssl._create_unverified_context
          
          allowSelfSignedHttps(True) # this line is needed if you use self-signed certificate in your scoring service.
          def call_api(user_input, height, width):
              # Build the JSON payload
              #user_input='tennis in space'
              #height=512
              #width  =512
              data = {
                  "input_data": {
                      "columns": ["prompt"],
                      "data": [user_input],
                      "parameters": {
                          "height": height,
                          "width": width
                      }
                  }
              }
              print(data)
              
              body = str.encode(json.dumps(data))
              
              url = '' #Rest endpoint
              # Replace this with the primary/secondary key or AMLToken for the endpoint
              api_key = ''
              
              # The azureml-model-deployment header will force the request to go to a specific deployment.
              # Remove this header to have the request observe the endpoint traffic rules
              headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key), 'azureml-model-deployment': 'text-to-image-deploy' }
          
              req = urllib.request.Request(url, body, headers)
              base64_string=''
              try:
                  response = urllib.request.urlopen(req)
                  # Parse the JSON string into a Python list  
                  data = json.loads(response.read()) 
                  #print(data)
            
                  # Access the 'generated_image' field of the first item in the list  
                  if data and 'generated_image' in data[0]:  
                      base64_string = data[0]['generated_image']
                  #return base64_string
                  #base64_string = response.read()
                  
              except urllib.error.HTTPError as error:
                  print("The request failed with status code: " + str(error.code))
                  # Print the headers - they include the requert ID and the timestamp, which are useful for debugging the failure
                  print(error.info())
                  print(error.read().decode("utf8", 'ignore'))
          
              
              if not base64_string:
                      base64_string= '' #add the base64 string of image to be rendered when content safety blocks the reponses
          
                  # Remove 'data:image/png;base64,' if present, as PIL doesn't need it
              if base64_string.startswith('data:image/png;base64,'):
                      base64_string = base64_string.replace('data:image/png;base64,', '')
          
                  # Decode the base64 string
              image_data = base64.b64decode(base64_string)
                  # Convert the image data to a PIL Image
              image = Image.open(io.BytesIO(image_data))
                  # Convert the PIL Image to a numpy array and return it
              return np.array(image)
          
          iface = gr.Interface(fn=call_api, inputs=["text", "number", "number"], outputs='image', allow_flagging='never')
          iface.launch()
