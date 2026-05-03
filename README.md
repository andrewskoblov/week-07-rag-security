# Week 7: RAG Security Knowledge Assistant
 
## 🤖 Live Chatbot
 
**[Click here to chat with the Security Knowledge Assistant](https://cloud.flowiseai.com/chatbot/81a833dd-d860-47b1-a62e-88d107b8e13e)**
 
---
 
## Project Overview
 
A Retrieval-Augmented Generation (RAG) chatbot built with Flowise and Groq that answers questions about fraud investigation procedures, SAR filing requirements, and MITRE ATT&CK cybersecurity techniques.
 
The chatbot uses document retrieval to ground its answers in uploaded knowledge base documents rather than relying solely on the LLM's training data.
 
---
 
## Tech Stack
 
| Component | Tool |
|-----------|------|
| LLM | llama-3.3-70b-versatile via Groq |
| Embeddings | sentence-transformers/all-MiniLM-L6-v2 via HuggingFace |
| Vector Store | In-Memory Vector Store |
| Pipeline | Flowise (Conversational Retrieval QA Chain) |
 
---
 
## Knowledge Base Documents
 
- `fraud-sar-filing-guidelines.txt` — SAR filing requirements, thresholds, and deadlines
- `fraud-red-flag-indicators.txt` — Money laundering and fraud red flag indicators
- `fraud-investigation-procedures.txt` — Fraud investigation workflow and case management
- `mitre-attack-techniques.txt` — MITRE ATT&CK Enterprise techniques (v19)
---
 
## Repository Structure
 
```
week-07/
├── screenshots/
│   ├── flowise-dashboard.jpg
│   ├── groq-test.jpg
│   ├── rag-chatflow.jpg
│   ├── rag-response-with-sources.jpg
│   └── chatbot-share-link.jpg
├── rag-documents/
│   ├── fraud-sar-filing-guidelines.txt
│   ├── fraud-red-flag-indicators.txt
│   ├── fraud-investigation-procedures.txt
│   └── mitre-attack-techniques.txt
└── week-07-report.md
```
 
---
 
## Evaluation Summary
 
All 5 test questions answered correctly using uploaded documents. Edge case questions ("What is the weather today?", "What are the latest CVEs from 2026?") returned "I'm not sure" — confirming RAG is properly constraining the LLM to the knowledge base.
 
See [week-07-report.md](week-07-report.md) for full evaluation results.
