# 🤖 LibreChat: Linux Philosophy Chatbot

## 📖 Overview
**LibreChat** is a Retrieval-Augmented Generation (RAG) chatbot designed to answer questions about Linux, free software, and influential figures like Linus Torvalds and Richard Stallman. It leverages diverse data sources, including a PDF book, web content, Wikipedia pages, and HTML files from stallman.org, to provide concise, accurate responses in Persian (up to 4 words). Built using **LangChain** and **Cohere**, LibreChat processes documents, embeds them in a vector store, and retrieves relevant information to answer 16 predefined questions for automated evaluation.

## 🎯 Objectives
- **Build a RAG Chatbot**: Implement a retrieval-augmented system to answer questions based on provided data sources.
- **Process Diverse Data**: Load and preprocess PDF, web, Wikipedia, and HTML content into unified documents.
- **Ensure Concise Responses**: Generate short (≤4 words) Persian answers in a structured JSON format.
- **Achieve High Accuracy**: Correctly answer at least 12 out of 16 evaluation questions to meet project requirements.

## ✨ Features
- **Multi-Source Data Handling**:
  - 📄 **PDF**: Extracts text from "Just for Fun" (Persian translation) using `PyPDFium2Loader`.
  - 🌐 **Web**: Scrapes "Linux and Life" from linuxbook.ir using `WebBaseLoader`.
  - 📚 **Wikipedia**: Loads Persian pages on Linux, free software, and key figures with `WikipediaLoader`.
  - 🖥️ **HTML**: Processes stallman.org pages using `DirectoryLoader` and `BSHTMLLoader`.
- **RAG Architecture**:
  - Splits documents into chunks (`RecursiveCharacterTextSplitter`).
  - Embeds using Cohere’s multilingual model (`embed-multilingual-light-v3.0`).
  - Stores embeddings in a Chroma vector store for semantic search.
  - Uses Cohere reranking (`rerank-multilingual-v3.0`) for improved retrieval.
- **Structured Output**: Returns JSON dictionaries with `question_number` (int) and `answer` (str) for evaluation.
- **Few-Shot Prompting**: Guides the model with example questions and answers to ensure format compliance.

## 🛠 Prerequisites
To run this project, you need:
- **Python** 3.9 or higher.
- **Libraries**:
  - `langchain`, `langchain-community`, `langchain-cohere`, `langchain-chroma`
  - `pypdfium2`, `pandas`, `beautifulsoup4`
- **Cohere API Key**: Obtain from [Cohere](https://cohere.ai/) and set as `COHERE_API_KEY`.
- **Dataset Files** (in `data` folder):
  - `justforfun_persian.pdf`
  - `html/` (containing stallman.org HTML files)
- **Environment**: Google Colab with GPU recommended for Hugging Face models (optional).

## 📦 Installation
1. Clone or download the repository:
   ```bash
   git clone <repository-url>
   ```
2. Navigate to the project directory:
   ```bash
   cd <project-directory>
   ```
3. Install required libraries:
   ```bash
   pip install langchain langchain-community langchain-cohere langchain-chroma pypdfium2 pandas beautifulsoup4
   ```
4. Set up the Cohere API key:
   ```python
   import os
   os.environ["COHERE_API_KEY"] = "your-api-key"
   ```
5. Ensure dataset files are in the `data` folder.

## 🚀 Usage
1. Open `librechat.ipynb` in Jupyter Notebook or Google Colab.
2. Run cells sequentially to:
   - Load and preprocess data (PDF, web, Wikipedia, HTML).
   - Split documents into chunks and embed them in a Chroma vector store.
   - Set up the RAG chain with Cohere LLM and reranker.
   - Answer 16 evaluation questions and save responses.
3. Generate the submission file:
   ```bash
   python -m notebook librechat.ipynb
   ```
4. Output:
   - `result.zip` containing:
     - `librechat.ipynb`
     - `answers.json` (16 JSON dictionaries with question numbers and answers)

## 📊 Code Structure
- **Data Loading**:
  - Loads PDF (`PyPDFium2Loader`), web (`WebBaseLoader`), Wikipedia (`WikipediaLoader`), and HTML (`DirectoryLoader` with `BSHTMLLoader`).
  - Saves preprocessed documents to `processed_docs.json`.
- **Document Splitting**:
  - Uses `RecursiveCharacterTextSplitter` (chunk size: 500, overlap: 50).
  - Saves chunks to `splitted_docs.json`.
- **Vector Store**:
  - Embeds documents with `CohereEmbeddings` (`embed-multilingual-light-v3.0`).
  - Stores in Chroma (`persist_directory="./vec_store_chroma"`).
- **RAG Chain**:
  - Retrieves documents with `vector_store.as_retriever` (k=50).
  - Reranks with `CohereRerank` (top_n=20).
  - Generates answers using `ChatCohere` with a custom prompt template.
  - Outputs JSON dictionaries via `rag_chain` function.
- **Submission**:
  - Combines answers (`answer1` to `answer16`) into `answers.json`.
  - Zips with notebook into `result.zip`.

## 🔍 Evaluation
- **Questions**: 16 predefined questions (e.g., "پرسش ۴: چه کسی بنیاد نرم‌افزارهای آزاد را بنا نهاد؟").
- **Output Format**: Each answer is a JSON dictionary:
  ```json
  {"question_number": 4, "answer": "ریچارد استالمن"}
  ```
- **Constraints**:
  - Answers must be ≤4 words in Persian.
  - At least 12 correct answers required for success.

## 🎓 Credits
Developed as part of a Quera academic project, leveraging LangChain and Cohere for RAG-based chatbot implementation.
