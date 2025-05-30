🤖 Multi-Agent Financial Voice Assistant

🌐 Project Summary

The Multi-Agent Financial Assistant is an intelligent, voice-enabled application designed to deliver real-time market summaries and insights through a modular, microservices architecture. Leveraging the power of modern LLMs, vector databases, and voice pipelines, this system can:

Ingest financial documents

Answer questions about market trends

Deliver audio-based responses using speech synthesis

Provide a Streamlit-based interactive interface for user interaction

This project showcases a complete multi-agent architecture involving data ingestion, retrieval, natural language understanding, and voice interaction.

🚀 Features

Real-time market data via APIs (Yahoo Finance / Alpha Vantage)

Document ingestion & semantic search (FAISS)

Natural language summarization via LLMs (LLaMA-3 70B on Groq)

STT and TTS voice pipelines (Whisper, Coqui, or equivalent)

Modular FastAPI microservices

User-friendly Streamlit interface

⚖️ Workflow Architecture

  [User Input] (Voice/Text)
        ↓
  [Streamlit Interface (Frontend)]
        ↓
  [Voice Agent (STT → Text)]
        ↓
  [Orchestrator]
    ├──→ Retriever Agent (FAISS Index Search)
    ├──→ Language Agent (LLM Summarization)
    └──→ Voice Agent (TTS → Audio Response)
        ↓
     [Streamlit Plays Response (Text/Audio)]

🚧 Agents Overview

✉️ API Agent

Role: Fetches real-time and historical financial data from Yahoo Finance and Alpha Vantage.

Endpoints: /get_stock_data, /get_index_data

🔍 Scraping Agent

Role: Extracts textual data from HTML filings using BeautifulSoup or LangChain loaders.

Endpoints: /scrape_url, /extract_filings

🔀 Retriever Agent

Role:

Ingests .txt files

Chunks and embeds content using Sentence Transformers

Stores embeddings in FAISS index

Returns top-k matched documents for a given query

Endpoints: /ingest, /query

🧠 Language Agent

Role:

Loads the FAISS index

Retrieves relevant chunks

Synthesizes a contextual answer using a Groq-hosted LLaMA 3 LLM via LangChain

LangChain Stack: PromptTemplate | ChatGroq | RunnableSequence

Endpoints: /summarize

🎤 Voice Agent

Role:

Accepts .wav file

Performs STT via Whisper

Sends transcript to Orchestrator

Converts LLM answer to audio via TTS

Endpoints: /voice

⚖️ Orchestrator

Role:

Acts as central hub between agents

Sends query to Retriever, then Language Agent, then Voice Agent

Fallbacks when confidence is low or no match

Logic:

Confidence = match count heuristic (configurable)

Handles response packaging (text + audio)

Endpoints: /orchestrate

📚 Streamlit App

Role: User Interface

Capabilities:

Input text or upload .wav file

Displays response from Orchestrator (text)

Plays audio (if generated)

Port: Default 8501

🔹 How to Run (Locally)

1. Clone & Install

git clone https://github.com/yourname/finance_assistant.git
cd finance_assistant
pip install -r requirements.txt

2. Run Agents

uvicorn agents.api_agent.main:app --port 8001
uvicorn agents.scraping_agent.main:app --port 8002
uvicorn agents.retriever_agent.main:app --port 8003
uvicorn agents.language_agent.main:app --port 8004
uvicorn agents.voice_agent.main:app --port 8005
uvicorn orchestrator.main:app --port 8006

3. Run Streamlit UI

streamlit run streamlit_app/main.py

🌐 Deployment

Note: Due to large files, model binaries are hosted externally.

FAISS indices and models stored in: Google Drive Link

Streamlit UI can be deployed on Streamlit Cloud or via Docker

📊 Performance Benchmarks

Module

Latency (avg)

Model Size

API Call Time

STT (Whisper)

~2 sec

500MB

Local

FAISS Query

<0.1 sec

10-100MB

Local

LLM (Groq)

~0.2 sec

N/A

Remote

TTS

~1.5 sec

400MB

Local

📃 Deliverables Summary

Folder

Purpose

/agents

All microservices for each agent

/streamlit_app

Frontend UI

/orchestrator

Agent router

/docs

Architecture, usage logs

/data_ingestion

Loaders, index builders

📜 Documentation Files

README.md: This file (project overview)

docs/ai_tool_usage.md: AI model logs, prompts, params

docs/architecture.png: Visual diagram of architecture

Dockerfile (Optional): Containerization setup

🚀 Acknowledgments

OpenAI Whisper

HuggingFace Sentence Transformers

LangChain + Groq LLM

FAISS Vector Search

Streamlit

FastAPI# Finance_Assistant
