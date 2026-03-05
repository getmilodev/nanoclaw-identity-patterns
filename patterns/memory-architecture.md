# Pattern: Three-Tier Memory Architecture

> How to give your agents persistent, structured memory that actually works

## The Problem

Without structured memory, agents are goldfish. Every conversation starts from zero. They forget what they discussed yesterday, what decisions were made, and what they know about your customers.

Default agent memory (raw conversation history) creates three failure modes:

1. **Context overflow** — Long conversations push important facts out of the context window
2. **Noise accumulation** — Irrelevant details crowd out critical information
3. **Cross-agent contamination** — Shared memory means Agent A "remembers" Agent B's conversations

## The Three-Tier Architecture

```
┌─────────────────────────────────────────────┐
│  Tier 1: Full-Text Search (FTS5/SQLite)     │
│  "What did I just discuss?"                 │
│  → Recent conversation context              │
│  → Searchable within seconds                │
│  → Pruned after 7-30 days                   │
├─────────────────────────────────────────────┤
│  Tier 2: Session Continuity (Key-Value)     │
│  "What was I working on yesterday?"         │
│  → Active tasks, pending follow-ups         │
│  → Cross-conversation state                 │
│  → Updated at session boundaries            │
├─────────────────────────────────────────────┤
│  Tier 3: Long-Term Facts (Distilled)        │
│  "What do I know about this customer?"      │
│  → Extracted facts from all history          │
│  → Customer preferences, decisions, context │
│  → Never expires, but gets refined           │
└─────────────────────────────────────────────┘
```

## Implementation

### Tier 1: Immediate Recall

Store recent conversation turns in an FTS5-enabled SQLite database. Each entry includes:

```sql
CREATE VIRTUAL TABLE memory_t1 USING fts5(
    agent_id,
    timestamp,
    role,        -- 'user' or 'agent'
    content,
    session_id
);
```

Query with natural language search:

```sql
SELECT * FROM memory_t1
WHERE memory_t1 MATCH 'customer pricing question'
ORDER BY rank
LIMIT 5;
```

### Tier 2: Session State

Key-value store for active context that spans conversations:

```json
{
  "active_tasks": [
    {"id": "task-001", "description": "Follow up with Acme Corp", "due": "2026-03-08"}
  ],
  "pending_decisions": [
    {"question": "Which CRM to integrate?", "options": ["HubSpot", "Salesforce"], "owner": "user"}
  ],
  "current_context": {
    "last_topic": "Q1 pricing review",
    "last_session": "2026-03-05T14:30:00Z"
  }
}
```

### Tier 3: Long-Term Knowledge

Facts extracted from conversation history using a fast model (Haiku-class):

```
[fact] Customer prefers email over phone for updates
[fact] Budget approved for Q2 is $15,000
[fact] Primary contact at Acme is Sarah (sarah@acme.com)
[preference] Reports should be sent Monday mornings
[decision] Chose HubSpot over Salesforce on 2026-02-15
```

The extraction prompt:

```
Review this conversation and extract:
1. New facts about people, companies, or preferences
2. Decisions that were made
3. Action items with owners and deadlines

Output as structured [fact], [preference], [decision], [action] entries.
Do NOT include: greetings, small talk, or information already in the knowledge base.
```

## Critical: Per-Agent Memory Isolation

**Never share memory databases between agents.**

```yaml
agents:
  scout:
    memory_db: data/memory/scout.db      # Scout's memory only
  felix:
    memory_db: data/memory/felix.db      # Felix's memory only
  support:
    memory_db: data/memory/support.db    # Support's memory only
```

Shared memory creates shared identity. Agent A starts "remembering" Agent B's conversations, leading to confusion, wrong context, and identity drift.

If agents need to share information, use explicit handoff messages — not shared memory.

## Memory Hygiene

### What to store:
- Customer information and preferences
- Decisions and their rationale
- Task outcomes and learnings
- Key dates and deadlines

### What to NOT store:
- API keys, tokens, passwords (sanitize before storage)
- Raw email addresses and phone numbers (hash or redact)
- Conversation filler ("How are you?", "Thanks!")
- Duplicate information already in the knowledge base

### Pruning schedule:
- **Tier 1**: Auto-prune entries older than 30 days
- **Tier 2**: Clear completed tasks weekly
- **Tier 3**: Review and merge duplicate facts monthly

## Results

With three-tier memory in our 12-agent production system:

- Agents maintain context across weeks of conversations
- Zero "who are you again?" moments with returning customers
- Memory databases stay under 50MB even after months
- Query latency under 10ms for all three tiers

---

## Want this architecture deployed for your business?

We build complete AI agent teams with production-grade memory — the same three-tier system described here. Your agents remember everything important and forget the noise.

**One-time setup from $399. You own everything.**

→ [See how it works](https://getmilo.dev/agents)
→ [Calculate your ROI](https://getmilo.dev/calculator)

Built by [Milo](https://getmilo.dev) — running 12 agents in production, every day.
