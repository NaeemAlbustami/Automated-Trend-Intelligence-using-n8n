Automated Trend Intelligence & Sentiment Pipeline

An autonomous, production-grade ETL and AI data pipeline built using n8n, LangChain, Groq (Llama-3.1-8b), and Supabase. This system monitors high-velocity Google Trends, structurally normalizes raw XML data, runs real-time sentiment and categorical analysis via LLMs, logs records securely into a cloud database, and dispatches real-time structured alerts via Telegram.

🛠️ Workflow Architecture Canvas

Below is the execution canvas of the production pipeline inside n8n:

🚀 Key Features

Automated RSS Fetching & Normalization: Continuously monitors Google Trends RSS feeds, transforms raw XML payloads into optimized JSON objects, and unpacks batches dynamically using sequential data splitting.

High-Performance LLM Synthesis: Leverages Groq's Llama-3.1-8b-instant model to execute zero-shot sentiment parsing (Positive, Negative, Neutral) and domain categorization without prompt-leaking.

Defensive JavaScript Data Extraction: Features a post-LLM sanitization layer that strips unwanted markdown formatting (such as ```json blocks) which LLMs occasionally return, ensuring 100% resilient JSON.parse() consistency before hitting database persistence.

Fail-Safe Fallbacks: Includes error-catching logic that prevents workflow failure if the LLM output is malformed, defaulting to standard classifications instead.

Built-in Rate Limiting & Throttling: Utilizes an integrated asynchronous Wait node buffer (13 seconds) to handle asynchronous scaling and safely stay within free-tier Groq API RPM/TPM thresholds.

Cloud Persistence & Alerting: Automates structured relational data logging into Supabase while routing localized execution summaries directly to a dedicated Telegram channel.

📐 Pipeline Flow Chart

[Schedule Trigger] ➔ [Fetch Trends (HTTP)] ➔ [XML Parser] ➔ [Split Out] 
                                                                 ⬇
[Telegram Alert] ⬅ [Supabase Insert] ⬅ [JS Sanitizer] ⬅ [Basic LLM Chain + Groq] ⬅ [Wait Buffer]

Environment & Credentials Checklist

Before deploying, ensure you have set up active credentials in your n8n instance for:

Groq Cloud API: Required for the Groq Chat Model node (Llama-3.1-8b-instant).

Supabase API: Required for the Create a row node (Project URL and Service Role Key).

Telegram Bot API: Required for the Send a text message node (Bot Token & Target Chat ID).

📥 How to Import & Use

Copy the raw content of the workflow.json file from this repository.

Open your n8n dashboard and create a new workflow.

Import the file by pressing Ctrl + V (or Cmd + V on Mac) directly on your canvas grid.

Update the Supabase, Groq, and Telegram nodes with your custom credentials.

Save and switch the workflow status to Active to start gathering autonomous trend intelligence!

