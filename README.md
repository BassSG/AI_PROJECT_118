# Hermes SnD XAUUSD — Aurora Glass Dashboard

GitHub Pages edition of the deployed **Hermes SnD XAUUSD** Google Apps Script dashboard.

## Live demo

- GitHub repository: https://github.com/BassSG/AI_PROJECT_117
- Original Apps Script deployment: https://script.google.com/macros/s/AKfycbweCV370sfJFtoCOR6g19j3cizvUzKZt9JMAbMqVtk5qF0jeG68VDypo6N0FcBNRRi9Iw/exec

## What is included

- Responsive Aurora Glass dashboard UI extracted from the deployed Web App.
- Sections for overview, trade plan, SnD evidence, pipeline, history, and system controls.
- Local Demo data for XAUUSD so GitHub Pages works without exposing API keys.
- Working refresh, analysis, simulation, preflight, and test workflow buttons.
- Visible safety notice: this static edition does **not** connect to FMP, OpenRouter, MT5, or any trading endpoint.
- Real Run / Demo Run controls are simulated locally and never send an order.

## Important source limitation

A Google Apps Script `/exec` URL serves the deployed web app, but it does not expose the original `.gs` source files. This repository therefore contains a complete, safe static dashboard edition rather than pretending to recover the private Apps Script backend. The original deployment remains linked above.

To connect a real backend later, implement a server-side adapter and keep all API keys/secrets on the server. Never place FMP, OpenRouter, MT5, or webhook secrets in `index.html`.

## Run locally

```bash
python3 -m http.server 4173
# open http://127.0.0.1:4173
```

## Disclaimer

This project is a dashboard/demo UI, not financial advice. The GitHub Pages edition is intentionally read-only with local simulated actions.
