# 🤖 Agentic AI — Cheatsheet

> A comprehensive reference on AI agents — architectures, design patterns, tools, and frameworks.

---

## Table of Contents

1. [What is Agentic AI?](#what-is-agentic-ai)
2. [Agents vs Chatbots](#agents-vs-chatbots)
3. [Core Agent Architecture](#core-agent-architecture)
4. [Agent Design Patterns](#agent-design-patterns)
5. [Reasoning Frameworks](#reasoning-frameworks)
6. [Tool Use & Function Calling](#tool-use--function-calling)
7. [Memory Systems](#memory-systems)
8. [Planning & Task Decomposition](#planning--task-decomposition)
9. [Multi-Agent Systems](#multi-agent-systems)
10. [RAG in Agentic Systems](#rag-in-agentic-systems)
11. [Evaluation & Reliability](#evaluation--reliability)
12. [Frameworks & Tools](#frameworks--tools)
13. [Real-World Applications](#real-world-applications)
14. [Best Practices](#best-practices)
15. [Glossary](#glossary)

---

## What is Agentic AI?

Agentic AI refers to AI systems that can **autonomously plan, reason, use tools, and take actions** to accomplish goals — going beyond simple question-answering to executing multi-step workflows.

```
Traditional LLM:   User → Prompt → Response → Done

Agentic AI:        User → Goal
                     ↓
                   Agent: Plan → Reason → Act → Observe
                     ↓          ↑__________________|
                   Result (after multiple iterations)
```

### Key Properties of AI Agents

| Property         | Description                                        |
|------------------|----------------------------------------------------|
| **Autonomy**     | Operate independently after receiving a goal       |
| **Reasoning**    | Think through problems step-by-step                |
| **Tool Use**     | Call APIs, search the web, execute code             |
| **Memory**       | Remember context across interactions               |
| **Planning**     | Break down complex tasks into sub-tasks            |
| **Adaptation**   | Adjust strategy based on results                   |
| **Reflection**   | Evaluate own outputs and self-correct              |

---

## Agents vs Chatbots

| Feature          | Chatbot                    | AI Agent                          |
|------------------|----------------------------|-----------------------------------|
| **Interaction**  | Single turn Q&A            | Multi-step autonomous execution   |
| **Tools**        | None or limited            | Multiple external tools           |
| **Planning**     | None                       | Task decomposition & planning     |
| **Memory**       | Conversation history only  | Short-term + long-term memory     |
| **Autonomy**     | Responds to prompts        | Initiates actions toward goals    |
| **Error Handling**| Returns error message     | Retries, adjusts strategy         |
| **Scope**        | Answer questions           | Complete complex workflows        |

---

## Core Agent Architecture

### The Agent Loop

```
┌──────────────────────────────────────────────┐
│                 AGENT LOOP                   │
│                                              │
│  ┌─────────┐    ┌─────────┐    ┌─────────┐  │
│  │ PERCEIVE│ →  │  REASON │ →  │   ACT   │  │
│  │(observe │    │(think & │    │(execute │  │
│  │ results)│    │  plan)  │    │  tool)  │  │
│  └────▲────┘    └─────────┘    └────┬────┘  │
│       │                             │        │
│       └─────────────────────────────┘        │
│              (loop until done)               │
└──────────────────────────────────────────────┘
```

### Core Components

```
┌────────────────────────────────────────────┐
│                AI AGENT                    │
│                                            │
│  ┌──────────────────────────────────────┐  │
│  │          LLM (Brain)                 │  │
│  │   Reasoning · Planning · Language    │  │
│  └──────────────────────────────────────┘  │
│                                            │
│  ┌──────────┐ ┌──────────┐ ┌───────────┐  │
│  │  Tools   │ │  Memory  │ │ Planning  │  │
│  │ API calls│ │ Short &  │ │ Task      │  │
│  │ Code exec│ │ Long-term│ │ decompose │  │
│  │ Search   │ │ Vector DB│ │ Prioritize│  │
│  └──────────┘ └──────────┘ └───────────┘  │
│                                            │
│  ┌──────────────────────────────────────┐  │
│  │       Orchestration Layer            │  │
│  │  Prompt management · Error handling  │  │
│  │  State management · Control flow     │  │
│  └──────────────────────────────────────┘  │
└────────────────────────────────────────────┘
```

---

## Agent Design Patterns

### 1. ReAct (Reasoning + Acting)

The agent **thinks** (reason), **acts** (use a tool), then **observes** the result in a loop.

```
Thought: I need to find the current weather in Tokyo.
Action: search_web("current weather Tokyo")
Observation: Tokyo is currently 18°C, partly cloudy.
Thought: Now I have the weather info. Let me formulate my response.
Answer: The current weather in Tokyo is 18°C and partly cloudy.
```

### 2. Plan-and-Execute

Separate planning from execution. Create a full plan first, then execute steps.

```
Goal: "Write a blog post about AI trends"

Plan:
  1. Research latest AI trends for 2026
  2. Identify top 5 trends
  3. Write introduction
  4. Write section for each trend
  5. Write conclusion
  6. Review and polish

Execute: step 1 → step 2 → ... → step 6
```

### 3. Reflection / Self-Critique

Agent evaluates its own output and iteratively improves.

```
Generate → Critique → Revise → Critique → Final Output

Step 1: Write initial code solution
Step 2: Review — "This doesn't handle edge case X"
Step 3: Revise code to handle edge case X
Step 4: Review — "Looks good now"
Step 5: Return final solution
```

### 4. Tool-Use Agent

Agent decides which tool to use based on the task.

```
User: "What's 15% tip on a $67.50 bill?"

Agent thinks: This is a math calculation. I'll use the calculator tool.
Action: calculator(67.50 * 0.15)
Result: $10.13
```

### 5. Router / Dispatch

Routes requests to specialized sub-agents or workflows.

```
User Query → Router Agent
  ├── "Code question"     → Code Agent
  ├── "Data analysis"     → Data Agent
  ├── "Creative writing"  → Writing Agent
  └── "General question"  → General Agent
```

### 6. Multi-Agent Collaboration

Multiple specialized agents work together on a task.

```
┌──────────┐     ┌──────────┐     ┌──────────┐
│ Research │ →   │ Writer   │ →   │ Editor   │
│  Agent   │     │  Agent   │     │  Agent   │
└──────────┘     └──────────┘     └──────────┘
  Gathers info    Drafts content   Reviews & polishes
```

### Pattern Comparison

| Pattern           | Strength                    | When to Use                    |
|-------------------|-----------------------------|--------------------------------|
| **ReAct**         | Flexible, general-purpose   | Dynamic tasks with tools       |
| **Plan-Execute**  | Structured, predictable     | Complex multi-step workflows   |
| **Reflection**    | Self-improving output       | Quality-critical tasks         |
| **Tool-Use**      | Extends LLM capabilities    | Tasks needing external data    |
| **Router**        | Efficient specialization    | Diverse query types            |
| **Multi-Agent**   | Division of labor           | Complex, multi-faceted tasks   |

---

## Reasoning Frameworks

### Chain-of-Thought (CoT)

Step-by-step reasoning before answering.

```
Q: A store has 45 apples. It sells 2/3 on Monday, then receives 20 on Tuesday.
   How many apples does it have?

Thought: Start with 45. Sell 2/3: 45 × 2/3 = 30 sold, 15 remain.
         Receive 20: 15 + 20 = 35.
Answer: 35 apples.
```

### Tree of Thoughts (ToT)

Explore multiple reasoning paths, evaluate, and select the best.

```
Problem → Path A → evaluate (score: 0.6)
        → Path B → evaluate (score: 0.9) ← selected
        → Path C → evaluate (score: 0.3)
```

### Self-Consistency

Generate multiple reasoning chains, take majority vote.

```
Attempt 1: ... → Answer: 42
Attempt 2: ... → Answer: 42
Attempt 3: ... → Answer: 38
Majority vote → 42 ✓
```

### ReAct + Scratchpad

Combine reasoning with working memory.

```
Scratchpad:
  - Found that revenue = $5.2M
  - Found that costs = $3.1M
  - Calculated profit = $5.2M - $3.1M = $2.1M
  - Need to compare with last quarter ($1.8M)
```

---

## Tool Use & Function Calling

### How Function Calling Works

```
1. Define tools (functions) with names, descriptions, parameters
2. LLM decides which tool to call and with what arguments
3. System executes the function
4. Result is fed back to the LLM
5. LLM incorporates result into its response
```

### Defining Tools

```python
tools = [
    {
        "type": "function",
        "function": {
            "name": "get_weather",
            "description": "Get current weather for a city",
            "parameters": {
                "type": "object",
                "properties": {
                    "city": {
                        "type": "string",
                        "description": "City name, e.g., 'London'"
                    },
                    "unit": {
                        "type": "string",
                        "enum": ["celsius", "fahrenheit"],
                        "description": "Temperature unit"
                    }
                },
                "required": ["city"]
            }
        }
    }
]
```

### Common Tool Categories

| Category           | Examples                                     |
|--------------------|----------------------------------------------|
| **Search**         | Web search, document search, vector search   |
| **Code Execution** | Python REPL, shell commands, sandboxes       |
| **Data**           | SQL queries, API calls, file read/write      |
| **Communication**  | Email, Slack, calendar                       |
| **Browser**        | Navigate, click, extract content             |
| **Math**           | Calculator, symbolic math                    |
| **Media**          | Image generation, audio processing           |

### MCP (Model Context Protocol)

An open standard by Anthropic for connecting AI agents to external tools and data sources.

```
┌─────────┐     MCP Protocol     ┌─────────────┐
│  Agent  │  ←──────────────────→ │  MCP Server  │
│ (Client)│   JSON-RPC over stdio │ (Tool Host)  │
└─────────┘                       └─────────────┘

MCP Server exposes:
  - Tools (functions the agent can call)
  - Resources (data the agent can read)
  - Prompts (templates for the agent)
```

---

## Memory Systems

### Types of Memory

| Type              | Scope                  | Implementation              |
|-------------------|------------------------|-----------------------------|
| **Working Memory**| Current conversation   | Context window / scratchpad |
| **Short-Term**    | Current session        | Conversation buffer         |
| **Long-Term**     | Across sessions        | Vector DB, knowledge graph  |
| **Episodic**      | Past experiences       | Stored interaction logs     |
| **Semantic**      | Facts & knowledge      | Embeddings in vector store  |
| **Procedural**    | How to do things       | Stored tool-use patterns    |

### Memory Implementation

```python
# Short-term: Conversation buffer
memory = ConversationBufferMemory()

# Sliding window (last N messages)
memory = ConversationBufferWindowMemory(k=10)

# Summary memory (summarize old messages)
memory = ConversationSummaryMemory(llm=llm)

# Long-term: Vector store
from langchain.memory import VectorStoreRetrieverMemory
memory = VectorStoreRetrieverMemory(retriever=vectorstore.as_retriever())
```

### Memory Management Strategies

```
Context Window Management:
  1. Sliding window — keep last N messages
  2. Summarization — compress old messages
  3. Retrieval — fetch relevant past context on demand
  4. Hierarchical — recent in full + older as summaries
```

---

## Planning & Task Decomposition

### Decomposition Strategies

```
Goal: "Build a data dashboard for sales data"

Hierarchical Decomposition:
├── 1. Data Pipeline
│   ├── 1.1 Connect to sales database
│   ├── 1.2 Extract relevant tables
│   └── 1.3 Clean and transform data
├── 2. Analysis
│   ├── 2.1 Compute KPIs (revenue, growth, churn)
│   ├── 2.2 Identify trends
│   └── 2.3 Segment by region
├── 3. Visualization
│   ├── 3.1 Create charts for each KPI
│   ├── 3.2 Build interactive filters
│   └── 3.3 Design layout
└── 4. Deployment
    ├── 4.1 Set up hosting
    └── 4.2 Configure auto-refresh
```

### Dynamic Re-Planning

```
Original Plan: A → B → C → D

After step B fails:
  Agent reflects: "Step B failed because API is down."
  New Plan: A → B' (alternative approach) → C → D

After step C reveals new info:
  Agent reflects: "Step C showed we also need E."
  New Plan: A → B' → C → E → D
```

---

## Multi-Agent Systems

### Common Architectures

#### 1. Sequential Pipeline

```
Agent A → Agent B → Agent C → Final Output
(research)  (write)   (review)
```

#### 2. Hierarchical (Manager-Worker)

```
        ┌───────────┐
        │  Manager  │
        │  Agent    │
        └─────┬─────┘
     ┌────────┼────────┐
     ▼        ▼        ▼
┌────────┐┌────────┐┌────────┐
│Worker A││Worker B││Worker C│
└────────┘└────────┘└────────┘
```

#### 3. Debate / Adversarial

```
┌──────────┐        ┌──────────┐
│ Agent A  │ ←───→  │ Agent B  │
│(Proposer)│ debate │ (Critic) │
└──────────┘        └──────────┘
       ↓
  ┌──────────┐
  │  Judge   │  → Final Decision
  └──────────┘
```

#### 4. Collaborative Swarm

```
┌────┐  ┌────┐  ┌────┐
│ A  │──│ B  │──│ C  │
└────┘  └────┘  └────┘
  │  \   │  \   │
┌────┐ ┌────┐ ┌────┐
│ D  │─│ E  │─│ F  │
└────┘ └────┘ └────┘
Agents communicate peer-to-peer
```

### Multi-Agent Frameworks

| Framework       | Key Feature                     | Best For                    |
|-----------------|-------------------------------- |-----------------------------|
| **CrewAI**      | Role-based agent teams          | Collaborative workflows     |
| **AutoGen**     | Conversational multi-agent      | Research, coding tasks      |
| **LangGraph**   | Graph-based agent orchestration | Complex stateful workflows  |
| **Swarm**       | Lightweight agent handoffs      | Simple multi-agent flows    |
| **Agency Swarm**| Genesis-based agent creation    | Business automation         |

---

## RAG in Agentic Systems

### Agentic RAG vs Naive RAG

| Feature           | Naive RAG                    | Agentic RAG                     |
|-------------------|------------------------------|---------------------------------|
| **Retrieval**     | Single query → retrieve      | Multiple queries, iterative     |
| **Reasoning**     | None                         | Decides what to retrieve        |
| **Re-ranking**    | Basic similarity             | LLM-guided relevance scoring    |
| **Query Rewrite** | None                         | Reformulates for better results |
| **Source Selection** | Fixed                     | Dynamically selects sources     |
| **Verification**  | None                         | Cross-checks retrieved info     |

### Agentic RAG Pattern

```
User Query → Agent
  ├── Analyze query
  ├── Decide which sources to search
  ├── Search Source A (vector DB)
  ├── Search Source B (web search)
  ├── Evaluate relevance of results
  ├── If insufficient → reformulate query → search again
  ├── Synthesize information
  └── Generate grounded response with citations
```

---

## Evaluation & Reliability

### Agent Evaluation Dimensions

| Dimension        | Measures                                       |
|------------------|------------------------------------------------|
| **Correctness**  | Does the agent produce the right answer?       |
| **Efficiency**   | How many steps / tokens / time to complete?    |
| **Reliability**  | Does it succeed consistently?                  |
| **Safety**       | Does it avoid harmful actions?                 |
| **Tool Use**     | Does it select the right tools correctly?      |
| **Robustness**   | How does it handle errors and edge cases?      |

### Common Failure Modes

| Failure               | Description                              | Mitigation                   |
|-----------------------|------------------------------------------|------------------------------|
| **Infinite loops**    | Agent repeats failed actions             | Max iteration limits         |
| **Wrong tool use**    | Calls incorrect tool or wrong params     | Better tool descriptions     |
| **Hallucination**     | Invents data instead of searching        | Mandatory tool use policies  |
| **Goal drift**        | Wanders from original objective          | Goal tracking, checkpoints   |
| **Error cascading**   | One mistake compounds across steps       | Validation at each step      |
| **Over-planning**     | Spends too long planning, never acts     | Planning time limits         |

### Reliability Techniques

```
1. Structured outputs   — Force JSON/schema responses
2. Retry with backoff   — Retry failed tool calls
3. Guardrails           — Input/output validation
4. Human-in-the-loop    — Approval for critical actions
5. Checkpointing        — Save state for recovery
6. Fallback chains      — Alternative paths on failure
7. Observability        — Log every step for debugging
```

---

## Frameworks & Tools

### Agent Frameworks

| Framework         | Language | Key Feature                           |
|-------------------|----------|---------------------------------------|
| **LangChain**     | Python   | Comprehensive agent toolkit           |
| **LangGraph**     | Python   | Stateful graph-based agents           |
| **CrewAI**        | Python   | Multi-agent role-based teams          |
| **AutoGen**       | Python   | Conversational multi-agent            |
| **Semantic Kernel**| C#/Py   | Microsoft's AI orchestration          |
| **Haystack**      | Python   | Production-ready NLP pipelines        |
| **Smolagents**    | Python   | HuggingFace lightweight agents        |
| **Google ADK**    | Python   | Google's Agent Development Kit        |

### Orchestration & Infrastructure

| Tool              | Purpose                                    |
|-------------------|--------------------------------------------|
| **LangSmith**     | Agent observability & tracing              |
| **Weights & Biases** | Experiment tracking                     |
| **Ollama**        | Run LLMs locally                           |
| **vLLM**          | High-throughput LLM serving                |
| **Modal / Fly.io**| Serverless deployment                      |
| **Vercel AI SDK** | Frontend AI integration                    |

### Vector Databases

| Database      | Key Feature                              |
|---------------|------------------------------------------|
| **Pinecone**  | Managed, scalable                        |
| **Weaviate**  | Open-source, hybrid search               |
| **ChromaDB**  | Simple, developer-friendly               |
| **Qdrant**    | High performance, filtering              |
| **FAISS**     | Facebook's similarity search library     |
| **Milvus**    | Open-source, distributed                 |

---

## Real-World Applications

| Domain              | Use Case                                       |
|---------------------|-------------------------------------------------|
| **Software Dev**    | Code generation, debugging, PR review agents   |
| **Customer Support**| Multi-step issue resolution, ticket routing     |
| **Research**        | Literature review, data analysis agents         |
| **Data Analysis**   | SQL query generation, dashboard creation        |
| **Sales/Marketing** | Lead research, content generation, outreach     |
| **Legal**           | Contract review, document analysis              |
| **Healthcare**      | Symptom triage, report summarization            |
| **Finance**         | Financial analysis, risk assessment             |
| **DevOps**          | Incident response, log analysis                 |
| **Education**       | Personalized tutoring, curriculum design        |

---

## Best Practices

### Agent Design

```
✅ Start simple — single agent before multi-agent
✅ Define clear tool descriptions with examples
✅ Implement human-in-the-loop for critical actions
✅ Set max iteration limits to prevent infinite loops
✅ Use structured outputs for reliable parsing
✅ Log every step for debugging and observability
✅ Test with diverse inputs and edge cases
✅ Implement graceful error handling and fallbacks

❌ Don't give agents too many tools at once
❌ Don't skip input validation
❌ Don't trust agent output without verification
❌ Don't deploy without guardrails
❌ Don't ignore cost — agent loops can be expensive
```

### Cost Optimization

```
1. Use cheaper models for simple reasoning steps
2. Cache frequent tool call results
3. Limit max iterations and token budgets
4. Use smaller context windows when possible
5. Batch similar operations
6. Monitor and alert on cost spikes
```

### Security Considerations

```
1. Sandbox code execution environments
2. Validate all tool inputs — prevent injection attacks
3. Limit agent permissions (principle of least privilege)
4. Audit trail for all agent actions
5. Rate limiting on tool calls
6. Never expose raw credentials to agent context
7. Content filtering on inputs and outputs
```

---

## Glossary

| Term                 | Definition                                          |
|----------------------|-----------------------------------------------------|
| **Agent**            | AI system that autonomously reasons and acts        |
| **Tool**             | External function an agent can call                 |
| **Function Calling** | LLM generating structured tool invocations          |
| **ReAct**            | Reasoning + Acting loop pattern                     |
| **Orchestration**    | Managing agent execution flow                       |
| **Guardrails**       | Safety constraints on agent behavior                |
| **Human-in-the-loop**| Human approval step in agent workflows             |
| **MCP**              | Model Context Protocol for tool integration         |
| **Scratchpad**       | Working memory for intermediate reasoning           |
| **Grounding**        | Anchoring responses to verified data                |
| **Handoff**          | Transferring task from one agent to another         |
| **Observation**      | Result from a tool call fed back to the agent       |
| **State**            | Current status and context of the agent             |
| **Trajectory**       | Sequence of thought-action-observation steps        |
| **Hallucination**    | Agent fabricating info instead of using tools       |

---

> **Last Updated:** April 2026
