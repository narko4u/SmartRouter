# SmartRouter — Patent Specification
**Australian Provisional Patent: AMCZ-2615798943**
**Filed: 26 May 2026**
**Applicant: Empire Labs Pty Ltd (ACN 693 862 145)**

---

## Title
System and Method for Quality-Aware Intelligent Routing of Large Language Model Inference Requests

## Patent Claims

### Claim 1: Composite Scoring Function
A method for routing inference requests across multiple LLM providers, comprising:
- A composite scoring function with tunable weights across dimensions including quality, cost, latency, reliability, and preference;
- Real-time computation of composite scores for each provider-task pair;
- Selection of the highest-scoring provider for each incoming request.

### Claim 2: Quality Score Engine
The method of Claim 1, wherein the quality dimension is scored by:
- Continuous benchmarking across 200+ task categories;
- Prompt classification into benchmark categories;
- Normalised quality scoring (0.0 - 1.0) per provider per category.

### Claim 3: Circuit Breaker State Machine
The method of Claim 1, further comprising a circuit breaker with states:
- CLOSED: normal operation, requests pass through;
- OPEN: provider removed from routing, requests blocked;
- HALF-OPEN: limited test requests permitted during recovery.

### Claim 4: Cost Governance State Machine
The method of Claim 1, further comprising a cost governor with states:
- NORMAL: unrestricted routing;
- WARN: restricted to cheaper providers (spent > 70% budget);  
- CRITICAL: restricted to low-cost providers (spent > 90% budget);
- HALT: all requests blocked (budget exhausted);
- RECOVERY: gradual restoration on budget reset.

### Claim 5: Hot-Reloadable Provider Registry
The method of Claim 1, wherein the provider registry supports:
- Runtime addition and removal of providers without service interruption;
- Dynamic credential rotation;
- Real-time configuration updates.

## Filing Strategy

| Phase | Timeline | Cost | 
|-------|----------|------|
| Australian Provisional | 26 May 2026 | $100 |
| PCT International | May 2027 | $2,000 - $4,000 |
| National Phase Entry | May 2028 | $5,000 - $15,000 |

## Patent Family

- SmartRouter (Filed) — Composite scoring + circuit breaker + cost governance
- AgentGuard (Q3 2026) — Action-level AI security
- CostGuard (Q3 2026) — Multi-currency budget enforcement
