# agentic-ai-roadmap
Learning and building agentic AI systems step by step using LangChain, LangGraph, CrewAI, AutoGen, and more.


# Week 1 — Agentic AI Core Concepts

A guide to the **fundamental building blocks** and **operational patterns** of intelligent, goal-oriented AI agents.

This module breaks down the concepts that move beyond simple LLM prompts into **dynamic, reasoning systems**.

---

## 🧠 Overview

Traditional AI **chains** follow a fixed path. In contrast, **Agentic AI** is a paradigm shift: instead of executing a predetermined sequence, we build systems that use a **Large Language Model (LLM) as a reasoning engine** to decide dynamically what actions to take and in what order to achieve a goal.

This document covers:

* The **core components** of agents
* The **operational patterns** that make them reliable and production-ready

---

## 🧱 Core Components

### 1. Agents vs. Chains

| Concept    | Description                                                                                 | Analogy                                                       | Example                                                                            |
| ---------- | ------------------------------------------------------------------------------------------- | ------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| **Chains** | Deterministic, predefined sequence of steps. Linear and rigid.                              | Following a recipe step-by-step.                              | A “Translate & Summarize” chain: always translates first, then summarizes.         |
| **Agents** | Goal-oriented system. Uses an LLM to reason about the sequence of actions. Path is dynamic. | A travel agent: given a goal, they pick flights, hotels, etc. | A research agent: searches the web, runs a calculation, then formulates an answer. |

**In short:** A chain is a path. An agent is a navigator.

---

### 2. Tools

Functions an agent can call to interact with the outside world. Tools extend the raw capabilities of an LLM.

* **What it is:** A function with a name, description, and input schema.
* **Why it matters:** Enables agents to do things like search, run code, or query databases.

**Examples:**

```python
web_search(query: str) -> str
python_repl(code: str) -> str     # Execute Python code
calculator(expression: str) -> float
sql_query(db: str, query: str) -> List[Dict]
```

---

### 3. Function Calling

The mechanism by which an LLM *requests* to use a tool.

* The LLM doesn’t run code itself. It outputs a **structured JSON request** specifying which function to call and with what arguments.

**Example:**
User: *“What is 15% of 280?”*

LLM Output:

```json
{
  "name": "calculator",
  "arguments": {"expression": "0.15 * 280"}
}
```

The framework executes the tool and returns `42` to the LLM.

---

### 4. Structured Output

For automation, agents must return **predictable, schema-conformant outputs** instead of free text.

**Why:** Makes integration with other systems safe and reliable.

**Example Schema:**

```python
class UserProfile(BaseModel):
    username: str
    email: str
    signup_date: date
    is_active: bool
```

**Agent Output:**

```json
{
  "username": "john_doe",
  "email": "john.doe@email.com",
  "signup_date": "2023-11-05",
  "is_active": true
}
```

---

## ⚙️ Operational Patterns

These patterns make agents **intelligent, safe, and production-ready**.

### 1. Routing

Directing queries to the right tool or sub-agent.

* Example: *“My order #12345 is late.”* → `get_order_status` tool
* Follow-up: *“This is unacceptable!”* → escalate to `human_escalation` tool

---

### 2. Planning

Breaking down a high-level goal into smaller steps.

**Example:** *“Write a blog post on AI in climate change with recent stats.”*

1. Search for “AI climate change impact 2024”
2. Search for “AI energy consumption 2024”
3. Draft outline
4. Write full post

---

### 3. Retries

Graceful handling of transient failures.

* Example: Weather API returns `503`. Agent waits 2 seconds, retries up to 3 times.

---

### 4. Timeouts

Prevent agents from stalling.

* **Per-tool timeout:** kill a single tool if >30s
* **Global timeout:** kill the agent if >2min

---

### 5. Tool Choice

Control how much autonomy the agent has in selecting tools:

* `"auto"` → Full autonomy (powerful but risky)
* `"any"` / `"none"` → Must or must not use tools
* `"required"` → Force a specific tool (e.g., always `web_search`)

---

### 6. Guardrails

Safety nets around input and output.

* **Input guardrails:** scrub sensitive data (e.g., PII removal).
* **Output guardrails:** filter toxic content, validate factual claims.

**Examples:**

* PII scrubber removes credit card numbers before processing.
* Toxicity filter censors harmful language.
* Fact-checker validates claims against a trusted source.


