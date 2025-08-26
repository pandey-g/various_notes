***<-------------------------notes on various inference hyperparameters----------------------------------->***

Got it 👍 — here are structured notes on **various inference hyperparameters** you’ll encounter when running inference with machine learning models, especially **LLMs, Transformers, and deep learning in general**:

---

# 🧠 Inference Hyperparameters

Inference hyperparameters control **how a trained model generates or predicts outputs**. They don’t affect training but strongly influence performance, latency, determinism, and output quality.

---

## 1. **General Deep Learning Inference Hyperparameters**

### ⚙️ Batch Size

* **Definition**: Number of samples processed together during inference.
* **Effect**:

  * Larger batch size → higher throughput, but more GPU memory usage.
  * Smaller batch size → lower latency per request (better for real-time).
* **Trade-off**: Throughput vs. Latency.

### ⚙️ Precision (FP32, FP16, INT8, etc.)

* **Definition**: Numerical precision for model weights/activations.
* **Effect**:

  * Lower precision (FP16, INT8) → faster, smaller, lower memory usage.
  * Risk of reduced accuracy if quantization not done carefully.

### ⚙️ Device Placement

* CPU vs GPU vs TPU

  * **GPU/TPU**: Faster for parallelizable workloads.
  * **CPU**: Slower but cheaper and better for small-scale real-time inference.

### ⚙️ Caching / KV Cache (LLMs)

* Stores previous **Key-Value attention states** to avoid recomputing in autoregressive generation.
* Greatly speeds up long text generation.

---

## 2. **LLM/Transformer-Specific Inference Hyperparameters**

### 🔹 Max Tokens / Max Length

* **Definition**: Maximum number of tokens generated in output.
* **Effect**:

  * Higher = longer responses, more compute.
  * Lower = shorter, faster, but may truncate answers.

### 🔹 Temperature

* **Definition**: Controls randomness in token sampling.
* **Range**: 0.0 → ∞ (usually \[0.0, 2.0]).
* **Effect**:

  * `0.0` → deterministic (always picks highest-prob token).
  * `0.7` → balanced creativity.
  * `1.5+` → very random, less coherent.

### 🔹 Top-k Sampling

* **Definition**: Restrict token selection to top *k* most likely tokens.
* **Effect**:

  * Small *k* (e.g., 10) → focused, deterministic.
  * Large *k* → more diversity, but risk of incoherence.

### 🔹 Top-p (Nucleus Sampling)

* **Definition**: Selects tokens from the smallest probability mass ≥ *p*.
* **Effect**:

  * `p = 0.9` → only consider tokens that cumulatively have 90% probability.
  * More adaptive than top-k (dynamic cutoff).

### 🔹 Repetition Penalty

* **Definition**: Penalizes tokens already used in generation.
* **Effect**:

  * Reduces loops, redundant phrases.
  * Too high → unnatural language.

### 🔹 Beam Search (num\_beams)

* **Definition**: Expands multiple parallel sequences at each step.
* **Effect**:

  * More beams → better exploration of outputs, more compute.
  * Useful for translation, summarization.

### 🔹 Length Penalty

* **Definition**: Penalizes or rewards longer sequences during beam search.
* **Effect**:

  * > 1.0 → favors longer text.
  * <1.0 → favors shorter text.

### 🔹 Early Stopping

* **Definition**: Stops generation when all beams are finished.
* **Effect**: Faster but might cut off mid-thought.

---

## 3. **System-Level Inference Hyperparameters**

### ⚙️ Concurrency / Parallelism

* Number of requests handled at once.
* Tuning helps balance throughput vs response time.

### ⚙️ Request Timeout

* Max time per inference call (important for APIs).

### ⚙️ Streamed vs Non-Streamed Output

* Streaming → sends tokens as they’re generated (better UX, lower perceived latency).
* Non-streaming → waits until full output generated.

---

## 4. **Trade-offs & Tuning Strategy**

* **Deterministic vs Creative**

  * Use `temperature=0` and greedy decoding for factual Q\&A.
  * Use higher temperature + top-p for creative writing.

* **Speed vs Quality**

  * Smaller batch, greedy decoding → faster but lower diversity.
  * Beam search, high top-k → slower but better output.

* **Memory vs Throughput**

  * Quantization + small batch helps on edge devices.
  * Full precision + large batch better for server inference.

---

✅ **Summary:**

* **General hyperparams**: batch size, precision, device, caching.
* **LLM-specific**: temperature, top-k, top-p, repetition penalty, beams, max tokens.
* **System-level**: concurrency, streaming, timeout.

