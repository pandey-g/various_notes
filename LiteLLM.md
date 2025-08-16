**LiteLLM** is an open-source Python library that acts as a **unified interface (or wrapper)** for calling multiple **Large Language Model (LLM)** providers through a single, standardized API — just like OpenAI’s API.

---

## 🧠 Think of LiteLLM as:

> A **universal adapter** for different LLMs — so you don’t have to learn or manage multiple APIs separately.

---

## 🔧 What It Does:

* Lets you call models from:

  * **OpenAI** (`gpt-3.5-turbo`, `gpt-4`)
  * **Anthropic** (`claude-3-opus`, `claude-3-sonnet`)
  * **Cohere**, **Mistral**, **Ollama**, **Together AI**, **Groq**, and more
* Uses one **OpenAI-like interface** for everything
* Handles:

  * Authentication
  * Rate limiting
  * Model switching
  * Proxying requests

---

## ✅ Example Usage:

```python
from litellm import completion

response = completion(
    model="claude-3-opus",  # Could also be "gpt-3.5-turbo" or "mistral-7b"
    messages=[{"role": "user", "content": "What's the capital of France?"}]
)

print(response['choices'][0]['message']['content'])
```

This looks like OpenAI’s API — but under the hood, it’s calling **Anthropic**, **Mistral**, or another provider depending on the `model` value.

---

## 🔌 Why People Use LiteLLM:

* 🚀 **Model Flexibility**: Easily switch models/providers without rewriting logic
* ⚡ **Rate Limit Handling**: Built-in retry and rate limit support
* 🔐 **Unified Key/Token Management**: Simplifies managing API keys
* 🛡 **Proxy Mode**: Can run as a proxy server to standardize API calls across apps

---

## 🔍 Who Is It For?

* Devs building LLM apps who want to **experiment with or switch between models**
* Teams managing multiple LLM vendors
* People deploying **LLMs in production** and need robustness + flexibility

---

## 📦 Install:

```bash
pip install litellm
```

---

Would you like to see how to use it with LangChain or in a Flask/FastAPI app?
