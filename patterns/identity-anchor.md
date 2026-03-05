# Pattern: The Identity Anchor

> How to prevent agent identity drift in multi-agent deployments

## The Problem

After 50+ conversation turns, most agents start sounding like generic assistants. Their personality fades. Their voice flattens. They forget who they are.

In multi-agent systems, it's worse — agents start blending into each other. Your sales agent sounds like your research agent. Your support agent starts giving strategic advice. Identity boundaries dissolve.

## The Solution: CLAUDE.md as Identity Anchor

Every agent gets a dedicated identity file loaded at the start of each turn. Not buried in a system prompt — it's the **first thing** the agent reads.

```markdown
# Agent Name — Role Title

- **Name:** Felix | **Role:** CEO | **Emoji:** 📋
- **Mission:** [One sentence. What this agent exists to do.]
- **Voice:** [2-3 adjectives defining communication style]

## Hard Stops
- [Things this agent must NEVER do]
- [Boundaries that prevent identity bleed]

## Tools & Capabilities
- [What this agent can access]
- [What it explicitly cannot — prevents role confusion]

## Verification
When starting a new task, confirm you've read this file
by stating your name and current mission.
```

## Why This Works

1. **Recency bias** — The identity file is loaded fresh every turn, creating a strong prior
2. **Explicit boundaries** — Hard stops prevent the agent from drifting into another agent's role
3. **Verification canary** — Forces the agent to re-anchor before each task
4. **Tool scoping** — An agent that can't send emails won't try to send emails

## Anti-Patterns

| Don't | Why |
|-------|-----|
| Share system prompts across agents with minor tweaks | Creates shared identity = no identity |
| Put identity info at the bottom of long prompts | Gets buried in context and ignored |
| Define identity by what the agent IS rather than what it DOES | "You are helpful" vs "You close deals" |
| Skip hard stops | Without explicit boundaries, roles blur within days |

## Results

Running this pattern across a 12-agent production system:
- Zero identity drift over 6+ months
- Each agent maintains distinct voice even in collaborative tasks
- New agents adopt coherent identity from day one

## Production-Tested Templates

See the [templates directory](../templates/) for copy-paste CLAUDE.md files for common agent roles.

---

## Want this built for your business?

We deploy multi-agent AI teams for businesses — the same architecture described here. Custom identity, memory, and tool configuration for each agent. One-time setup from $399.

→ [See how it works](https://getmilo.dev/agents)
→ [Calculate your savings](https://getmilo.dev/calculator)

Built by [Milo](https://getmilo.dev) — running 12 agents in production, every day.
