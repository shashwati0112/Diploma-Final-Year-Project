# RAG Implementation - Setup & Usage Instructions

## Overview
This project implements a **Retrieval-Augmented Generation (RAG)** system for Java code analysis. It uses:
- **LangChain** for document processing and retrieval
- **FAISS** for vector storage and similarity search
- **Ollama** for embeddings
- **Google Gemini API** for response generation
- **Gradio** for the web UI

---

## Prerequisites

### System Requirements
- Python 3.10 or higher
- Windows/Linux/macOS
- Internet connection (for Gemini API)
- At least 4GB RAM

### Required Software
- **Ollama**: Download from [https://ollama.ai](https://ollama.ai)
- **Python**: Download from [https://www.python.org](https://www.python.org)

---

## Installation Steps

### 1. Set Up Python Environment
```bash
# Create a virtual environment
python -m venv venv

# Activate it
# On Windows:
venv\Scripts\activate
# On Linux/macOS:
source venv/bin/activate
```

### 2. Install Dependencies
```bash
pip install -r requirement.txt
```

Or install manually:
```bash
pip install langchain langchain-community langchain-core
pip install faiss-cpu
pip install gradio
pip install requests
pip install google-generativeai
```

### 3. Set Up Ollama
```bash
# Pull the embedding model
ollama pull mxbai-embed-large

# Verify Ollama is running locally (default: http://localhost:11434)
```

### 4. Configure Gemini API
1. **Get API Key**:
   - Go to [Google AI Studio](https://makersuite.google.com/app/apikey)
   - Create a new API key
   - Copy the key

2. **Set Environment Variable**:
   ```bash
   # On Windows (PowerShell):
   $env:GEMINI_API_KEY = "your-api-key-here"
   
   # On Windows (Command Prompt):
   set GEMINI_API_KEY=your-api-key-here
   
   # On Linux/macOS:
   export GEMINI_API_KEY="your-api-key-here"
   ```

3. **Or update in code** (line in cell 3):
   ```python
   API_KEY = "your-api-key-here"
   ```

---

## Project Structure

```
Capstone_Project/
├── RAG_Implimentation.ipynb    # Main notebook with RAG pipeline
├── requirement.txt              # Python dependencies
├── README.md                    # Project description
├── Group1/, Group2/, Group3/   # Java code directories
├── faiss_index_folder/          # Vector store (cached)
├── chroma_db/                   # Alternative vector store
└── output/                      # Response outputs
```

---

## Running the Application

### Step 1: Start Ollama
```bash
ollama serve
```
Keep this terminal open. Ollama will run on `http://localhost:11434`.

### Step 2: Run the Notebook
1. Open `RAG_Implimentation.ipynb` in Jupyter or VS Code
2. Run cells in order:
   - Cell 1: Check Python version
   - Cell 2: Install dependencies
   - Cell 3: Import FAISS
   - Cell 6-7: Pull embedding model
   - Cell 8: Import libraries
   - Cell 9: Load Java code files
   - Cell 11: Create embeddings
   - Cell 15: Load/create vector store
   - Cell 18-19: Set up RAG prompt and retriever
   - Cell 20: Define response generation function
   - Cell 21: Set up Gradio UI
   - **Cell 22**: Launch the web interface

### Step 3: Access the Web UI
Once you run the final cell, Gradio will provide a **local URL** (typically `http://localhost:7860`).

Open this URL in your browser to interact with the RAG system.

---

## Common Errors & Solutions

### ❌ Error: `Max retries exceeded` / DNS Resolution Failed

**Cause**: Network connectivity issue or DNS cannot resolve `generativelanguage.googleapis.com`

**Solutions**:
1. **Check Internet Connection**
   - Verify your internet is working
   - Try pinging Google: `ping google.com`

2. **Check Firewall/Proxy**
   - Firewall might be blocking API calls
   - Check corporate proxy settings if on company network

3. **Check API Key**
   - Ensure `GEMINI_API_KEY` is set correctly
   - No extra spaces or quotes around the key

4. **Verify Endpoint**
   - URL should be: `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash:generateContent`

5. **Restart & Wait**
   - Restart your terminal and notebook kernel
   - Wait 1-2 minutes before retrying

---

### ❌ Error: `ConnectionRefusedError` for Ollama

**Cause**: Ollama is not running

**Solutions**:
1. Start Ollama: `ollama serve`
2. Verify it's running: `curl http://localhost:11434`
3. Check model is available: `ollama list`

---

### ❌ Error: `FAISS.load_local()` fails

**Cause**: First run - no cached index yet

**Solutions**:
1. The system will create a new FAISS index automatically
2. First run will take longer (5-10 minutes)
3. Subsequent runs will load the cached index (faster)

---

### ❌ Error: `No Java files found`

**Cause**: Java files not in expected location

**Solutions**:
1. Verify Java files are in `Group1/`, `Group2/`, `Group3/` directories
2. Check file extensions are `.java`
3. Update the path in Cell 9 if needed:
   ```python
   code_doc = code_reader_func(r"C:\your\path\here")
   ```

---

## Usage Guide

### Asking Questions
In the Gradio UI, type your query:

**Examples**:
- "What does the `main` method do?"
- "Debug this error: NullPointerException at line 45"
- "Explain the class structure"
- "What are the dependencies in this project?"

### Response Format
- Responses include relevant code context
- Java code is formatted with syntax highlighting
- Debugging suggestions are provided when applicable

---

## Performance Tips

1. **First Load**: Takes 5-15 minutes (building embeddings)
2. **Subsequent Loads**: 30-60 seconds (loading cached index)
3. **Query Response**: 10-30 seconds (API call + retrieval)

### To Speed Up:
- Add more server RAM
- Reduce chunk size (in Cell 11): `chunk_size=1000`
- Use fewer documents for testing

---

## File Descriptions

| File | Purpose |
|------|---------|
| `RAG_Implimentation.ipynb` | Main implementation notebook |
| `faiss_index_folder/index.faiss` | Cached vector index |
| `requirement.txt` | Python package dependencies |
| `Group1-3/` | Source Java code to analyze |
| `output/` | Generated responses (if batch processing) |
| `responses/` | API responses (cached) |

---

## Troubleshooting Checklist

- [ ] Python 3.10+ installed
- [ ] Virtual environment activated
- [ ] Dependencies installed (`pip install -r requirement.txt`)
- [ ] Ollama running (`ollama serve` in separate terminal)
- [ ] Embedding model pulled (`ollama pull mxbai-embed-large`)
- [ ] Gemini API key set (environment variable or in code)
- [ ] Java files present in project directory
- [ ] Internet connection verified
- [ ] Firewall not blocking API calls

---

## Support & Debugging

### Enable Debug Mode
Add to any cell to see detailed logs:
```python
import logging
logging.basicConfig(level=logging.DEBUG)
```

### Check Ollama Status
```bash
curl http://localhost:11434/api/tags
```

### Verify Gemini API
```python
import requests
url = f"https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash:generateContent?key={API_KEY}"
response = requests.post(url, json={"contents": [{"parts": [{"text": "test"}]}]})
print(response.status_code)
```

---

## Next Steps

1. Customize the RAG prompt in **Cell 18** for specific use cases
2. Adjust chunk size and overlap in **Cell 11** for better results
3. Add more Java files to the `Group` directories
4. Fine-tune the retriever `k` parameter for more/fewer results

---

**Last Updated**: April 2026
