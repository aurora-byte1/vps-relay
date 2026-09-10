# VPS Relay Tailscale & Funnel Architecture Notes

## 1. Core Problem: Hostname Drift & Certificate Breakage
- Tailscale identifies nodes via Machine Key in `/var/lib/tailscale/tailscaled.state`.
- When an ephemeral runner boots without persistent state:
  - It generates a fresh random machine key.
  - If it requests `--hostname=silver-trial`, the coordination server sees another node (or lingering node) registered as `silver-trial`.
  - It assigns `silver-trial-1`, `silver-trial-2`, etc.
  - Renaming via API breaks certificates because Let's Encrypt certificates are issued to the exact registered FQDN (`silver-trial.tail042f54.ts.net`).
  - Requesting new certs on every rotation exhausts Let's Encrypt rate limits (5 duplicate certs per week).

## 2. The Solution (Golden State & Cert Persistence)
- Exactly like the historical `silver-proxy` setup:
  - Generate a single clean `tailscaled.state` + `cert.crt` + `cert.key` for `silver-trial`.
  - Store them in `Silver-kun/THE-VPS-SET-UP` under `configs/tailscale/` (or via base64 secret).
  - Every rotating runner restores `/var/lib/tailscale/tailscaled.state` and `/var/lib/tailscale/certs/` BEFORE running `tailscale up`.
  - Every runner presents the exact same Machine Key -> 0 hostname drift, 0 duplicate cert requests, permanent `silver-trial.tail042f54.ts.net`.

## 3. Current Code Status
- Local `.github/workflows/rdp.yml` updated with:
  - Latest Tailscale installer (`curl -fsSL https://tailscale.com/install.sh | sudo sh`).
  - Full restore logic for persistent state/certs.
  - Funnel background proxying port 7860 over HTTPS 443.
  - Control plane sync restart (`systemctl restart tailscaled`).
  - Auto-sync of session tokens (`AIStudioToAPI/auth*`) and updated state back to `Silver-kun/THE-VPS-SET-UP`.
