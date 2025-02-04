# Azure AI Foundry: Using DeepSeek R1 with Azure AI Python SDK
DeepSeek R1 is a large language model (LLM) focused on reasoning and code generation, with a large context window intended for handling extended interactions.  This repo provides code examples demonstrating its use in Azure AI Foundry with **Azure AI Python SDK**.

> [!NOTE]
> Data file used in this repo was borrowed from the [Microsoft's Azure OpenAI + Azure AI Search open-source solution](https://github.com/Azure-Samples/azure-search-openai-demo)

## Table of contents:
- [Pre-requisites]()
- [Scenario 1: Logical Puzzle]()
- [Scenario 2: DeepSeek R1 with OpenAI SDK]()

## Pre-requisites
1. You can deploy _DeepSeek R1_ from Azure AI Foundry's model catalog as a serverless API. Specifics of the deployment process is described [here](https://learn.microsoft.com/en-us/azure/ai-studio/how-to/deploy-models-serverless).

## Scenario 1: Logical Puzzle
Solving the following logical puzzle:
> Alice, Bob, and Carol each have a different favorite color: red, blue, or green.  Alice doesn't like red. Bob's favorite color is not blue. Carol's favorite color is red. What is each person's favorite color? Explain your reasoning step by step.

## Scenario 2: DeepSeek R1 with OpenAI SDK