---

Do you want me to also create a **cheat sheet / table** (like a one-pager quick reference) with recommended values for **different use cases** (Q\&A, summarization, creative writing, etc.)?


***<-----------------------explain beam search .----------------------------->***

Great question 👌 — let’s dive into **Beam Search**, one of the most common decoding strategies for sequence generation in models like LLMs, machine translation, and summarization.

---

# 🚀 Beam Search

## 🔹 What is Beam Search?

Beam Search is a **search strategy** used during inference (decoding) that tries to find the most likely sequence of tokens.
Instead of picking just the *single best next token* (greedy decoding), Beam Search keeps track of **multiple candidate sequences** at each step.

---

## 🔹 How it Works (Step by Step)

Let’s say the model predicts a probability distribution over the vocabulary at each decoding step.

1. **Start with \[BOS] (beginning of sentence token).**

   * Model predicts possible next tokens with probabilities.

2. **Keep top `k` candidates** (where `k` = beam size).

   * If `beam_size = 3`, you keep the top 3 most probable continuations.

3. **Expand each candidate**:

   * At the next step, for each candidate sequence, the model predicts probabilities for the next token.
   * You multiply (or sum log-probabilities of) the sequence score with the new token probability.

4. **Prune candidates**:

   * From all possible continuations, keep only the top `k` scoring sequences.
   * Discard the rest.

5. **Repeat** until:

   * An end-of-sequence (`[EOS]`) token is generated, or
   * You hit the maximum token limit.

---

## 🔹 Example (Simplified)

Vocabulary = {cat, dog, runs, sleeps}
Beam size = 2

1. Step 1: Start → Model predicts:

   * "cat" (0.5), "dog" (0.4), "runs" (0.05), "sleeps" (0.05)
   * Keep top 2 → \["cat"], \["dog"]

2. Step 2: Expand both:

   * "cat runs" (0.5 × 0.6 = 0.30)
   * "cat sleeps" (0.5 × 0.4 = 0.20)
   * "dog runs" (0.4 × 0.7 = 0.28)
   * "dog sleeps" (0.4 × 0.3 = 0.12)

   Keep top 2 → \["cat runs", "dog runs"]

3. Step 3: Expand again … until EOS.

Final output = best-scoring sequence(s).

---

## 🔹 Key Hyperparameters

* **Beam Size (`num_beams`)**

  * Larger beam → explores more sequences (better quality, higher compute).
  * Smaller beam → faster, but may miss good candidates.

* **Length Penalty**

  * Beam search tends to favor shorter sequences.
  * Applying a **length penalty** ensures longer sequences are not unfairly penalized.

* **Early Stopping**

  * Stop when all beams have reached EOS.

---

## 🔹 Pros & Cons

✅ **Advantages**:

* Finds better (more likely) sequences than greedy decoding.
* Good balance between quality and efficiency.

❌ **Disadvantages**:

* More computationally expensive.
* Can still lack diversity (all beams converge to similar outputs).
* May produce generic or repetitive outputs (e.g., "I don’t know").

---

## 🔹 Variants / Improvements

* **Diverse Beam Search**: encourages diversity among beams.
* **Beam Search + Sampling**: hybrid of exploration and exploitation.
* **Top-k / Top-p Sampling**: often preferred for creative tasks.

---

👉 In practice:

* **Machine Translation, Summarization** → Beam Search (num\_beams = 4–10).
* **Open-ended text (chat, stories)** → Top-p / Temperature sampling (Beam Search often too rigid).

---

Would you like me to also **draw a diagram / flow chart** of how beam search works step by step (like beams branching and pruning)? That makes it super easy to visualize.


***<--------------------explain various prompting techniques.---------------------->***

Got it 👍 — prompting is the **art of framing inputs to get better outputs from LLMs**. Different prompting techniques exploit the way these models are trained (next-token prediction with context).

Here’s a structured breakdown of **various prompting techniques**:

---

# 🧠 Prompting Techniques

## 1. **Zero-Shot Prompting**

* **Definition**: Directly ask the model to perform a task without giving examples.
* **Example**:

  ```
  Translate this sentence to French: "The weather is nice today."
  ```
* **Use case**: Quick tasks where model already has strong prior knowledge.
* **Limitations**: Sometimes vague or inaccurate.

---

## 2. **Few-Shot Prompting**

