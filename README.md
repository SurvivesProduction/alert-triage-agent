# Alert Triage Agent (n8n)

An AI agent that handles the first 15 minutes of incident response: it reads a production alert, figures out what actually broke, and posts a clean summary to Slack — with a severity call and a suggested first step — before you've opened your laptop.

Built on the **Survives Production** YouTube channel: https://www.youtube.com/@survivesproduction

**Watch the full build:** [FILL IN: video link after publish]

## What it does

```
Alert (webhook) → AI classification → context enrichment → router
                                                            ├─ noise      → quiet log
                                                            ├─ real issue → Slack summary (severity + first step)
                                                            └─ critical   → escalation path
```

And the part most tutorials skip — the 3 layers that keep it alive in production:

1. **Retries with delay** — a failed API call or rate limit doesn't kill the workflow.
2. **Error workflow** — when a step fails for real, the failure lands somewhere a human will see it. Never silently.
3. **Heartbeat** — if the agent goes quiet, the silence itself alerts you. A down agent and an agent with nothing to say look identical until a customer tells you the difference.

## Quick start

1. Import `alert-triage-agent.json` into n8n (self-hosted or [n8n Cloud free trial](https://n8n.io) — 14 days).
2. Add your credentials: an LLM API key and a Slack connection (use a test workspace first).
3. Point your monitoring webhook at the workflow's webhook URL — or simulate alerts with the payloads in `test-payloads.json`:
   ```bash
   curl -X POST <your-webhook-url> -H "Content-Type: application/json" -d @test-payloads.json
   ```
4. Run the checklist in `checklist.md` before you trust it with anything real.

## Files

| File | What it is |
|---|---|
| `alert-triage-agent.json` | The complete n8n workflow — import and go |
| `checklist.md` | The production error-handling checklist I use at work |
| `test-payloads.json` | Simulated alerts (P1, P2, noise, heartbeat) for testing |

## …but does it survive production?

Every project on the channel ends with a stress test. This one: kill the Slack credential mid-run and watch the failure get caught, logged, and escalated instead of vanishing. If you break it in a way I didn't handle, open an issue — the best breakage gets a video.

---

*New build every week: https://www.youtube.com/@survivesproduction*
