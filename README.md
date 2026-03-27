# Seed Signal Sell — Demo Control Panel

Backstage trigger UI for the **Signal Autopilot** demo. Used during live walkthroughs to fire lifecycle signals that trigger automated outbound actions in the Lead Activation Dashboard (LAD).

**Live demo:** https://charlesalzon-alt.github.io/slf-1371/

## What is this?

A single-page HTML control panel with two trigger buttons:

| Button | Signal | What it triggers |
|---|---|---|
| **Soft Signal** (amber) | Property valuation +10% | Custom event in LAD → SMS to customer → activity logged |
| **Hard Signal** (red) | Listing detected | Custom event in LAD → voice agent call → transcript logged → contact request |

Each button fires a webhook to an ActivePieces flow that orchestrates the full automation sequence.

## Demo Flow

```
Act 1: Onboarding (Jasser on-screen)
  → Tour LAD, create lead, send microsite

Act 2: Soft Signal (operator fires from this UI)
  → Valuation +10% event → SMS sent → engagement tracked

Act 3: Hard Signal (operator fires from this UI)
  → Listing detected event → voice call → contact request
```

## Setup

1. Open `index.html` in a browser (or visit the GitHub Pages URL)
2. Expand **Configuration** at the bottom
3. Enter the ActivePieces webhook URLs for Flow A (soft) and Flow B (hard)
4. Enter the LAD API bearer token
5. Click **Save Configuration** (persists to localStorage)
6. Fill in lead details (name, phone, email, property address)
7. Hold a trigger button for ~1 second to fire

## Configuration

| Field | Description |
|---|---|
| Webhook URL — Soft Signal | `https://pricehubble.activepieces.com/api/v1/webhooks/EflDWLq9Wm3EX1qZtw3YF` |
| Webhook URL — Hard Signal | `https://pricehubble.activepieces.com/api/v1/webhooks/G0js5MO2f00Nd9xP9LFbD` |
| LAD API Bearer Token | Auth token for `api-predev.pricehubble.net` |
| LAD Events API URL | `https://api-predev.pricehubble.net/api/v1/leads/events` |

## Webhook Payload

Both signals send the same structure:

```json
{
  "signal_type": "valuation_increase | listing_detected",
  "lead_name": "Jack Miller",
  "phone_number": "+1 555 123 4567",
  "email": "jack@example.com",
  "property_address": "742 Evergreen Terrace, Springfield",
  "details": {
    "change_percent": 10,
    "description": "..."
  }
}
```

## Technical Details

- **Zero dependencies** — vanilla HTML/CSS/JS
- **Hosted on GitHub Pages** — no build step, no server
- **Config persisted** in localStorage
- **Hold-to-confirm** on trigger buttons (800ms hold) to prevent accidental fires
- **Event log** with timestamped entries

## Project Context

- **Jira Epic:** [SLF-1371](https://pricehubble.atlassian.net/browse/SLF-1371)
- **Slack:** #signal-autopilot-demo
- **Owner:** Charles Alzon
- **Deadline:** Tuesday noon
