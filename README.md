<div align="center">

# ⋆౨ৎ⋆ Music Downloader ⋆౨ৎ⋆

***Batch-download a whole list of tracks as high-quality MP3s, straight from a text file***

<br>

![Python](https://img.shields.io/badge/Python-3.10+-FFB5C2?style=flat-square&logo=python&logoColor=white)
![yt-dlp](https://img.shields.io/badge/yt--dlp-downloader-C8B6FF?style=flat-square&logo=youtube&logoColor=white)
![ffmpeg](https://img.shields.io/badge/ffmpeg-MP3%20convert-B5D8FF?style=flat-square&logo=ffmpeg&logoColor=white)
![CLI](https://img.shields.io/badge/interface-CLI-B8E6D9?style=flat-square)
![MP3](https://img.shields.io/badge/output-MP3%20♡-FFE0B5?style=flat-square)

<br>

*Give it a plain `.txt` list of song titles - it searches each one on YouTube,*
*grabs the best audio, and saves tidy MP3s with metadata & album art baked in*

</div>

---

## What is this?

A tool which utomatically downloads music as high-quality MP3 from a plain text list of track titles. The script searches each title on YouTube and downloads the best available audio, with metadata and album art embedded.

Spotify/Youtube playlist option coming soon /•᷅‎‎•᷄\੭

I created this project because I needed to batch download content to train DJing from home.

---

## Features

- **Batch downloads** - one run turns your whole list into MP3s
- **Best-quality audio** - pulls `bestaudio` and converts to **MP3 at max VBR** (`quality 0`)
- **Auto-tagging** - embeds title & artist metadata *and* album art via ffmpeg post-processors
- **Skips duplicates** - files that already exist aren't re-downloaded (`nooverwrites`)
- **Comment-friendly lists** - lines starting with `#` are ignored
- **Polite by default** - a configurable delay between downloads to dodge rate-limiting
- **Pretty terminal output** - colored progress, per-track status, and a final summary ✧

---

## How it works

```
   titles.txt                    for each title
   ┌──────────────┐              ─────────────────────────────────────────
   │ # my tracks  │              ① search YouTube  →  ytsearch1:"<title> audio officiel"
   │ Daft Punk -… │  ──parse──▶  ② download bestaudio      (yt-dlp)
   │ PLK - ça …   │   (skip #)   ③ convert → MP3 max VBR    (ffmpeg)
   └──────────────┘              ④ embed metadata + cover art
                                 ⑤ wait `--delay` seconds, next track
                                 ─────────────────────────────────────────
                                          │
                                          ▼
                              ♫  ./musiques/Artist - Title.mp3
                              + a tidy success / failure summary
```

Under the hood it's a single script wrapping **yt-dlp**: it builds a `ytsearch1:` query (taking the first YouTube result), downloads the best audio stream, and runs three ffmpeg post-processors - extract-to-MP3, embed-metadata, and embed-thumbnail - so every file lands tagged and cover-arted. A quiet custom logger + progress hook keep the terminal output clean and readable

---

## Getting started

### Requirements

- **Python 3.10+**
- **yt-dlp** - `pip install yt-dlp`
- **ffmpeg** (needed for the MP3 conversion):

| OS | Command |
|----|---------|
| Windows | Download from [ffmpeg.org](https://ffmpeg.org/download.html) and add it to your PATH |
| Linux (Debian/Ubuntu) | `sudo apt install ffmpeg` |
| macOS | `brew install ffmpeg` |

### Your titles file

Make a `.txt` file with one song per line - **`Artist - Title`** is strongly recommended for accurate matches. Lines starting with `#` are treated as comments.

```text
# my favourite tracks
Daft Punk - Touch
Veridis Project - ça va ensemble II
PLK - ça mène à rien
```

### Run it

```bash
# basic
python music_downloader.py --list titles.txt

# custom output folder
python music_downloader.py --list titles.txt --output ./music

# custom delay between downloads (seconds)
python music_downloader.py --list titles.txt --delay 3

# all together
python music_downloader.py --list titles.txt --output ~/Music --delay 2
```

| Flag | Short | Default | What it does |
|------|:-----:|:-------:|--------------|
| `--list` | `-l` | *(required)* | your titles `.txt` file |
| `--output` | `-o` | `./musiques` | where the MP3s are saved |
| `--delay` | `-d` | `2` | seconds to wait between downloads |

---

## Project structure

```
music-downloader/
├── music_downloader.py   ⋆ the whole thing: parse list → search → download → tag
└── README.md
```

---

## Notes & tips 🍮

- **Wrong song?** The script takes the *first* YouTube result, so `Artist - Title` gives far better matches than a bare title
- **Blocked or slow?** Bump the delay (e.g. `--delay 5`) to be gentler on YouTube
- Prefer output paths **without spaces** (`./my_music` over `./My Music`) 

---

<div align="center">

*Made by [**natsxki**](https://github.com/natsxki) 

</div>
