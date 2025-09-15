# 📄 Lexi-flow
Intelligent PDF Chat, Analysis & Comparison Platform  

[![Python](https://img.shields.io/badge/Python-3.11.7-blue.svg)](https://www.python.org/downloads/release/python-3117/)  
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)  
[![AWS](https://img.shields.io/badge/Deployed%20on-AWS%20Fargate-orange)](https://aws.amazon.com/fargate/)  
[![RAG](https://img.shields.io/badge/RAG-Powered-blueviolet)]()  

---

## Overview  

**Lexi-flow** is a **production-grade Generative AI system** for **intelligent analysis, comparison, and conversational search over PDF documents**.  

It combines **Retrieval-Augmented Generation (RAG)** pipelines, **multi-LLM orchestration**, and **enterprise-ready AWS deployment** to help:  

- Extract insights from large, complex documents  
- Track changes across document versions  
- Chat with **single or multiple PDFs**  
- Dynamically switch between **OpenAI, Google Gemini, Groq** models  

**Target Users:** Legal teams, compliance officers, researchers, policy analysts, and enterprises.  

---

## Core Features  

| Feature                  | Description                                                                             |
|--------------------------|-----------------------------------------------------------------------------------------|
| `document_analyzer`     | Extracts metadata (title, author, pages, structured summary)                            |
| `document_compare`     | Compares PDFs and highlights key changes/differences                                    |
| `document_chat` | Semantic search + LLM chat over single or multiple PDFs with contextual top-k retrieval |
| Configurable LLMs     | Switch LLMs/embeddings via YAML without code changes                                    |
| AWS Deployment Ready  | **ECR + ECS (Fargate) + S3 + Secrets Manager + CloudWatch** integration                 |

> ⚠️ Supported Format: `.pdf` only  
> 🧩 Extendable to: `HTML`, `YAML`, `.docx`, `.txt`, `CSV`, OCR-based scanned documents etc  

---

## ⚙️ Tech Stack  

| Layer             | Technology                                                                                       |
|-------------------|--------------------------------------------------------------------------------------------------|
| Language          | Python 3.11.7                                                                                    |
| Framework         | FastAPI, LangChain v0.3, Pydantic                                                                |
| LLMs              | OpenAI GPT-4o, Google Gemini 2.0 Flash, Groq DeepSeek                                            |
| Embeddings        | OpenAI `text-embedding-3-small`, Google `text-embedding-004`                                     |
| Vector Store      | Implemented with FAISS, easily extensible to other vector databases such as Qdrant, Pinecone etc |
| Storage           | AWS S3 (prod), Local Disk (dev)                                                                  |
| Deployment        | **ECR + ECS (Fargate) + Secrets Manager + CloudWatch** (prod), Docker (dev)                      |
| Monitoring        | AWS CloudWatch (logs, metrics, alarms)                                                           |

---

## 📁 Project Structure  

```
├── .dockerignore
├── .github
│   └── workflows
│       ├── aws.yml
│       ├── ci.yaml
│       ├── task_definition.json
│       └── template.yml
├── .gitignore
├── Dockerfile
├── README.md
├── api
│   └── main.py
├── config
│   └── config.yaml
├── data
│   ├── document_analysis
│   ├── document_compare
│   ├── multi_doc_chat
│   └── single_document_chat
├── exception
│   └── custom_exception.py
├── git_tree.py
├── logger
│   └── custom_logger.py
├── model
│   └── models.py
├── notebook
│   ├── exception_exoeriment.ipynb
│   └── experiment.ipynb
├── prompt
│   └── prompt_library.py
├── requirements.txt
├── settings.json
├── src
│   ├── document_analyzer
│   │   └── data_analysis.py
│   ├── document_chat
│   │   └── retrieval.py
│   ├── document_compare
│   │   └── document_comparator.py
│   └── document_ingestion
│       └── data_ingestion.py
├── static
│   └── style.css
├── templates
│   └── index.html
├── test.py
├── tests
│   └── test_unit_case.py
├── utils
│   ├── config_loader.py
│   ├── document_ops.py
│   ├── file_io.py
│   ├── load_env_secrets.py
│   └── model_loader.py
└── versions.py

```  

---

## 🔧 Configuration – `config.yaml`  

### ✅ Embedding Models  

```yaml
embedding_model:
  google:
    provider: "google"
    model_name: "models/text-embedding-004"

  openai:
    provider: "openai"
    model_name: "text-embedding-3-small"
```  

### 🧠 LLM Models  

```yaml
llm:
  groq:
    provider: "groq"
    model_name: "deepseek-r1-distill-llama-70b"
    temperature: 0
    max_output_tokens: 2048

  google:
    provider: "google"
    model_name: "gemini-2.0-flash"
    temperature: 0
    max_output_tokens: 2048

  openai:
    provider: "openai"
    model_name: "gpt-4o"
    temperature: 0.0
    max_output_tokens: 2048
```  

### 🔁 Retriever  

```yaml
retriever:
  top_k: 10
```  

### 📂 Qdrant Vector Store  

```yaml
qdrant_db:
  host: "localhost"
  port: 8080
  collection_name: "lexiflow"
```  

---

## Environment Variables – `.env`  

```dotenv
OPENAI_API_KEY=your_openai_api_key
GOOGLE_API_KEY=your_google_api_key
GROQ_API_KEY=your_groq_api_key
DATA_STORAGE_PATH=./data/document_analysis
```  

---

## Usage Instructions  

### 1️⃣ Setup  

```bash
git clone https://github.com/bhagwat-chate/document_portal.git
cd document_portal
python -m venv venv
source venv/bin/activate  # Windows: venv\Scriptsctivate
pip install -r requirements.txt
```  

### 2️⃣ Configure  

Update `.env` and `configs/config.yaml` with API keys & model configs.  

### 3️⃣ Run Locally  

```bash
python main.py
```  

---

## Docker Deployment  

```bash
# Build Image
docker build -t document-portal .

# Run Container
docker run -p 8000:8000 --env-file .env document-portal
```  

---

## AWS Deployment (Production-Ready)  

| AWS Component       | Role                                                                 |
|---------------------|----------------------------------------------------------------------|
| **ECR**             | Container registry for image storage                                 |
| **ECS Fargate**     | Serverless container runtime for FastAPI app                         |
| **Secrets Manager** | Secure storage for API keys (OpenAI, Gemini, Groq)                   |
| **S3**              | Document storage for uploads                                         |
| **CloudWatch**      | Application logging, metrics, alarms                                 |
| **Route 53 + ALB** (planned) | Custom domain + HTTPS + Load balancing                      |

✅ Current: Deployment on ECS Fargate (HTTP)  
🔜 Next: SSL with ALB + Route 53 + WAF, autoscaling, blue/green rollouts  

---

## ✅ Feature Checklist  

- [x] PDF upload + session-specific storage  
- [x] Metadata extraction (title, author, pages, summary)  
- [x] RAG-powered Q&A (single + multi PDF)  
- [x] Configurable LLM/embeddings via YAML  
- [x] Production deployment on AWS ECS Fargate  
- [ ] FastAPI API endpoints for external apps  
- [ ] RAG evaluation (RAGAS, TruLens)  
- [ ] Guardrails against hallucination  
- [ ] Visual PDF diff for comparisons  
- [ ] Web UI (Streamlit/Next.js)  
- [ ] Persistent storage with S3/EFS  

---

## 📌 Future Enhancements  

- 🔐 RBAC & secure multi-tenant setup  
- 🧠 LLM fallback + voting mechanism  
- 🌍 Multilingual PDF intelligence  
- 🧾 JSONSchema/Pydantic validated outputs  
- 📈 Analytics dashboard (Streamlit / Grafana)  
- ☁️ HuggingFace + Ollama + Bedrock adapters  

---

## 🤝 Contributing  

1. Fork the repo  
2. Create a feature branch  
3. Submit PR with description, example use case, and tests  

---

## 📜 License  

This project is licensed under the [MIT License](LICENSE).  

---

## 👨‍💻 Maintainer  

**Bhagwat Chate**  
AI Architect | GenAI Expert | Multi-Agent RAG | AI System Design  
[🌐 GitHub](https://github.com/bhagwat-chate) · [💼 LinkedIn](https://www.linkedin.com/in/aimlbhagwatchate/)  

---

> Built with ❤️ for **scalable, compliance-ready document intelligence**.  
