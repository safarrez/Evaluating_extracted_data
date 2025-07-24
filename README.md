# Table Extraction Service

[![Python Version](https://img.shields.io/badge/python-3.11-blue)](https://www.python.org/)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)

---

## 📖 Table of Contents

1. [Overview](#overview)
2. [Features](#features)
3. [Models](#models)
4. [Data Sources](#data-sources)
5. [Prerequisites](#prerequisites)
6. [Installation](#installation)
7. [Usage](#usage)
8. [API Reference](#api-reference)
9. [Testing](#testing)
10. [Development](#development)
11. [Contributing](#contributing)
12. [License](#license)

---

## 🔍 Overview
A dual-service library for extracting structured table fields from scientific PDF papers:
- **Prompt-based Extraction**: Leverages `llama-3.1-8b-instant` (Groq) with prompt engineering.
- **Fine-tuned Extraction**: Uses a LoRA-adapted `meta-llama/Llama-3.2-1B-Instruct` model for specialized performance.

## ✨ Features
- End-to-end pipeline: PDF → JSON
- Two extraction strategies:
  - **Prompt-driven** via Groq LLM
  - **Fine-tuned** via LoRA adapter
- Easy-to-use Python API
- Unit tests for validation

## 🧠 Models
| Type             | Name                                     | Source Directory                    |
|------------------|------------------------------------------|-------------------------------------|
| Prompt-based     | `llama-3.1-8b-instant`                   | n/a (on-the-fly via Groq)           |
| Fine-tuned (LoRA)| `meta-llama/Llama-3.2-1B-Instruct` + LoRA | `./llama3-extraction-lora`          |

## 📂 Data Sources
[!(Data)(https://drive.google.com/drive/u/0/folders/1zurB2MibVbrhiobBe5oyvxcK6n5hdbB_)](Data)

- 32 annotated PDF–table pairs
- Example papers:
  - Coyne et al. (2004)
  - Vaknin‑Nusbaum & Nevo (2017)
  - Yeh & Connell (2008)
- Directory structure:
  - `lab/files/` — raw PDFs
  - `data/table/` — ground-truth tables

## 🔧 Prerequisites
- **Python** ≥ 3.11
- **CUDA-capable GPU** (for fine-tuned model inference)

## 🚀 Installation
```bash
git clone https://github.com/your-org/table_extraction_service.git
cd table_extraction_service
python -m venv env
source env/bin/activate  # On Windows use `env\Scripts\activate`
pip install -r requirements.txt
