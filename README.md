# FluxerRichPresenceClient (FRPC) 🟣 

[![Python Version](https://img.shields.io/badge/python-3.10%2B-blue.svg)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Platform](https://img.shields.io/badge/platform-windows-lightgrey.svg)](https://www.microsoft.com/windows)

> [!CAUTION]
> **Active Development Notice:** This client is currently in heavy active development. Full source code and compiled `.exe` releases will be published ASAP. As I maintain a full-time career, updates are pushed during my personal time—thank you for your patience and support!

**FluxerRichPresenceClient** is a sophisticated, standalone desktop utility designed specifically for [Fluxer](https://fluxer.app/). It bridges the gap between your local Windows environment and your Fluxer profile, providing a high-performance framework for automated activity tracking and visual status customization.

---

## 🚀 Why FluxerRichPresenceClient?

[Fluxer](https://fluxer.app/) is an incredible, rapidly growing platform for community interaction, but it currently lacks a native desktop framework for **Rich Presence (RP)**. Users often find themselves manually updating statuses or appearing "Offline" while deep in a gaming session or a coding sprint.

I built this client to provide a "Set it and Forget it" solution. Whether you are battling in a game, designing in Creative Cloud, or debugging in PyCharm, this client ensures your Fluxer community knows exactly what you're up to—complete with high-resolution emotes and real-time "Time Elapsed" counters.

## ✨ Core Feature Set

### 🔍 Smart Activity Engine
The heart of the client is a non-intrusive scanning engine. 
* **Worthy App Filter:** Automatically filters out system background noise (Windows services, telemetry, etc.) to only show "worthy" applications.
* **Smart Context Sensing:** For specific apps like **PyCharm**, **VS Code**, or **Chrome**, the client can read window titles to display exactly which project or video you are working on.
* **Priority System:** If multiple configured apps are running, the client uses a user-defined priority system to decide which one takes center stage.

### 🎨 Visual Emote Vault
This client introduces the first dedicated **Emote Picker** for Fluxer desktop users.
* **Direct API Sync:** Securely queries your server memberships to pull every custom emote available to your account.
* **Animated WebP Support:** Full support for animated emotes. Unlike standard implementations, this client uses a custom rendering engine to preserve aspect ratios, ensuring emotes are never squished or cropped.
* **Local Caching:** Stores and manages emote data locally to ensure lightning-fast load times and minimal API impact.

### 🔒 Security-by-Design
Privacy is the foundation of this project.
* **Local Auth Only:** Your Fluxer token is never transmitted to any third-party servers. All connections are made strictly and directly to `api.fluxer.app`.
* **Hardware-Level Encryption:** On the full version of this client, sensitive data is vaulted using local machine security protocols to ensure your session remains private.

### ⚡ Performance Optimized
* **Asynchronous Architecture:** Built on `asyncio` and `websockets`, the client maintains a constant connection to the Fluxer Gateway with near-zero CPU overhead.
* **Smart Pacing:** Includes built-in rate-limit protection to ensure your account remains in good standing with Fluxer's infrastructure.

---

## 🛠️ Technical Stack

* **GUI Framework:** [PySide6](https://pypi.org/project/PySide6/) (Qt for Python)
* **Networking:** `websockets` for real-time Gateway communication.
* **Process Monitoring:** `psutil` for low-level system interaction.
* **Storage:** Windows Registry integration and secure local vaulting.
* **Image Handling:** `QtNetwork` and `QMovie` with WebP decoding.

---

## 📦 Getting Started

### Prerequisites
* Windows 10/11
* Python 3.10 or higher

### Installation
1.  **Clone the Repository:**
    ```bash
    git clone [https://github.com/g00bPy/FluxerRichPresenceClient.git](https://github.com/g00bPy/FluxerRichPresenceClient.git)
    cd FluxerRichPresenceClient
    ```
2.  **Install Dependencies:**
    ```bash
    pip install PySide6 psutil websockets pywin32
    ```
3.  **Launch the Client:**
    ```bash
    python main.py
    ```

## 📜 Development Note
> [!IMPORTANT]
> To maintain the security and integrity of the Fluxer ecosystem, specific encryption logic and registry vaulting functions are excluded from this public source. The provided code uses "Stubs" for these functions to demonstrate the application's structural logic while keeping the security implementation private.

---
**Developed with 💜 by [g00by](https://github.com/g00bPy)**

*I am **not** affiliated with, nor do I work for, Fluxer. This is an independent open-source project.*
