# FluxerRichPresenceClient (FRPC) 🟣 

[![Python Version](https://img.shields.io/badge/python-3.10%2B-blue.svg)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Platform](https://img.shields.io/badge/platform-windows-lightgrey.svg)](https://www.microsoft.com/windows)

> [!CAUTION]
> **Active Development Notice:** This client is currently in heavy active development. Full source code and compiled `.exe` releases will be published ASAP. As I maintain a full-time career, updates are pushed during my personal time—thank you for your patience and support!

**FluxerRichPresenceClient** is a sophisticated, standalone desktop utility designed specifically for [Fluxer](https://fluxer.app/). It bridges the gap between your local Windows environment and your Fluxer profile, providing a high-performance framework for automated activity tracking and visual status customization.

> [!NOTE]
> **What this client does:** FRPC automates your **Custom Status message** within Fluxer — the same status you'd set manually by clicking your profile and typing in a status with an emoji. Fluxer does not currently have a native Rich Presence SDK, so this client works within the existing Custom Status system to show what you're up to.
>
> **What this client does NOT do:** It does not pull in-game data from game SDKs (lobbies, character info, match details, etc.). That level of integration would require either a native Fluxer RP framework or a bridge between Discord's Game SDK and Fluxer — neither of which exists today. FRPC detects *which* app is running and reads window titles for context, but it cannot reach into a game's internal state.

---

## 🚀 Why FluxerRichPresenceClient?

[Fluxer](https://fluxer.app/) is an incredible, rapidly growing platform for community interaction, but it currently lacks a native desktop framework for activity status automation. Users often find themselves manually updating statuses or appearing "Offline" while deep in a gaming session or a coding sprint.

I built this client to provide a "Set it and Forget it" solution. Whether you are battling in a game, designing in Creative Cloud, or debugging in PyCharm, this client ensures your Fluxer community knows exactly what you're up to — complete with high-resolution emotes and real-time "Time Elapsed" counters.

## 🛡️ Architectural Transparency & Safety

To ensure account safety and platform integrity, it is important to understand exactly how FRPC operates:

* **Non-Invasive Architecture:** This application **does not modify, inject code into, or "mod" the Fluxer client** in any way. It runs as a completely independent "sidecar" process on your operating system.
* **Direct-to-Fluxer Only:** The application communicates strictly and directly with `api.fluxer.app`. **No data is ever sent to third-party servers or external services.**
* **Pure Status Automation:** The client's scope is purely limited to automating your **Custom Status message**. It uses the official Fluxer REST API to keep your profile updated based on your local activity.
* **Local Token Security:** Your account token is handled exclusively within your local machine's secure environment (Windows DPAPI encryption) and is never exposed to external network calls outside of official Fluxer endpoints.

## ✨ Core Feature Set

### 🔍 Smart Activity Engine
The heart of the client is a non-intrusive scanning engine.
* **Worthy App Filter:** Automatically filters out system background noise (Windows services, telemetry, etc.) to only show "worthy" applications.
* **Smart Context Sensing:** For specific apps like **PyCharm**, **VS Code**, or **Chrome**, the client can read window titles to display exactly which project or video you are working on.
* **Priority System:** If multiple configured apps are running, the client uses a user-defined priority system to decide which one takes center stage.
* **Automatic Music Detection:** Optional Windows SMTC integration detects what you're listening to in Spotify, YouTube Music, Apple Music, and more — without needing to configure each player individually.

### 🎨 Visual Emote Vault
This client introduces the first dedicated **Emote Picker** for Fluxer desktop users.
* **Direct API Sync:** Securely queries your server memberships to pull every custom emote available to your account from all guilds.
* **Animated WebP Support:** Full support for animated emotes via hover-to-play rendering. Static thumbnails keep the grid fast, and hovering over an animated emote plays it live.
* **High-Performance Canvas Renderer:** The emote picker uses a custom `QPainter`-based virtual canvas — zero child widgets, viewport-culled rendering, and instant scrolling even with 1000+ emotes.
* **Full Unicode Emoji Library:** All 1,800+ Unicode emojis organized into 9 categories (Smileys, People, Animals, Food, Travel, Activities, Objects, Symbols, Flags), sourced from the official Unicode Consortium database.
* **Proper Emoji API Integration:** Emojis are sent as structured API fields (`emoji_id` + `emoji_name`), not embedded in the status text — so custom Fluxer emotes render correctly on your profile.

### ⚡ Performance & Reliability
* **REST-Based Sync:** Status updates are sent via direct REST API calls to `api.fluxer.app` with a fixed 60-second sync interval. This replaced the earlier WebSocket/Gateway approach for significantly improved reliability and reduced overhead.
* **Instant Local Detection:** While API syncs happen every 60 seconds, the client detects app changes locally every second — so status changes appear instant on switch, with the 60-second floor preventing API abuse.
* **Smart Idle & Lock Detection:** Automatically detects Windows idle time, screen lock (Win+L), and system sleep/hibernate events to clear or pause your status appropriately.
* **Emergency Status Clear:** Status is automatically cleared on any exit — graceful shutdown, crash, `Ctrl+C`, Windows restart, or force-kill — via `atexit`, signal handlers, and Windows power event monitoring.
* **Disk I/O Caching:** Settings, app configs, and templates are cached in memory and only re-read from disk when the file's modification time changes.

---

## 🛠️ Technical Stack

| Component | Technology |
|-----------|-----------|
| **GUI Framework** | [PySide6](https://pypi.org/project/PySide6/) (Qt for Python) |
| **API Communication** | REST via `urllib.request` (direct to `api.fluxer.app`) |
| **Process Monitoring** | `psutil` + `win32gui` / `win32process` for window title reading |
| **Token Storage** | Windows DPAPI encryption via `win32crypt` + Registry |
| **Emoji Database** | Unicode `emoji-test.txt` (auto-downloaded + cached) with `carpedm20/emoji` fallback |
| **Emote Rendering** | Custom `QPainter` virtual canvas with `QMovie` hover-to-animate |
| **Music Detection** | Windows SMTC via `WinRT` (optional, for Spotify/YouTube Music/etc.) |

---

## 📦 Getting Started

### Prerequisites
* Windows 10/11
* Python 3.10 or higher

### Installation
1.  **Clone the Repository:**
    ```bash
    git clone https://github.com/g00bPy/FluxerRichPresenceClient.git
    cd FluxerRichPresenceClient
    ```
2.  **Install Dependencies:**
    ```bash
    pip install PySide6 psutil pywin32 emoji
    ```
3.  **Launch the Client:**
    ```bash
    python main.py
    ```

### Optional Dependencies
```bash
# For automatic music detection (Spotify, YouTube Music, Apple Music, etc.)
pip install winrt-Windows.Media.Control winrt-Windows.Foundation winrt-Windows.Foundation.Collections
```

---

## ⚠️ Current Limitations

It's important to set honest expectations about what FRPC can and cannot do:

* **No In-Game SDK Integration:** FRPC detects running applications and reads window titles, but it cannot access game-internal data (match scores, character names, lobby info, etc.). This would require game developers to integrate with a Fluxer SDK — which doesn't exist yet.
* **No Discord-to-Fluxer Bridge:** Discord's Rich Presence system pulls data from game SDKs via its own IPC protocol. There is currently no way to bridge that data from Discord to Fluxer. If Fluxer ever implements a native RP framework or an IPC bridge, FRPC could be extended to support it.
* **Windows Only:** The client relies heavily on Windows-specific APIs (DPAPI, Registry, `win32gui`, SMTC) and is not cross-platform.
* **Custom Status Scope:** FRPC only controls your Custom Status message. It does not modify your online/idle/DND presence state or interact with any other Fluxer features.

---

## 📜 Development Note
> [!IMPORTANT]
> To maintain the security and integrity of the Fluxer ecosystem, specific encryption logic and registry vaulting functions are excluded from this public source. The provided code uses "Stubs" for these functions to demonstrate the application's structural logic while keeping the security implementation private.

---

**Developed with 💜 by [g00by](https://github.com/g00bPy)**

*I am a passionate Python developer who loves building tools for the communities I enjoy. Please note that I am **not** affiliated with, nor do I work for, Fluxer. This is an independent open-source project.*
