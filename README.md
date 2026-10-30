# OpenRouter Usage for Home Assistant

Custom integration exposing your **OpenRouter credits and spend** in Home Assistant — including rolling consumption (last hour / 2 hours / 24 hours).

[![hacs_badge](https://img.shields.io/badge/HACS-Custom-41BDF5.svg)](https://github.com/hacs/integration)
![hassfest](https://github.com/e-p-s/ha-openrouter-usage/actions/workflows/hassfest.yaml/badge.svg)

## What it does

Polls `https://openrouter.ai/api/v1/credits` and `/api/v1/auth/key` every 5 minutes and creates:

| Sensor | Type | Description |
|---|---|---|
| Credit balance | USD | `total_credits − total_usage` |
| Total usage | USD | Cumulative spend since account creation |
| Key usage (month) | USD | Monthly usage of the current key |
| Usage last hour / 2 hours / 24 hours | USD | Rolling-window consumption |

### How the delta sensors work

Every poll is stored as a sample in a rolling 48 h history (`.storage/openrouter_usage.samples`, private file). Since OpenRouter usage is a cumulative counter, deltas are sums of positive increments. History starts empty; all windows are filled after a few polling cycles (24 h window after ~24 h).

## Requirements

- Home Assistant ≥ 2024.12 (tested on 2026.10)
- An OpenRouter API key ([openrouter.ai → Keys](https://openrouter.ai/settings/keys))

## Installation

### HACS (custom repository)

1. HACS → Integrations → ⋮ (top right) → **Custom repositories**
2. Add `https://github.com/e-p-s/ha-openrouter-usage`, category **Integration**
3. Install → restart Home Assistant

### Manual

1. Copy `custom_components/openrouter_usage/` to `/config/custom_components/`
2. Restart Home Assistant (a full restart, not just a reload)

## Configuration

Settings → Devices & Services → Add Integration → **“OpenRouter Usage”** → paste your API key.
The key stays editable afterwards via the integration's **Configure** (gear) button.

## Privacy

- The API key is stored only in the local config entry (`.storage`, private).
- The only outbound calls go to `openrouter.ai`.
- The sample history contains USD amounts only.

## Troubleshooting

| Problem | Cause / fix |
|---|---|
| Integration not in the search list | Full restart required after first install |
| Entry shows an error | Restart Core; check Settings → System → Logs |
| Key usage sensor missing | Key endpoint was unreachable at first poll — reload the entry |

## License

MIT — see [LICENSE](LICENSE).
