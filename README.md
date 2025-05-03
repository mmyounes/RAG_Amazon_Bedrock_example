# RAG (Retrieval Augmented Generation) example with Amazon Bedrock and Couchbase

Retrieval Augmented Generation (RAG) is a machine learning approach that combines retrieval systems with large language models (LLMs) to generate output that has contextual information that can take advantage of private data and external knowledge sources.

#### Prepare Embeddings
![Embeddings](./RAG_pipeline_Couchbase_Bedrock.png)

Before being able to answer the questions, the documents must be processed and a stored in a document store index
- Create a numerical vector representation of each document using Amazon Bedrock Titan Embeddings model
- Create an vector index using the corresponding embeddings

#### Ask question
![Question](./RAG_runtime_Couchbase_Bedrock.png)

When the vector index is prepared, you are ready to ask the questions and relevant documents will be fetched based on the question being asked. Following steps will be executed.
- Create an embedding of the input question
- Compare the question embedding with the embeddings in the index
- Fetch the (top N) relevant document chunks
- Add those chunks as part of the context in the prompt
- Send the prompt to the model under Amazon Bedrock
- Get the contextual answer based on the documents retrieved

## Usecase
#### Dataset
In this example, you will use the Couchbase travel sample dataset.

## Implementation
In order to follow the RAG approach, this notebook is using the LangChain framework where it has integrations with different services and tools that allow efficient building of patterns such as RAG. We will be using the following tools:

- **LLM (Large Language Model)**: Anthropic Claude available through Amazon Bedrock

- **Embeddings Model**: Amazon Titan Embeddings available through Amazon Bedrock

  This model will be used to generate a numerical representation of the hotel description and reviews

- **Vector Store**: Couchbase available through LangChain

- **Index**: VectorIndex
  The index helps to compare the input embedding and the document embeddings to find relevant document.

### Python 3.10

⚠️⚠️⚠️ For this lab we need to run the notebook based on a Python 3.10 runtime. ⚠️⚠️⚠️

If you carry out the workshop from your local environment outside of the Amazon SageMaker studio please make sure you are running a Python runtime > 3.10.

### Note
It is possible to choose other models available with Bedrock. You can replace the `model_id` as follows to change the model.

`llm = Bedrock(model_id="...")`

## Sign Up for Capella
Creating a Capella cluster and setting up a vector database takes a few minutes. Follow the below steps:
- Go to https://cloud.couchbase.com to create a free tier account.
- Select your preferred region
- Configure the cluster access credentials
- Add an allowed IP address to connect to your cluster

We will use the travel-sample bucket for this demo. The next step is to create the vector index:
- On the **Databases** page, select the trial cluster where you want to create the vector index.
- Go to **Data Tools > Search**.
- Click **Import Search Index**.
- Upload the **hotel-vector-search.json** file. This file contains the vector index definition.
- Click **Import** then **Create Index**

### Check out this [blog post](https://www.couchbase.com/blog/couchbase-bedrock-rag-applications/) for more details