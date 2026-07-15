# Vocal Pitch Trainer

A browser-based ear-training and vocal intonation tool. Hear a note, sing it back, and see how close you were — measured in cents against the target pitch.

Created by **N. Scherbak**.

No build step, no dependencies, no server. One HTML file that runs entirely in your browser using the Web Audio API. Nothing is uploaded; the microphone audio never leaves your device.

## Modes

The tool is laid out as four steps, top to bottom:

1. **Selected tones** — build a phrase of up to 9 tones with the dropdown and Add button, or generate a **random arpeggio** (see below).
2. **Sound & playback** — pick pure tone or piano, and the playback tempo.
3. **Practice** — choose the format and how singing is graded, then Play.
4. **Expected vs. sung** — your results.

The tool is a five-step flow, each panel doing one thing:

1. **Selected tones** — choose the tones (add them, or generate a random arpeggio).
2. **Sound & playback** — pure tone or piano, and playback tempo.
3. **Grading** — Follow me or On the clock, plus singing time.
4. **Play & sing** — two buttons: **Play a tone** hears each tone on its own, one at a time; **Play a phrase** plays the whole sequence, then you sing it back. With a single tone selected, only **Play a tone** is available.
5. **Expected vs. sung** — your results.

Build any sequence of up to 9 notes with the note picker, which spans the full singing range (**E2–C6**, bass through soprano). When two or more notes are in the sequence, a **Play sequence** button appears so you can hear the whole phrase and repeat it in one go.

Choose the playback sound between a **pure tone** and a **piano** (a real sampled grand loaded over the network when online — see below). A live tuning meter shows your pitch in real time while you sing, and a results grid grades each note (on pitch ≤15¢, close ≤35¢, off beyond that).

## Random arpeggio

The **Random arpeggio** generator builds a musically real arpeggio — the notes of a chord played in sequence — rather than random pitches. Pick the chord type (major/minor triad, major/dominant/minor 7th, or octave), the number of **distinct tones**, and a **range**, and it generates a fitting arpeggio, cycling up through octaves for longer lengths. Turn on **Ascending + descending** to mirror the shape back down without repeating the top (e.g. C–E–G–E–C). If a range is too small for the requested length, it tells you.

## Getting started

The tool opens with no tones selected — build your phrase from scratch in step 1 (add tones or generate an arpeggio). The Play button stays disabled until at least one tone is chosen.

When a sung tone can't be graded, the feedback distinguishes two cases: **no sound detected** (nothing came through the mic) versus **tone too short** (sound was heard but not held long enough to read) — so you know whether to sing louder or hold longer.

## Feedback for beginners

- **You sang** shows the nearest note to what you actually sang, plus its own small deviation (e.g. `D3 +12¢`).
- **Off target** shows how far that is from the note you were asked to sing, in cents.
- A plain-English line translates the gap into musical terms — for example, *"You sang D3, which is about one and a half tones lower than C4."* No jargon required.
- A collapsible **"New to this?"** panel explains notes, semitones, tones, sharp/flat, and cents in plain language.

An info panel beside the Play buttons narrates each step: an animated wave while a note plays, a friendly **"Sing!"** cue with a soft, unlabelled progress bar (no stressful countdown) during your turn, and **"Complete — check the summary below"** at the end.

## Feedback

A **send feedback** link sits next to the author credit. It opens a small form (optional email + your message) that hands off to your own email app — nothing is sent anywhere else, and no address is stored in the page.

## Timing controls

**Singing time for each tone** (used in **On the clock** mode — it fades out in Follow me) sets how long you get to sing each tone:

- **Default** — adaptive: 3 seconds for a single tone, 2 seconds per tone when there are two or more.
- **1 sec / 2 sec / 3 sec** — a fixed time per tone for every tone in the sequence.

The dot timer adapts automatically to whatever the total window is: the final 5 seconds are shown as 10 fine dots (one every half-second), and everything before that as one dot per second, so long phrases stay readable.

**Playback tempo** (Default / Slow / Medium / High) sets how fast the app plays the reference tones to you — Slow is about one tone per second, up to High at three or four. It affects **playback only**; your singing time is governed entirely by the setting below, in On the clock mode.

## How singing is graded

Two ways to grade a sung phrase:

