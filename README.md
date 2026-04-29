# Resume-screening-ai
<div align="center">

# 🎯 Resume Screening AI

### *Stop reading resumes. Start finding talent.*

[![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![Streamlit](https://img.shields.io/badge/Streamlit-1.35-FF4B4B?style=for-the-badge&logo=streamlit&logoColor=white)](https://streamlit.io)
[![spaCy](https://img.shields.io/badge/spaCy-3.7-09A3D5?style=for-the-badge&logo=spacy&logoColor=white)](https://spacy.io)
[![License: MIT](https://img.shields.io/badge/License-MIT-22C55E?style=for-the-badge)](LICENSE)

An AI-powered resume analysis system that ranks candidates, surfaces skill gaps, and gives actionable feedback — all in seconds.

[✨ Features](#-features) • [🏗️ Architecture](#%EF%B8%8F-architecture) • [🚀 Quick Start](#-quick-start) • [🧠 How It Works](#-how-it-works) • [⚙️ Configuration](#%EF%B8%8F-configuration)

</div>

---

## ✨ Features

| | Feature | What it does |
|---|---|---|
| 🎯 | **Semantic Match Score** | Cosine similarity via Sentence-BERT — catches meaning, not just keywords |
| 🔍 | **Skill Gap Analysis** | Identifies exactly which skills a candidate has, lacks, or brings as a bonus |
| 🏆 | **Multi-Candidate Ranking** | Weighted composite score across semantic match, skills, and experience |
| 💡 | **Improvement Suggestions** | Actionable, prioritized tips to help candidates level up |
| 📄 | **PDF & TXT Support** | Drag-and-drop upload for any resume format |
| 📊 | **Visual Leaderboard** | Interactive Plotly charts for at-a-glance comparison |

---

## 🏗️ Architecture

```
resume-screening-ai/
├── app/
│   ├── extractor.py        # PDF/TXT text extraction (PyMuPDF)
│   ├── preprocessor.py     # NLP cleaning pipeline (spaCy)
│   ├── embedder.py         # Sentence-BERT embeddings
│   ├── scorer.py           # Cosine similarity scoring
│   ├── skill_extractor.py  # NER + curated skill keyword matching
│   ├── ranker.py           # Multi-candidate composite ranking
│   ├── advisor.py          # Resume improvement suggestions
│   └── pipeline.py         # Main orchestration
├── data/
│   └── sample_resumes/     # Sample resumes for testing
├── tests/
│   └── test_pipeline.py    # Unit tests
├── streamlit_app.py        # Streamlit UI
├── Dockerfile
└── requirements.txt
```

---

## 🚀 Quick Start

### 1. Clone the repo
```bash
git clone https://github.com/yourusername/resume-screening-ai.git
cd resume-screening-ai
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
python -m spacy download en_core_web_sm
```

### 3. Launch the app
```bash
streamlit run streamlit_app.py
```

### 4. Run tests
```bash
pytest tests/ -v
```

### 🐳 Or run with Docker
```bash
docker build -t resume-screening-ai .
docker run -p 8501:8501 resume-screening-ai
```

---

## 🧠 How It Works

The system is a 6-step AI pipeline that goes from raw resume bytes to a ranked candidate list:

```
PDF/TXT  ──▶  Extract  ──▶  Preprocess  ──▶  Embed  ──▶  Score  ──▶  Rank  ──▶  Results
              (fitz)       (spaCy NLP)   (SBERT)   (cosine)  (weighted)
```

**Step 1 — Text Extraction**
PyMuPDF (fitz) extracts text from PDFs page-by-page. TXT files are read with UTF-8 encoding.

**Step 2 — NLP Preprocessing**
spaCy lemmatizes and cleans the text — turning "managing developers" into "manage developer" — and strips stopwords, URLs, emails, and phone numbers.

**Step 3 — Semantic Embeddings**
Sentence-BERT (`all-MiniLM-L6-v2`) encodes the resume and job description into 384-dimensional vectors. Similar meaning = vectors close together, regardless of exact wording.

**Step 4 — Cosine Similarity Scoring**
```
similarity = dot(resume_vec, jd_vec) / (|resume_vec| × |jd_vec|)
```
Ranges 0 → 1, displayed as a percentage match score.

**Step 5 — Skill Gap Analysis**
A curated database of ~100 skills across 7 categories is matched against both the resume and JD using NER + keyword matching. The delta is your skill gap.

**Step 6 — Weighted Composite Ranking**
```
composite = 0.60 × semantic_score
          + 0.25 × skill_match_rate
          + 0.15 × experience_score
```

---

## 📊 Sample Output

```
=== CANDIDATE RANKING RESULTS ===

Rank #1: alice_chen.pdf
  Composite Score : 76.4%
  Semantic Match  : 78.5%
  Skill Match     : 82.0%
  Experience Score: 46.7%

Rank #2: bob_smith.pdf
  Composite Score : 41.2%
  Semantic Match  : 42.0%
  Skill Match     : 28.0%
  Experience Score: 33.3%
  Missing Skills  : pytorch, docker, kubernetes
```

---

## ⚙️ Configuration

### Switch embedding models
Edit `app/pipeline.py` to trade speed for accuracy:

```python
pipeline = ScreeningPipeline(embedding_model="fast")      # ~80MB  — default
pipeline = ScreeningPipeline(embedding_model="balanced")  # ~420MB — better quality
pipeline = ScreeningPipeline(embedding_model="quality")   # multilingual support
```

### Tune ranking weights
Edit `app/ranker.py` to match your hiring priorities:

```python
ranker = CandidateRanker(
    semantic_weight=0.60,    # How much meaning matters
    skill_weight=0.25,       # How much skill coverage matters
    experience_weight=0.15   # How much years of experience matter
)
```

---

## 🔧 Tech Stack

| Layer | Technology |
|---|---|
| Language | Python 3.10+ |
| NLP | spaCy `en_core_web_sm` |
| Embeddings | Sentence-Transformers `all-MiniLM-L6-v2` |
| Similarity | scikit-learn `cosine_similarity` |
| PDF Parsing | PyMuPDF (fitz) |
| UI | Streamlit + Plotly |
| Containerization | Docker |

---

## 🙏 Acknowledgments

- [Sentence-Transformers](https://www.sbert.net/) by UKPLab — the backbone of semantic matching
- [spaCy](https://spacy.io/) by Explosion AI — fast, production-grade NLP
- [Streamlit](https://streamlit.io/) — for making ML apps stupidly easy to ship
- [PyMuPDF](https://pymupdf.readthedocs.io/) — reliable PDF parsing

---

<div align="center">

Made with 🤖 + ☕ &nbsp;|&nbsp; MIT License

*If this saved you from reading another bad resume, leave a ⭐*

</div>
