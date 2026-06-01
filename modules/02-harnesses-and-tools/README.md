# Module 02 — Harnesses & Tools

> Understand the system that wraps the model — how it manages the loop, handles tool calls, and keeps the agent on track.

**Prerequisites:** [Module 01 — AI Agent Fundamentals](../01-ai-agent-fundamentals/)

---

## What you'll understand after this module

- What a harness is and why it exists
- How the harness drives the agent loop
- How tool definitions, calls, and results flow through the system
- What Claude Code looks like as a real harness
- How to build a minimal harness from scratch

---

## Lessons

> Lessons are released alongside the video series. Check [ROADMAP.md](../../ROADMAP.md) for status.

### 1. What is a harness?

The **harness** is everything around the model. It's the code that:

- Sends messages to the model
- Receives the model's response
- Detects when the model wants to call a tool
- Executes the tool and feeds the result back
- Decides when to stop

The model itself is stateless — it just predicts the next token. The harness gives it persistence, tools, and the ability to run in a loop.

Think of the harness as the environment the agent lives in. The model is the brain; the harness is the body and the world.

### 2. Anatomy of an agent harness

A minimal harness has five components:

```
┌─────────────────────────────────────────┐
│                 Harness                  │
│                                          │
│  ┌──────────┐     ┌────────────────┐    │
│  │  History  │────▶│  Model Client  │    │
│  │ (messages)│◀────│  (API calls)   │    │
│  └──────────┘     └────────────────┘    │
│        │                  │             │
│        ▼                  ▼             │
│  ┌──────────┐     ┌────────────────┐    │
│  │   Loop   │     │  Tool Executor │    │
│  │ Controller│────▶│  (runs tools) │    │
│  └──────────┘     └────────────────┘    │
│                                          │
└─────────────────────────────────────────┘
```

- **History** — the growing list of messages (user, assistant, tool results)
- **Model client** — makes API calls to the underlying model
- **Loop controller** — decides whether to keep going or stop
- **Tool executor** — maps tool names to actual functions and runs them

### 3. Tool definitions and JSON schemas

Every tool is described by a JSON schema the model sees in its context:

```json
{
  "name": "bash",
  "description": "Run a shell command and return stdout/stderr.",
  "input_schema": {
    "type": "object",
    "properties": {
      "command": {
        "type": "string",
        "description": "The shell command to execute"
      },
      "timeout": {
        "type": "integer",
        "description": "Timeout in milliseconds (default: 30000)"
      }
    },
    "required": ["command"]
  }
}
```

When the model wants to call this tool, it returns a structured response containing the tool name and inputs. The harness parses it, runs the function, and appends the result as a `tool_result` message.

### 4. Managing the conversation loop

Here's what one iteration of the loop looks like in pseudocode:

```python
def run_agent(task: str, tools: list[Tool]) -> str:
    messages = [{"role": "user", "content": task}]

    while True:
        response = model.complete(messages=messages, tools=tools)

        if response.stop_reason == "end_turn":
            return response.content  # done

        if response.stop_reason == "tool_use":
            tool_results = []
            for tool_call in response.tool_calls:
                result = execute_tool(tool_call.name, tool_call.inputs)
                tool_results.append({
                    "tool_use_id": tool_call.id,
                    "content": result
                })

            messages.append({"role": "assistant", "content": response.content})
            messages.append({"role": "user", "content": tool_results})
            # loop continues
```

This is the core of every agent harness. Everything else is built on top of this.

### 5. Claude Code as a harness (case study)

Claude Code is a production harness built by Anthropic. It wraps Claude with:

- A rich tool set (Bash, Read, Write, Edit, WebFetch, etc.)
- Permission controls (which tools require user approval)
- Conversation compaction (to handle long sessions)
- Hooks (shell commands that run before/after tool calls)
- A skill system (slash commands that inject additional context)

Studying Claude Code is one of the best ways to understand what a serious harness looks like in practice.

### 6. Building a minimal harness

> Code example coming alongside the video lesson.

We'll build a harness from scratch in ~100 lines of Python using the Anthropic SDK. It will support two tools (`bash` and `read_file`) and run a simple task end-to-end.

---

## Code examples

> Coming soon — will be added alongside the video lessons.

---

## Further reading

- [Anthropic API Tool Use docs](https://docs.anthropic.com/en/docs/tool-use)
- [Claude Code documentation](https://docs.anthropic.com/en/docs/claude-code)

---

**Previous:** [Module 01 — AI Agent Fundamentals](../01-ai-agent-fundamentals/)  
**Next:** [Module 03 — Operators & Principals](../03-operators-and-principals/)