* **Definition**: Provide a few task examples before asking the model to generalize.
* **Example**:

  ```
  Translate English to French:
  "Hello" -> "Bonjour"
  "Good morning" -> "Bonjour"
  "How are you?" -> "Comment ça va?"
  "I love apples" -> 
  ```
* **Use case**: When task is unusual or requires context.
* **Advantage**: Helps model “understand” the task pattern.

---

## 3. **Chain-of-Thought (CoT) Prompting**

* **Definition**: Encourage the model to **reason step-by-step** before answering.
* **Example**:

  ```
  Q: A shopkeeper buys an item for $5 and sells it for $7. What is the profit? 
  Let's think step by step.
  ```
* **Effect**: Produces more logical, explainable answers.
* **Variants**: "Self-consistency" (sample multiple CoTs and pick majority).

---

## 4. **Few-Shot CoT Prompting**

* Combine **examples** + **step-by-step reasoning**.
* Example:

  ```
  Q: If I have 2 apples and eat 1, how many left?
  A: Let's think step by step. I start with 2. Eat 1. Now 1 left. Answer: 1.
  ---
  Q: John has 5 pens and gives away 2. How many left?
  A: Let's think step by step.
  ```

---

## 5. **Instruction / Task-Oriented Prompting**

* Explicitly frame the task with instructions.
* Example:

  ```
  You are a helpful assistant. Summarize the following text in 3 bullet points.
  ```

---

## 6. **Role Prompting**

* Assign the model a role to guide tone, format, or persona.
* Example:

  ```
  You are a senior data scientist. Explain Random Forest to a beginner.
  ```

---

## 7. **Delimiters / Structured Prompting**

* Use quotes, triple backticks, or XML-like tags to clearly mark input/output.
* Example:

  ```
  Summarize the following text delimited by triple quotes:
  """
  Artificial intelligence is transforming industries...
  """
  ```

---

## 8. **ReAct Prompting (Reason + Act)**

* Combine **reasoning** and **tool use** (e.g., calling APIs, search).
* Example:

  ```
  Question: What is the population of France?
  Thought: I should look this up.
  Action: Search("population of France")
  ```

---

## 9. **Self-Consistency Prompting**

* Run multiple reasoning paths → pick the majority/most consistent answer.
* Reduces hallucinations compared to single CoT run.

---

## 10. **Generated Knowledge Prompting**

* Ask the model to first generate background knowledge, then solve the task.
* Example:

  ```
  Step 1: List facts you know about the French Revolution.
  Step 2: Use these facts to explain why it started.
  ```

---

## 11. **Progressive Hinting**

* Provide hints progressively to steer the model if it struggles.
* Example:

  ```
  Solve: 24 ÷ 6 × 2.
  Hint: Do division first, then multiplication.
  ```

---

## 12. **Persona + Style Prompting**

* Control **tone, format, or audience**.
* Example:

  ```
  Write a summary of this article in the style of Shakespeare.
  ```

---

## 13. **Few-Shot with Counterexamples**

* Provide both correct and incorrect examples → helps model disambiguate.
* Example:

  ```
  Classify sentiment:
  "I love this!" -> Positive
  "This is terrible" -> Negative
  "I don’t know" -> Neutral
  ```

---

## 14. **Reflexion Prompting**

* Ask the model to reflect on and refine its own answer.
* Example:

  ```
  Draft an answer, then review it for correctness and improve it.
  ```

---

## 15. **Multimodal Prompting** (if model supports images/audio/code/etc.)

* Combine text + other inputs.
* Example:

  ```
  [Image of chart]
  Describe the trend shown in this chart in plain English.
  ```

---

# ✅ Summary

* **Basic**: Zero-shot, Few-shot, Instruction.
* **Reasoning-focused**: CoT, Self-consistency, Generated Knowledge.
* **Control/Tone**: Role, Persona, Delimiters.
* **Advanced**: ReAct, Reflexion, Multimodal.

---

👉 Do you want me to also create a **visual mindmap / cheat sheet** of all these prompting techniques, so you have a one-glance reference?


***<------------------------Explain Jinja2 prompting !!----------------------->***

Ahh, nice one 👍 — **Jinja2 prompting** is becoming popular in the LLM world because it combines **prompt templating** with **programmatic control** over inputs.

Let’s unpack it:

---

# 🧩 Jinja2 Prompting

## 🔹 What is Jinja2?