- **On the clock** — the classic mode. Each tone is expected during its highlighted slot on a fixed timer (the "Singing time for each tone" setting). Predictable and good for strict practice.
- **Follow me** — sing the phrase at your own pace and the app follows you. It records the whole take, finds the tones you actually *held* (a tone must be held for at least **half a second** to count, so an accidental sweep through the right pitch doesn't score), and matches them in order to the expected sequence. Timing no longer matters — only which tones you held, and in what order.

In **Follow me** mode the "Singing time for each tone" selector becomes an overall time *budget* per tone rather than a strict slot, with a floor that always leaves room to hold every tone for a full second. If the app detects a different number of held tones than expected, it tells you (e.g. "detected 4 held tones of 5"), which usually means two tones were slurred together or one wobbled into two.

Follow me is heuristic and works best for clear, deliberate singing with a small gap between tones; very legato phrases or heavy vibrato can confuse the tone-splitting. It's aimed at beginners and amateur singers who want natural practice rather than clock discipline.

## Recording environment

**Quiet room** (default) uses your raw microphone signal, which reads pitch most accurately. **Noisy room** turns on the browser's noise suppression and automatic gain control — it reduces background noise while recording your voice, but can slightly affect pitch accuracy. Switching this re-acquires the microphone, since the setting is applied when the mic starts.

## Mobile notes

The app works on phones, with a few browser-imposed limits worth knowing:

- **The microphone requires https.** Opening `index.html` from local storage on a phone will not work — use the GitHub Pages URL. The app now says so explicitly if it detects an insecure context.
- **On iOS every browser is Safari underneath**, so Safari's rules apply everywhere. Audio only starts from a tap (the Play buttons handle this), and the physical silent switch can mute playback.
- **Backgrounding the app** stops a recording in progress; the app detects this and aborts cleanly rather than grading noise.
- Pitch detection is throttled to ~30Hz to keep CPU use reasonable on phones.

## Sound: tone vs. piano

The **pure tone** is generated with the Web Audio oscillator and works fully offline.

The **piano** uses real recorded samples, with a three-step fallback so it works in every situation:

1. **Local samples** — if `samples/piano/` is present in the repo, the piano plays from your own copy with no external dependency. Samples are fetched individually as notes are played, not loaded as one package.
2. **CDN** — if there are no local samples but you're online, it loads a piano via [smplr](https://github.com/danigb/smplr).
3. **Synth tone** — if neither is available (e.g. `index.html` opened alone, offline), it falls back to the built-in synthesized tone.

### Adding the local piano samples

The app expects **Salamander Grand Piano V3** MP3 samples, velocity layer 8, in `samples/piano/`:

```bash
npm install @audio-samples/piano-mp3-velocity8
# copy node_modules/@audio-samples/piano-mp3-velocity8/audio/*.mp3 into samples/piano/
```

Filenames follow the pattern `C4v8.mp3`, `D#4v8.mp3`, `A2v8.mp3` — note name, then the `v8` velocity suffix. Salamander is sampled in minor thirds (C, D#, F#, A of each octave), and the app pitch-shifts by at most one semitone to fill the gaps, which is inaudible in practice. Only 16 files are needed to cover the app's E2–C6 range.

These samples are third-party content — see [NOTICES.md](NOTICES.md).


## Run locally

Just open `index.html` in a browser — or, because microphones need a secure context in some browsers, serve it:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Deploy to GitHub Pages

1. Create a new repository and push these files:

   ```bash
   git init
   git add .
   git commit -m "Pitch trainer"
   git branch -M main
   git remote add origin https://github.com/YOUR-USERNAME/pitch-trainer.git
   git push -u origin main
   ```

2. In the repository, go to **Settings → Pages**.
3. Under **Build and deployment**, set **Source** to *Deploy from a branch*, pick **main** / **`/root`**, and save.
4. After a minute your tool is live at `https://YOUR-USERNAME.github.io/pitch-trainer/`.

GitHub Pages serves over HTTPS, which is required for microphone access — so the deployed version will always be able to request the mic.

## Notes and limits

- Pitch detection uses **autocorrelation**, which is dependency-free and good for clean sustained notes but can wobble on breathy or heavy-vibrato singing. For more robust detection, swap in [pitchy](https://github.com/ianprime0509/pitchy) (McLeod method) or CREPE.
- Full-sequence mode uses a fixed time window per note rather than detecting note onsets, so sing at a steady pace. Proper onset segmentation (or dynamic time warping against the target) would make it timing-independent.
- Works in any modern browser. Requires microphone permission.

## Third-party content

The piano samples in `samples/piano/` are **Salamander Grand Piano V3**, recorded by Alexander Holm and packaged as MP3 by darosh (Jan Forst), used under the MIT license. They are not covered by this project's own license — see [NOTICES.md](NOTICES.md) for the full notice and attribution.

## License

MIT — see [LICENSE](LICENSE).
