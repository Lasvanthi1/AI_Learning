# 🛍️ Smart Shop Assistant

A simple AI-powered shopping assistant built with **Python, Groq, Llama 3.3, function/tool calling, and Gradio**.

The assistant can understand a user's request, decide whether it needs to use a tool, call the `get_price()` function to retrieve an item's price, and then generate a natural-language response.

## 🚀 Project Overview

This project demonstrates the basic workflow of an **AI agent with tool calling**:

```text
User
  │
  ▼
Gradio Chat Interface
  │
  ▼
AI Agent
  │
  ▼
Llama 3.3 via Groq
  │
  ├── Normal conversation
  │       │
  │       ▼
  │    AI Response
  │
  └── Price-related request
          │
          ▼
      get_price() Tool
          │
          ▼
      Tool Result
          │
          ▼
      Llama 3.3
          │
          ▼
      Final Response
```

## ✨ Features

* 🤖 AI-powered shopping assistant
* 🧠 Uses **Llama 3.3 70B Versatile**
* ⚡ Uses **Groq** for fast inference
* 🔧 Demonstrates **LLM function/tool calling**
* 💰 Retrieves prices from a Python function
* 💬 Interactive **Gradio ChatInterface**
* 🔐 API key loaded using `.env`
* 🔌 Uses the OpenAI Python SDK with Groq's OpenAI-compatible API

## 🛠️ Technologies Used

| Technology    | Purpose                         |
| ------------- | ------------------------------- |
| Python        | Application development         |
| Groq          | LLM inference                   |
| Llama 3.3 70B | Language model                  |
| OpenAI SDK    | API client and tool calling     |
| Gradio        | Chat UI                         |
| python-dotenv | Environment variable management |

## 📁 Project Structure

```text
project/
│
├── agent.py
├── app.py
├── .env
└── README.md
```

### `agent.py`

Contains the main AI agent logic.

It:

1. Creates the Groq client.
2. Defines product prices.
3. Defines the `get_price()` tool.
4. Sends the user's message and available tools to the LLM.
5. Detects tool calls.
6. Executes the requested Python function.
7. Sends the tool result back to the LLM.
8. Returns the final response.

The Groq client is configured using the OpenAI SDK with Groq's OpenAI-compatible endpoint.

### `app.py`

Provides the Gradio user interface.

The Gradio `ChatInterface` passes the user's message to the `agent()` function.

---

## 🔧 Product Price Tool

The application contains a simple product database:

```python
PRICES = {
    "shoes": 799,
    "hat": 399,
    "bag": 1420,
    "shorts": 1299,
    "pants": 1699
}
```

The `get_price()` function retrieves the price:

```python
def get_price(item):
    return f"₹{PRICES.get(item.lower(), 'unknown')}"
```

The function is exposed to the LLM as a tool named `get_price`.

---

## 🧠 How Tool Calling Works

The important concept demonstrated by this project is **LLM tool calling**.

When the user asks:

```text
How much are the shoes?
```

The LLM can determine that it needs the `get_price` tool.

### Step 1 — User sends a request

```text
How much are the shoes?
```

### Step 2 — LLM receives the available tools

The agent sends the message together with the `get_price` tool definition to Llama 3.3.

### Step 3 — LLM requests the tool

The response contains a tool call similar to:

```text
get_price({"item": "shoes"})
```

### Step 4 — Python executes the tool

The application extracts the arguments and calls:

```python
result = get_price(args["item"])
```

The tool result is then added to the conversation.

### Step 5 — LLM generates the final answer

The tool result is sent back to the model:

```text
Tool result: ₹799
```

The model then generates the final user-friendly response.

---

## 🔐 Environment Setup

Create a `.env` file in the project root:

```env
GROQ_API_KEY=your_groq_api_key
```

The application loads the environment variables using `python-dotenv`.

**Do not commit `.env` to GitHub.**

Add this to `.gitignore`:

```gitignore
.env
.venv/
__pycache__/
```

