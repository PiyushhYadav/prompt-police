# 🛡️ Prompt Police

🚀 **Live Demo:** https://promptpolice.netlify.app/

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/c1f165c7-2687-4d30-bfbb-80b55d4b04d8" />

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/23f81ecf-5cc0-4a5d-99ca-c188600db6b7" />

A minimalist, editorial-first AI safety system that inspects, intercepts, and classifies generative AI prompts **before they reach the language model**.

Prompt Police combines **Latent Space Mapping (Sentence-Transformers)** with a **Hybrid Lexical Firewall** to detect adversarial prompts in real time. It analyzes linguistic structure and compares it against known attack patterns like prompt injections, DAN overrides, and deepfake instructions — all with ultra-low latency.

---

## 🚀 Features

* **Real-time Latent Mapping**
  Uses `all-MiniLM-L6-v2` to detect even unseen (“zero-day”) jailbreaks based on semantic similarity.

* **Hybrid Firewall System**
  A lexical layer catches explicit threats (violence, illegal instructions) before they bypass embeddings.

* **Base64 Evasion Detection**
  Identifies and blocks obfuscated prompts targeting system-level bypasses.

* **Clean UI Architecture**
  Lightweight frontend using Tailwind CSS with real-time threat feedback.

* **Evaluation Suite**
  Includes `evaluate.py` for performance testing (Accuracy, Precision, Recall, FPR).

---

## 📊 Evaluation Metrics

Results from internal benchmark (60-prompt dataset):

* **Accuracy:** 98.3%
* **Precision:** 96.7%
* **Recall:** 100.0%
* **False Positive Rate:** < 3.5%

---

## ⚙️ Installation

### 1. Clone the repository

```bash
git clone https://github.com/your-username/prompt-police.git
cd prompt-police
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Run backend server

```bash
uvicorn main:app --host 0.0.0.0 --port 8000
```

> On first run, the model (~80MB) will download automatically.

### 4. Launch frontend

Open `index.html` in your browser.

---

## 🧠 How It Works

1. Input prompt is received
2. Semantic embedding is generated
3. Compared with adversarial clusters
4. Lexical firewall scans for explicit threats
5. Final classification: **SAFE / ADVERSARIAL**

---

## 📝 License

MIT License — open-sourced for AI security researchers and developers.
