# Azure AI Foundry: Using DeepSeek R1 with Azure AI Python SDK
[DeepSeek R1](https://github.com/deepseek-ai/DeepSeek-R1) is a large language model (LLM) focused on reasoning and code generation, with a large context window intended for handling extended interactions.  This repo provides code examples demonstrating its use in Azure AI Foundry with **Azure AI Python SDK**.

> [!NOTE]
> Data file used in this repo was borrowed from the [Microsoft's Azure OpenAI + Azure AI Search open-source solution](https://github.com/Azure-Samples/azure-search-openai-demo)

## Table of contents:
- [Pre-requisites]()
- [Scenario 1: Logical Puzzle]()
- [Scenario 2: DeepSeek R1 with OpenAI SDK]()

## Pre-requisites
1. Deploy _DeepSeek R1_ from Azure AI Foundry's model catalog as a serverless API. Specifics of the deployment process is described [here](https://learn.microsoft.com/en-us/azure/ai-studio/how-to/deploy-models-serverless).
2. Add required environment variables:

| Environment Variable | Description |
| --- | --- |
| ```AZURE_FOUNDRY_DEEPSEEK``` | Deployment name of the **_DeepSeek R1_** model |
| `````` |  |

3. Install the required Python packages using the **pip** command and the provided requirements.txt file:
``` PowerShell
pip install -r requirements.txt
```
> [!NOTE]
> Jupyter notebooks utilise the _DefaultAzureCredential_ class. Depending on your environment, the Python code will search for available Azure identities in the order described [here](https://learn.microsoft.com/en-us/python/api/azure-identity/azure.identity.defaultazurecredential?view=azure-python).

## Scenario 1: Logical Puzzle
Solving the following logical puzzle:
> Alice, Bob, and Carol each have a different favorite color: red, blue, or green.  Alice doesn't like red. Bob's favorite color is not blue. Carol's favorite color is red. What is each person's favorite color? Explain your reasoning step by step.
1. Initialise Azure AI Inference client with DeepSeek R1's deployment endpoint and Azure credentails:
``` Python
client = ChatCompletionsClient(
    endpoint = DS_Endpoint,
    credential = Az_Credential
)
```
2. _complete_ function allows you to pass system / user messages, enable streaming, restrict volume of the model's output (completion) and configure other inference parameters:
``` Python
response = client.complete(
    messages = [
        System_Message,
        User_Message,
    ],
    max_tokens = 2048,
    stream = True
)
```

## Scenario 2: DeepSeek R1 with OpenAI SDK
