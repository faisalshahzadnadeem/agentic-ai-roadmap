# agentic-ai-roadmap
Learning and building agentic AI systems step by step using LangChain, LangGraph, CrewAI, AutoGen, and more.

# week 1 Agentic AI Core Concepts:
A guide to the fundamental building blocks and operational patterns of intelligent, goal-oriented AI agents. This repository breaks down the concepts that move beyond simple LLM prompts into dynamic, reasoning AI systems.

🧠 Overview
Traditional AI chains follow a predetermined path. Agentic AI represents a paradigm shift: instead of a fixed sequence, we create a system that uses a Large Language Model (LLM) as a reasoning engine to dynamically decide what actions to take and in what order to achieve a goal.

This document covers the core components that make up an agent and the operational patterns that make it robust and reliable.

🧱 Core Components
1. Agents vs. Chains
The fundamental shift from a static path to a dynamic navigator.

Concept	Description	Analogy	Example
Chains	A deterministic, predefined sequence of steps. The execution flow is fixed and linear.	Following a recipe. Step 1, then 2, then 3.	A "Translate & Summarize" chain that always translates text first, then summarizes the result.
Agents	A goal-oriented system that uses an LLM to reason about and decide the sequence of actions. The path is dynamic.	A travel agent. You state a goal, and they plan the route, choosing tools (flights, hotels) as needed.	A research agent that decides to search the web, then perform a calculation based on the results, then formulate an answer.
In essence: A chain is a path, an agent is a navigator.

2. Tools
Functions that an agent can call to interact with the outside world, extending the capabilities of the LLM.

What it is: A function with a name, description, and input schema.

Purpose: Allows the agent to perform actions it cannot do by itself (e.g., search the web, run code, query a database).

Examples:

web_search(query: str) -> str

python_repl(code: str) -> str (Execute Python code)

calculator(expression: str) -> float

sql_query(db: str, query: str) -> List[Dict]

3. Function Calling
The primary mechanism by which an LLM interacts with Tools. The model outputs a structured request to call a function.

How it works: The LLM is given a user prompt and a list of available tools. It doesn't execute code; instead, it outputs a JSON object specifying which function to call and with what arguments.

Example:

User Query: "What is 15% of 280?"

LLM Output (Structured JSON):

json
{
  "name": "calculator",
  "arguments": {"expression": "0.15 * 280"}
}
The agent framework executes calculator("0.15 * 280") and returns the result 42 to the LLM.

4. Structured Output
Constraining the LLM's final output to a specific, predictable format instead of free-form text.

Why it matters: Essential for integrating agents with other software systems. Enables reliable parsing and automation.

Example: An agent that returns user data must conform to a schema.

python
# Pydantic Model (Schema)
class UserProfile(BaseModel):
    username: str
    email: str
    signup_date: date
    is_active: bool
Agent Output:

json
{
  "username": "john_doe",
  "email": "john.doe@email.com",
  "signup_date": "2023-11-05",
  "is_active": true
}
⚙️ Operational Patterns
These patterns are the control logic that makes agents intelligent, safe, and production-ready.

1. Routing
The agent's decision-making process for selecting the next tool or sub-agent based on the current context.

Example: A customer service chatbot receives "My order #12345 is late."

The agent routes this to the get_order_status tool.

The user follows up with "This is unacceptable!", so the agent routes the next step to the human_escalation tool.

2. Planning
High-level goal decomposition. The agent breaks a complex problem into smaller, manageable steps.

Example: Goal: "Write a blog post about AI in climate change with recent stats."

Plan:

Search for "AI climate change impact 2024".

Search for "AI energy consumption statistics 2024".

Write a blog outline based on findings.

Write the full post.

3. Retries
Handling transient failures gracefully by automatically re-attempting failed operations.

Example: An agent calls a weather API that returns a 503 Service Unavailable error. Instead of crashing, the agent waits 2 seconds and retries the call up to 3 times.

4. Timeouts
Preventing agents from getting stuck in infinite loops or waiting indefinitely for a response.

Types:

Per-Tool Timeout: Kills a single tool call if it exceeds a limit (e.g., 30 seconds).

Overall Timeout: Kills the entire agent process if it doesn't complete within a limit (e.g., 2 minutes).

5. Tool Choice
Controlling the agent's autonomy in selecting tools. This balances power with safety.

Modes:

"auto": Full autonomy. (Most powerful, least safe).

"any" / "none": Must use any tool, or must use no tools.

"required": Force the use of a specific tool. (e.g., tool_choice="web_search").

6. Guardrails
Constraints applied to the agent's inputs and outputs to ensure safety, security, and compliance. They are the "AI safety net."

Input Guardrails: Sanitize user input.

Example: A PII Scrubber removes sensitive data (credit card numbers) from the user's query before the agent processes it.

Output Guardrails: Validate and filter agent output.

Example: A Toxicity Filter scans the agent's response for harmful language and redacts it.

Example: A Fact-Checking Rail cross-references factual claims against a trusted knowledge base before presenting the answer.





