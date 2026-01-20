---
title: About LangChain
date: 2026-12-02
summary: 📝 Quick notes on LangChain...
tags:
  - ai
  - agent
  - llm
  - langchain
---

LangChain is an orchestration framework designed to simplify the development of applications powered by LLMs.

At its core, raw LLMs are text-processing engines: you put text in, and get text out. Building a real application, however, requires connecting that engine to the outside world—databases, user context, API tools, and conversation history. LangChain provides the glue to make these connections robust and reusable.

---

## 1. The Core Philosophy: "Chains" and Composition

The fundamental problem LangChain solves is **composability**.

In a standard script, you might manually send a prompt to OpenAI, get the string back, parse it with RegEx, and then send it to a database. LangChain standardizes these steps into reusable components that can be "chained" together.

### The Shift to LCEL (LangChain Expression Language)

Modern LangChain uses a declarative syntax called **LCEL**. Instead of writing Python functions to call other functions, you define a workflow using the Unix pipe operator (`|`).

- **The Old Way:** "Take variable A, pass it to Function B, save result to C, pass C to Function D."
- **The LCEL Way:** `A | B | D`

This makes pipelines easy to read and modify. Every object in this pipeline follows a standard protocol called the **Runnable Interface**, meaning every component behaves the same way (they all have methods like `.invoke()`, `.stream()`, and `.batch()`).

---

## 2. The Atomic Building Blocks

Before understanding complex architectures, you must understand the three specific primitives that make up 90% of LangChain apps.

### A. Models (The Engine)

LangChain distinguishes between two types of models, though they often use the same underlying technology:

1.  **LLMs:** Pure text completion. Input is a string; output is a string. (Legacy)
2.  **Chat Models:** Structured conversation. Input is a list of Messages (System, Human, AI); output is an AI Message. (Modern Standard)

**Why this matters:** Chat Models allow you to separate "instructions" (System Message) from "user queries" (Human Message), which significantly reduces prompt injection risks and improves adherence to instructions.

### B. Prompts (The Instructions)

Hardcoding strings into your code is brittle. **Prompt Templates** allow you to treat prompts like code. They separate the _structure_ of the prompt from the _variables_ inside it.

- _Concept:_ Think of a Prompt Template as a "Python f-string on steroids." It handles logic, distinct roles (System vs. User), and formatting automatically.

### C. Output Parsers (The Translator)

LLMs return unstructured text. If you are building a software application, you usually need structured data (JSON, Lists, Dates) to pass to the next step of your code.

- **Output Parsers** take the raw text from the LLM and transform it into a specific Python object.

---

## 3. Retrieval Augmented Generation (RAG)

RAG is the most common use case for LangChain. It solves the problem: "How do I get the LLM to know about my private data?"

The process is a pipeline that runs in two phases:

### Phase 1: Indexing (The Setup)

1.  **Load:** You bring data in from a source (PDF, SQL, Website).
2.  **Split:** LLMs have a limit on how much text they can read (context window). You must break large documents into smaller "chunks."
3.  **Embed:** You turn those chunks into **Vectors** (lists of numbers) using an Embedding Model. This represents the _meaning_ of the text mathematically.
4.  **Store:** You save these vectors in a **Vector Database**.

### Phase 2: Retrieval (The Runtime)

When a user asks a question:

1.  LangChain converts the user's question into a vector.
2.  It scans the database for chunks that are mathematically similar (relevant) to the question.
3.  It retrieves those text chunks.
4.  It pastes those chunks into the System Prompt context.
5.  It sends the combined prompt to the LLM to generate an answer.

---

## 4. Agents: From "Chains" to "Reasoning"

If a **Chain** is a railroad track (train goes from A to B to C), an **Agent** is a taxi driver.

In a Chain, the sequence of steps is hardcoded by you. In an Agent, the **LLM acts as the reasoning engine**.

1.  You give the Agent a suite of **Tools** (e.g., "Google Search", "Calculator", "Check Weather API").
2.  The User asks a question.
3.  The Agent decides: "Do I have the answer? No. I need to use the 'Google Search' tool."
4.  It generates the search input, executes the tool, observes the output, and _then_ decides what to do next.

This creates a dynamic loop: **Thought -> Action -> Observation -> Thought**.

---

## 5. Minimal Code Example (LCEL)

Here is a necessary example to visualize how the "Atomic Blocks" (Prompt, Model, Parser) connect using the LCEL pipe syntax (`|`).

This example translates user input into a Joke structure.

```python
from langchain_openai import ChatOpenAI
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.output_parsers import StrOutputParser

# 1. The Model
model = ChatOpenAI(model="gpt-4")

# 2. The Prompt (Dynamic instructions)
prompt = ChatPromptTemplate.from_template(
    "Tell me a short joke about {topic}."
)

# 3. The Output Parser (Sanitizes the output)
parser = StrOutputParser()

# 4. The Chain (Connecting them with Pipes)
chain = prompt | model | parser

# 5. Execution
# The dictionary input fills the {topic} variable in the prompt
result = chain.invoke({"topic": "ice cream"})

print(result)
# Output: "Why did the ice cream truck break down? Because of the rocky road!"
```

---

Powered by Gemini.
