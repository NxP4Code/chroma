<p align="center">
  <a href="https://trychroma.com"><img src="https://user-images.githubusercontent.com/891664/227103090-6624bf7d-9524-4e05-9d2c-c28d5d451481.png" alt="Chroma logo"></a>
</p>

<p align="center">
    <b>Chroma - the open-source embedding database</b>. <br />
    The fastest way to build Python or JavaScript LLM apps with memory!
</p>

<p align="center">
  <a href="https://discord.gg/MMeYNTmh3x" target="_blank">
      <img src="https://img.shields.io/discord/1073293645303795742" alt="Discord">
  </a> |
  <a href="https://github.com/chroma-core/chroma/blob/master/LICENSE" target="_blank">
      <img src="https://img.shields.io/static/v1?label=license&message=Apache 2.0&color=white" alt="License">
  </a> |
  <a href="https://docs.trychroma.com/" target="_blank">
      Docs
  </a> |
  <a href="https://www.trychroma.com/" target="_blank">
      Homepage
  </a>
</p>


<p align="center">
  <a href="https://github.com/chroma-core/chroma/actions/workflows/chroma-integration-test.yml" target="_blank">
    <img src="https://github.com/chroma-core/chroma/actions/workflows/chroma-integration-test.yml/badge.svg?branch=main" alt="Integration Tests">
  </a> |
  <a href="https://github.com/chroma-core/chroma/actions/workflows/chroma-test.yml" target="_blank">
    <img src="https://github.com/chroma-core/chroma/actions/workflows/chroma-test.yml/badge.svg?branch=main" alt="Tests">
  </a>
</p>

```bash
pip install chromadb # python client
# for javascript, npm install chromadb!
```

The core API is only 4 functions (run our [💡 Google Colab](https://colab.research.google.com/drive/1QEzFyqnoFxq7LUGyP1vzR4iLt9PpCDXv?usp=sharing) or [Replit template](https://replit.com/@swyx/BasicChromaStarter?v=1)):

```python
import chromadb
# setup Chroma in-memory, for easy prototyping. Can add persistence easily!
client = chromadb.Client()

# Create collection. get_collection, get_or_create_collection, delete_collection also available!
collection = client.create_collection("all-my-documents")

# Add docs to the collection. Can also update and delete. Row-based API coming soon!
collection.add(
    documents=["This is document1", "This is document2"], # we handle tokenization, embedding, and indexing automatically. You can skip that and add your own embeddings as well
    metadatas=[{"source": "notion"}, {"source": "google-docs"}], # filter on these!
    ids=["doc1", "doc2"], # unique for each doc
)

# Query/search 2 most similar results. You can also .get by id
results = collection.query(
    query_texts=["This is a query document"],
    n_results=2,
    # where={"metadata_field": "is_equal_to_this"}, # optional filter
    # where_document={"$contains":"search_string"}  # optional filter
)
```

## Features
- __Simple__: Fully-typed, fully-tested, fully-documented == happiness
- __Integrations__: [`🦜️🔗 LangChain`](https://blog.langchain.dev/langchain-chroma/) (python and js), [`🦙 LlamaIndex`](https://twitter.com/atroyn/status/1628557389762007040) and more soon
- __Dev, Test, Prod__: the same API that runs in your python notebook, scales to your cluster
- __Feature-rich__: Queries, filtering, density estimation and more
- __Free & Open Source__: Apache 2.0 Licensed

## Chroma DB Server Deployment with Persistance Storage
This method doesnt need cloning of the entire codebase and rebuilding process. 
Docker container used in this process are using docker volumes to store data. As long as volumes are retained during the restarting containers the data will persist in the chromadb.
1. Download docker-compose.server.example.yml file from the root and rename it to docker-compose.yml
2. Install docker on your local machine. [`Docker Engine`](https://docs.docker.com/engine/install/) or [`Docker Desktop`](https://docs.docker.com/desktop/install/)
3. Install docker compose [`Docker Compose`](https://docs.docker.com/compose/install/)
4. run following commands from the directory where earlier downloaded docker compose file is saved.

- __Command to Start Containers__
``` bash
docker compose up -d
or 
docker-compose up -d
```

- __Command to Stop Containers__
``` bash
docker compose down
or 
docker-compose down
```
- __Command to Stop Containers and delete volumes__
This is distructive command. With this command volumes created earlier will be deleted along with data stored.
``` bash
docker compose down -v
or 
docker-compose down -v
```

## Use case: ChatGPT for ______

For example, the `"Chat your data"` use case:
1. Add documents to your database. You can pass in your own embeddings, embedding function, or let Chroma embed them for you.
2. Query relevant documents with natural language.
3. Compose documents into the context window of an LLM like `GPT3` for additional summarization or analysis.

## Embeddings?

What are embeddings?

- [Read the guide from OpenAI](https://platform.openai.com/docs/guides/embeddings/what-are-embeddings)
- __Literal__: Embedding something turns it from image/text/audio into a list of numbers. 🖼️ or 📄 => `[1.2, 2.1, ....]`. This process makes documents "understandable" to a machine learning model.
- __By analogy__: An embedding represents the essence of a document. This enables documents and queries with the same essence to be "near" each other and therefore easy to find.
- __Technical__: An embedding is the latent-space position of a document at a layer of a deep neural network. For models trained specifically to embed data, this is the last layer.
- __A small example__: If you search your photos for "famous bridge in San Francisco". By embedding this query and comparing it to the embeddings of your photos and their metadata - it should return photos of the Golden Gate Bridge.

Embeddings databases (also known as **vector databases**) store embeddings and allow you to search by nearest neighbors rather than by substrings like a traditional database. By default, Chroma uses [Sentence Transformers](https://docs.trychroma.com/embeddings#default-sentence-transformers) to embed for you but you can also use OpenAI embeddings, Cohere (multilingual) embeddings, or your own.

## Get involved

Chroma is a rapidly developing project. We welcome PR contributors and ideas for how to improve the project.
- [Join the conversation on Discord](https://discord.gg/MMeYNTmh3x)
- [Review the roadmap and contribute your ideas](https://docs.trychroma.com/roadmap)
- [Grab an issue and open a PR](https://github.com/chroma-core/chroma/issues)

## License

[Apache 2.0](./LICENSE)