* **Jinja2** is a lightweight templating engine (commonly used in Python & Flask apps).
* It allows you to define **templates** with placeholders, loops, and conditionals, and then render them with dynamic values.
* Syntax looks like this:

  ```jinja2
  Hello {{ name }}, welcome to {{ company }}!
  ```

  If rendered with:

  ```python
  {"name": "Alice", "company": "OpenAI"}
  ```

  → **Output**: `Hello Alice, welcome to OpenAI!`

---

## 🔹 Why use Jinja2 for LLMs?

When prompting LLMs, you often need:

* **Reusable templates** (for consistency).
* **Dynamic injection** of variables (user queries, context, retrieved docs).
* **Conditional formatting** (different structure depending on task).

Instead of manually concatenating strings, Jinja2 makes prompts **clean, modular, and maintainable**.

---

## 🔹 Example: Simple Prompt

```jinja2
You are a helpful assistant.

Answer the following question:

Question: {{ user_question }}
```

If `user_question = "What is the capital of France?"` →
Rendered prompt:

```
You are a helpful assistant.

Answer the following question:

Question: What is the capital of France?
```

---

## 🔹 Example: Few-Shot Prompting with Jinja2

```jinja2
Classify the sentiment of the following sentences:

{% for example in examples %}
Sentence: "{{ example.text }}"
Sentiment: {{ example.label }}
{% endfor %}

Now classify this one:

Sentence: "{{ input_text }}"
Sentiment:
```

If you pass:

```python
{
  "examples": [
    {"text": "I love this!", "label": "Positive"},
    {"text": "This is terrible.", "label": "Negative"}
  ],
  "input_text": "It's okay, not great."
}
```

→ Rendered prompt:

```
Classify the sentiment of the following sentences:

Sentence: "I love this!"
Sentiment: Positive

Sentence: "This is terrible."
Sentiment: Negative

Now classify this one:

Sentence: "It's okay, not great."
Sentiment:
```

---

## 🔹 Example: Conditional Logic

```jinja2
{% if task == "summarize" %}
Summarize the following text in 3 bullet points:
{{ document }}
{% elif task == "translate" %}
Translate the following text to French:
{{ document }}
{% endif %}
```

---

## 🔹 Benefits of Jinja2 Prompting

✅ **Cleaner code** → no messy string concatenations.
✅ **Reusable prompt templates** across tasks.
✅ **Dynamic adaptation** to different inputs.
✅ Works great in **production pipelines** (FastAPI, LangChain, LlamaIndex, etc.).

---

## 🔹 Where It’s Used

* **LangChain** → supports Jinja2 prompt templates (`PromptTemplate.from_template(...)`).
* **OpenAI function calling** → Jinja2 often used to build structured JSON prompts.
* **RAG systems** → Jinja2 fills in retrieved context dynamically.

---

✅ **Summary:**
Jinja2 prompting = using **Jinja2 templates** to build LLM prompts in a structured, reusable, and programmatic way. It’s essentially **“prompt engineering with templating”**.

---

Do you want me to show you a **side-by-side comparison** of a normal Python string-built prompt vs a Jinja2-based prompt (to see why it’s cleaner)?


***<--------------------------In Jinja2 prompt we can have variables , if , for? what elese we can have in Jinja2 prompt ?-------------------------->***

Exactly 👌 — in **Jinja2 prompts**, you can use far more than just `variables`, `if`, and `for`.
Since Jinja2 is a full **templating language**, you get a whole toolbox to build **dynamic, flexible prompts**.

Here’s a breakdown:

---

# 🧩 Jinja2 Prompt Features (Useful for Prompting)

## 1. **Variables**

* Inject dynamic values into your prompt.

```jinja2
Hello {{ name }}! Today is {{ day }}.
```

---

## 2. **Filters**

* Transform variables inline.

```jinja2
Hello {{ name | upper }}!
Your score: {{ score | round(2) }}
```

Common filters:

* `upper`, `lower`, `capitalize`
* `join`, `length`
* `replace`, `default`
* `round`, `int`

---

## 3. **Conditionals (`if`, `elif`, `else`)**

```jinja2
{% if language == "fr" %}
Traduire en français: {{ text }}
{% elif language == "es" %}
Traducir al español: {{ text }}
{% else %}
Translate to English: {{ text }}
{% endif %}
```

---

## 4. **Loops (`for`)**

```jinja2
Examples:
{% for ex in examples %}
Q: {{ ex.question }}
A: {{ ex.answer }}
{% endfor %}
```

Also supports:

* `loop.index` (1-based index)
* `loop.index0` (0-based index)
* `loop.last`, `loop.first`

---

## 5. **Macros (Reusable Blocks)**

Define reusable templates (like functions).

