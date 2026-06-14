# 📊 Automated Trend Intelligence & Sentiment Pipeline

An autonomous, production-grade ETL and AI data pipeline built using **n8n**, **LangChain**, **Groq (Llama-3.1-8b)**, and **Supabase**. This system monitors high-velocity Google Trends, structurally normalizes raw XML data, runs real-time sentiment and categorical analysis via LLMs, logs records securely into a cloud database, and dispatches real-time structured alerts via Telegram.

---

## 🚀 Pipeline Features

- **Automated RSS Fetching & Normalization:** Continuously monitors Google Trends RSS feeds, transforms raw XML payloads into optimized JSON objects, and unpacks batches dynamically using sequential data splitting.
- **High-Performance LLM Synthesis:** Leverages **Groq's Llama-3.1-8b-instant** model to execute zero-shot sentiment parsing (`Positive`, `Negative`, `Neutral`) and domain categorization without prompt-leaking or markdown code blocks.
- **Built-in Rate Limiting & Throttling:** Utilizes an integrated asynchronous `Wait` node buffer (13 seconds) to handle asynchronous scaling and safely stay within free-tier Groq API RPM/TPM thresholds.
- **Defensive JavaScript Data Extraction:** Features a post-LLM sanitization layer that handles edge-case string formatting, ensuring 100% resilient `JSON.parse()` consistency before hitting the data persistence layer.
- **Cloud Persistence & Alerting:** Automates structured relational data logging into **Supabase** while routing localized execution summaries directly to a dedicated Telegram interface (`NaimTrendBot`).

---

## 🛠️ Workflow Architecture Diagram

As visible on the n8n execution canvas:

```text
[Manual Trigger] ➔ [Fetch Trends (HTTP)] ➔ [XML Parser] ➔ [Split Out] 
                                                               ⬇
[Telegram Alert] ⬅ [Supabase Insert] ⬅ [JS Parser] ⬅ [Basic LLM Chain + Groq] ⬅ [Wait Buffer]