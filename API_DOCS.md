# SmartRouter API Reference

## Authentication

All API requests require an API key passed via the `Authorization` header:

```
Authorization: Bearer sr_live_xxxxxxxxxxxxxxxx
```

Contact Empire Labs (contact@empirelabs.com.au) for a due diligence key.

---

## Endpoints

### POST `/v1/route`

Route a prompt to the optimal LLM provider.

**Request:**

```json
{
    "prompt": "Explain quantum computing to a 10-year-old",
    "quality_floor": 0.7,
    "max_cost": 0.05,
    "preferred_providers": ["deepseek-chat"],
    "mode": "weighted"
}
```

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `prompt` | string | (required) | The input text to route |
| `quality_floor` | float | 0.0 | Minimum acceptable quality score (0.0 - 1.0) |
| `max_cost` | float | null | Maximum acceptable cost per request in USD |
| `preferred_providers` | array | [] | Optional list of preferred provider names |
| `mode` | string | "weighted" | "weighted" or "pareto" |

**Response:**

```json
{
    "request_id": "a1b2c3d4e5f6",
    "selected_provider": "deepseek-chat",
    "quality_score": 0.912,
    "cost": 0.00014,
    "latency_ms": 800.0,
    "composite_score": 0.8473,
    "mode": "weighted",
    "all_scores": [
        {
            "provider": "deepseek-chat",
            "quality_score": 0.912,
            "cost": 0.000140,
            "latency_ms": 800.0,
            "reliability": 1.000,
            "composite_score": 0.8473
        },
        {
            "provider": "claude-sonnet-4",
            "quality_score": 0.880,
            "cost": 0.003000,
            "latency_ms": 1200.0,
            "reliability": 1.000,
            "composite_score": 0.7916
        },
        {
            "provider": "gpt-4o",
            "quality_score": 0.820,
            "cost": 0.002500,
            "latency_ms": 900.0,
            "reliability": 1.000,
            "composite_score": 0.7734
        }
    ]
}
```

---

### POST `/v1/stats`

Get routing statistics.

**Response:**

```json
{
    "total_requests": 1452,
    "total_cost": 0.284510,
    "active_providers": 3,
    "weights": {
        "quality": 0.45,
        "cost": 0.25,
        "latency": 0.15,
        "reliability": 0.10,
        "preference": 0.05
    },
    "budget": {
        "state": "NORMAL",
        "daily_spent": 0.2845,
        "daily_budget": 5.00,
        "daily_ratio": 0.057,
        "monthly_spent": 0.2845,
        "monthly_budget": 100.00,
        "monthly_ratio": 0.003
    }
}
```

---

## Modes

### Weighted Composite (Default)
Balanced optimisation across all five dimensions. The composite score is computed as:

```
composite = (0.45 × quality) - (0.25 × cost_norm) - (0.15 × latency_norm) + (0.10 × reliability) + (0.05 × preference)
```

### Pareto Mode
Minimises cost while maintaining a minimum quality floor. Use when budget is the primary constraint:

```json
{
    "mode": "pareto",
    "quality_floor": 0.80
}
```

---

## Pricing Tiers

| Tier | Price | Requests/month | Features |
|------|-------|---------------|----------|
| Free | $0 | 10,000 | SmartRouter core, 3 providers |
| Pro | $15/mo | 100,000 | Advanced routing, quality dashboard |
| Enterprise | $39/mo | 1,000,000 | Custom scoring, audit logs, SLA |
| Bundle | $99/mo | Unlimited | SmartRouter + AgentGuard + CostGuard |

---

## SDK Examples

### Python

```python
import requests

router = requests.post(
    "https://api.empirelabs.com.au/v1/route",
    headers={"Authorization": "Bearer sr_live_xxxxxxxx"},
    json={
        "prompt": "Write a Python function to sort a list of dictionaries",
        "quality_floor": 0.7,
        "mode": "weighted"
    }
)

result = router.json()
print(f"Routed to: {result['selected_provider']}")
print(f"Quality: {result['quality_score']}")
print(f"Cost: ${result['cost']}")
```

### curl

```bash
curl -X POST https://api.empirelabs.com.au/v1/route \
  -H "Authorization: Bearer sr_live_xxxxxxxx" \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "Explain machine learning simply",
    "quality_floor": 0.6
  }'
```

---

## Support

- Email: contact@empirelabs.com.au
- Patent: AMCZ-2615798943 (Filed 26 May 2026)
- ACN: 693 862 145
