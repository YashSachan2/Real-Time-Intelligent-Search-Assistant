# 🧠 Real-Time Intelligent Search Assistant

A high-performance **Retrieval-Augmented Generation (RAG)** chatbot capable of conducting real-time web research to answer user queries. Unlike static RAG systems, this project dynamically searches the web using the Serper Dev API, processes live content, and utilizes Groq's fast inference engine to provide up-to-date answers with source citations.

---

## 🚀 Key Features

* **Real-Time Web Search**: Integrates **Serper Dev API** to perform live Google searches for up-to-the-minute information (e.g., "Who won the 2024 T20 World Cup?").
* **High-Speed Inference**: Utilizes **Groq's LPU** (Language Processing Unit) for near-instantaneous LLM generation.
* **Dynamic RAG Pipeline**:
    * **Search**: Fetches relevant URLs based on the user query.
    * **Scrape & Load**: Extracts text content from search results using `WebBaseLoader`.
    * **Chunking**: Splits text into manageable segments using `RecursiveCharacterTextSplitter`.
    * **Vectorization**: Embeds text segments into a **FAISS** vector store using HuggingFace embeddings.
    * **Retrieval**: Finds the most relevant chunks to answer the question.
* **Performance Monitoring**: Logs detailed latency metrics for every stage (Search, Content Fetch, Embedding, LLM Inference).
* **Retriever Evaluation**: Includes a built-in evaluation module to benchmark the accuracy of the retrieval system against expected source URLs.

---

## 🛠️ Tech Stack

* **Language**: Python 3
* **Orchestration**: [LangChain](https://www.langchain.com/) (RetrievalQA Chain)
* **LLM Engine**: [Groq](https://groq.com/) (via `GROQ_API_KEY`)
* **Search Engine**: [Serper.dev](https://serper.dev/) (Google Search API via `SERPER_API_KEY`)
* **Vector Store**: FAISS (Facebook AI Similarity Search)
* **Embeddings**: HuggingFace (`sentence-transformers`)

---

## ⚙️ Installation & Setup

1.  **Clone the Repository**
    ```bash
    git clone [https://github.com/yourusername/Real-Time-Question-Answering-System.git](https://github.com/yourusername/Real-Time-Question-Answering-System.git)
    cd Real-Time-Question-Answering-System
    ```

2.  **Install Dependencies**
    Ensure you have the required Python libraries installed:
    ```bash
    pip install langchain langchain-groq faiss-cpu google-search-results sentence-transformers numpy pandas
    ```

3.  **Configure API Keys**
    You need API keys for Groq (LLM) and Serper (Search). You can set them in your environment variables or directly in the notebook execution block:
    ```python
    import os
    os.environ["SERPER_API_KEY"] = "your_serper_api_key"
    os.environ["GROQ_API_KEY"] = "your_groq_api_key"
    ```

---

## 📊 Performance Metrics

The system automatically logs execution time for each component of the RAG pipeline to help identify bottlenecks:

| Component | Metric Captured |
| :--- | :--- |
| **Search** | Time taken to query Serper API and retrieve URLs. |
| **Content Fetch** | Time taken to scrape and load data from the retrieved URLs. |
| **Text Split** | Time taken to chunk the loaded text. |
| **Embedding** | Time taken to generate embeddings and build the FAISS index. |
| **LLM Inference** | Time taken by Groq to generate the final answer. |

*Example Log Output:*
> `[Search] Time taken: 1.185s`
> `[Content Fetch] Time taken: 0.218s`
> `[LLM Inference] Time taken: 0.697s`
> `[Total Research Query] Total time: 2.968s`

---

## 🧪 Evaluation

The project includes a `Researcher` class with an `evaluate_retriever` method. This allows you to benchmark the retrieval accuracy using a labeled test dataset.

**Test Data Format:**
```python
TEST_DATA = [
    {
        "question": "Who won the 2024 T20 World Cup?",
        "expected_source": "[https://www.espncricinfo.com/](https://www.espncricinfo.com/)..."
    },
    {
        "question": "What are the features of the new iPhone 16?",
        "expected_source": "[https://www.apple.com/](https://www.apple.com/)..."
    }
]
