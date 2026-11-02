# Building AI Agents with LangGraph — Lab

Practical Lab — Building AI Agents with LangGraph.

## Setup

- Python 3.10+ (tested on Google Colab, Python 3.13)
- Free Groq API key: https://console.groq.com/keys

```bash
pip install -U langgraph langchain langchain-community langchain-groq wikipedia pandas
```

The API key is never written in the code. Set it as an environment variable:

```bash
export GROQ_API_KEY="your_key_here"
```

In Colab, the notebook asks for it with `getpass` (hidden input).

## How to run

- **Notebook:** open `langgraph_lab_Statement.ipynb` in Colab and run all cells in order (restart the session after the first `pip install`).

## Design choices

| Choice | Reason |
|---|---|
| Groq, model `openai/gpt-oss-120b` | Free API with good tool calling; the OpenAI account had no credit. Only the LLM definition changes. |
| Safe calculator (`ast`) | `eval()` can run arbitrary code; `ast` allows arithmetic only. |
| Keyword-based routing | Robust if the LLM adds extra words; unknown labels go to GENERAL. |
| `add_messages` + `MemorySaver` | History is appended, and each `thread_id` keeps its own memory. |
| One shared `tools` node | Tools defined once; control returns to the agent that called the tool. |
| Error handling | Empty input → fallback; API errors caught; `handle_tool_errors=True`; Wikipedia retries limited to 2. |
| Local Mermaid rendering | mermaid.ink rejects the `tools` node label (error 400). |
| `phd_database` (SQLite) | Optional extension; parameterized queries; fictitious data. |
