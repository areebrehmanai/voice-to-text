# 🎙️ Voice to Text

<p>
  <img src="https://img.shields.io/badge/python-3.10-blue?logo=python&logoColor=white" alt="Python">
  <img src="https://img.shields.io/badge/OpenAI-Whisper-412991?logo=openai&logoColor=white" alt="Whisper">
  <img src="https://img.shields.io/badge/domain-speech%20recognition%20%7C%20NLP-informational" alt="NLP">
  <img src="https://img.shields.io/badge/status-complete-brightgreen" alt="Status">
  <img src="https://img.shields.io/badge/license-MIT-lightgrey" alt="License">
</p>

A simple, interactive speech-to-text notebook that transcribes an uploaded `.mp3` or `.wav` audio file into written text using OpenAI's Whisper model, with a lightweight upload widget for quick testing in Google Colab or Jupyter.

---

## Problem Statement & Overview

Converting spoken audio into accurate written text is a core NLP task with applications in transcription, accessibility, meeting notes, and voice-driven interfaces. This project provides a minimal, self-contained demo: upload an audio file through a simple widget, and get back an automatic transcription powered by Whisper — OpenAI's general-purpose speech recognition model trained on large-scale multilingual and multitask supervised data.

## System Flow

```
┌─────────────────┐     ┌─────────────────────┐     ┌───────────────────┐
│  User uploads       │────▶│  Whisper ("base")      │────▶│  Transcribed text     │
│  .mp3 / .wav file    │     │  speech-to-text model   │     │  printed + audio       │
│  via ipywidgets      │     │  (model.transcribe)     │     │  playback widget       │
└─────────────────┘     └─────────────────────┘     └───────────────────┘
```

## Key Features

- **One-click upload widget** — `ipywidgets.FileUpload` provides a simple in-notebook UI to select an audio file without needing a separate script or CLI.
- **Whisper-powered transcription** — uses OpenAI's open-source Whisper `base` model (`whisper.load_model("base")`) for automatic speech recognition, transcribing the uploaded audio to text via `model.transcribe()`.
- **Inline audio playback** — the uploaded file is played back in-notebook via `IPython.display.Audio` alongside its transcription, making it easy to verify accuracy.
- **Zero local setup required** — designed to run directly in Google Colab; the notebook installs `openai-whisper` and `ffmpeg` (required for audio decoding) at the top of the run.

## Bug Fix Applied

The original notebook read the uploaded file using `ipywidgets`' older (7.x) `FileUpload` API — indexing `.value` as a dict via `.values()` and reading the filename from a nested `'metadata'` key. Current `ipywidgets` (8.x, what actually installs today) changed `FileUpload.value` to a plain tuple/list of entries with `'name'`/`'content'` set directly, no `'metadata'` wrapper — so the original code raised `AttributeError: 'tuple' object has no attribute 'values'` the moment a file was uploaded. Verified by inspecting the installed `ipywidgets` package directly; fixed by reading `uploader.value[0]['name']` instead, and confirmed working with a mocked upload event.

## Tech Stack

| Layer | Technology |
| :--- | :--- |
| Speech-to-text model | [OpenAI Whisper](https://github.com/openai/whisper) (`base` model) |
| Audio decoding | `ffmpeg` |
| Notebook UI | `ipywidgets` (`FileUpload`), `IPython.display` |
| Runtime | Python 3.10, Jupyter / Google Colab |

## Getting Started

### Option 1: Google Colab (recommended, zero setup)
1. Upload `Voice_to_Text.ipynb` to [Google Colab](https://colab.research.google.com/).
2. Run all cells in order.
3. Use the upload widget to select a `.mp3` or `.wav` file — the transcription prints below the widget once processing finishes.

### Option 2: Local Jupyter

```bash
git clone https://github.com/areebrehmanai/voice-to-text.git
cd voice-to-text

pip install -r requirements.txt
# ffmpeg is also required for audio decoding:
#   Ubuntu/Debian: sudo apt-get install ffmpeg
#   macOS (Homebrew): brew install ffmpeg
#   Windows: https://ffmpeg.org/download.html

jupyter notebook Voice_to_Text.ipynb
```

Run all cells, then use the upload widget the same way as in Colab.

## Repository Structure

```
voice-to-text/
├── Voice_to_Text.ipynb   # Whisper-based speech-to-text notebook with upload widget
├── requirements.txt
├── .gitignore
├── LICENSE
└── README.md
```

## License

Released under the [MIT License](LICENSE).
