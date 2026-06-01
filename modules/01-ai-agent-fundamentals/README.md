# Module 01 — AI Agent Fundamentals

> Understand what an AI agent actually is, how it reasons, and how it acts in the world.

**Prerequisites:** None. This is the starting point.

---

## What you'll understand after this module

- The difference between a plain LLM call and an agent
- How the agent loop works at a conceptual level
- What tool use looks like under the hood
- The different types of memory an agent can have
- Why agents fail and how to think about that

---

## Lessons

> Lessons are released alongside the video series. Check [ROADMAP.md](../../ROADMAP.md) for status.

### 1. What is an AI agent?

An AI agent is a system that perceives its environment, reasons about it, takes actions, and observes the results — in a loop, until a goal is reached.

A plain LLM takes input and returns output, once. An agent wraps that capability in a loop and gives it the ability to *do things* — browse the web, run code, read files, call APIs.

The key properties:
- **Autonomy** — it decides what to do next without a human approving each step
- **Tool use** — it can interact with systems beyond the model itself
- **Goal-directedness** — it keeps going until the task is done or it gets stuck

### 2. The agent loop

Every agent runs some version of this loop:

```
while task not complete:
    observation = perceive(environment)
    thought     = model.think(observation + history)
    action      = model.decide(thought)
    result      = execute(action)
    history.append(result)
```

The model never "runs" between steps — it just generates the next token given everything that came before. The **harness** (see Module 02) manages the loop, calls tools, and feeds results back in.

### 3. ReAct — Reasoning + Acting

ReAct is the dominant pattern for agent behavior. At each step, the model:

1. **Reasons** — writes a `<thought>` about what it knows and what to do next
2. **Acts** — calls a tool or returns a final answer

The reasoning step matters because it forces the model to make its intermediate logic legible — both to humans and to itself.

### 4. Tool use

Tools are functions the agent can call. The model doesn't execute them — it *requests* them in a structured format, the harness executes them, and the result is fed back as an observation.

Example tool definition (JSON schema):

```json
{
  "name": "read_file",
  "description": "Read the contents of a file at a given path.",
  "input_schema": {
    "type": "object",
    "properties": {
      "path": {
        "type": "string",
        "description": "Absolute path to the file"
      }
    },
    "required": ["path"]
  }
}
```

The model sees this schema in its context and knows it can call `read_file`. When it does, the harness intercepts the call, runs the actual function, and returns the output.

### 5. Memory types

Agents have access to several kinds of memory:

| Type | Where it lives | Persists across sessions? |
|------|---------------|--------------------------|
| In-context | The active conversation window | No |
| External (retrieval) | A vector store or database | Yes |
| Episodic | Logs of past actions and outcomes | Yes (if stored) |
| Semantic | Structured facts about the world | Yes (if stored) |

Most simple agents only use in-context memory. More sophisticated systems combine all four.

---

## Code examples

> Coming soon — will be added alongside the video lessons.

---

## Further reading

- [The Operator's Edge — Turi Labs](../../resources/The%20Operator's%20Edge%20%E2%80%94%20Turi%20Labs.pdf)
- ReAct: Synergizing Reasoning and Acting in Language Models (Yao et al., 2022)
- Toolformer: Language Models Can Teach Themselves to Use Tools (Schick et al., 2023)

---

**Next:** [Module 02 — Harnesses & Tools](../02-harnesses-and-tools/)
