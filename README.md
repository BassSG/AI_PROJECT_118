# Hermes SnD · AI_PROJECT_117

Premium GitHub Pages portal for the deployed Hermes SnD XAUUSD Google Apps Script dashboard.

## Live application

- Secure Apps Script Web App: <https://script.google.com/macros/s/AKfycbweCV370sfJFtoCOR6g19j3cizvUzKZt9JMAbMqVtk5qF0jeG68VDypo6N0FcBNRRi9Iw/exec>
- GitHub Pages after merge: <https://basssg.github.io/AI_PROJECT_117/>

## Why this is a portal

GitHub Pages is public and static. The actual dashboard uses Google authentication and server-side `google.script.run`, so the public site intentionally acts as a secure launcher and availability monitor. It does not duplicate trading logic, expose private dashboard data, or send commands to the MT5 endpoint.

## Features

- Premium responsive interface matching the Hermes SnD dashboard
- Deployment health check with timeout, loading, online/offline, and error states
- PWA manifest and offline shell caching
- Strict Content Security Policy with no third-party scripts
- No API keys, endpoint secrets, credentials, or trading payloads
- GitHub Actions workflow for GitHub Pages deployment

## Local preview

Serve the repository root with any static HTTP server. Opening `index.html` directly also works, but service workers require HTTP or HTTPS.

## Configuration

The public deployment URL and display metadata are stored in `config.js`. Never place secrets in this file or anywhere in this repository.
