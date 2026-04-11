# 🛡️ Prompt Police

A minimalist, editorial-first AI Safety protocol designed to inspect, intercept, and categorize generative AI prompts before they reach the language model. 

Prompt Police uses **Latent Space Mapping** (Sentence-Transformers) and a **Hybrid Lexical Firewall** to classify text in real-time. It analyzes the linguistic structure of a prompt and measures its similarity against known adversarial clusters (DAN overrides, prompt injections, deepfake generation syntax, etc.) at ultra-low latency.

## 🚀 Features
* **Real-time Latent Mapping:** Uses `all-MiniLM-L6-v2` to map semantic intent, catching "Zero-Day" jailbreaks based purely on structural similarity without needing explicit hardcoded regex.
* **Hybrid Firewall Bounds:** A secondary lexical engine traps obvious explicit threats dynamically (e.g., violent acts, bomb creation) before they confuse the embedding clusters.
* **Base64 Evasion Detection:** Automatically scans and intercepts prompts that have been obfuscated using encoding specifically targeting system parsers.
* **Clean UI Architecture:** A fully local, responsive frontend using Tailwind CSS with immersive analytical threat reporting.
* **Local Evaluation Suite:** Includes an isolated `evaluate.py` script calculating Accuracy, F1 Scores, and False Positive Rates against an unseen 60-prompt benchmark dataset.

## ⚙️ Installation

You will need Python 3.9+ to run the backend smoothly.

1. **Clone the repository**
   ```bash
   git clone https://github.com/your-username/prompt-police.git
   cd prompt-police
   ```

2. **Install core dependencies**
   ```bash
   pip install -r requirements.txt
   ```
   *(Ensure you have basic PyTorch compatibilities if running in a unique environment).*

3. **Start the Classifier Engine**
   ```bash
   uvicorn main:app --host 0.0.0.0 --port 8000
   ```
   *Note: On its very first run, it will automatically download the 80MB HuggingFace MiniLM mapping weights.*

4. **Launch the UI**
   Simply open `index.html` in any modern web browser! (No Node/NPM required).

## 📊 Evaluation Metrics
Running the internal offline test suite (`python evaluate.py`) yields the following baseline metrics against our current safe/adv distribution:

* **Accuracy:** 98.3%
* **Precision:** 96.7%
* **Recall:** 100.0%
* **False Positive Rate:** < 3.5%

## 📝 License
MIT License - open sourced for AI Security researchers and enthusiasts.
