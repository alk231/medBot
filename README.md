# 🏥 MedBot - AI-Powered Medical Chatbot

A Retrieval-Augmented Generation (RAG) based medical chatbot that provides accurate, context-aware responses to medical queries using PDF documents as knowledge sources.

## ✨ Features

- **RAG Architecture**: Combines document retrieval with LLM generation for accurate responses
- **Vector Database**: Uses Pinecone for efficient similarity search
- **Web Interface**: Clean Flask-based chat interface
- **Session Memory**: Maintains conversation history within sessions
- **PDF Knowledge Base**: Processes medical PDFs to build a comprehensive knowledge base
- **Groq Integration**: Leverages Groq's API with Llama3-70B for fast inference

## 🛠️ Tech Stack

- **Framework**: Flask
- **LLM**: Llama3-70B-8192 (via Groq API)
- **Vector Database**: Pinecone
- **Embeddings**: HuggingFace (sentence-transformers/all-MiniLM-L6-v2)
- **RAG Framework**: LangChain
- **Document Processing**: PyPDF, RecursiveCharacterTextSplitter

## 📁 Project Structure

```
medBot/
├── Medical_Chatbot/
│   ├── app.py                          # Main Flask application
│   ├── creat_memory_for_llm.py        # PDF ingestion & vectorization script
│   ├── connect_memory_with_llm.py     # Memory connection utilities
│   ├── requirements.txt                # Python dependencies
│   ├── data/                          # PDF documents folder
│   └── templates/
│       └── index.html                 # Chat UI
```

## 🚀 Getting Started

### Prerequisites

- Python 3.8+
- Pinecone account and API key
- Groq API key

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/alk231/medBot.git
   cd medBot/Medical_Chatbot
   ```

2. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

3. **Set up environment variables**
   
   Create a `.env` file in the `Medical_Chatbot` directory:
   ```env
   PINECONE_API_KEY=your_pinecone_api_key
   PINECONE_ENV=your_pinecone_environment
   PINECONE_INDEX=your_index_name
   GROQ_API_KEY=your_groq_api_key
   ```

4. **Prepare your data**
   
   Place your medical PDF documents in the `data/` folder.

5. **Create vector embeddings**
   ```bash
   python creat_memory_for_llm.py
   ```
   This will:
   - Load all PDFs from the `data/` directory
   - Split documents into 500-character chunks with 50-character overlap
   - Generate embeddings using HuggingFace model
   - Upload to Pinecone index

6. **Run the application**
   ```bash
   python app.py
   ```
   Access the chatbot at `http://localhost:5000`

## 💡 How It Works

### Document Ingestion Pipeline

1. **Load PDFs**: Reads all PDF files from the `data/` directory
2. **Text Splitting**: Chunks documents into 500-character segments with 50-character overlap
3. **Embedding Generation**: Converts chunks to 384-dimensional vectors using sentence-transformers
4. **Vector Storage**: Stores embeddings in Pinecone for efficient retrieval

### Query Processing Flow

1. User submits a medical question via the web interface
2. Question is embedded using the same HuggingFace model
3. Pinecone retrieves the most relevant document chunks
4. Retrieved context + question sent to Llama3-70B via Groq
5. LLM generates a grounded response based on the context
6. Response displayed in the chat interface with session history

## 🎯 Key Components

### Custom Prompt Template

The chatbot uses a carefully designed prompt that:
- Restricts responses to provided context only
- Prevents hallucination by returning "I don't know" when information isn't available
- Delivers concise, natural responses without mentioning sources

### Retrieval Chain

- **Chain Type**: "stuff" (all retrieved docs passed to LLM)
- **Retriever**: Pinecone vector store retriever
- **LLM**: Llama3-70B-8192 with 8192 token context window
- **Return Source Documents**: Enabled for transparency

## 🔧 Configuration

### Chunking Parameters

Modify in `creat_memory_for_llm.py`:
```python
chunk_size=500      # Characters per chunk
chunk_overlap=50    # Overlap between chunks
```

### LLM Parameters

Modify in `app.py`:
```python
model="llama3-70b-8192"  # Change model
# Add temperature, max_tokens, etc.
```

## 📝 Usage Examples

**Example Query**: "What are the symptoms of diabetes?"

The chatbot will:
1. Search the vector database for relevant medical information
2. Retrieve the most similar document chunks
3. Generate a response grounded in the retrieved context

## 🌐 API Endpoints

- `GET /` - Home page with chat interface
- `POST /ask` - Submit a question and get response
- `GET /clear` - Clear conversation history

## 🚧 Future Enhancements

- [ ] Add user authentication
- [ ] Implement multi-turn conversation memory
- [ ] Support multiple file formats (DOCX, TXT)
- [ ] Add citation/source display for responses
- [ ] Implement semantic caching for faster responses
- [ ] Add conversation export functionality

## 📄 License

This project is open-source and available for educational purposes.

## 🤝 Contributing

Contributions are welcome! Feel free to:
- Report bugs
- Suggest new features
- Submit pull requests

## 📧 Contact

For questions or feedback, open an issue on this repository.

---

**Built with ❤️ using LangChain, Pinecone, and Groq**