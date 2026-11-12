# 🛡️ Prompt Police

A real-time AI prompt safety classifier that acts as a bouncer between the user and the language model — intercepting, analyzing, and categorizing prompts as **SAFE** or **ADVERSARIAL** before they execute.

Prompt Police uses a **Groq LLM-as-Judge** pipeline (`llama-3.3-70b-versatile`) to classify text with zero-shot accuracy. It also includes a lightweight **Base64 obfuscation pre-filter** that decodes encoded payloads before classification, catching evasion attempts transparently.

## 🚀 Features

* **LLM-as-Judge Classifier:** Sends every prompt to `llama-3.3-70b-versatile` via the Groq API at `temperature=0.0` with a structured JSON `response_format`, returning a `label`, `score`, `category`, and `reason` for every request.
* **Base64 Evasion Detection:** Automatically detects and decodes base64-encoded prompts before classification — catching obfuscation attempts that try to hide adversarial intent from safety layers.
* **Structured Output Schema:** Every classification returns `label` (`SAFE`/`ADVERSARIAL`), `score` (0.0–1.0), `category` (DAN, prompt-injection, obfuscation, hypothetical, extraction, safe, other), and a one-sentence `reason`.
* **Clean UI Architecture:** A fully local, responsive frontend using Tailwind CSS with immersive analytical threat reporting — no Node/NPM required, just open `index.html`.
* **Local Evaluation Suite:** Includes an isolated `evaluate.py` script calculating Accuracy, Precision, Recall, F1 Score, and False Positive Rate against an unseen 60-prompt benchmark dataset (30 adversarial, 30 safe).

## ⚙️ Installation

You will need Python 3.9+ and a free [Groq API key](https://console.groq.com/).

1. **Clone the repository**
   ```bash
   git clone https://github.com/your-username/prompt-police.git
   cd prompt-police
   ```

2. **Install core dependencies**
   ```bash
   pip install -r requirements.txt
   ```

3. **Set your Groq API key**
   ```bash
   cp .env.example .env
   # Edit .env and paste your GROQ_API_KEY
   ```

4. **Start the Classifier Engine**
   ```bash
   uvicorn main:app --host 0.0.0.0 --port 8000
   ```

5. **Launch the UI**
   Simply open `index.html` in any modern web browser! (No Node/NPM required).

## 🏗️ Architecture

```
User Prompt
    │
    ▼
Base64 Pre-Filter (try_decode_b64)
    │  Detects & decodes encoded payloads before classification
    ▼
Groq LLM Judge (llama-3.3-70b-versatile, temp=0.0)
    │  Structured JSON prompt → { label, score, category, reason }
    ▼
FastAPI Response → Frontend UI
```

> **Note:** The classifier runs entirely via the Groq API — no local model weights are downloaded. No Sentence-Transformers or lexical firewall is used in the current pipeline.

## 📊 Evaluation Metrics

Running the benchmark suite (`python evaluate.py` against a live server) on the current 60-prompt dataset (30 adversarial, 30 safe) yields:

* **Accuracy:** 98.33%
* **Precision:** 96.77%
* **Recall (Detection Rate):** 100.00%
* **F1 Score:** 98.36%
* **False Positive Rate:** 3.33%

> These results are captured in `eval_results.txt`. Re-run `python evaluate.py` with the server running to refresh them.

## 📝 License

MIT License — open sourced for AI Security researchers and enthusiasts.
