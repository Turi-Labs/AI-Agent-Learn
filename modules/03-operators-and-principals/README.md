# Module 03 — Operators & Principals

> Understand the trust hierarchy that governs how AI agents behave — who can instruct them, how much, and under what conditions.

**Prerequisites:** [Module 02 — Harnesses & Tools](../02-harnesses-and-tools/)

---

## What you'll understand after this module

- What a "principal" is in the context of AI agents
- The full trust hierarchy: Anthropic → Operator → User → Model
- How operators configure agent behavior through system prompts
- How trust levels affect what a user can and can't instruct the agent to do
- Common operator patterns you'll encounter in the wild

---

## Lessons

> Lessons are released alongside the video series. Check [ROADMAP.md](../../ROADMAP.md) for status.

### 1. What is a principal?

A **principal** is any entity whose instructions an agent is expected to follow. In practice, every AI agent deployment involves multiple principals with different levels of authority.

Getting this wrong leads to either:
- An agent that does whatever any user asks (dangerous)
- An agent that is too locked down to be useful

The principal hierarchy is how you reason about this systematically.

### 2. The principal hierarchy

```
┌─────────────────────────────────────────────────────┐
│                    Anthropic                         │
│  Sets hard limits. Trains the base model values.     │
│  Cannot be overridden by anyone below.               │
└────────────────────────┬────────────────────────────┘
                         │
┌────────────────────────▼────────────────────────────┐
│                     Operator                         │
│  Deploys the agent. Writes the system prompt.        │
│  Can restrict or expand user permissions.            │
│  Operates within Anthropic's limits.                 │
└────────────────────────┬────────────────────────────┘
                         │
┌────────────────────────▼────────────────────────────┐
│                      User                            │
│  Interacts with the agent at runtime.                │
│  Can be granted elevated trust by the operator.      │
│  Operates within what the operator allows.           │
└────────────────────────┬────────────────────────────┘
                         │
┌────────────────────────▼────────────────────────────┐
│              Model (the agent itself)                │
│  Follows the hierarchy. Has its own judgment too.    │
│  Can refuse requests that violate values even if     │
│  technically permitted by the layers above.          │
└─────────────────────────────────────────────────────┘
```

Each layer can only grant permissions it has itself. An operator cannot give a user more authority than Anthropic allows. A user cannot exceed what the operator has granted.

### 3. Operators and system prompts

The operator's primary tool is the **system prompt**. It runs before any user message and sets the context for the entire session.

A system prompt can:
- Define the agent's persona and purpose
- Restrict what topics the agent will engage with
- Grant users elevated trust ("Trust the user's claims about their job title")
- Define what tools are available
- Set the tone and output format

Example operator system prompt for a coding assistant:

```
You are a coding assistant for Acme Corp engineers. You help with code review, 
debugging, and architecture questions.

Only answer questions related to software engineering. Do not discuss 
company financials, personnel matters, or anything outside your role.

Users are verified Acme engineers. You may trust their claims about 
the codebase they're working on.
```

### 4. Trust levels and permission escalation

By default, users get less inherent trust than operators — because operators have agreed to usage policies and are accountable for their deployments.

Operators can explicitly elevate user trust:

| Grant | What it means |
|-------|--------------|
| `Trust user claims about X` | Take the user's word about their role, context, or intent |
| `User has operator-level trust` | Full escalation — user can do anything the operator can |
| No grant | Default user trust — follow Anthropic's guidelines for general public |

The model must infer appropriate behavior when the operator hasn't addressed a specific situation. The guiding question: would a thoughtful operator want the agent to do this, given the evident purpose of the deployment?

### 5. Real-world operator patterns

**Restrictive operator** (customer support bot):
```
Only answer questions about our product. If the user asks about 
anything else, politely redirect them. Never discuss competitors.
```

**Expansive operator** (developer tool):
```
The user is a senior software engineer. Trust their technical judgment. 
You may execute code, read files, and make git commits when asked.
```

**Role-granting operator** (internal tool):
```
Users are verified employees. Treat their requests with the same 
trust level you would give a manager at their company.
```

**Persona operator** (consumer product):
```
You are Aria, a friendly assistant for HealthApp. Always stay in 
character. Do not reveal that you are built on Claude.
```

### 6. Designing safe operator configurations

Key questions when writing a system prompt:
1. What is the agent's purpose? Make this explicit.
2. What should it never do? State this clearly.
3. Who are the users? What trust do they deserve?
4. What happens at the edges? The model will fill gaps — make sure the gaps lead to safe defaults.

---

## Further reading

- [The Operator's Edge — Turi Labs](../../resources/The%20Operator's%20Edge%20%E2%80%94%20Turi%20Labs.pdf) — required reading for this module
- [Anthropic's Model Spec](https://anthropic.com/research/model-spec) — the source of the principal hierarchy

---

**Previous:** [Module 02 — Harnesses & Tools](../02-harnesses-and-tools/)  
**Next:** [Module 04 — Evals & Benchmarks](../04-evals-and-benchmarks/)
