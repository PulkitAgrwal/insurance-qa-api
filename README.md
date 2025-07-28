```markdown
# Insurance Policy Q&A API

A **Retrieval‑Augmented Generation (RAG)** system that lets users pose natural‑language questions about insurance policy documents (PDF, DOCX, email) and receive precise, clause‑backed answers in JSON—via a single POST endpoint.

---

## 🚀 Features

- **Multi‐format ingestion**: Parse PDF, DOCX, and email bodies into text.  
- **Clause‐level semantic search**: Split policies into clauses, embed with OpenAI’s `text-embedding-ada-002`, and index in Pinecone (Starter tier).  
- **LLM‐powered answers**: Use GPT‑4 to generate answers grounded in retrieved clauses.  
- **Structured JSON output**: Includes `question`, `answer`, `source_clause`, and `explanation`.  
- **Bearer‐token auth** on `/hackrx/run`.  
- **Test suite**: Smoke tests for the endpoint and pipeline.

---


## ⚙️ Prerequisites

- **Python 3.10+**  
- **Git**  
- **Pinecone** account (Starter tier)  
- **OpenAI** API key (GPT‑4 + ADA embeddings)  
- **(Optional)** Docker & Docker Compose

---

## 🔧 Installation

1. **Clone your personal fork**  
   ```bash
   git clone https://github.com/<YOUR_USERNAME>/insurance-qa-api.git
   cd insurance-qa-api
````

2. **Create & activate a virtualenv**

   ```bash
   python3 -m venv .venv
   source .venv/bin/activate
   ```

3. **Install dependencies**

   ```bash
   pip install -r requirements.txt
   ```

4. **Configure environment**
   Copy and fill in your keys in `.env.example` → `.env`:

   ```ini
   OPENAI_API_KEY=sk-...
   PINECONE_API_KEY=...
   PINECONE_ENVIRONMENT=us-west1-gcp
   PINECONE_INDEX_NAME=insurance-qa
   API_BEARER_TOKEN=supersecrettoken
   ```

---

## 🚀 Running Locally

```bash
# start FastAPI with auto‑reload
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

Your API is now reachable at `https://localhost:8000/hackrx/run` (or `http://` if you skip HTTPS locally).

---

## 🔌 API Usage

### Endpoint

```
POST /hackrx/run
Authorization: Bearer <API_BEARER_TOKEN>
Content-Type: application/json
Accept: application/json
```

### Request

```jsonc
{
  "documents": "https://example.com/policy.pdf",
  "questions": [
    "What is the waiting period for pre-existing diseases?",
    "Does this policy cover maternity expenses?"
  ]
}
```

* **`documents`**: Public URL of PDF/DOCX/email (or comma‑separated list).
* **`questions`**: Array of plain‑language queries.

### Response

```json
{
  "answers": [
    {
      "question": "What is the waiting period for pre-existing diseases?",
      "answer": "36 months of continuous coverage.",
      "source_clause": "Section 4.2: Pre‑Existing Disease exclusion until 36 months of continuous coverage.",
      "explanation": "The policy states that pre‑existing conditions are excluded until 36 months after inception."
    },
    {
      "question": "Does this policy cover maternity expenses?",
      "answer": "Yes — covers maternity (childbirth & lawful termination) after 36 months, up to ₹50,000.",
      "source_clause": "Section 3.a: Maternity Expenses — expenses traceable to childbirth ... after 36 months.",
      "explanation": "Per Section 3.a, maternity benefits begin after 36 months of coverage."
    }
  ]
}
```

* **Latency target:** < 30 seconds end‑to‑end.
* **Error:** Returns HTTP 4xx/5xx with `{"error": "<message>"}`.

---

## ✅ Pre‑Submission Checklist

Before you submit your webhook on the HackRx dashboard, ensure:

* [ ] `/hackrx/run` is live & reachable
* [ ] HTTPS (valid SSL certificate)
* [ ] Bearer‐token auth enabled
* [ ] Handles POST & returns valid JSON
* [ ] Tested with sample payloads
* [ ] Avg. response time < 30 s

---

## 🧪 Testing

```bash
# run all tests
pytest -q
```

* **`test_api.py`**: Endpoint smoke tests.
* **`test_pipeline.py`**: Ingestion → indexing → query flow.

---

## 🚢 Deployment & Submission

1. **Docker build (optional)**

   ```bash
   docker build -t insurance-qa-api .
   ```

2. **Push to your hosting platform** (Heroku/Railway/Render/Netlify).

3. **Submit** your webhook URL (`https://your-domain.com/hackrx/run`) on the HackRx portal, plus a brief tech‑stack note (e.g. “FastAPI + GPT‑4 + Pinecone RAG”).

4. **Verify** responses with the sample questions.

---

## 🎯 Scoring & Metrics

* **Accuracy** = correct answers / total questions
* **Token Efficiency** = minimal context + prompt size
* **Latency** = response time (< 30 s)
* **Explainability** = clause‐cite in each answer
* **Reusability** = modular, easy to extend

---

## 🤝 Contributing

Feel free to open issues or PRs on your personal fork. For merging back to the original repo:

1. **Push** your branch:

   ```bash
   git push -u origin main
   ```
2. **Open a Pull Request** against `PulkitAgrwal/insurance-qa-api:main`.

---

© 2025 Your Name · [LICENSE](LICENSE)

```
```
