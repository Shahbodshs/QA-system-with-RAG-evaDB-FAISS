# QA System with RAG using EVA-DB, FAISS, and Gemini

This repository implements a **Question Answering (QA) system** using **Retrieval-Augmented Generation (RAG)**, leveraging **EVA-DB** for structured retrieval, **FAISS** for vector search, and **Gemini** for LLM-based reasoning. 

## 🔹 How It Works

The system follows these steps:

1. **User Query**: The system receives a question from the user.
2. **Subquestion Breakdown**: A complex question is broken down into **simpler subquestions** using a few-shot learning approach.
3. **Vector Retrieval**: Relevant documents are retrieved using FAISS-based **vector similarity search**.
4. **LLM Processing**: The final response is generated using **Google Gemini**.



---

## 🔹 Database & Embeddings

The **EVA-DB** is used as the main storage system for the QA pipeline. The key components of the database setup include:

- **Embedding Model**: `SentenceFeatureExtractor` is used to generate embeddings for the documents.
- **Vector Storage**: FAISS is used to create **vector indices** for similarity search.
- **Data Source**: Wikipedia API is used to fetch documents.

Example of how documents are loaded:

```python
doc_names = ["Toronto", "Chicago", "Houston", "Boston", "Atlanta"]
wiki_docs = load_wiki_pages(page_titles=doc_names)
```

---
Once stored in EVA-DB, the documents are retrieved based on semantic similarity using:
```python 
SELECT * FROM documents
ORDER BY Similarity(SentenceFeatureExtractor('User Query'), embedding)
LIMIT 5;
```

## 🔹 Subquestion Generation with Few-Shot Learning

Since complex questions may require multi-step reasoning, the system automatically breaks down a question into simpler subquestions. This allows:
- **Vector Retrieval** for fact-based questions like "What is the population of Toronto?"
- **LLM Retrieval** for summarization-based queries like "Summarize the history of Chicago."
A structured prompt is used to generate subquestions:

You are an AI assistant that specializes in breaking down complex questions into simpler, manageable sub-questions.
You have at your disposal a pre-defined set of functions and files to utilize in answering each sub-question.
Each sub-question should be a full question that can be answered using a single function and a single file.

---

The system dynamically generates subquestions, linking them to the correct retrieval function:

```python
{
    "subquestion_bundle_list": [
        {
            "question": "What is the population of Atlanta?",
            "function": "vector_retrieval",
            "file_name": "Atlanta"
        },
        {
            "question": "Summarize the history of Chicago?",
            "function": "llm_retrieval",
            "file_name": "Chicago"
        }
    ]
}
``` 

## 🔹 Google Gemini for LLM Processing (Free API)

Google Gemini-1.5-flash is used to generate responses while keeping the project cost-free. The system retrieves relevant document chunks and appends them as context before making an API call to Gemini:

```python
response = free_llm_call(
    model="gemini-1.5-flash",
    system_prompt=system_prompt,
    user_prompt="What is the population of Toronto?"
)
```
By combining EVA-DB vector search, subquestion generation, and LLM-based summarization, this system enhances accuracy while ensuring efficient retrieval-augmented generation.


## Appreciation 🙌

This project is inspired by the original work from [pchunduri6/rag-demystified](https://github.com/pchunduri6/rag-demystified). The original implementation used GPT-4 API, which required a premium subscription. To make the system accessible for free, I adapted the approach by integrating **Google Gemini-1.5-flash** while maintaining the core Retrieval-Augmented Generation (RAG) workflow.

The modifications include:
- **Replacing GPT-4 API with Gemini** to provide free access to LLM-based responses.


Credit goes to the original authors for their excellent work in designing an advanced RAG pipeline. This project builds upon their foundation while making it more accessible for users who want a free alternative.


