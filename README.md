# Multimodel-RAG-with-Langchain

# Problem Statement
To develop an intelligent AI-powered assistant that can understand complete internal deployment-related PDF documents, including older RFC documents. The system should be able to process written content, structured tables, and images with valid content, and provide quick and accurate answers through natural language queries, reducing manual document review effort.


## 1. Environment Setup

### Installed required libraries:

LangChain

LangChain OpenAI

LangChain Community

Chroma (vector database)

Unstructured (for PDF parsing)

--> Unzipped the RFC sample documents.

## 2. Load and Parse Documents

Used DirectoryLoader and Unstructured to load RFC PDF documents.

The parser automatically separated content into:

Text

Tables

Images

Each document chunk had metadata like category (Text/Table).

## 3. Separate Text and Tables

Filtered documents based on metadata:

Extracted all text sections

Extracted all tables separately

Converted tables into readable markdown format using htmltabletomd.

This ensured tables were not ignored.

## 4. Extract and Process Images

Extracted images from PDFs.

Converted images to Base64 format.

Sent images to a multimodal LLM.

Generated image summaries (so images could be searchable as text).

## 5. Generate Summaries for All Content

To make retrieval efficient:

Created summaries for:

Text chunks

Tables

Images

Used an LLM (AzureChatOpenAI / GPT-4o mini) to generate concise summaries.

Now everything (text, tables, images) was converted into searchable textual summaries.

## 6. Generate Embeddings

Used AzureOpenAIEmbeddings model(text-embedding-3-small) to generate vector embeddings.

Created embeddings for all summaries.

Stored them in a Chroma vector database.

## 7. Multi-Vector Retrieval Setup

Used MultiVectorRetriever.

Stored:

Summary embeddings (for search)

Original content (for final answer generation)

This ensures accurate retrieval while keeping original rich content.

## 8. Query Handling

When a user asks a question:

Convert query into embedding.

Retrieve most relevant summaries from vector DB.

Fetch original content (text/table/image).

Separate image and text data if needed.

## 9. Final Answer Generation (RAG Chain)

Built a custom multimodal_rag_qa() function.

Passed:

User question

Retrieved content (text + tables + image summaries)

Sent everything to the LLM.

Generated final context-aware answer.

## Overall Flow

PDF → Extract Text/Tables/Images → Summarize → Generate Embeddings → Store in Vector DB → Retrieve Relevant Content → Send to LLM → Generate Answer

## What This Shows Technically

* End-to-end RAG pipeline implementation

* Multimodal document handling

* Vector database integration

* LangChain retriever customization

* LLM-based summarization + QA
