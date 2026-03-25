# FluxerRichPresenceClient (FRPC) 🟣 

[![Python Version](https://img.shields.io/badge/python-3.10%2B-blue.svg)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Platform](https://img.shields.io/badge/platform-windows-lightgrey.svg)](https://www.microsoft.com/windows)

> [!CAUTION]
> **Active Development Notice:** This client is currently in heavy active development. Full source code and compiled `.exe` releases will be published ASAP. As I maintain a full-time career, updates are pushed during my personal time—thank you for your patience and support!

**FluxerRichPresenceClient** is a standalone Windows desktop utility for [Fluxer](https://fluxer.app/). It watches what's running on your PC and automatically updates your Fluxer Custom Status to reflect your current activity — the app you're using, how long you've been using it, and an emoji of your choice.

Fluxer is an incredible, rapidly growing platform — but right now if you're deep in a gaming session or a coding sprint, your profile just shows "Online" with no indication of what you're actually doing. FRPC is a "Set it and Forget it" fix. Configure your apps once, and your status automatically shows what you're doing with a minute-by-minute elapsed timer and a custom emoji.

> [!NOTE]
> **This is a temporary solution.** Fluxer doesn't currently have a native Rich Presence framework. FRPC fills that gap by automating your Custom Status message using the existing Fluxer REST API. Once Fluxer ships native RP support, this client will likely become obsolete when that time comes.
>
> **What this actually is:** A status message automator. It sets the same Custom Status you'd type in manually, just automatically based on what's running on your machine.
>
> **What this is NOT:** A game SDK integration. FRPC cannot pull in-game data (match scores, character info, lobby details). If you want that level of detail, check out [fluxer-rpc](https://github.com/letruxux/fluxer-rpc) — it mirrors your Discord Rich Presence data to Fluxer via Lanyard and is a great project for users who already have Discord running. FRPC takes a different approach entirely: no Discord needed, no Lanyard setup — it works standalone by detecting running apps and reading window titles directly from your OS.

---

## 🛡️ Safety & Transparency

* **Non-Invasive:** FRPC does **not** modify, inject into, or "mod" the Fluxer client. It runs as a completely independent process.
* **Direct-to-Fluxer Only:** All network calls go strictly to `api.fluxer.app`. No data is ever sent to third-party servers.
* **Custom Status + Presence:** The client automates your Custom Status message via the Fluxer REST API. It also properly manages your online/away presence state — correcting Fluxer's tendency to mark you "Away" when you're simply active in another application.
* **Token Security:** Your token is encrypted locally using Windows DPAPI and stored in the Windows Registry. It never leaves your machine except in authenticated calls to `api.fluxer.app`.

---

## ✨ What It Does

### App Detection & Status Updates
- Scans running processes and matches them against your configured app list
- Reads window titles for context (e.g., which file is open in PyCharm, what video is playing in Chrome)
- Displays the app name, a custom verb ("Playing", "Coding in", "Watching", etc.), a minute-by-minute elapsed timer, and an emoji
- Priority system decides which app shows when multiple configured apps are running
- Filters out Windows system noise automatically (services, background processes, etc.)

### Emoji & Emote Picker
- Pulls your custom Fluxer emotes from all your servers via the Fluxer API
- Full Unicode emoji library (1,800+ emojis across 9 categories) sourced from the official Unicode database
- Emotes are sent as proper API fields (`emoji_id` + `emoji_name`) so they render correctly on your profile
- Animated emotes play on hover in the picker

### Idle & Presence Management
- Detects Windows idle time (no keyboard/mouse input) and screen lock (Win+L) to properly set your status to Away — fixing Fluxer's habit of marking you Away just because you're active in another application
- Detects system sleep/hibernate events and clears your Custom Status accordingly
- Automatically clears Custom Status on any exit — graceful, crash, Ctrl+C, or Windows shutdown

### Optional: Automatic Music Detection
- Uses Windows SMTC (System Media Transport Controls) to detect what's playing in Spotify, YouTube Music, Apple Music, and other media players
- Shows artist and track name as your status when no configured app is running
- Disabled by default — opt-in via Settings

---

## 🛠️ Technical Details

| Component | What's Used |
|-----------|------------|
| GUI | PySide6 (Qt for Python) |
| API | REST calls to `api.fluxer.app` via `urllib.request` |
| Process Scanning | `psutil` + `win32gui` / `win32process` |
| Token Storage | Windows DPAPI (`win32crypt`) + Registry |
| Emoji Database | Unicode `emoji-test.txt` (auto-cached) with `carpedm20/emoji` fallback |
| Emote Rendering | QPainter virtual canvas with QMovie hover-to-animate |
| Music Detection | Windows SMTC via WinRT (optional) |

Status syncs to Fluxer every 60 seconds via REST. Local app detection runs every second so status changes feel instant — the 60-second floor prevents API abuse.

---

## 📦 Installation

### Requirements
- Windows 10/11
- Python 3.10+

### Setup
```bash
git clone https://github.com/g00bPy/FluxerRichPresenceClient.git
cd FluxerRichPresenceClient
pip install PySide6 psutil pywin32 emoji
python main.py
```

### Optional: Music Detection
```bash
pip install winrt-Windows.Media.Control winrt-Windows.Foundation winrt-Windows.Foundation.Collections
```

---

## ⚠️ Limitations

- **No in-game data.** FRPC sees that a game is running and can read its window title, but it cannot access match info, character data, or lobby details. Discord's Game SDK sends that data through a private local IPC pipe directly to the Discord client, and from there it travels over Discord's encrypted network — there's nothing static or readable that FRPC could intercept or reuse.
- **Different tool than fluxer-rpc.** [fluxer-rpc](https://github.com/letruxux/fluxer-rpc) is an excellent project that mirrors your Discord Rich Presence to Fluxer via Lanyard — giving you detailed game info, music status, and more. It requires Discord running + Lanyard setup. FRPC is a standalone alternative for users who want activity status without depending on Discord at all.
- **Windows only (for now).** FRPC depends on Windows APIs (DPAPI, Registry, win32gui, SMTC). Linux and macOS versions are planned for the future.
- **Custom Status + presence scope.** FRPC controls your Custom Status text/emoji and manages your online/away presence state. It does not interact with any other Fluxer features (messages, servers, etc.).

---

## 🗺️ Roadmap

- [ ] Compiled `.exe` release (no Python install required)
- [ ] Linux client
- [ ] macOS client

---

## 📜 Development Note
> [!IMPORTANT]
> To maintain the security and integrity of the Fluxer ecosystem, specific encryption logic and registry vaulting functions are excluded from this public source. The provided code uses "Stubs" for these functions to demonstrate the application's structural logic while keeping the security implementation private.

---

**Developed with 💜 by [g00by](https://github.com/g00bPy)**

*Passionate Python developer building tools for communities I enjoy. Not affiliated with Fluxer — this is an independent open-source project.*
