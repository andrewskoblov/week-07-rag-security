> **Chatbot Share Link:** https://cloud.flowiseai.com/chatbot/81a833dd-d860-47b1-a62e-88d107b8e13e

# Week 7: RAG Security Knowledge Assistant — Evaluation Report

## 1. Setup Summary

- **LLM:** llama-3.3-70b-versatile via Groq
- **Embeddings:** sentence-transformers/all-MiniLM-L6-v2 via HuggingFace Inference API
- **Vector Store:** In-Memory Vector Store
- **Documents loaded:**
  - `fraud-sar-filing-guidelines.txt` (~2 pages) — SAR filing requirements and thresholds
  - `fraud-red-flag-indicators.txt` (~2 pages) — Common behavioral and transactional red flags
  - `fraud-investigation-procedures.txt` (~2 pages) — Step-by-step investigation workflow
  - `mitre-attack-techniques.txt` (~3 pages) — MITRE ATT&CK Enterprise techniques and sub-techniques

---

## 2. Test Results

| # | Question | Did it use your documents? (Yes/No) | Quality (Good / Partial / Wrong) | Notes |
|---|----------|--------------------------------------|----------------------------------|-------|
| 1 | What triggers a Suspicious Activity Report and what are the filing deadlines? | Yes | Good | Correctly cited BSA categories, 30-day filing deadline, and 60-day maximum from the SAR guidelines doc |
| 2 | What are common red flags for money laundering or structuring? | Yes | Good | Retrieved 10 specific indicators including structuring, layering, third-party cash deposits, and unusual wire activity |
| 3 | What are the steps in a fraud investigation from case initiation to closure? | Yes | Good | Outlined all 13 steps accurately from the fraud investigation procedures document |
| 4 | What is the difference between spearphishing attachment and spearphishing link according to MITRE ATT&CK? | Yes | Good | Correctly distinguished T1566.001 (malicious attachment) vs T1566.002 (malicious link) with accurate descriptions |
| 5 | What techniques do adversaries use for credential access? | Yes | Good | Retrieved T1003, T1555, T1110, T1078, T1134 with accurate descriptions directly from the MITRE document |

---

## 3. Edge Case Observations

- **Unrelated question ("What is the weather like today?"):** The chatbot responded "Hmm, I'm not sure" — it did not hallucinate or make up an answer. This confirms RAG is correctly constraining the LLM to only answer based on the uploaded documents.
- **Topic not in documents ("What are the latest CVEs from 2026?"):** The chatbot responded "Hmm, I'm not sure" rather than fabricating an answer. This is correct behavior — the LLM refused to answer because the information was not present in any of the retrieved document chunks.

---

## 4. Settings Experiments

- **Temperature change (0.3 → 0.7):** At higher temperature, answers became noticeably more verbose and occasionally rephrased the same point multiple ways. For a compliance-focused knowledge assistant, 0.3 is clearly better — precision matters more than variety.
- **Chunk size change (1000 → 500):** Smaller chunks produced more targeted retrieval — source documents shown were more directly relevant to the question. However, some answers lost surrounding context and felt incomplete. 1000 remains the better default for these document types.
- **Top K change (4 → 6):** Increasing Top K gave the LLM more context to work with, which improved answers to broader questions like "what are the steps in a fraud investigation." For narrow questions (specific SAR deadlines), the extra chunks added noise without improving accuracy.

---

## 5. Reflection

**What surprised you about how RAG works?**
The most surprising part was how much the chunk size affects answer quality. I expected the LLM to be the main variable, but the retrieval layer turned out to be just as important — the model can only be as good as the chunks it receives. Seeing source documents cited below each answer made it clear exactly why some responses were better than others, which made troubleshooting intuitive.

**How could you improve this chatbot for real-world use?**
The biggest improvement would be switching from an in-memory vector store to a persistent one like Pinecone or Chroma so documents don't need to be re-embedded on every load. Additionally, the knowledge base could be expanded with actual FinCEN guidance documents, BSA compliance manuals, and case study examples to improve coverage. Adding metadata filtering (e.g., by document type or jurisdiction) would also help with precision on specific regulatory questions.

**How might you use RAG in your capstone project?**
A RAG pipeline could power a compliance assistant that helps analysts quickly query internal policy documents, SAR filing procedures, and regulatory updates without manually searching through PDFs. Rather than memorizing every rule, an investigator could ask natural language questions and get sourced, document-backed answers — reducing lookup time and the chance of missing a filing requirement.