---

## 📦 Installation

### 1. Create a virtual environment

Windows:

```powershell
python -m venv .venv
```

Activate it:

```powershell
.venv\Scripts\activate
```

Linux/macOS:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 2. Install dependencies

```bash
pip install openai groq gradio python-dotenv
```

> The code uses the OpenAI Python SDK to communicate with Groq's OpenAI-compatible API.

### 3. Configure the API key

Create `.env`:

```env
GROQ_API_KEY=your_groq_api_key
```

---

## ▶️ Run the Application

Because `app.py` uses a relative import:

```python
from .agent import agent
```

run the application as a Python module from the project root.

For example, if your files are inside a package named `class_2`:

```bash
python -m class_2.app
```

Gradio starts the web interface using:

```python
gr.ChatInterface(
    fn=chat,
    title="🛍️ Smart Shop Assistant"
).launch(share=True)
```

---

## 💬 Example

### Price query

**User:**

```text
How much are the shoes?
```

**Agent:**

```text
The shoes cost ₹799.
```

The `get_price()` tool is triggered to retrieve the price. The current implementation contains prices for shoes, hat, bag, shorts, and pants.

### Normal conversation

**User:**

```text
Hi! What can you help with?
```

The agent can respond normally without calling the price tool.

---

## 🔍 Key Concepts Demonstrated

### 1. OpenAI-compatible API

Although the application uses the OpenAI Python SDK, the client is configured to communicate with Groq:

```python
client = OpenAI(
    api_key=os.getenv("GROQ_API_KEY"),
    base_url="https://api.groq.com/openai/v1",
)
```

### 2. Function Calling

The LLM receives a description of the available function:

```text
get_price(item)
```

and can decide when that function is required.

### 3. Tool Execution

The LLM does **not directly execute Python code**.

Instead:

```text
LLM
 ↓
Tool Call
 ↓
Python Function
 ↓
Tool Result
 ↓
LLM
 ↓
Final Answer
```

This distinction is fundamental when building AI agents.

### 4. Message-Based Agent Loop

The conversation is maintained as a list of messages:

```python
messages = [
    {"role": "user", "content": user_message}
]
```

When a tool is requested, the assistant message and tool result are appended before making the second LLM request.

---

## 🧩 Current Limitations

This is a learning/demo project, so the implementation is intentionally simple.

* Product data is hard-coded in Python.
* There is only one tool: `get_price`.
* No database is used.
* No product search functionality.
* No inventory management.
* No order placement.
* No persistent conversation memory.
* Tool calls are handled in a single agent iteration.

---

## 🚀 Possible Improvements

The project can be extended into a more complete shopping agent.

### Product Management

Add tools such as:

```text
get_product_details()
search_products()
check_stock()
```

### Shopping

Add:

```text
add_to_cart()
remove_from_cart()
view_cart()
calculate_total()
```

### Orders

Add:

```text
place_order()
get_order_status()
cancel_order()
```

### Database

Replace the hard-coded dictionary:

```python
PRICES = {...}
```

with a database such as:

```text
PostgreSQL
MySQL
SQLite
MongoDB
```

### Memory

Add conversation memory so the assistant can remember context across messages.

### Better Agent Architecture

The current flow can eventually evolve into:

```text
User
 ↓
LLM
 ↓
Tool Selection
 ↓
Tool Execution
 ↓
Tool Result
 ↓
LLM
 ↓
Final Response
```

with multiple tools and multiple iterations.

---

## 🎯 Learning Objective

This project is primarily a demonstration of how to build a **tool-using AI agent**.

The most important takeaway is:

> **The LLM decides which tool to use; your Python application executes the tool and sends the result back to the LLM.**

This is the foundation for building more advanced AI agents that can interact with databases, APIs, files, browsers, cloud services, and other external systems.

---

## 👩‍💻 Author

**Lasvanthi R.**

Learning and building AI-powered applications with Python, LLMs, tool calling, and agent architectures.
