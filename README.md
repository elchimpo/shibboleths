# Shibboleths — Gaz Wills

A single-page static album player. Centered digipack cover, minimal play/stop/skip
controls, a clickable tracklist, and a muted WebGL audio-reactive background.

Open-source pieces used:
- **three.js** (MIT) — the WebGL background shader (loaded from CDN).
- **Web Audio API** (browser standard) — playback + the frequency analyser that
  drives the visualization. WAV files stream natively (progressive download).

## Files
- `index.html` — the page (a copy of `Shibboleths.dc.html`).
- `support.js` — runtime for the component. **Must sit next to `index.html`.**
- `playlist.json` — **edit this to configure the album.** No code changes needed.
- `assets/cover.png` — the centered front cover.

## Configuring the playlist
Edit `playlist.json`:

```json
{
  "artist": "Gaz Wills",
  "album": "Shibboleths",
  "cover": "assets/cover.png",
  "tracks": [
    { "title": "kalimab", "src": "https://www.dropbox.com/scl/fi/.../01-kalimab.wav?rlkey=...&dl=0" }
  ]
}
```

- `src` is the **normal Dropbox share link** for each `.wav` (the one ending in
  `?dl=0` / `?rlkey=...`). The player rewrites it to a direct-stream URL
  automatically (`dl.dropboxusercontent.com`, `dl=1`) — you don't need to.
- Re-order tracks by re-ordering the array; add/remove freely.

### Dropbox note (CORS)
For the background visualization to *react* to the audio, the browser must be able
to read the audio stream cross-origin. Dropbox direct links generally send the
right header, but if a track plays while the background stays idle, that track's
host blocked cross-origin reads. Playback still works; the visualization just falls
back to its gentle ambient motion. Hosting the WAVs somewhere with
`Access-Control-Allow-Origin: *` (e.g. an S3/R2 bucket) guarantees reactivity.

## Deploying to GitHub Pages
1. Commit `index.html`, `support.js`, `playlist.json`, and `assets/` to your repo.
2. Settings → Pages → deploy from your branch root (`/`).
3. Visit the published URL. Edit `playlist.json` anytime and push — no rebuild.

## Tweaks
The background brightness is adjustable via the `vizIntensity` prop (0–1, default
0.5) in the component editor.
