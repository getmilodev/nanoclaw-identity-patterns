# AI Agent Blueprint: Pest Control Companies

> How a 3-agent AI team handles emergency calls, books recurring service, and reactivates seasonal customers — 24/7.

## The Problem

Pest control is emergency-driven. When a homeowner finds a scorpion in the nursery or a rat in the kitchen, they call the first company they find and go with whoever picks up. The second company doesn't get a callback — they get a 1-star "never answered" review.

**The math:**
- Average pest control job: $200-800
- Calls missed per month (industry average): 20-30%
- Revenue lost to voicemail: $3,000-5,000/month
- Cost of an AI agent team that never misses a call: $30-60/month

## The 3-Agent System

### Agent 1: Emergency Triage (`pest-triage`)

**What it does:** Answers every call/text/email instantly. Identifies the pest type, urgency level, and location. Routes emergencies (scorpions, rats, wasps near children) for same-day dispatch. Queues routine requests (quarterly spray, annual termite inspection) for next-available.

**Identity configuration:**
```yaml
name: pest-triage
role: Emergency intake and triage specialist
personality: Calm, reassuring, efficient
knowledge:
  - Pest identification (common species by region)
  - Urgency classification (health risk, property damage, nuisance)
  - Service area boundaries and tech availability
  - Pricing tiers (emergency vs. scheduled vs. recurring)
guardrails:
  - Never diagnose or guarantee outcomes
  - Always confirm address and access instructions
  - Escalate if customer mentions injury or allergic reaction
```

**Example interaction:**
```
Customer: "There's a huge spider in my bathroom and I have kids"
Agent: "I understand — that's stressful, especially with kids in the house. 
Let me get someone to you quickly. Can you tell me your address? 
And if possible, can you describe the spider — brown, black, 
any markings? This helps our tech bring the right treatment."
```

### Agent 2: Booking & Scheduling (`pest-scheduler`)

**What it does:** Manages the full service calendar. Books new appointments based on tech availability and location clustering (routes techs efficiently). Sends confirmation texts, reminder texts (24hr and 2hr before), and handles rescheduling without human intervention.

**Key capabilities:**
- Checks real calendar availability (Google Calendar / ServiceTitan integration)
- Clusters appointments by geography (reduces drive time)
- Handles rescheduling via text ("Running late, can we push to 3pm?")
- Sends post-service follow-up ("How did everything go? Any concerns?")

### Agent 3: Seasonal Reactivation (`pest-reactivator`)

**What it does:** Identifies customers who haven't booked in 6+ months. Sends personalized reactivation messages timed to their local pest season. Tracks which customers convert and optimizes messaging over time.

**Seasonal triggers:**
- **Spring:** Ant season → target homes that had ant issues last year
- **Summer:** Mosquito/wasp season → offer yard treatment packages
- **Fall:** Rodent season → "They're looking for warmth, let's keep them out"
- **Winter:** Termite inspection reminder → annual contracts

**Example reactivation message:**
```
"Hey [Name], it's [Company]. Ant season is starting early this year 
in [City] — we treated your home last April and wanted to check in. 
Want us to schedule your spring treatment? Same tech, same time slot 
if it works for you. Reply YES and we'll lock it in."
```

## Results You Can Expect

| Metric | Before AI | After AI |
|--------|-----------|----------|
| Calls answered | 70-80% | 100% |
| After-hours coverage | None | 24/7 |
| Average response time | 2-4 hours | Under 60 seconds |
| Seasonal reactivation rate | 5-10% | 25-35% |
| Monthly admin hours saved | 0 | 15-20 hours |

## Setup

One-time setup: $299 (case study rate, normally $399)  
Monthly running cost: $30-60 (hosting + AI API)  
Timeline: Live within 48 hours

→ [Get started](https://getmilo.dev/agents)

---

*Built by [Milo](https://getmilo.dev) — AI agent teams for service businesses.*
