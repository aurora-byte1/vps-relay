# Cloud VPS Relay Node

Continuous cloud development environment provisioned with XRDP, Dockerized AI Studio API proxy, and Tailscale mesh networking.

## Static Access Endpoints
- **Tailscale Hostname:** `silver-trial`
- **Public HTTPS Funnel:** `https://silver-trial.tail042f54.ts.net/`
- **Direct Tailnet Port:** `http://silver-trial:7860/`
- **XRDP GUI:** `silver-trial:3389`
- **Default Username:** `runner`
- **Default Password:** Configured via workflow dispatch inputs

## Decentralized Ring Architecture
Operates on a 6-account rotating relay ring. At minute 345, the node hands off execution to the next relay peer and gracefully dereferences from the Tailnet, ensuring uninterrupted uptime without hostname conflicts.
