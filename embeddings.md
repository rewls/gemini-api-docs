# Embeddings guide

- The embedding service in the Gemini API generates state-of-the-art embeddings for words, phrases, and sentences.

- This page describes what embeddings are and highlights some key use cases for the embedding service to help you get started.

## What are embeddings?

- Text embeddings are a natural language processing (NLP) technique that converts text into numerical vectors.

- Embeddings capture semantic meaning and context which results in text with similar meanings having closer embeddings.

- You can use these embeddings or vectors to compare different texts and understand how they relate.

## Use cases

- Information Retrieval

    - The goal is to retrieve semantically similar text given a piece of input text.

    - A variety of applications can be supported by an information retrieval system such as semantic search, answering questions, or summarization.

    - Refer to the document search notebook for an example.

- Vector DB

    - You can store you generated embeddings in a vector DB to improve the accuracy and efficiency of your NLP application.

    - Refer to https://cloud.google.com/alloydb/docs/ai/work-with-embeddings to learn how to use a vector DB to translate text prompts into numerical vectors.

## Elastic embeddings

- The Gemini Text Embedding model, starting with `text-embedding-004`, offers elastic embedding sizes under 768.

- You can use elastic embeddings to generate smaller output dimensions and potentially save computing and storage costs with minor performance loss.
