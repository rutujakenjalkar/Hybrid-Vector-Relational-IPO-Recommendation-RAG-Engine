

## Hybrid Vector-Relational IPO Recommendation & RAG Engine

**Hybrid Vector-Relational IPO Recommendation & RAG Engine is an AI-powered financial advisory agent that optimizes investor participation in upcoming IPO markets. It dynamically aggregates historical user portfolios into multi-dimensional profile vectors to execute native KNN similarity matches within PostgreSQL (pgvector), processes real-time news data via a specialized FinBERT transformer for domain-specific sentiment analysis, and routes unstructured prospectus queries through an AstraDB-backed RAG pipeline to generate deterministic, zero-hallucination investment recommendations.**
---

## 📂 Project Structure

```text
.
├── agents/
├── data_pipeline/
├── database/
│   └── schema.sql
├── tools/
├── requirements.txt
└── README.md
```

---

# 🛠 Prerequisites

| Requirement | Purpose |
|-------------|---------|
| Python 3.12+ | Backend runtime |
| PostgreSQL | Structured IPO & transaction storage |
| pgvector | Vector extension for PostgreSQL |
| pgAdmin *(optional)* | Database management |
| Pinata | Prospectus PDF hosting |
| Astra DB Serverless | Vector database for prospectus embeddings |
| OpenAI API Key | LLM-powered reasoning |

---

# ⚙️ Installation

```powershell
python -m venv venv
.\venv\Scripts\activate
pip install -r requirements.txt
```

---

# 🔑 Environment Variables

Copy `.env.example` to `.env` and configure:

## PostgreSQL

```text
POSTGRES_HOST=
POSTGRES_DB=
POSTGRES_USER=
POSTGRES_PASSWORD=
POSTGRES_PORT=
```

## Astra DB

```text
ASTRA_DB_APPLICATION_TOKEN=
ASTRA_DB_API_ENDPOINT=
```

## Pinata

```text
PINATA_GATEWAY=gateway.pinata.cloud
```

## OpenAI

```text
OPENAI_API_KEY=
```

## Optional Pipeline Settings

```text
CHUNK_SIZE=
CHUNK_OVERLAP=
BATCH_SIZE=
```

---

# 🗄 PostgreSQL Setup

Create a database named:

```text
POLYQUITY_DATA
```

Run the schema:

```powershell
psql -U postgres -d POLYQUITY_DATA -f .\database\schema.sql
```

Or execute `database/schema.sql` using pgAdmin Query Tool.

---

# 📄 Pinata Setup

1. Upload the IPO prospectus PDF.
2. Copy the generated CID (`bafy...`).

---

# ☁️ Astra DB Setup

1. Create an Astra Serverless database with Vector Search enabled.
2. Create a collection named:

```text
demo
```

3. Generate an Application Token (`AstraCS:...`).
4. Copy the API Endpoint.

Example:

```text
https://<DB_ID>-<REGION>.apps.astra.datastax.com
```

---

# 📊 ETL Pipeline

For each IPO:

1. Select an IPO from Chittorgarh.
2. Upload its prospectus to Pinata.
3. Generate a UUID.
4. Update `data_pipeline/extractor.py`:

```python
ipo_id
name
ipo_cid
```

5. Run:

```powershell
python .\data_pipeline\extractor.py
```

Repeat for approximately **9 IPOs**.

---

# 💰 Add Sample Transactions

Insert sample rows into the `transaction` table.

These records are used by the recommendation engine.

---

# 🤖 Run the Recommendation Agent

Use a wallet address that exists in the `transaction` table.

```powershell
python -m agents.recommendation_agent
```

---

# 🏗 Technology Stack

| Category | Technology |
|-----------|------------|
| Language | Python |
| Database | PostgreSQL |
| Vector Database | Astra DB |
| Vector Extension | pgvector |
| LLM | OpenAI |
| Storage | Pinata (IPFS) |
| Framework | LangChain |

---

# 📌 Workflow

```text
IPO Prospectus
      │
      ▼
 Pinata (IPFS)
      │
      ▼
 ETL Pipeline
 ┌────┴────┐
 ▼         ▼
PostgreSQL Astra DB
 └────┬────┘
      ▼
AI Advisory Agent
      ▼
Personalized IPO Recommendation
```

