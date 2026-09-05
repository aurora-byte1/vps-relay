# Ubuntu VPS with XRDP, AIStudioToAPI & Tailscale

This repository provisions an Ubuntu GitHub Actions runner configured as a cloud VPS:
- Connected to your Tailscale mesh network under hostname **`silver-trial`**
- **AIStudioToAPI** running inside Docker Compose on port `7860`
- Exposes port `7860` over Tailscale Serve / Funnel
- Remote Desktop GUI via **XRDP + XFCE4**

---

## Connection Details

- **Tailscale Hostname:** `silver-trial`
- **RDP Address:** `silver-trial` (or its `100.x.y.z` Tailscale IP)
- **RDP Port:** `3389`
- **RDP Username:** `runner`
- **RDP Password:** Provided during workflow trigger (Default: `P@ssw0rd12345!`)
- **AIStudioToAPI:** `http://silver-trial:7860` or `https://silver-trial.<tailnet>.ts.net`

---

## Secrets Configured

- `TAILSCALE_AUTH_KEY`: Ephemeral Tailscale auth key.
- `GH_PAT`: Personal Access Token to clone `Silver-kun/THE-VPS-SET-UP`.
