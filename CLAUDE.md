# 禁毒连连看 — 案件调查档案

## Versioning

- Version format: `v1.6.x`
- **Patch (1.6.x)**: Bug fixes only
- **Minor (1.x.0)**: New features
- Update version number in `ver-label` span AND commit message on every push

## Tech Stack

- Single-file HTML5 game (no build tools)
- Web Audio API for sound effects and BGM
- LocalStorage for game progress
- GitHub Pages for deployment

## Audio Architecture

- **SFX**: Web Audio oscillator/noise synthesis (8 sound effects)
- **BGM**: MP3 fetched → `decodeAudioData` (callback form for Safari/Quark) → `AudioBufferSourceNode.loop`
- Both share single `AudioContext` via `masterGain`
- Mobile: `ctx.resume()` must happen inside `click` event (not `touchstart`)
- `source.start(ctx.currentTime)` — never `start(0)`

## Key Browser Quirks

- Quark/UC browsers: AudioContext only via `click`, `decodeAudioData` callback form required
- iOS Safari: `decodeAudioData` Promise form unreliable
- All mobile: autoplay blocked, first-tap audio unlock needed

## File Structure

```
jd/
├── 禁毒连连看.html     # The game
├── bgm/
│   └── bgm_alt1.mp3    # Active BGM (tracked)
│   ├── bgm_*.mp3       # Inactive (gitignored, local only)
├── 毒品图片-1/          # Drug evidence images
└── .gitignore
```

## Deploy

```sh
git push  # GitHub Pages auto-deploys from main
# URL: https://you-chen0.github.io/jd/禁毒连连看.html
```
