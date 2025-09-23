# WhatsApp Chat Analyzer

Analyze your exported WhatsApp chats with an interactive Streamlit app. Get message statistics, timelines, activity heatmaps, most common words, and emoji usage — with robust parsing and proper emoji rendering on Windows/macOS/Linux.

---
## Demo : https://whatsapp-chat-analyzer-afpv9rz6xrfyzazhtzlnkn.streamlit.app/

## Features

- **Top stats**: total messages, words, media, links
- **Timelines**: monthly and daily trends
- **Activity maps**: busiest days and months, weekly heatmap by hour
- **Word analysis**: stopword-filtered word cloud and most common words
- **Emoji analysis**: frequency table and pie chart with proper emoji fonts
- **Per-user filtering**: analyze overall or a single participant

---

## Project structure

- `app.py` – Streamlit UI and charts
- `preprocessor.py` – Chat parsing into a clean `pandas` DataFrame
- `helper.py` – Analysis helpers (stats, wordcloud, emoji extraction, etc.)
- `stop_hinglish.txt` – Stopwords for word analysis
- `requirements.txt` – Python dependencies
- `WhatsApp Chat with ... .txt` – Example exported chats

---

## Prerequisites

- Python 3.9+ recommended
- pip (or pipx/conda)

---

## Setup

```bash
pip install -r requirements.txt
```

If you are using a virtual environment:

```bash
python -m venv .venv
.venv/Scripts/activate  # Windows
# source .venv/bin/activate  # macOS/Linux
pip install -r requirements.txt
```

---

## Export your WhatsApp chat

On your phone:

- Open the chat or group
- Tap More > Export Chat
- Choose “Without Media” (recommended for first run)
- Save the `.txt` file to your computer

Place the exported file anywhere; you’ll upload it from the app sidebar.

---

## Run the app

```bash
streamlit run app.py
```

Then open the browser URL shown (e.g., http://localhost:8501), upload your exported chat `.txt`, choose a user filter, and click “Show Analysis”.

---

## How parsing works

Parsing is handled in `preprocessor.py`:

- Robust timestamp regex handles both `am/pm` and `AM/PM`, single-digit hours, and special spaces used by WhatsApp exports.
- After splitting messages, it extracts `user`, `message`, and datetime parts and derives helpful columns like `year`, `month`, `day_name`, `hour`, and `period`.

The parser normalizes the narrow no‑break space `\u202F` used in some exports when needed. If you see unexpected parsing, see Troubleshooting below.

---

## Emoji handling

`helper.py` includes a version‑compatible emoji extractor that works with both old and new versions of the `emoji` package. The app configures Matplotlib to use emoji‑capable fonts so charts render emoji labels correctly.

---

## Troubleshooting

- **Emojis show as blank squares/tofu**
  - On Windows we use `Segoe UI Emoji`. On macOS `Apple Color Emoji`. On Linux install `Noto Color Emoji`.
  - After installing fonts, restart Python so Matplotlib re-caches fonts.

- **Emoji labels overlap on the pie chart**
  - We render emoji labels via a legend instead of on the wedges to avoid clipping. Resize the browser pane or use Streamlit’s wide layout if needed.

- **Messages are not splitting correctly**
  - Some exports include `\u202F` (narrow no‑break space) between time and am/pm. Update the raw text or ensure you’re on the latest code. The parser’s regex in `preprocessor.py` is designed to handle typical exports.

- **AttributeError: module `emoji` has no attribute `UNICODE_EMOJI`**
  - Fixed in this repo by using a compatibility extractor (`extract_emojis`) that supports `emoji` v1 and v2+. If upgrading/downgrading `emoji`, no code changes should be necessary.

---

## Example outputs

- Monthly and daily message timelines
- Activity heatmap by weekday vs hour
- Top users, common words, and emoji distribution

---

## Development

Run Streamlit in dev mode and make edits:

```bash
streamlit run app.py
# Edit files; Streamlit will auto-reload charts
```

Code highlights:

- `helper.emoji_helper()` builds a frequency table using a robust `extract_emojis()` implementation.
- `app.py` sets Matplotlib `rcParams['font.family']` to include emoji fonts and uses a legend to render emoji labels.

---

## License

This project is provided as-is for educational use. Adapt and extend freely within your organization or personal projects.
