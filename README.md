# Local-RAG-agent-using-LLaMA-3

In this project, we will combine concepts from three RAG papers into a RAG agent:

1. Routing: Adaptive RAG (paper). Route questions to different retrieval approaches
2. Fallback: Corrective RAG (paper). Fallback to web search if docs are not relevant to query
3. Self-correction: Self-RAG (paper). Fix answers w/ hallucinations or don’t address question

<p align="center">
  <img src="https://github.com/kunaldudhavat/Local-RAG-agent-using-LLaMA-3/blob/main/workflow.png" alt="RAG-workflow" title="RAG-workflow">
</p>

## Requirements
1. Install [Ollama](https://ollama.com/download/) on your machine and use LLaMa 3 model using:
   ```bash
    ollama run llama3
    ```
   
3. We will use [Tavily](https://tavily.com/#api) search api as the search tool and [Langsmith](https://www.langchain.com/langsmith) for tracing.

  Set the api_keys for both in the notebook:
   ```bash
    import os

    os.environ['LANGCHAIN_TRACING_V2'] = 'true'
    os.environ['LANGCHAIN_ENDPOINT'] = 'https://api.smith.langchain.com'
    os.environ['LANGCHAIN_API_KEY'] = 'your_api_key' # Replace with your key
  
    os.environ['TAVILY_API_KEY'] = 'your_api_key' # Replace with your key
   ```
4. The workflow for the RAG is as follows:

## RAG Workflow Summary

- **Document Loading and Splitting**:
  - Use `WebBaseLoader` to load documents from specified URLs.
  - Split documents into smaller chunks with `RecursiveCharacterTextSplitter`.

- **Vector Store and Embeddings**:
  - Add split documents to a Chroma vector store.
  - Generate local text embeddings using `NomicEmbeddings`.

- **Language Model**:
  - Employ `ChatOllama` with the Llama3 model for LLM operations.

- **Retrieval Grader**:
  - Assess the relevance of retrieved documents using a grader with `ChatOllama`.

- **Hallucination Grader**:
  - Ensure the generated answers are grounded in the documents using a hallucination grader.

- **Answer Grader**:
  - Evaluate the usefulness of the answers with an answer grader.

- **Question Router**:
  - Direct questions to either the vector store or a web search based on content using a router.

- **Web Search Integration**:
  - Integrate `TavilySearchResults` as a fallback for insufficient vector store retrieval.

- **State Management and Control Flow**:
  - Manage and execute the process using `StateGraph` for efficient question answering.

   
