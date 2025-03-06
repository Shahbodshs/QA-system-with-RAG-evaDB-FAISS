# Question & Answer system with RAG 


This repository implements a **Question Answering (QA) system** using **Retrieval-Augmented Generation (RAG)** with **FAISS** and **EVA-DB** for efficient retrieval and document indexing.

## 🔹 How it Works

1. **User Question**: The system receives a query.
2. **Retrieval Phase**: It searches for relevant documents using **FAISS** vector indexing.
3. **Augmentation**: The retrieved documents are added as context.
4. **LLM Processing**: The LLM generates a response using the augmented data.


## 🔹 Components

- **LLM**: A large language model (e.g., GPT-4) for text generation.
- **Vector Database**: FAISS for efficient similarity search.
- **EVA-DB**: For structured retrieval from databases.

![QA System Architecture]([images/qa_architecture.png](Images/_gcW4KUYDWgbeDOFKxqOx.png))

