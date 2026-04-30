# audio-reaper

Local command-line tool for downloading audio from YouTube and SoundCloud — no ads, no paywalls, no browser needed.

## Commands

| Command | Source | Quality |
|---------|--------|---------|
| `yta <url>` | YouTube | 320 kbps MP3 |
| `sca <url>` | SoundCloud | Best available (128–256 kbps) |

Both save to `~/Downloads`.

## Requirements

- macOS (or any Unix)
- [yt-dlp](https://github.com/yt-dlp/yt-dlp) — the actual downloader
- [ffmpeg](https://ffmpeg.org) — audio conversion

Install dependencies on macOS:

```bash
brew install yt-dlp ffmpeg
```

## Install

```bash
git clone https://github.com/da0101/audio-reaper.git
cd audio-reaper
./install
```

This creates symlinks in `~/.local/bin`. Make sure that's on your PATH:

```bash
# Add to ~/.zshrc if needed
export PATH="$HOME/.local/bin:$PATH"
```

## Usage

```bash
# YouTube — downloads as 320 kbps MP3
yta https://youtu.be/dQw4w9WgXcQ

# SoundCloud
sca https://soundcloud.com/artist/track

# Different format (mp3, m4a, wav, flac)
yta https://youtu.be/... flac
sca https://soundcloud.com/... wav
```

Files are named by title and saved to `~/Downloads`.

## How it works

The scripts are minimal wrappers around `yt-dlp`. No Python packages, no Node, no runtime dependencies beyond yt-dlp and ffmpeg. Each script is ~30 lines of bash.

## License

MIT
