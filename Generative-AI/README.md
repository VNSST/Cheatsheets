# 🧠 Generative AI — Cheatsheet

> A comprehensive reference covering GenAI foundations, architectures, prompting, fine-tuning, and applications.

---

## Table of Contents

1. [What is Generative AI?](#what-is-generative-ai)
2. [Key Concepts](#key-concepts)
3. [Foundation Models & Architectures](#foundation-models--architectures)
4. [Transformer Architecture](#transformer-architecture)
5. [Large Language Models (LLMs)](#large-language-models-llms)
6. [Prompt Engineering](#prompt-engineering)
7. [Retrieval-Augmented Generation (RAG)](#retrieval-augmented-generation-rag)
8. [Fine-Tuning Techniques](#fine-tuning-techniques)
9. [Image Generation](#image-generation)
10. [Evaluation & Safety](#evaluation--safety)
11. [GenAI Application Stack](#genai-application-stack)
12. [Key Models Directory](#key-models-directory)
13. [Tools & Frameworks](#tools--frameworks)
14. [Glossary](#glossary)

---

## What is Generative AI?

Generative AI refers to AI systems that can **create new content** — text, images, code, audio, video — by learning patterns from training data.

```
Discriminative AI:  Input → "Is this a cat or dog?"   (classifies)
Generative AI:      Prompt → "Generate an image of a cat"  (creates)
```

### GenAI Timeline

| Year  | Milestone                                            |
|-------|------------------------------------------------------|
| 2014  | GANs introduced (Goodfellow)                        |
| 2017  | Transformer architecture ("Attention Is All You Need")|
| 2018  | GPT-1, BERT released                                |
| 2020  | GPT-3 (175B parameters)                             |
| 2021  | DALL-E, Codex                                       |
| 2022  | ChatGPT, Stable Diffusion, Midjourney               |
| 2023  | GPT-4, LLaMA, Claude, Gemini, Mixtral               |
| 2024  | GPT-4o, Claude 3.5, Gemini 1.5, open-weight boom    |
| 2025  | Reasoning models, multi-modal agents, Gemini 2.0    |

---

## Key Concepts

### Tokens

The basic unit of text processing. Words are split into subword tokens.

```
"Hello world"     → ["Hello", " world"]          (2 tokens)
"Unbelievable"    → ["Un", "believ", "able"]      (3 tokens)

Rule of thumb: 1 token ≈ 4 characters ≈ ¾ of a word
```

### Context Window

The maximum number of tokens a model can process at once (input + output).

| Model              | Context Window     |
|--------------------|--------------------|
| GPT-3.5            | 4K – 16K tokens    |
| GPT-4o             | 128K tokens        |
| Claude 3.5 Sonnet  | 200K tokens        |
| Gemini 1.5 Pro     | 1M – 2M tokens     |
| Llama 3            | 8K – 128K tokens   |

### Temperature & Sampling

Controls randomness/creativity in output generation.

| Parameter          | Low Value              | High Value              |
|--------------------|------------------------|-------------------------|
| **Temperature**    | Deterministic, focused | Creative, diverse       |
| **Top-p (nucleus)**| Fewer token choices    | More token choices      |
| **Top-k**          | Only top k tokens      | Wider selection         |

```python
# Example API call
response = client.chat.completions.create(
    model="gpt-4o",
    messages=[{"role": "user", "content": "Write a poem"}],
    temperature=0.7,     # 0.0 = deterministic, 2.0 = very random
    top_p=0.9,           # Nucleus sampling
    max_tokens=500
)
```

### Embeddings

Dense vector representations of text that capture semantic meaning.

```python
# Similar meanings → similar vectors
embed("king") - embed("man") + embed("woman") ≈ embed("queen")

# Use cases: semantic search, clustering, recommendations
```

---

## Foundation Models & Architectures

### Types of Models

| Architecture      | Training Objective           | Examples               | Best For             |
|-------------------|------------------------------|------------------------|----------------------|
| **Encoder-only**  | Masked token prediction      | BERT, RoBERTa          | Classification, NER  |
| **Decoder-only**  | Next token prediction        | GPT, LLaMA, Mistral   | Text generation      |
| **Encoder-Decoder**| Seq-to-seq                  | T5, BART               | Translation, summary |
| **Diffusion**     | Denoising                    | Stable Diffusion, DALL-E| Image generation    |
| **GAN**           | Generator vs discriminator   | StyleGAN               | Image synthesis      |
| **VAE**           | Variational encoding         | VQ-VAE                 | Image generation     |

### Model Sizes

```
Parameters = Weights the model learns during training

Small:    < 1B parameters    (efficient, edge deployment)
Medium:   1B – 13B           (good balance)
Large:    13B – 70B          (high capability)
Frontier: 70B+               (best performance)
```

---

## Transformer Architecture

The backbone of modern GenAI, introduced in "Attention Is All You Need" (2017).

### Core Components

```
Input → Tokenization → Embedding + Positional Encoding
  → [Attention → Feed-Forward → Layer Norm] × N layers
  → Output Projection → Token Probabilities
```

### Self-Attention Mechanism

```
Attention(Q, K, V) = softmax(QKᵀ / √dₖ) × V

Q = Query   (what am I looking for?)
K = Key     (what do I contain?)
V = Value   (what information do I provide?)
dₖ = dimension of keys (scaling factor)
```

### Multi-Head Attention

```
MultiHead(Q, K, V) = Concat(head₁, head₂, ..., headₕ) × Wᴼ

Each head learns different relationships:
  - Head 1: Subject-verb agreement
  - Head 2: Coreference resolution
  - Head 3: Positional relationships
```

### Key Innovations

| Component                  | Purpose                                    |
|----------------------------|--------------------------------------------|
| **Self-Attention**         | Relate every token to every other token    |
| **Multi-Head Attention**   | Learn multiple types of relationships      |
| **Positional Encoding**    | Preserve word order information            |
| **Layer Normalization**    | Stabilize training                         |
| **Residual Connections**   | Enable deep networks                       |
| **Feed-Forward Networks**  | Process each position independently        |

---

## Large Language Models (LLMs)

### How LLMs Work

```
Training:    Massive text corpus → Learn next-token prediction
Inference:   Prompt → Generate tokens one at a time (autoregressive)

"The cat sat on the" → P(mat)=0.3, P(roof)=0.2, P(chair)=0.15 ...
```

### Pre-training → Fine-tuning → Alignment Pipeline

```
1. Pre-training (unsupervised)
   └─ Next token prediction on massive text data
   └─ Internet text, books, code (trillions of tokens)

2. Supervised Fine-Tuning (SFT)
   └─ Train on curated instruction→response pairs
   └─ Makes model follow instructions

3. Alignment (RLHF / DPO / RLAIF)
   └─ Reinforcement Learning from Human Feedback
   └─ Makes model helpful, harmless, honest
```

### Capabilities

| Capability            | Examples                                        |
|-----------------------|-------------------------------------------------|
| **Text Generation**   | Essays, stories, emails, code                   |
| **Summarization**     | Condense long documents                         |
| **Translation**       | Between languages                               |
| **Q&A**               | Answer questions from context                   |
| **Reasoning**         | Multi-step logic problems                       |
| **Code Generation**   | Write, debug, explain code                      |
| **Conversation**      | Multi-turn dialogue                             |
| **Analysis**          | Sentiment, classification, extraction           |

### Limitations

- **Hallucination** — generates plausible but false information
- **Knowledge cutoff** — no info beyond training data date
- **Context limits** — finite window size
- **Bias** — reflects biases in training data
- **No real understanding** — pattern matching, not reasoning (debated)
- **Expensive** — large compute costs

---

## Prompt Engineering

### Prompt Structure

```
[System Message]     — Sets behavior, role, constraints
[Context/Examples]   — Background info, few-shot examples
[User Instruction]   — The actual task/question
[Output Format]      — How to structure the response
```

### Core Techniques

#### Zero-Shot

```
Classify the sentiment of this review as positive, negative, or neutral:
"The movie was absolutely breathtaking!" → 
```

#### Few-Shot

```
Classify sentiment:
"Great product!" → Positive
"Terrible service." → Negative
"It was okay." → Neutral
"The movie was absolutely breathtaking!" →
```

#### Chain-of-Thought (CoT)

```
Q: If a store has 45 apples and sells 3/5 of them, how many remain?
A: Let me think step by step.
   - Total apples: 45
   - Sold: 3/5 × 45 = 27
   - Remaining: 45 - 27 = 18
   The answer is 18.
```

#### System Prompts

```
You are a professional data scientist. When answering:
- Use precise technical language.
- Always include code examples in Python.
- Cite your reasoning.
- If unsure, say so rather than guessing.
```

### Advanced Techniques

| Technique            | Description                                   |
|----------------------|-----------------------------------------------|
| **ReAct**            | Reasoning + Acting (think → act → observe)    |
| **Tree of Thought**  | Explore multiple reasoning paths              |
| **Self-Consistency** | Generate multiple answers, take majority vote  |
| **Structured Output**| Request JSON, XML, or specific formats        |
| **Role Prompting**   | Assign an expert persona                      |
| **Constraint Setting**| Define boundaries and rules                  |

### Prompt Best Practices

```
✅ Be specific and clear
✅ Provide examples when possible
✅ Break complex tasks into steps
✅ Specify output format explicitly
✅ Use delimiters (```, ---, ###) to separate sections
✅ Include relevant context
✅ Iterate and refine

❌ Vague instructions ("make it better")
❌ Overly complex single prompts
❌ Assuming model has current information
❌ Ignoring output format specification
```

---

## Retrieval-Augmented Generation (RAG)

RAG combines LLMs with external knowledge retrieval to generate grounded, up-to-date responses.

### RAG Pipeline

```
1. Document Ingestion
   └─ Load documents → Chunk text → Generate embeddings → Store in vector DB

2. Query Time
   └─ User query → Embed query → Search vector DB → Retrieve top-k chunks

3. Generation
   └─ Combine: [System prompt + Retrieved chunks + User query] → LLM → Response
```

### Key Components

| Component         | Options                                       |
|-------------------|-----------------------------------------------|
| **Embedding Model**| OpenAI, Cohere, Sentence-Transformers, Gemini|
| **Vector Database**| Pinecone, Weaviate, ChromaDB, Qdrant, FAISS  |
| **Chunking**      | Fixed-size, semantic, recursive text splitting |
| **Reranking**      | Cohere Rerank, cross-encoders                 |
| **LLM**           | GPT-4o, Claude, Gemini, Llama                 |

### RAG Code Example

```python
from langchain.document_loaders import PyPDFLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain.embeddings import OpenAIEmbeddings
from langchain.vectorstores import ChromaDB
from langchain.chains import RetrievalQA

# 1. Load & chunk
loader = PyPDFLoader("document.pdf")
docs = loader.load()
splitter = RecursiveCharacterTextSplitter(chunk_size=1000, chunk_overlap=200)
chunks = splitter.split_documents(docs)

# 2. Embed & store
embeddings = OpenAIEmbeddings()
vectorstore = ChromaDB.from_documents(chunks, embeddings)

# 3. Query
retriever = vectorstore.as_retriever(search_kwargs={"k": 5})
qa_chain = RetrievalQA.from_chain_type(llm=llm, retriever=retriever)
answer = qa_chain.run("What is the main finding?")
```

### RAG vs Fine-Tuning

| Aspect              | RAG                          | Fine-Tuning                  |
|---------------------|------------------------------|------------------------------|
| **Knowledge**       | External, dynamic            | Baked into weights           |
| **Cost**            | Low (no training)            | High (GPU training)          |
| **Freshness**       | Always up-to-date            | Snapshot at training time    |
| **Hallucination**   | Reduced (grounded)           | Still possible               |
| **Best For**        | Knowledge-heavy Q&A          | Style/behavior adaptation    |

---

## Fine-Tuning Techniques

### Full Fine-Tuning

Train all parameters on task-specific data. Expensive but powerful.

### Parameter-Efficient Fine-Tuning (PEFT)

| Method       | Description                              | Parameters Modified |
|--------------|------------------------------------------|---------------------|
| **LoRA**     | Low-Rank Adaptation — inject small matrices | ~0.1-1% of total |
| **QLoRA**    | LoRA on quantized model (4-bit)          | ~0.1% + quantized  |
| **Prefix Tuning** | Learn virtual token prefixes        | ~0.1%               |
| **Adapters** | Insert small modules between layers       | ~1-5%               |

### LoRA Explained

```
Original layer:  y = Wx

LoRA modification:  y = Wx + BAx
  where B (d×r) and A (r×d), r << d

Only A and B are trained (rank r ≈ 4-64)
Drastically reduces trainable parameters
```

### RLHF (Reinforcement Learning from Human Feedback)

```
1. Collect human preferences on model outputs
2. Train a Reward Model on those preferences
3. Use PPO/DPO to optimize the LLM against the Reward Model
```

### DPO (Direct Preference Optimization)

Simpler alternative to RLHF — directly optimize from preference pairs without a separate reward model.

---

## Image Generation

### Diffusion Models

```
Forward Process:   Image → Add noise gradually → Pure noise
Reverse Process:   Pure noise → Denoise step by step → Image

Training: Learn to predict/remove noise at each step
Inference: Start from random noise, iteratively denoise
```

### Text-to-Image Models

| Model              | Organization | Key Feature                       |
|--------------------|-------------|-----------------------------------|
| **DALL-E 3**       | OpenAI      | High quality, safety filters      |
| **Stable Diffusion**| Stability AI| Open-source, highly customizable |
| **Midjourney**     | Midjourney  | Artistic, high aesthetic quality  |
| **Imagen 3**       | Google      | Photorealistic, text rendering    |
| **Flux**           | Black Forest| Open, high quality                |

### Other Modalities

| Modality          | Models / Tools                             |
|-------------------|--------------------------------------------|
| **Video**         | Sora, Runway Gen-3, Kling, Veo            |
| **Audio/Music**   | Suno, Udio, MusicLM                       |
| **Speech**        | ElevenLabs, Bark, XTTS                    |
| **3D**            | Point-E, Shape-E, Meshy                   |
| **Code**          | Codex, Copilot, Cursor, Codeium           |

---

## Evaluation & Safety

### LLM Evaluation Metrics

| Metric          | What It Measures                              |
|-----------------|-----------------------------------------------|
| **Perplexity**  | How surprised the model is (lower = better)   |
| **BLEU**        | N-gram overlap with reference (translation)   |
| **ROUGE**       | Recall of reference n-grams (summarization)   |
| **BERTScore**   | Semantic similarity using embeddings           |
| **Human Eval**  | Pass@k for code generation                    |
| **MMLU**        | Multi-task language understanding benchmark   |
| **HumanEval**   | Human preference rankings                     |

### Safety & Responsible AI

| Concern              | Description                                   |
|----------------------|-----------------------------------------------|
| **Hallucination**    | Generating false/fabricated information        |
| **Bias**             | Reinforcing stereotypes from training data     |
| **Privacy**          | Memorizing/leaking training data              |
| **Misuse**           | Deepfakes, spam, disinformation               |
| **Copyright**        | Training on copyrighted content               |
| **Environmental**    | Large compute carbon footprint                |

### Mitigation Strategies

- **Guardrails**: Input/output filtering, content moderation
- **RLHF/Constitutional AI**: Align with human values
- **Watermarking**: Mark AI-generated content
- **Red Teaming**: Adversarial testing for vulnerabilities
- **Transparency**: Disclose when AI-generated
- **Grounding**: Use RAG to reduce hallucination

---

## GenAI Application Stack

```
┌─────────────────────────────────────────┐
│          User Interface                 │  Chat, API, plugins
├─────────────────────────────────────────┤
│       Application Layer                 │  Orchestration, agents
├─────────────────────────────────────────┤
│       RAG / Memory / Tools              │  Vector DB, search, APIs
├─────────────────────────────────────────┤
│       Model Layer                       │  LLM, embeddings, vision
├─────────────────────────────────────────┤
│       Infrastructure                    │  GPU cloud, serving, MLOps
└─────────────────────────────────────────┘
```

---

## Key Models Directory

### Proprietary Models

| Model          | Provider   | Strengths                          |
|----------------|-----------|-------------------------------------|
| GPT-4o         | OpenAI    | Multi-modal, strong reasoning       |
| Claude 3.5     | Anthropic | Long context, safety-focused        |
| Gemini 1.5/2.0 | Google    | Massive context, multi-modal        |
| Command R+     | Cohere    | Enterprise RAG, multilingual        |

### Open-Weight Models

| Model          | Provider   | Sizes          | Strengths                |
|----------------|-----------|----------------|--------------------------|
| Llama 3        | Meta      | 8B, 70B, 405B  | Versatile, strong base   |
| Mistral/Mixtral| Mistral   | 7B, 8×7B, 8×22B| Efficient, MoE           |
| Gemma 2        | Google    | 2B, 9B, 27B    | Efficient, high quality  |
| Qwen 2.5       | Alibaba   | 0.5B–72B       | Multilingual, coding     |
| Phi-3/4        | Microsoft | 3.8B, 14B      | Small but capable        |
| DeepSeek V3/R1 | DeepSeek  | 67B, 685B MoE  | Reasoning, math          |

---

## Tools & Frameworks

| Tool               | Purpose                                    |
|--------------------|--------------------------------------------|
| **LangChain**      | LLM application framework (chains, agents) |
| **LlamaIndex**     | Data framework for RAG                     |
| **Hugging Face**   | Model hub, Transformers library            |
| **OpenAI API**     | GPT, DALL-E, Whisper access                |
| **Google AI Studio**| Gemini access and prototyping             |
| **Ollama**         | Run open models locally                    |
| **vLLM**           | Fast LLM serving                           |
| **Weights & Biases**| Experiment tracking                       |
| **Gradio**         | Quick ML demos / UI                        |
| **Streamlit**      | Data app framework                         |

---

## Glossary

| Term                   | Definition                                        |
|------------------------|---------------------------------------------------|
| **Token**              | Smallest unit of text processed by a model        |
| **Embedding**          | Dense vector representation of data               |
| **Attention**          | Mechanism to weigh relevance of input parts       |
| **Transformer**        | Architecture using self-attention                 |
| **Fine-tuning**        | Adapting a pre-trained model to a specific task   |
| **Inference**          | Using a trained model to generate outputs         |
| **Hallucination**      | Model generating false information confidently    |
| **Grounding**          | Connecting model output to verified sources       |
| **Context window**     | Max tokens a model can process at once            |
| **Foundation model**   | Large model trained on broad data, adaptable      |
| **RLHF**              | Training with human preference feedback           |
| **LoRA**              | Parameter-efficient fine-tuning via low-rank matrices|
| **RAG**               | Combining retrieval with generation               |
| **Multi-modal**        | Processing multiple data types (text+image+audio) |
| **Quantization**       | Reducing model precision (FP32→INT8/INT4)        |
| **MoE**               | Mixture of Experts — activate subset of parameters|
| **Distillation**       | Training a small model to mimic a large one       |

---

> **Last Updated:** April 2026
