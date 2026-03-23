# Ad-Wise Agent

Ad-Wise Agent is an AI-powered advertising assistant that helps users generate optimized ad content and analyze campaign performance.  
The system combines **FastAPI**, **LLM-based generation**, and **retrieval-augmented generation (RAG)** over real product listing data to produce more relevant, benchmark-aware outputs.

---

## Overview

This project supports both **ad creation** and **performance analysis** in a single application.

It can:

- Generate a **full product ad listing**
- Generate a **headline only**
- Suggest **exactly 5 must-use keywords**
- Analyze ad performance metrics such as **CTR**, **ROI**, and **conversion rate**
- Retrieve inspiration examples using **Pinecone + embeddings**
- Fall back to a **local SQLite FTS database** if Pinecone is unavailable

The project also includes a simple **conversation wizard flow** and a static frontend.

---

## Main Features

- **FastAPI backend**
- **LLM-powered ad copy generation**
- **RAG retrieval pipeline** using Pinecone
- **Hugging Face embedding API** for query embeddings
- **Local SQLite FTS fallback** for retrieval
- **Ad performance analysis** against built-in benchmarks
- **Conversation manager** for guided user interaction
- **Static frontend UI**
- **Data preprocessing and Pinecone upload scripts**

---

## Supported Modes

### 1. Full Ad Generation
Generates:

- Headline
- 5 bullet points
- Short description
- Keywords
- Publishing tips

### 2. Headline Only
Returns a single optimized headline.

### 3. 5 Must-Use Keywords
Returns exactly 5 keywords or phrases suitable for inclusion in the headline.

### 4. Performance Analysis
Analyzes user-provided ad metrics such as:

- CTR
- ROI
- Conversion rate
- Impressions
- Clicks

The system compares these metrics to predefined benchmark values and returns actionable insights.

---

## Project Structure

```text
ad-wise-agent-main/
│
├── app/
│   ├── agent.py
│   ├── analyzer.py
│   ├── conversation_manager.py
│   ├── llm_client.py
│   ├── main.py
│   ├── retriever.py
│   └── settings.py
│
├── data/
│   ├── con_to_text.py
│   └── upload_data.py
│
├── static/
│   ├── architecture.png
│   └── index.html
│
├── requirements.txt
└── .gitignore
```

---

## How It Works

### Ad Generation Flow
1. The user sends a product request.
2. The system detects the requested mode.
3. A rewritten retrieval query is generated.
4. The retriever fetches relevant ad examples from Pinecone.
5. The examples are condensed into a context block.
6. The LLM generates the final ad output in the required format.
7. The system returns both the response and execution steps.

### Performance Analysis Flow
1. The user sends campaign metrics in natural language.
2. The analyzer extracts structured values.
3. The metrics are compared to built-in benchmarks.
4. The analysis is passed into the LLM context.
5. The final response includes interpretation and recommendations.

---

## Technologies Used

- **Python**
- **FastAPI**
- **Uvicorn / Gunicorn**
- **Pydantic**
- **Requests**
- **python-dotenv**
- **Sentence Transformers**
- **Pinecone**
- **Hugging Face Inference API**
- **SQLite FTS**

---

## Installation

Clone the repository and install dependencies:

```bash
git clone https://github.com/your-username/ad-wise-agent.git
cd ad-wise-agent
pip install -r requirements.txt
```

---

## Environment Variables

Create a `.env` file in the project root.

Example:

```env
# LLM
LLM_API_KEY=your_api_key
LLM_MODEL=reasoning
LLM_BASE_URL=https://api.llmod.ai

# Pinecone
PINECONE_API_KEY=your_pinecone_api_key
PINECONE_INDEX_NAME=amazon-ads-index
PINECONE_NAMESPACE=amazon_ads

# Embeddings
EMBED_MODEL_NAME=all-MiniLM-L6-v2
HF_TOKEN=your_huggingface_token

# Optional settings
MAX_PROMPT_CHARS=4000
MAX_CTX_CHARS=7000
TOP_K=5
MAX_ADS_PER_MATCH=25
ENABLE_REPAIR=false

# Optional local fallback
LOCAL_AMAZON_FTS_DB=data/amazon_ads_fts.sqlite
GLOBAL_ADS_BUDGET=50
RAG_KEYWORDS=
```

---

## Running the Application

Start the FastAPI server with:

```bash
uvicorn app.main:app --reload
```

Then open the app in your browser at:

```text
http://127.0.0.1:8000
```

---

## API Endpoints

### `POST /api/execute`
Main execution endpoint for ad generation or analysis.

Example request:

```json
{
  "prompt": "I'm selling a matte black insulated water bottle. Write a full ad listing."
}
```

---

### `POST /api/chat`
Wizard-style endpoint that manages guided user interaction and state.

---

### `GET /api/team_info`
Returns project team information.

---

### `GET /api/agent_info`
Returns the agent description, purpose, prompt template, and example usage.

---

## Retrieval System

The retriever supports two modes:

### Pinecone Retrieval
- Uses embeddings generated through the Hugging Face Inference API
- Queries a Pinecone index
- Supports optional category filtering
- Returns relevant ad inspiration examples

### Local Fallback
If Pinecone is unavailable, the system can search a local SQLite full-text search database.

This makes the project more robust during development and testing.

---

## Data Utilities

### `data/con_to_text.py`
Builds a single CSV file with:
- `category`
- `ads`

This script converts category-based source CSV files into one row per category with a combined ads field.

### `data/upload_data.py`
Processes the prepared CSV data, generates embeddings, chunks the ads, and uploads vectors into Pinecone.

---

## Output Format

Depending on the selected mode, the system returns one of the following:

### Full Ad
- Headline
- Bullets
- Short description
- Keywords
- Publishing tips

### Headline Only
- One line beginning with `Headline:`

### 5 Keywords
- One line beginning with `Keywords:`

### Analysis
- A free-form analytical response based on campaign metrics and benchmark comparison

---

## Benchmarks Used in Analysis

The built-in analysis currently compares user performance against default benchmark values:

- **CTR**: 0.045
- **ROI**: 3.2
- **Conversion Rate**: 0.07

These values can be extended or replaced in future versions.

---

## Example Use Cases

- Generate a full e-commerce listing for a new product
- Improve a weak product headline
- Identify 5 strong keywords for title optimization
- Review campaign performance against expected benchmarks
- Retrieve real listing inspiration from product-category data

---

## License

This project is intended for educational and academic use.
