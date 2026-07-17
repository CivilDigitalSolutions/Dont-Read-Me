# Don't Read This — Text-to-Speech Web Tool

A zero-cost, zero-backend text-to-speech tool that runs entirely in your browser
using the built-in Web Speech API. Paste text, pick a voice, press **Read**.
Nothing is uploaded — synthesis happens on your device.

## Status

**Stage 1 shipped** — static single-page app, Web Speech API only. This is the
whole tool for most users. Later stages (premium OpenAI voices via a Cloudflare
Worker, graceful degradation, PWA polish) are planned but deliberately deferred;
each stage ships standalone.

## Features (Stage 1)

- Textarea with a live character counter
- Read / Pause–Resume / Stop controls
- Voice dropdown populated from your device (English voices, remote voices first)
- Speed slider (0.5×–2.0×) and pitch slider (0–2)
- Voice, speed, and pitch remembered for the session

### Browser-quirk handling baked in

- **Async voice loading** — loads voices immediately *and* on `voiceschanged`,
  so both Chrome (async) and Safari (synchronous, no event) populate correctly.
- **Chrome long-utterance truncation** — text is split into sentence-sized
  chunks (with a 200-char hard-split on word boundaries for long sentences and
  pasted URLs) and queued as separate utterances, so 5,000-word pastes play
  start to finish.
- **Chrome pause bug** — a 10-second `resume()` keep-alive ping runs while
  speaking (and is suppressed while paused so Pause actually holds).
- **Mobile gesture requirement** — speech only starts from a real Read click;
  nothing autoplays on load.

## Usage

Open `index.html` in any modern browser, or visit the hosted version.

## Deploy (GitHub Pages)

No build step. It's a single static `index.html` at the repo root.

1. Repo **Settings → Pages**
2. Source: **Deploy from a branch**
3. Branch: **main** / **/ (root)**

Published URL: `https://civildigitalsolutions.github.io/Dont-Read-Me/`

`.nojekyll` is included so Pages serves files as-is. `manifest.webmanifest` is a
PWA stub (seam toward the optional Stage 4 installable app); no service worker
is registered in Stage 1.

## Roadmap

| Stage | What | State |
|---|---|---|
| 1 | Static page, Web Speech API | ✅ Done |
| 2 | Cloudflare Worker + OpenAI TTS, hard-capped | Deferred |
| 3 | Graceful degradation back to Web Speech on cap | Deferred |
| 4 | PWA polish (manifest + service worker) | Deferred |

Stages 2–4 require a Cloudflare Worker and an OpenAI key that aren't set up yet,
and the plan says to build them only after Stage 1 has been used enough to want
better voices. The code leaves clean seams for them (isolated chunker, an engine
interface the cloud path can slot behind).
