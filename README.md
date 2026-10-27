# Tahcia CLI

**Tahcia CLI for Mac & Linux**  
A local automation runtime and native AI companion for browser, desktop, and terminal workflows.

Tahcia allows scripts, AI agents, and MCP clients to automate:

- Google Chrome & active web sessions (multi-tab workflows, forms, downloads/uploads)
- macOS desktop & native application control (windows, clicks, keystrokes, shortcuts)
- Local file workflows, downloads, and uploads
- Multimodal Screen Vision & OCR (hardware-resampled screen captures for DeepSeek vision)
- Hands-free, voice-driven Cognitive Construct Companions (continuous listening, Whisper STT, and Kokoro Neural TTS)
- Model Context Protocol (MCP) server for Claude Desktop, Cursor, and OpenClaw

Tahcia bridges browser automation, native desktop control, and personalized AI thinking partners into a single unified CLI.

---

> [!NOTE]  
> This CLI repository does not bundle the Tahcia Google Chrome extension.  
> [Download and install the extension here.](https://github.com/tahcia/chrome)

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

## Getting Help

```bash
# General CLI help
tahcia --help
# or
tahcia -h

# Construct companion help & flag reference
tahcia construct --help
```

---

## Features & Usage

### 1. Laptop Construct Companion (`tahcia construct`)
Run your personalized AI Construct directly in your terminal. Engage through hands-free speech inquiry, optional screen vision, and neural audio synthesis.

```bash
tahcia construct <hash> [gap_seconds] [options]
```

#### Example Commands
```bash
# Default: Fast conversational mode (Vision/OCR off) with Kokoro neural voice:
tahcia construct cst_8f1a2b3c4d -v kokoro:af_heart

# Enable DeepSeek Screen Vision (hardware-resampled to 1280w max):
tahcia construct cst_8f1a2b3c4d --screenshot

# Enable active screen text OCR (scans screen every 15s):
tahcia construct cst_8f1a2b3c4d 15 --ocr

# Enable both Screen Capture & OCR:
tahcia construct cst_8f1a2b3c4d 10 --screenshot --ocr

# Run companion with native macOS system voice:
tahcia construct cst_8f1a2b3c4d -v mac:Samantha

# Run companion silently (terminal text only):
tahcia construct cst_8f1a2b3c4d -v none

# Use ultra-fast BYOK Groq Whisper STT (whisper-large-v3-turbo):
tahcia construct cst_8f1a2b3c4d --groq-key gsk_...

# Explicit speech language (prevents language drift with accents):
tahcia construct cst_8f1a2b3c4d --lang english --stt-model small
```

#### Voice Interaction & Keyboard Shortcuts
- **Continuous Listening (`\\`)**: Type or double-tap `\\` to activate continuous listening mode. Speak freely anytime. Tahcia automatically pauses listening while querying and resumes when speech finishes.
- **Push-to-Talk (`\`)**: Press a single `\` to toggle one-shot voice recording. Ambient noise-floor tracking automatically detects when you stop talking and submits. (Press `\` again to stop immediately).
- **Interrupt Speech (`\`)**: Press `\` at any time while the construct is speaking to instantly halt audio playback.
- **Typing Protection**: Whenever you type any letter in the terminal, passive vision intervals are automatically postponed so background cycles will never interrupt you while writing.
- **Prompting Status**: While querying the server, Tahcia displays `[Prompting...]` and locks keyboard input to prevent accidental duplicate requests.
- **Exit Anytime**: Press `Ctrl+C` in your terminal to safely exit.

#### Voice Options Reference
| Flag Option | Engine | Description |
| :--- | :--- | :--- |
| `kokoro:<voice>` | Kokoro Neural TTS | Ultra-realistic neural speech. Default: `af_heart` (warm & expressive). Alternatives: `am_michael`, `am_fenrir`, `bm_george`, `bf_emma`. |
| `kokoro:<v1>+<v2>` | Voice Blending | Blend two voice styles (e.g. `kokoro:af_heart+af_bella` or `kokoro:am_michael:0.7+am_fenrir:0.3`). |
| `mac:<voice>` | macOS `say` | Instant, zero-latency system voices (`Samantha`, `Daniel`, `Karen`, `Alex`). |
| `none` | Headless / Muted | Suppresses speech audio; logs directly to standard out. |

#### Construct CLI Flags Reference
| Flag | Default | Description |
| :--- | :--- | :--- |
| `-p, --profile <hash>` | — | Construct profile hash to interact with |
| `-g, --gap <seconds>` | `10` | Passive vision observation interval in seconds |
| `-v, --voice <voice>` | `mac:Daniel` | Voice output engine & voice name |
| `--screenshot, --vision` | `false` | Captures active screen, resamples to 1280w JPEG, and attaches for multimodal vision |
| `--ocr, --enable-ocr` | `false` | Extracts active screen text via macOS Vision OCR (default: off) |
| `--lang, --language <lang>` | `english` | Speech recognition language (`english`, `en`, `id`, `es`, `auto`) |
| `--stt-model <model>` | `small` | Local Whisper model size (`small`, `base`, `tiny`) |
| `--stt <provider>` | `local` | STT engine: `local` (default), `groq`, `openai` |
| `--groq-key <key>` | `$GROQ_API_KEY` | BYOK Groq API key for cloud `whisper-large-v3-turbo` |
| `--openai-key <key>` | `$OPENAI_API_KEY` | BYOK OpenAI API key for cloud `whisper-1` |
| `--no-ocr` | — | Explicitly disables screen OCR |
| `-k, --key <key>` | `$TAHCIA_API_KEY` | Tahcia API key |
| `-h, --help` | — | Show help message |

---

### 2. Audio & Vision Prerequisites (for Construct Companion)

To use voice synthesis, microphone STT, and screen vision on macOS:

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

Let the Tahcia operator control your Mac or Chrome browser session:

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

Launch Tahcia as a standardized MCP server for Anthropic Claude Desktop, Cursor, or OpenClaw:

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

## Privacy & Screen Vision Guarantee

- **Local Extraction**: Screen OCR and hardware downsampling (1280w max) run 100% on your local machine.
- **Zero Raw Image Storage**: Screen captures and OCR text exist only in volatile memory during the prompt cycle and are **never stored** in any database.
- **Off by Default**: Screen monitoring does not run unless you explicitly pass `--screenshot` or `--ocr`.

---

## License

MIT © [Tahcia](https://www.tahcia.com)
