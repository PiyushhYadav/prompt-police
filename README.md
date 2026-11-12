# 🛡️ Prompt Police

🚀 **Live Demo:** https://promptpolice.netlify.app/

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/c1f165c7-2687-4d30-bfbb-80b55d4b04d8" />

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/23f81ecf-5cc0-4a5d-99ca-c188600db6b7" />

A real-time AI prompt safety classifier that acts as a bouncer between the user and the language model — intercepting, analyzing, and categorizing prompts as **SAFE** or **ADVERSARIAL** before they execute.

Prompt Police uses a **Groq LLM-as-Judge** pipeline (`llama-3.3-70b-versatile`) to classify text with zero-shot accuracy. It also includes a lightweight **Base64 obfuscation pre-filter** that decodes encoded payloads before classification, catching evasion attempts transparently.

---

## 🚀 Features

* **LLM-as-Judge Classifier:** Sends every prompt to `llama-3.3-70b-versatile` via the Groq API at `temperature=0.0` with a structured JSON `response_format`, returning a `label`, `score`, `category`, and `reason` for every request.
* **Base64 Evasion Detection:** Automatically detects and decodes base64-encoded prompts before classification — catching obfuscation attempts that try to hide adversarial intent from safety layers.
* **Structured Output Schema:** Every classification returns `label` (`SAFE`/`ADVERSARIAL`), `score` (0.0–1.0), `category` (DAN, prompt-injection, obfuscation, hypothetical, extraction, safe, other), and a one-sentence `reason`.
* **Clean UI Architecture:** A fully local, responsive frontend using Tailwind CSS with immersive analytical threat reporting — no Node/NPM required, just open `index.html`.
* **Evaluation Suite:** Includes an isolated `evaluate.py` test suite to benchmark classification performance against a 60-prompt test dataset (30 adversarial, 30 safe).

---

## 🧠 How It Works

Prompt Police operates as an **LLM-as-Judge** security gatekeeper positioned in front of downstream language models:

```
User Prompt
    │
    ▼
[Phase 1] Base64 Pre-Filter (try_decode_b64)
    │  Detects & decodes obfuscated payloads
    ▼
[Phase 2] Security Meta-Prompt Injection
    │  Wraps prompt with strict safety rules & JSON schema
    ▼
[Phase 3] Groq LLM-as-Judge (llama-3.3-70b-versatile, temp=0.0)
    │  Deterministic zero-shot evaluation & categorization
    ▼
[Phase 4] Structured Verdict Extraction
    │  { label, score, category, reason }
    ▼
[Phase 5] FastAPI Response ──▶ UI Dashboard / Downstream App
```

1. **Base64 Pre-Filter & De-Obfuscation:**
   Incoming prompts first pass through `try_decode_b64()`. If an encoded payload is detected, it is decoded into plaintext so hidden adversarial instructions cannot bypass safety evaluation.
2. **Security Meta-Prompt Wrapping:**
   The decoded prompt is injected into a comprehensive security meta-prompt. The meta-prompt instructs the judge to identify adversarial patterns — including DAN overrides, prompt injections, system prompt extractions, and malicious roleplay/hypotheticals — while permitting benign roleplay and normal queries.
3. **Deterministic LLM-as-Judge Evaluation:**
   The wrapped prompt is evaluated zero-shot by `llama-3.3-70b-versatile` via the Groq API at `temperature=0.0` with `response_format={"type": "json_object"}`. Inference runs entirely via Groq's high-speed API — no local weights or heavy dependencies required.
4. **Structured Threat Telemetry:**
   The LLM judge returns a validated JSON object with four core signals:
   * `label`: Binary verdict (`SAFE` or `ADVERSARIAL`).
   * `score`: Threat severity / confidence rating from `0.0` (definitely safe) to `1.0` (definitely adversarial).
   * `category`: Identified threat vector (`DAN`, `prompt-injection`, `obfuscation`, `hypothetical`, `extraction`, `safe`, or `other`).
   * `reason`: A concise, human-readable explanation of why the prompt was flagged or permitted.
5. **Enforcement & Frontend Dispatch:**
   FastAPI dispatches the verdict to the caller, allowing downstream systems to block malicious queries or forward safe prompts, while updating the frontend threat dashboard in real time.

---

## ⚙️ Installation

You will need Python 3.9+ and a free [Groq API key](https://console.groq.com/).

### 1. Clone the repository
```bash
git clone https://github.com/PiyushhYadav/prompt-police.git
cd prompt-police
```

### 2. Install core dependencies
```bash
pip install -r requirements.txt
```

### 3. Set your Groq API key
```bash
cp .env.example .env
# Edit .env and paste your GROQ_API_KEY
```

### 4. Start the Classifier Engine
```bash
uvicorn main:app --host 0.0.0.0 --port 8000
```

### 5. Launch the UI
Simply open `index.html` in any modern web browser! (No Node/NPM required).

### 6. (Optional) Run the Evaluation Suite
With the server running, evaluate the classifier against the 60-sample benchmark dataset:
```bash
python evaluate.py
```

---

## 📝 License

MIT License — open-sourced for AI security researchers and developers.
