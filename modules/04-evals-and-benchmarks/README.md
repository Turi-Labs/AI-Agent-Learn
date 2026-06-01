# Module 04 — Evals & Benchmarks

> Learn how to measure whether your agent actually works — and build the eval infrastructure to prove it.

**Prerequisites:** [Module 01 — AI Agent Fundamentals](../01-ai-agent-fundamentals/)

---

## What you'll understand after this module

- Why evals are the hardest and most important part of building agents
- The different types of evals and when to use each
- How to design rubrics for open-ended agent tasks
- What macro evals are and why they matter for agentic systems
- How to build a minimal eval suite for your own agent

---

## Lessons

> Lessons are released alongside the video series. Check [ROADMAP.md](../../ROADMAP.md) for status.

### 1. Why evals matter

You cannot improve what you cannot measure. This is true for software generally, but it's especially true for agents — because:

- Agent behavior is non-deterministic
- Tasks are often open-ended with no single correct answer
- Failure modes are subtle (the agent *does* something, just the wrong thing)
- Small prompt changes can have large and unexpected behavioral effects

Without evals, you're flying blind. You might fix one thing and break three others without knowing.

### 2. Types of evals

#### Unit evals
Test a single, well-defined capability in isolation.

```
Input:  "What is 2 + 2?"
Expected: "4"
Pass/fail: exact match or semantic equivalence
```

Good for: factual retrieval, format compliance, tool call correctness

#### Trajectory evals
Test the *sequence* of actions the agent takes, not just the final output.

```
Task: "Find all Python files modified in the last 24 hours and summarize changes"
Eval: Did the agent call the right tools in a reasonable order? 
      Did it read before writing? Did it avoid unnecessary actions?
```

Good for: multi-step tasks, efficiency, safety (checking the agent doesn't do harmful things along the way)

#### End-to-end evals
Test whether the final output achieves the stated goal.

```
Task: "Refactor this function to be more readable"
Eval: Is the output code cleaner? Does it still pass tests? 
      Is the logic preserved?
```

Good for: overall capability, regression testing, comparing model versions

### 3. Macro evals for agentic systems

**Macro evals** test the agent's behavior across a diverse set of realistic, complex tasks — not toy examples. They are designed to catch systemic issues that unit tests miss.

Key properties of a good macro eval:

| Property | What it means |
|----------|--------------|
| **Realistic** | Tasks that reflect actual use cases, not synthetic benchmarks |
| **Diverse** | Covers many task types, edge cases, and failure modes |
| **Gradable** | Has a clear rubric, even if scoring is partially manual |
| **Reproducible** | Same task + same agent → same evaluation conditions |

See the [Macro Evals for Agentic Systems paper](../../resources/macro-evals-agentic-systems.pdf) for a deep treatment of this.

### 4. Rubrics and scoring

For open-ended tasks, you need a rubric — a structured breakdown of what "good" looks like.

Example rubric for a code review task:

```
Score 0-3 on each dimension:

Correctness:    Did the agent identify real bugs? (0 = missed all, 3 = found all)
Relevance:      Did it avoid false positives? (0 = noisy, 3 = precise)
Actionability:  Are suggestions concrete and implementable? (0 = vague, 3 = specific)
Completeness:   Did it address the full scope? (0 = partial, 3 = thorough)

Total: /12
```

Rubric-based scoring lets you use LLM-as-judge for automated evaluation while keeping the criteria auditable and human-interpretable.

### 5. Building your own eval suite

A minimal eval suite has three components:

**1. Task bank** — a curated set of tasks with clear success criteria
```python
tasks = [
    {"input": "...", "expected_behavior": "...", "rubric": {...}},
    ...
]
```

**2. Runner** — executes each task against the agent
```python
def run_eval(agent, tasks):
    results = []
    for task in tasks:
        output = agent.run(task["input"])
        results.append({"task": task, "output": output})
    return results
```

**3. Scorer** — grades each result
```python
def score(results, rubric):
    # can be rule-based, LLM-as-judge, or human review
    ...
```

Start small: 10 well-designed tasks tell you more than 1000 poorly designed ones.

### 6. Common failure modes

| Failure | What it looks like | What to test for |
|---------|--------------------|-----------------|
| Hallucination | Agent invents facts, files, or tool results | Ground truth checks |
| Sycophancy | Agent agrees with wrong user assertions | Adversarial inputs |
| Incomplete tasks | Agent stops before finishing | Completion checks |
| Unsafe actions | Agent takes irreversible actions without confirmation | Action logging |
| Loop failure | Agent repeats the same action endlessly | Step count limits |
| Over-caution | Agent refuses valid requests | False refusal rate |

---

## Code examples

> Coming soon — will include a working eval runner and a sample task bank.

---

## Further reading

- [Macro Evals for Agentic Systems](../../resources/macro-evals-agentic-systems.pdf) — required reading for this module
- HELM: Holistic Evaluation of Language Models (Liang et al., 2022)
- AgentBench: Evaluating LLMs as Agents (Liu et al., 2023)

---

**Previous:** [Module 03 — Operators & Principals](../03-operators-and-principals/)
