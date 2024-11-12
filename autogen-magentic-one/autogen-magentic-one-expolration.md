
# Setup and Usage of Azure OpenAI GPT-4o

This guide provides step-by-step instructions to set up autogen magnetic one.
 # Please read the caution and notes before beginning the setup from [autogen repo ](https://github.com/microsoft/autogen/tree/main/python/packages/autogen-magentic-one)

## Prerequisites

- Visual Studio Code on Windows
- Azure subscription with access enabled for the Azure OpenAI service with GPT-4o model deployed
- [Docker](https://docs.docker.com/engine/install/)

## Steps

### Step 1

Follow the instructions in the repo setup steps [here](https://github.com/microsoft/autogen/tree/main/python/packages/autogen-magentic-one#setup-and-usage) to complete Step 1
![image](https://github.com/user-attachments/assets/f587c308-a0d8-4a89-a0cc-ad0107388b54)


### Step 2: Configure Environment Variables

1. One approach for setting environment variables is to Create a `.env` file in the `python` folder.
2. Add the required variables, including the Azure OpenAI endpoint and model name.

```env
CHAT_COMPLETION_PROVIDER='azure'
CHAT_COMPLETION_KWARGS_JSON='{
  "api_version": "2024-02-15-preview",
  "azure_endpoint": "https://YourAzureopenAI.openai.azure.com/",
  "model_capabilities": {
    "function_calling": true,
    "json_output": true,
    "vision": true
  },
  "azure_ad_token_provider": "DEFAULT",
  "model": "gpt-4o" # your gpt-4o model name
}'
```
3. Add below code in autogen\python\packages\autogen-magentic-one\examples\example.py
    ```
      from dotenv import load_dotenv
      load_dotenv()
    ```
### Step 3: Azure OpenAI with AAD Authentication

Follow the steps listed below to use Azure OpenAI client with Azure Active Directory (AAD) authentication:

- [Azure OpenAI with AAD Auth — AutoGen](https://microsoft.github.io/autogen/dev/user-guide/core-user-guide/cookbook/azure-openai-with-aad-auth.html)
- [How to configure Azure OpenAI Service with Microsoft Entra ID authentication - Azure OpenAI | Microsoft Learn](https://learn.microsoft.com/en-us/azure/ai-services/openai/how-to/managed-identity#chat-completions)

### Step 4: Install AutoGen

Before executing the example code, you may need to install `autogen`:

```bash
pip install autogen
```

### Example Code

The following code creates a directory `my_logs` in autogen\python\packages\autogen-magentic-one\my_logs to store logs and saves every screenshot of the websurfer agent:

```bash
# Save screenshots of browser
python examples/example.py --logs_dir ./my_logs --save_screenshots
```

### Additional Notes

- **User Confirmation**: Whenever a code execution may happen, user confirmation is required.
- **Content Safety**: Azure OpenAI Content Safety may detect some executions and block them as jailbreak attempts.
- **Monitoring**: Monitor the Docker containers.

The `log.jsonl` file and screenshots of the browser saved under the log folder provide adequate logging and enable monitoring.

## Conclusion

Follow these steps to set up and use the Azure OpenAI GPT-4o model effectively. Ensure to monitor the logs and screenshots for any issues.
