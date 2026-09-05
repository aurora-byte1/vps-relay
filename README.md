# Continuous Ubuntu VPS via 3-Repo Rotating Relay

This setup provisions an Ubuntu GitHub Actions runner configured with XRDP, Dockerized AIStudioToAPI, and Tailscale mesh networking. It runs as a **self-sustaining rotating relay across 3 repositories** to achieve continuous uptime.

---

## 🔁 The 3-Repository Relay Ring

Every runner operates for up to **350 minutes**. At minute **345**, it automatically dispatches the next repository in the ring, allowing a 5-minute warm-up overlap for the new runner before cleanly deregistering from Tailscale.

```
┌────────────────────────────────┐
│  silverxcutonic/windows-rdp    │
└───────────────┬────────────────┘
                │ (at 345m)
                ▼
┌────────────────────────────────┐
│ silverxcutonic/windows-rdp-spare1
└───────────────┬────────────────┘
                │ (at 345m)
                ▼
┌────────────────────────────────┐
│ silverxcutonic/windows-rdp-spare2
└───────────────┬────────────────┘
                │ (at 345m)
                ▼
(Loops back to windows-rdp)
```

---

## 🌐 Endpoints & Connection (Static Across All Rotations)

- **Public HTTPS Funnel URL:** `https://silver-trial.tail042f54.ts.net/`
- **Tailscale Hostname:** `silver-trial`
- **Private Tailnet Web UI:** `http://silver-trial:7860`
- **RDP GUI:** `silver-trial:3389`
- **RDP User:** `runner`
- **RDP Password:** Provided at workflow dispatch (Default: `P@ssw0rd12345!`)

---

## 🔐 Configured Secrets in All 3 Repositories

- `GH_PAT`: Personal Access Token with `repo` and `workflow` permissions to dispatch subsequent runs and clone private repositories.
- `TAILSCALE_AUTH_KEY`: Ephemeral, reusable auth key for automatic Tailnet registration and node cleanup.
