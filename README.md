# Kernel Memory with Elasticsearch
**By Free Mind Labs, Inc.** - *Dive into your Stream*

[![License: MIT](https://img.shields.io/github/license/microsoft/kernel-memory)](https://github.com/freemindlabsinc/FreeMindLabs.SemanticKernel/blob/main/LICENSE)

Use [Elasticsearch](https://www.elastic.co/) as vector storage for Microsoft **[Kernel Memory](https://github.com/microsoft/semantic-memory)** (KM)

Kernel Memory (KM) is an open-source service and plugin specialized in the efficient indexing of datasets through custom continuous data hybrid pipelines.

<img src="content/images/Pipelines.jpg"/>

Utilizing advanced embeddings and LLMs, the system enables Natural Language querying for obtaining answers from the indexed data, complete with citations and links to the original sources.

<img src="content/images/RAG.jpg"/>


This repository contains the **Elasticsearch adapter** that allows KM to use Elasticsearch as vector database, thus allowing developers to perform hybrid and semantic search directly on  Elasticsearch, on-premise or in the cloud.

Tokenization can be done using commercial models from OpenAI, Azure Open AI or open source models hosted on Hugging Face, including those used by [Sentence Transformers](https://sbert.net/)

<p align="center">
    <img src="https://sbert.net/_static/logo.png" width=200 />
</p>

## Goals
1. To implement an maintain an open source Elasticsearch [IMemoryDB](https://github.com/microsoft/kernel-memory/blob/adce865a472728f2549428cf6b82ca79a601582b/service/Abstractions/MemoryStorage/IMemoryDb.cs#L9) connector for Kernel Memory.

    1. Free Mind Labs require such connector to complete [Videomatic](https://github.com/freemindlabsinc/videomatic).
        
    1. The basic connector (i.e. the complete implementation of IMemoryDb) will be free of charge and open source.

1. In the future we hope to add additional features (*e.g. advanced search options for pre and post filtering, analytics, ES-specific features, etc.**) that could generate some revenue to support this and other projects. 
    1. Patreon?
    1. GitHub donations?
    1. Other?

We'd love to hear what you think about this.

> Click on :notebook: [DIARY](DIARY.md) to read daily thoughts and what is happening behind the scenes.

## How to setup a running instance of Elasticsearch

You need to have [Docker](https://docs.docker.com/get-docker/) and [Docker Compose](https://docs.docker.com/compose/install/) installed before continuing.

To simplify the setup of a running instance of Elasticsearch we created the article [Installing the Elastic Stack using Docker Compose](docker/README.md) that guides you through the process. It is simple - *3 steps* - and it takes less than 5 minutes to complete.

The following diagram shows what we will be running once we complete the installation.

<div align="center">
    <img src="docker/images/ELKStack.png" width="500px"</img>
</div>




## The .NET Solution

This is a screenshot of the solution. 
We highlighted some of the most important files for you to explore.

<p align="center">
    <img src="content/images/Solution.jpg" width=500 />
</p>

Here are some screenshots of the tests included in the project. Look at the output window to see what they do.

<p align="center">
 <img src="/content/images/BehavesLike.jpg" width=500 />
</p>

Click [here](tests/UnitTests/MemoryStorage/MemoryStorageTests.cs) to see the source code of the test.

<p align="center">
 <img src="/content/images/CarbonBondTo.jpg" width=500 />
</p>

Click [here](tests/UnitTests/Serverless/ServerlessTest.cs) to see the source code of the test.

### Mappings
The examples use the OpenAI's text-embedding-ada-002.
It is possible to use any other embedding model supported by SK (e.g. Azure Open AI and Hugging Face).

<p align="center">
 <img src="/content/images/Mappings.jpg" width=900 />
</p>

### Kibana
Here are some screenshots of the data stored in ES, after running the tests in the solution.

<p align="center">
 <img src="/content/images/DataPageAllRows.jpg" width=600 />
</p>

<p align="center">
 <img src="/content/images/DataPage1.jpg" width=600 />
</p>

<p align="center">
 <img src="/content/images/DataPage2.jpg" width=600 />
</p>

### KNN Query
Here's an example of how to run semantic search directly on ES.

<p align="center">
 <img src="/content/images/KnnQuery.jpg" width=600 />
</p>



## Current status
1. The connector is currently in development and not ready for production use yet.
1. The connector is not yet available as a NuGet package.

## Timeline
1. We hope to complete this project and the associated documentation by the end of 2023.


## Challenges
The new API in the Elasticsearch client is not yet feature complete and there are bugs.

1. AutoMap(),etc. missing
    1. [CreateIndexDescriptor Mappings -> Map Api usage issue #7929](https://github.com/elastic/elasticsearch-net/issues/7929)
    1. [FEATURE - Support AutoMap to allow creation of mappings using type inference #6610](https://github.com/elastic/elasticsearch-net/issues/6610)
        1. flobernd comment on Aug 17 suggests this:
```
var mapResponse = client.Indices.PutMapping("index", x => x
    .Properties<Person>(p => p
        .DenseVector(x => x.Data, d => d
            .Index(true)
            .Similarity("dot_product"))));
```

## Resources

1. Elastic's official docs on the client.
    1. NEST 7.17: https://www.elastic.co/guide/en/elasticsearch/client/net-api/7.17/nest-getting-started.html
    1. New client 8.9: https://www.elastic.co/guide/en/elasticsearch/client/net-api/8.9/introduction.html
        1. This client is not yet feature complete.
            1. Look here for details: https://www.elastic.co/guide/en/elasticsearch/client/net-api/current/release-notes-8.0.0.html
        1. In addition, the docs are not up to date. For some stuff we need to lok at NEST's docs.

1. [Elasticsearch.net GitHub repository](https://github.com/elastic/elasticsearch-net)

1. Semantic Kernel/Memory-Kernel
    1. [Introduction to Semantic Memory (feat. Devis Lucato) | Semantic Kernel](https://www.youtube.com/watch?v=5JYW_uAxwYM)
    1. [11.29.2023 - Semantic Kernel Office Hours (US/Europe Region)](https://www.youtube.com/watch?v=JSca9mVUUJo)