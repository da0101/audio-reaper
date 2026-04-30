# audio-reaper

Local command-line tool for downloading audio from YouTube and SoundCloud. No ads, no paywalls, no browser needed.

## Install

```bash
git clone https://github.com/da0101/audio-reaper.git
cd audio-reaper
./install
```

The install script handles everything:
- Installs `yt-dlp` and `ffmpeg` via Homebrew (if missing)
- Creates symlinks in `~/.local/bin`
- Adds `noglob` aliases to `~/.zshrc` so you can paste URLs without quoting

After install, open a new terminal or run `source ~/.zshrc`.

## Commands

| Command | Source | Quality |
|---------|--------|---------|
| `reap` | Auto-detects YouTube / SoundCloud / other | 320 kbps (YT) · best available (SC) |
| `yta` | YouTube only | 320 kbps |
| `sca` | SoundCloud only | Best available (128–256 kbps) |
| `reap-reset` | — | Reset download folder to ~/Downloads |

`reap` is the main command — it detects the URL source and applies the right quality settings automatically. `yta` and `sca` are available if you want to target a specific platform directly.

## Usage

### Search by name

Just type what you're looking for — no URL needed:

```bash
reap "michael jackson thriller"
yta "the weeknd blinding lights"
sca "flume say it"
```

With a [Gemini API key](#smart-search-with-gemini), vague queries work too:

```bash
reap "that kate bush song from stranger things"
# -> resolves to "Kate Bush - Running Up That Hill"
```

Without the API key, your query goes directly to YouTube/SoundCloud search (works fine for specific queries).

### Single URL

```bash
reap https://www.youtube.com/watch?v=dQw4w9WgXcQ
```

### Multiple URLs (mixed sources)

```bash
reap https://youtu.be/abc https://soundcloud.com/artist/track https://youtu.be/xyz
```

`reap` groups them by source, downloads YouTube tracks at 320 kbps and SoundCloud at best available quality.

### From a text file

Create a file with one URL per line:

```
# my-playlist.txt
https://youtu.be/abc
https://youtu.be/xyz
https://soundcloud.com/artist/track
```

Lines starting with `#` are ignored. Then:

```bash
reap my-playlist.txt
```

### Choose format

Default is MP3. Override with `--format` or `-F`:

```bash
reap https://youtu.be/abc --format flac
reap https://youtu.be/abc --format m4a
reap https://youtu.be/abc --format wav
```

### Custom download folder

Default is `~/Downloads`. Set a custom folder with `--output` or `-o`:

```bash
reap --output ~/Music/beats https://youtu.be/abc
```

The path **persists** — all future downloads go to that folder until you reset:

```bash
# These all go to ~/Music/beats now
reap https://youtu.be/xyz
yta https://youtu.be/123
sca https://soundcloud.com/artist/track

# Reset back to ~/Downloads
reap-reset
```

The output path is shared across `reap`, `yta`, and `sca`. Set it from any of them.

### Platform-specific commands

```bash
# YouTube only
yta https://youtu.be/abc
yta https://youtu.be/abc https://youtu.be/xyz
yta playlist.txt

# SoundCloud only
sca https://soundcloud.com/artist/track
sca playlist.txt --format flac
```

## Smart search with Gemini

Set a Gemini API key to enable AI-powered query refinement. Vague descriptions like "that sad radiohead song" get resolved to the exact artist and title before searching.

```bash
echo 'export GEMINI_API_KEY=your-key' >> ~/.zshrc
source ~/.zshrc
```

Get a free API key at [Google AI Studio](https://aistudio.google.com/apikey).

Without the key, search still works — queries go directly to YouTube/SoundCloud search. The key just makes fuzzy queries smarter.

## How it works

The scripts are thin wrappers around [yt-dlp](https://github.com/yt-dlp/yt-dlp) and [ffmpeg](https://ffmpeg.org). No Python packages, no Node, no runtime dependencies beyond those two CLI tools. Each script is ~70 lines of bash.

Downloaded files include embedded thumbnails and metadata (artist, title) so they show up correctly in music players and Finder.

## Requirements

- macOS (or any Unix with bash)
- [Homebrew](https://brew.sh) (for dependency install)
- [yt-dlp](https://github.com/yt-dlp/yt-dlp)
- [ffmpeg](https://ffmpeg.org)

`./install` installs yt-dlp and ffmpeg automatically if they're missing.

## Uninstall

```bash
rm ~/.local/bin/yta ~/.local/bin/sca ~/.local/bin/reap ~/.local/bin/reap-reset
rm -rf ~/.config/audio-reaper
```

Remove the `# audio-reaper` block from `~/.zshrc`.

## License

MIT
