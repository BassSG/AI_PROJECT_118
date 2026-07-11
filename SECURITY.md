# Security policy

This repository contains only the public GitHub Pages portal. It must never contain API keys, endpoint secrets, MT5 credentials, Google session tokens, private trading payloads, or personal account data.

## Architecture

- GitHub Pages performs a network availability check and opens the deployed Google Apps Script Web App.
- Trading data, AI review, order controls, and endpoint calls remain inside the authenticated Apps Script application.
- The public portal does not call `google.script.run`, cannot place orders, and does not expose the Apps Script read-only API.
- The deployment URL is public configuration, not a secret. Authorization is enforced by Google and the server-side Apps Script gateway.

## Reporting

Report security concerns privately to the repository owner. Do not open a public issue containing secrets, account identifiers, endpoint payloads, or trading credentials.
