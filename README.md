# Azure AI Foundry: Using DeepSeek R1 with Azure AI Python SDK / LangChain
[DeepSeek R1](https://github.com/deepseek-ai/DeepSeek-R1) is a large language model (LLM) focused on reasoning and code generation, with a large context window intended for handling extended interactions.  This repo provides code examples demonstrating its use in Azure AI Foundry with **Azure AI Python SDK** / **LangChain**.

## Table of contents:
- [Pre-requisites](#pre-requisites)
- [Scenario 1: Logical Puzzle](#scenario-1-logical-puzzle)
- [Scenario 2: Financial Loan Approval](#scenario-2-financial-loan-approval)
- [Scenario 3: RAG for Quantum Computing in Arxiv](#scenario-3-rag-for-quantum-computing-in-arxiv)

## Pre-requisites
1. Deploy _DeepSeek R1_ from Azure AI Foundry's model catalog as a serverless API. Specifics of the deployment process is described [here](https://learn.microsoft.com/en-us/azure/ai-studio/how-to/deploy-models-serverless).
2. Add required environment variable:

| Environment Variable | Description |
| --- | --- |
| ```AZURE_FOUNDRY_DEEPSEEK``` | Deployment name of the **_DeepSeek R1_** model |

3. Install the required Python packages using the **pip** command and the provided requirements.txt file:
``` PowerShell
pip install -r requirements.txt
```
> [!NOTE]
> Jupyter notebooks utilise the _DefaultAzureCredential_ class. Depending on your environment, the Python code will search for available Azure identities in the order described [here](https://learn.microsoft.com/en-us/python/api/azure-identity/azure.identity.defaultazurecredential?view=azure-python).

## Scenario 1: Logical Puzzle
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
> [!NOTE]
> User prompt: _Alice, Bob and Carol each have a different favourite colour: red, blue or green. Alice doesn't like red. Bob's favourite colour is not blue. Carol's favourite colour is red. What is each person's favourite colour? Explain your reasoning step by step._

## Scenario 2: Financial Loan Approval
1. Define DeepSeek R1 model as an LLM for LangChain:
``` Python
model = AzureAIChatCompletionsModel(
    endpoint = DS_Endpoint,
    credential = Az_Credential
)
```
2. Define loan approval requirements as a Prompt Template for Langchain:
``` Python
loan_approval_prompt = PromptTemplate(
    input_variables = ["applicant_info"],
    template = """
    ...
    """
)
```
3. Configure and invoke the chain
``` Python
chain = loan_approval_prompt | model
chain.invoke(applicant_info)
```
> [!NOTE]
> System prompt: _You are a financial advisor at a bank. You need to decide whether to approve a loan application. Consider the following applicant information: {applicant_info}. Provide your decision (approve or deny) AND a detailed explanation of your reasoning, including specific factors you considered and how they influenced your decision. Be transparent and thorough, as this is for a regulated industry._

## Scenario 3: RAG for Quantum Computing in Arxiv
1. Define DeepSeek R1 model as an LLM for LangChain:
``` Python
model = AzureAIChatCompletionsModel(
    endpoint = DS_Endpoint,
    credential = Az_Credential
)
```
2. Define Arxiv document retrieval
``` Python
arxiv_retriever = ArxivRetriever(
    load_max_docs=2,
    get_ful_documents=True,
)
```
3. Define research paper analysis as a Prompt Template for Langchain:
``` Python
prompt_template = PromptTemplate(
    input_variables = ["query", "relevant_info"],
    template = """
    ...
    """
)
```
4. Configure and invoke the chain
``` Python
chain = prompt_template | model
relevant_papers = arxiv_retriever.invoke(query)
relevant_info = "\n".join([paper.page_content for paper in relevant_papers])
chain.invoke({"query": query, "relevant_info": relevant_info})
```
> [!NOTE]
> System prompt: _You are an expert scientific researcher. Use the following information from arXiv to answer the user's question. If there is no sufficient information, say 'I need more information to answer this question'. Question: {query} Relevant Information: {relevant_info} Answer:_
