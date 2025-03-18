# CrewAI-Langgraph
# CrewAI Email and Weather Workflow

This repository demonstrates a state-driven workflow using CrewAI and LangGraph. The workflow is designed to process user input and decide whether to:
- **Classify and reply to an email**
- **Retrieve weather information**
- **Generate a simple reply for any other queries**

## Overview

The code defines a set of agents and tasks that are orchestrated in a workflow graph. Based on the input query, the system:
1. **Classifies the query:** An external language model (accessed via `openai_llm.invoke`) categorizes the query into one of three types:
   - `email_query`
   - `weather_query`
   - `other`
2. **Executes the corresponding task:**
   - For an **email query**:
     - A classifier agent analyzes the email.
     - An email writing agent generates a response.
   - For a **weather query**:
     - A weather agent retrieves current weather details.
   - For any **other query**:
     - A reply agent generates a generic response.

The workflow uses the following components:
- **Agents:**  
  - **Email Classifier:** Classifies the email as either "Important" or "Casual".  
  - **Email Writing Expert:** Crafts a reply based on the email classification.  
  - **Weather Expert:** Retrieves weather information from an external API.
- **Tasks:**  
  - **Classification Task:** Processes the email classification.  
  - **Writer Task:** Generates the email reply.  
  - **Weather Task:** Retrieves weather information based on the query.
- **Workflow Nodes:**  
  - **entryNode:** Uses an LLM to categorize the query and sets up state for the workflow.  
  - **emailNode (writerNode):** Runs the email crew (classification and reply generation).  
  - **weatherNode:** Executes the weather task to retrieve current weather details.  
  - **responder (replyNode):** Generates a reply for other query types.
- **Conditional Routing:**  
  The function `where_to_go(state)` determines which node (or branch) the workflow will follow based on the category extracted from the initial query.

## Prerequisites

- Python 3.7+
- [CrewAI](https://github.com/crewai-ai/crewai) and its tools
- [LangGraph](https://github.com/langgraph/langgraph) for workflow management
- An OpenAI API key (and any other required keys) stored in your environment variables (e.g., via a `.env` file)

Install the required packages using pip:

```bash
%pip install -U 'crewai[tools]'
%pip install -U crewai

