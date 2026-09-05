# Windows RDP via Tailscale

This repository provisions a GitHub Actions runner running Windows, connects it to your Tailscale mesh network, enables Remote Desktop (RDP), and keeps the session active.

## Setup & Secrets
- `TAILSCALE_AUTH_KEY`: Configured in GitHub Action Secrets.

## How to Connect
1. Trigger the workflow manually from **Actions** -> **Windows RDP via Tailscale** -> **Run workflow**.
2. Specify the RDP password (or use the default).
3. Once the workflow starts and registers on Tailscale:
   - Check your Tailscale admin console or devices list for `gha-windows-rdp`.
   - Open Remote Desktop Connection (`mstsc.exe`).
   - Computer: `gha-windows-rdp` (or its Tailscale 100.x.y.z IP).
   - User: `runneradmin`
   - Password: Password provided when triggering the workflow.
