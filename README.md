# Accessible Medical Protocols for Non-French-Speaking Patients

A pipeline that turns medical protocols in French into simplified, translated and audio versions for patients with gestational diabetes who don't speak French.

Master 1 project, Language Industries (NLP), Université Grenoble Alpes, 2025–2026.
Programming part: Darya ZDRELYUK

## Why

The endocrinology department at Hôpital Nord in Marseille regularly treats patients with gestational diabetes who don't speak French. The language barrier, combined with medical jargon, makes it hard for them to:

- do technical tasks on their own (injections, blood glucose checks)
- understand and follow dietary guidelines
- keep up with their medical follow-up safely

## What it does

The pipeline takes a French protocol (`.txt`, `.pdf` or `.docx` formats) and:

1. Simplifies it into FALC (*Facile à lire et à comprendre*, the French easy-to-read standard)
2. Translates it into the selected languages
3. Generates an audio version of each text
4. Publishes each audio on GitHub Pages with a player page and a QR code
5. Builds one Word document per version (FALC + each translation)

For example, FALC + 4 languages gives 5 Word documents per protocol.

## Audio player

Each audio file gets its own simple HTML player page:

- large pictogram buttons: Play, Pause, −5s, +5s
- a red Pause button
- a title in the patient's language

The QR code and link in each Word document open this page.

## Project structure

```
├── scripts/
│   ├── main.py            # runs the full pipeline
│   ├── falc.py            # FALC simplification
│   ├── traduction.py      # translation
│   ├── generer_audio.py   # text-to-speech
│   ├── upload_audio.py    # publishing to GitHub Pages
│   ├── qr.py              # QR codes
│   ├── docx_builder.py    # Word documents
│   ├── config.py          # API keys and settings
│   ├── requirements.txt   # dependencies required
│
├── docs/                  # inputs and outputs (also served by GitHub Pages)
│   ├── input/             # original protocols
│   ├── falc/              # simplified texts
│   ├── traductions/       # translations
│   ├── audio/             # MP3 files
│   ├── qr/                # QR codes
│   └── docx/              # final Word documents
│
├── cahier_fr.md          # Documentation in french
└── README.md             # Documentation
```

## Setup

Requirements: Python 3.8+, Git, and a GitHub account (to host the audio files).

**1. Clone and install**

```bash
git clone https://github.com/daryazdr/Gestion-de-projet-M1-IDL-UGA.git
cd Gestion-de-projet-M1-IDL-UGA

python -m venv venv
source venv/bin/activate      # Windows: venv\Scripts\activate

pip install -r requirements.txt
```

**2. Add your API keys**

Fill in `scripts/config.py`:

- `GROQ_API_KEY`
- `DEEPL_API_KEY`
- `MICROSOFT_TRANSLATOR_KEY` and `MICROSOFT_TRANSLATOR_REGION`
- `GITHUB_TOKEN`
- `GITHUB_REPO` (`owner/repo` or the repo URL)
- `GITHUB_PAGES_BASE_URL`

**3. Set up GitHub Pages**

The audio files and player pages are pushed to the `docs/` folder on the `main` branch, so GitHub Pages needs to serve from `main` / `docs`.

## Run

```bash
python scripts/main.py
```

The input file is set at the bottom of `scripts/main.py`. Change the path there to process another protocol.

## Notes

- The pipeline pauses at some steps so you can check the output by hand before continuing.
- If a file can't be read, check its encoding or the source file itself.