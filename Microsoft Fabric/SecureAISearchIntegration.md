
# Secure Data Integration between Azure AI Search and Microsoft Fabric via Managed Private Endpoints

## Introduction  
In the era of AI, the ability to securely and efficiently access, analyze, and utilize data is crucial. Azure AI Search and Microsoft Fabric are powerful tools that can assist us in achieving this. The need to access enterprise data sources securely, especially those behind a firewall or inaccessible from the public internet, is critical. This is where Managed Private Endpoints for Microsoft Fabric, currently in public preview, come into play. They enable secure and effortless connections to data sources, without the need to expose them to the public network or require complex network configurations.  
  
## Prerequisites  
Before diving into the specifics, it's essential to review the prerequisites for setting up Azure AI Search and Microsoft Fabric Spark Notebooks. We will need an Azure subscription, Azure AI Search service, and a Microsoft Fabric workspace. For detailed setup instructions, you can refer to the official Microsoft documentation.  
Please note that currently Managed Private Endpoints are only supported for Fabric Trial capacity and Fabric capacities with F64 or higher SKUs.  
  
## Creating Managed Private Endpoints for Microsoft Fabric  
Managed Private Endpoints facilitate secure access to data sources for Fabric Data Engineering elements. This is done without having to expose these sources to the public network or implement complex network configurations. These private endpoints provide a secure route to connect and retrieve data from sources like Azure AI Search, Azure SQL DB, and more, which are shielded from public access. Consequently, Fabric Spark Notebooks or Spark Job Definitions can access data sources securely.  

<img width="580" alt="image" src="https://github.com/mahes-a/2024/assets/120069348/67a13c9f-65c3-4a36-8573-1869d94d8bc6">

  
Creating a Managed Private Endpoint is a straightforward process. Within your Fabric Workspace settings, you have the ability to create and approve Managed Private Endpoints for a variety of data sources. Please [refer](https://learn.microsoft.com/en-us/fabric/security/security-managed-private-endpoints-create#supported-data-sources) to official documentation for complete list of supported sources  
  
## Step by Step walkthrough Demo  
  
- From your Fabric Trial or F64 (or higher SKUs) Workspace, navigate to the Workspace Settings, select Network Security, and create a Managed Private Endpoint.

  ![image](https://github.com/mahes-a/2024/assets/120069348/f1a89f83-8677-4b17-8aad-80847cba161e)

 - Enter the Name, Resource Idenifier of the Secure Azure AI Search Resource which can be found in the properties section and Request message

   ![image](https://github.com/mahes-a/2024/assets/120069348/b8ab06be-f717-4538-8f97-0432cb18b8c0)


   ![image](https://github.com/mahes-a/2024/assets/120069348/b752b7b2-96e9-4594-9adf-db6c732d5ab2)

   ![image](https://github.com/mahes-a/2024/assets/120069348/7321691d-41e9-40ee-b44c-4aab34076901)

 - Wait for the Activation of the managed private endpoint in Fabric and the managed endpoint created would be in pending state

   ![image](https://github.com/mahes-a/2024/assets/120069348/a50a88bc-9274-43f8-800c-92e9f9350c1b)


 - Navigate to the Private endpoint section under networking of the Azure AI Search resource and approve the Fabric Managed private endpoint

   ![image](https://github.com/mahes-a/2024/assets/120069348/cbbcc1af-a2df-4a85-a464-10bc75c2b1d0)

   ![image](https://github.com/mahes-a/2024/assets/120069348/255f028b-d14f-48eb-ac22-1373ea4684e3)

- Once Approved , Navigate to Fabric workspace and confirm the approved status in Managed endpoint

  ![image](https://github.com/mahes-a/2024/assets/120069348/3cf81910-a4c1-4ff2-ade7-59248013029c)

- Now , We would be able to securely connect from Fabric Data Engineering and Data Science notebooks and Spark Jobs to the AI Search.
  
- Refer to [Sample Notebook](https://github.com/mahes-a/2024/blob/main/Microsoft%20Fabric/FabricCogSrchPrivateendpoint.ipynb) to connect and retrieve Index data from AI search Resource
