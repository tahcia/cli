# Tahcia CLI

**Tahcia CLI for Mac & Linux**  
A local automation runtime and native AI companion for browser, desktop, and terminal workflows.

It allows scripts, AI agents, and MCP clients to automate:

- Google Chrome & active web sessions
- macOS desktop & native application control
- Keyboard, mouse, and window orchestration
- Local file workflows, downloads, and uploads
- Real-time on-device Screen Vision (macOS OCR)
- Hands-free, voice-driven Cognitive Construct Companions (Whisper STT + Neural TTS)

Tahcia bridges browser automation, native desktop control, and personalized AI thinking partners into a single unified CLI.

---

> [!NOTE]  
> This CLI repository does not bundle the Tahcia Google Chrome extension.  
> [Download and load the extension here.](https://github.com/tahcia/chrome)

---

## Installation

### Option 1: Homebrew (macOS & Linux)
```bash
brew tap tahcia/tahcia && brew install tahcia
```

### Option 2: npm (Global)
```bash
npm install -g @tahc.ia/tahcia
```

---

## Authentication & Configuration

Tahcia commands look for your API key in the following order:

1. Flag: `-k <your_api_key>`
2. Environment Variable: `export TAHCIA_API_KEY="your_api_key"`
3. Local `.env` file in current working directory
4. User Config file: `~/.tahcia/config` (`{"api_key": "..."}`)

> **Where to find your key**: Log in to [tahcia.com](https://www.tahcia.com) &rarr; **Edit Profile** &rarr; **API Keys**.

---

## Features & Usage

### 1. Laptop Construct Companion (`tahcia construct`)
Run your personalized AI Construct directly in your terminal. Tahcia ambiently observes your active screen context via local OCR, speaks reflections aloud, and engages in real-time voice inquiry.

```bash
tahcia construct <hash> [gap_seconds] [options]
```

#### Example Commands
```bash
# Run companion with neural Kokoro voice (scans screen every 10s)
tahcia construct cst_8f1a2b3c4d 10 -v kokoro:af_heart

# Run companion with native macOS system voice
tahcia construct cst_8f1a2b3c4d 15 -v mac:Samantha

# Run companion silently (terminal text only)
tahcia construct cst_8f1a2b3c4d 10 -v none
```

#### Voice Interaction (Push-to-Talk)
- **Ambient Awareness**: Checks active screen text at your configured interval (`gap_seconds`) and only speaks if it observes something noteworthy.
- **Press `\` to Talk**: Press the backslash key (`\`) to begin speaking. Tahcia tracks your ambient noise floor continuously and automatically stops recording when you pause. (Press `\` again to stop immediately).
- **Local Whisper STT**: Audio is transcribed on-device and delivered directly to your construct.
- **Press `Ctrl+C`** to exit at any time.

#### Voice Options Reference
| Flag Option | Engine | Description |
| :--- | :--- | :--- |
| `kokoro:<voice>` | Kokoro Neural TTS | High-fidelity neural speech (`af_heart`, `af_bella`, `am_adam`, `am_michael`, `bf_emma`) |
| `mac:<voice>` | macOS `say` | Instant, zero-latency system voices (`Samantha`, `Daniel`, `Karen`, `Alex`) |
| `none` | Headless / Muted | Suppresses speech audio; logs directly to standard out |

---

### 2. Audio & Vision Prerequisites (for Construct Companion)

To use real-time screen OCR and voice synthesis on macOS:

```bash
# 1. System audio & speech drivers
brew install espeak-ng soundlayer sox

# 2. Kokoro neural TTS engine (Python)
pip install kokoro-cli sounddevice numpy soundfile kokoro-onnx

# 3. Speech-to-Text & microphone capture (Node.js)
npm install -g node-record-lpcm16 @xenova/transformers
```

> **Permissions**: Ensure your terminal emulator (Terminal, iTerm2, or VSCode) has **Screen Recording** and **Microphone** permissions granted in *macOS System Settings > Privacy & Security*.

---

### 3. Tahcia Computer Operator Shell Extension

Let tahcia operator control your mac.

```bash
tahcia session:pass

# Execute inline
tahcia session:pass /command params
```

---

### 4. Browser & Desktop Script Execution

Run deterministic browser automations, DOM extractions, and multi-step desktop workflows defined in `.tahcia.json` scripts:

```bash
# Execute a local automation script
tahcia run ./automation.tahcia.json

# Execute with debug telemetry
tahcia run ./automation.tahcia.json --debug
```

---

### 5. Model Context Protocol (MCP) Server

Launch Tahcia as a standardized MCP server for Anthropic Claude, Cursor, or OpenClaw:

```bash
tahcia mcp
```

Add to your `claude_desktop_config.json`:
```json
{
  "mcpServers": {
    "tahcia": {
      "command": "tahcia",
      "args": ["mcp"],
      "env": {
        "TAHCIA_API_KEY": "your_api_key_here"
      }
    }
  }
}
```

---

## Privacy & Screen OCR Guarantee

- **Local Extraction**: Screen OCR runs entirely on your local machine using native macOS Vision APIs.
- **Zero Raw Storage**: Only the extracted text context is transmitted to the model API endpoint during active inference.
- **Transient Memory**: Tahcia **does NOT store** desktop screenshots, display image captures, or raw OCR logs in any database. The data exists strictly in volatile memory during the prompt cycle and is immediately discarded.

---

## Troubleshooting

- **Microphone Sensitivity**: Run with `TAHCIA_DEBUG_MIC=1` to display real-time RMS audio levels and adaptive noise-floor thresholds.
- **Mute Browser Chimes**: Background CLI inference via `/v1/chat/completions` runs silently and will not broadcast notification sounds to open `tahcia.com/chat` browser tabs.
- **Python Path**: If using a custom virtual environment for Kokoro, set `export PYTHON_PATH="/path/to/venv/bin/python3"`.

---

## License

MIT © [Tahcia](https://www.tahcia.com)