```jinja2
{% macro qa_pair(question, answer) %}
Q: {{ question }}
A: {{ answer }}
{% endmacro %}

{{ qa_pair("What is AI?", "Artificial Intelligence") }}
```

---

## 6. **Blocks & Inheritance**

Useful if you have a base prompt format and extend it.

```jinja2
{% block task %}
Default task instructions
{% endblock %}
```

Child template can override this block.

---

## 7. **Whitespace Control**

Add `-` to trim spaces.

```jinja2
{{ name -}}
{{ surname }}
```

---

## 8. **Comments**

```jinja2
{# This is ignored, won’t show in final prompt #}
```

---

## 9. **Set Variables**

You can define variables inside prompts.

```jinja2
{% set topic = "Machine Learning" %}
Explain {{ topic }} in simple terms.
```

---

## 10. **Expressions**

You can do math & logic inline.

```jinja2
You scored {{ correct / total * 100 | round(1) }}%.
```

---

# ✅ Summary

In Jinja2 prompting you can use:

* **Variables** (`{{ var }}`)
* **Filters** (`| upper`, `| round`)
* **Conditionals** (`if / elif / else`)
* **Loops** (`for … in …`)
* **Macros** (reusable blocks)
* **Blocks & inheritance** (prompt composition)
* **Whitespace control** (`-` in tags)
* **Comments** (`{# ... #}`)
* **Inline expressions & math**
* **Set variables inside prompt**

---

👉 So, in short: Jinja2 is like having **Python-lite inside your prompt templates**.

Would you like me to create a **cheat sheet table (syntax + example + output)** for all Jinja2 features you’d commonly use in LLM prompting? That way you can use it as a quick reference.


***<-----------------------explain 6. Blocks & Inheritance !!--------------------------------------->***

Nice, you’ve caught one of the **power features of Jinja2** 👌 — **Blocks & Inheritance**.
This is especially useful in **prompt engineering**, because many prompts share a **common structure** but differ in only certain sections.

---

# 🧩 Jinja2 Blocks & Inheritance

## 🔹 Core Idea

* You define a **base template** with placeholders called **blocks**.
* Then you create **child templates** that *extend* the base and **override those blocks**.
* This keeps prompts **modular, DRY (don’t repeat yourself), and reusable**.

---

## 🔹 Example: Base Template

```jinja2
{# base_prompt.jinja #}
You are a helpful assistant.

{% block task %}
Your task will appear here.
{% endblock %}

Provide your answer in a clear, structured way.
```

Here:

* `block task` is a placeholder section.
* If no child overrides it → default text is used.

---

## 🔹 Example: Child Template (Override Block)

```jinja2
{# summarize_prompt.jinja #}
{% extends "base_prompt.jinja" %}

{% block task %}
Summarize the following text in 3 bullet points:
{{ document }}
{% endblock %}
```

Rendered output:

```
You are a helpful assistant.

Summarize the following text in 3 bullet points:
Artificial intelligence is transforming industries...

Provide your answer in a clear, structured way.
```

---

## 🔹 Multiple Blocks

Base template can define several blocks:

```jinja2
{# base_prompt.jinja #}
You are an assistant specialized in {{ domain }}.

{% block instructions %}Default instructions.{% endblock %}

{% block content %}Default content.{% endblock %}
```

Child template overrides only what it needs:

```jinja2
{% extends "base_prompt.jinja" %}

{% block instructions %}
Answer concisely in 2 sentences.
{% endblock %}
```

---

## 🔹 Why it’s Useful for Prompts

✅ Keeps **prompt boilerplate** (system role, formatting rules) in one place.
✅ Different tasks (summarization, translation, Q\&A) only override the task-specific part.
✅ Easy to maintain: update base → all children inherit changes.
✅ Encourages **clean separation** between general instructions & task details.

---

## 🔹 Analogy

Think of it like **HTML + CSS templating**:

* Base template = `layout.html` (header, footer, common structure).
* Child template = `page.html` (custom body).

For prompting:

* Base = general assistant instructions.
* Child = task-specific behavior.

---

✅ **Summary:**

* **Blocks** = placeholders in templates.
* **Inheritance** = child templates extend base ones and override blocks.
* In prompting → lets you create **a master prompt style guide** and reuse it across tasks with minimal duplication.

---

👉 Do you want me to show you a **practical CRM/AI assistant example** where we use blocks & inheritance to handle *different user intents* (like "summarize email" vs "draft reply") from the same base prompt?


***<----------------------I mean how actually we use inheritence , will the base prompt and child prompt will be in same prompt ? ----------------------------------->***

Great question 🙌 — you’re thinking very practically now: *“In LLM prompting, how do I actually use inheritance? Do I pass base + child prompt together?”*

Here’s how it really works:

---

# 🧩 How Jinja2 Inheritance Works in Prompting

## 🔹 1. **Separation of Templates**

* The **base prompt** and the **child prompt** are usually defined in **separate files** (or strings).
* At **render time**, Jinja2 automatically merges them:

  * It loads the **base template**.
  * Then it replaces the `block` sections with the content from the **child template**.
* So in your final prompt that goes to the LLM → **only the merged result** exists (the model never sees "base" + "child" separately).

---

## 🔹 2. **Example**

### Base template (`base_prompt.jinja`)

```jinja2
You are a helpful AI assistant.

{% block task %}
Your task will appear here.
{% endblock %}

Provide the answer in bullet points.
```

### Child template (`summarize_prompt.jinja`)

```jinja2
{% extends "base_prompt.jinja" %}

{% block task %}
Summarize the following email:
{{ email_text }}
{% endblock %}
```

---

## 🔹 3. **How It Renders**

When you render `summarize_prompt.jinja` with:

```python
template = env.get_template("summarize_prompt.jinja")
print(template.render(email_text="Hi, let's meet tomorrow at 5pm."))
```

**Final prompt (what the LLM sees):**

```
You are a helpful AI assistant.

Summarize the following email:
Hi, let's meet tomorrow at 5pm.

Provide the answer in bullet points.
```

---

## 🔹 4. **Key Point**

👉 You **don’t pass both base + child prompt to the LLM**.
👉 Jinja2 **resolves inheritance before runtime**.
👉 The **child template output is a complete prompt** (base + overrides merged).

So the **LLM only ever sees the final expanded prompt**, not the templating logic.

---

✅ **Answer to your question:**

* No, the base prompt and child prompt are **not combined inside one LLM prompt manually**.
* The child inherits the base **at template rendering time**, and the merged text (single prompt) is what gets sent to the LLM.

---

Would you like me to show you a **mini working Python example** (with Jinja2 environment setup, base file + child file + render → final prompt string) so you can actually test inheritance in action?


***<--------------------------------yes please------------------------------------------>***

Perfect 👍 let’s build a **mini working Python example** that shows how **Jinja2 inheritance** works for prompts.

---

# 🐍 Python Example: Jinja2 Prompt Inheritance

### 1. Install Jinja2 (if not already installed)

```bash
pip install jinja2
```

---

### 2. Project Structure

Let’s say you have this folder:

```
prompts/
  base_prompt.jinja
  summarize_prompt.jinja
main.py
```

---

### 3. Base Prompt (`prompts/base_prompt.jinja`)

```jinja2
You are a helpful AI assistant.

{% block task %}
Your task goes here.
{% endblock %}

Always answer clearly and concisely.
```

---

### 4. Child Prompt (`prompts/summarize_prompt.jinja`)

```jinja2
{% extends "base_prompt.jinja" %}

{% block task %}
Summarize the following email in 3 bullet points:
{{ email_text }}
{% endblock %}
```

---

### 5. Python Script (`main.py`)

```python
from jinja2 import Environment, FileSystemLoader

# 1. Point Jinja2 to the "prompts" folder
env = Environment(loader=FileSystemLoader("prompts"))

# 2. Load the child template (which inherits base)
template = env.get_template("summarize_prompt.jinja")

# 3. Render with variables
final_prompt = template.render(email_text="Hi John, let's meet tomorrow at 5pm in the office to discuss the project.")

# 4. Print final expanded prompt
print(final_prompt)
```

---

### 6. Output (Final Prompt Sent to LLM)

```
You are a helpful AI assistant.

Summarize the following email in 3 bullet points:
Hi John, let's meet tomorrow at 5pm in the office to discuss the project.

Always answer clearly and concisely.
```

---

✅ Notice:

* You only call the **child template** (`summarize_prompt.jinja`).
* Jinja2 automatically **pulls in the base**, replaces the `{% block task %}`, and outputs a **single merged prompt**.
* That final text is what you send to the LLM.

---

👉 Do you want me to extend this example and show how you can have **multiple child prompts** (e.g., `translate_prompt.jinja`, `classify_prompt.jinja`) all inheriting from the same `base_prompt.jinja` to handle different tasks?


***<--------------------Can you explain ReAct prompting technique in details.---------------------->***

