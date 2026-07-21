# Vocal Pitch Trainer

A browser-based ear-training and vocal intonation tool. Hear a note, sing it back, and see how close you were — measured in cents against the target pitch.

Created by **N. Scherbak**.

No build step, no dependencies, no server. The app is a single self-contained HTML file that runs entirely in your browser using the Web Audio API. Nothing is uploaded; the microphone audio never leaves your device. (`test.html` next to it is a developer self-check, not part of the app — see below.)

## For beginners — it works out of the box

You don't need to change any settings. Every default is chosen to be beginner-friendly:

- **Note detection is forgiving** — it accepts low, breathy, or slightly unsteady notes rather than insisting on a perfectly clean tone, so ordinary singing registers instead of reading as "no sound".
- **The pitch dial is in Beginner mode** — the needle keeps moving even when you start far off, so you can tell you're getting closer rather than just seeing it pegged.
- **Grading follows you** — sing at your own pace; the app matches the notes you actually held, in order, without holding you to a stopwatch.
- **Timing is relaxed** — you get a roomy window to find and hold each note.

Just pick a note, press Play, and sing. Everything below this is optional.

**One thing to know:** if your singing reads as "no sound", it's almost always the microphone, not you — some call/meeting headsets process the signal in a way that hides the pitch. See **Choosing a microphone** below; a phone or a cheap wired earbud usually works better than a fancy headset.

## For advanced singers — the Settings section

Open **Settings** (collapsed by default) to tailor the app to a trained voice. The one-line summary under the header always shows your current choices. What you can change:

- **Note detection: Forgiving / Strict** — Strict scores only clean, steady sustained tones and rejects breathy or unsteady singing. Use it if you have a steady low voice and want the app to hold you to a higher standard; leave it on Forgiving otherwise.
- **Pitch dial: Beginner / Advanced** — Advanced gives a precise ±50¢ scale across the dial (a quartertone full-width) instead of compressing a whole octave onto it.
- **Grading: Follow me / On the clock** — On the clock expects each note during a fixed timed slot, for strict practice.
- **Singing time, playback tempo, sound, and microphone environment** — all adjustable; see the sections below.

## Layout

The interface is deliberately compact, top to bottom:

- **Tones** — build your phrase. Toggle between **Manual** (pick tones with the dropdown) and **Arpeggio** (generate a random arpeggio). Up to 9 tones.
- **Settings** (collapsed by default) — sound, playback tempo, grading mode, singing time, pitch dial scale, and microphone environment. The defaults suit most people; the collapsed row shows a one-line summary of the current settings.
- **Play & sing** — **Play a tone** hears each tone on its own, one at a time; **Play a phrase** plays the whole sequence, then you sing it back. With a single tone selected, only **Play a tone** is available.
- **Results** — expected vs. sung for each tone.

Build any sequence of up to 9 notes with the note picker, which spans the full singing range (**E2–C6**, bass through soprano). When two or more notes are in the sequence, **Play a phrase** appears so you can hear the whole phrase and sing it back in one go.

Choose the playback sound between a **pure tone** and a **piano** (a real sampled grand loaded over the network when online — see below). A live tuning meter shows your pitch in real time while you sing, and a results grid grades each note (on pitch ≤15¢, close ≤35¢, off beyond that).

## Random arpeggio

The **Random arpeggio** generator builds a musically real arpeggio — the notes of a chord played in sequence — rather than random pitches. Pick the chord type (major/minor triad, major/dominant/minor 7th, or octave), the number of **distinct tones**, and a **range**, and it generates a fitting arpeggio, cycling up through octaves for longer lengths. Turn on **Ascending + descending** to mirror the shape back down without repeating the top (e.g. C–E–G–E–C). If a range is too small for the requested length, it tells you.

## Getting started

The tool opens with no tones selected — build your phrase from scratch in step 1 (add tones or generate an arpeggio). The Play button stays disabled until at least one tone is chosen.

When a sung tone can't be graded, the feedback says which of three things happened: **no sound detected** (nothing came through the mic), **tone too short** (sound was heard but not held long enough to read), or **pitch kept moving** (a tone was heard but never settled — a slide or a hunt rather than a held note). So you know whether to sing at all, hold longer, or settle on one pitch.

Quiet singing is fine. The app judges whether a signal is *periodic*, not whether it's loud, so a soft sustained vowel reads normally — you shouldn't need to sing up to be heard.

## The pitch dial

The live meter shows how far you are from the target while you sing. It has two scales, in **Settings → Pitch dial**:

- **Beginner** (default) — the needle stays 1:1 near the target, then compresses further out, so a whole octave fits on the dial. If you start a fifth or an octave off, the needle still moves as you close in — it tells you you're getting warmer, not just that you're flat.
- **Advanced** — ±50¢ across the full dial. Precise, but it pegs at the edge and stops moving if you're further off than that.

The green zone (±15¢) occupies exactly the same screen space in both, so switching scales doesn't change what "close" feels like — only what happens once you're well outside it. This affects the display only; scoring is identical in both modes.

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
- **Follow me** (default) — sing the phrase at your own pace and the app follows you. It records the whole take, finds the tones you actually *held* (a tone must be held for at least **half a second** to count, so an accidental sweep through the right pitch doesn't score), and matches them in order to the expected sequence. Timing no longer matters — only which tones you held, and in what order.

Both modes work with a single tone as well as a phrase.

In **Follow me** mode the "Singing time for each tone" selector becomes an overall time *budget* per tone rather than a strict slot, with a floor that always leaves room to hold every tone for a full second.

Matching is an order-preserving alignment, so a missed or extra note stays a local error: if you skip the middle note of a three-note phrase, the other two are still scored correctly and the missing one is reported as missing — rather than every later note being compared against the wrong target. Notes you didn't sing count against your score rather than quietly vanishing from the average.

A brief dropout mid-note — a breath catch, or a moment where the voice dips — no longer splits one tone into two, or makes it disappear for being too short. Gaps under ~150 ms on the same pitch are treated as one continuous tone; longer silences, or a return on a different pitch, are a real pause.

Follow me works best for clear, deliberate singing with a small gap between tones. Very legato phrases can still confuse the tone-splitting, since the app can only separate tones by pitch change when there's no silence between them.

## Note detection

**Forgiving** (default) accepts low, breathy, or slightly unsteady notes — the app looks for a periodic signal and, in this mode, tolerates a less-than-perfect one. This suits most singers, and especially low notes (around C3 and below) where the voice is naturally less stable. **Strict** tightens the thresholds so only clean, steady sustained tones are scored; a trained voice practising precise low notes may prefer it, but it will reject breathy or wavering singing as "no sound".

If a note you clearly sang reads as "no sound", Forgiving mode usually fixes it. If it doesn't, the cause is almost always the microphone rather than the app — see **Choosing a microphone** below.

## Choosing a microphone

This matters more than you'd expect, and it's not about cost. The app needs to see the actual *waveform* of your voice to measure its pitch. Some microphones — especially headsets and earpieces designed for calls and meetings — run the signal through built-in processing (noise reduction, voice isolation, automatic gain) that is tuned to make **speech** clear, not to preserve **pitch**. That processing can smear or scramble the low frequencies of a sung note before the audio ever reaches the browser, so a note you sang cleanly arrives as something the app correctly reads as "not a tone".

The tell-tale sign: a note reads as "no sound" or a wildly wrong pitch on one device but works fine on another — a phone, for instance — with the same voice.

What works well:

- A phone (the built-in mics on phones handle sung pitch well)
- Plain wired earbuds/earphones with an inline mic (inexpensive is fine)
- A laptop's built-in microphone
- Any basic USB or plugged-in microphone that isn't a "communications" headset

What to avoid for singing practice:

- Headsets and earpieces that advertise noise cancellation or "crystal-clear calls" — the same processing that helps calls hurts pitch detection
- Bluetooth headsets used as a microphone (call mode heavily downsamples and processes the mic)

You don't need anything expensive — a cheap wired earbud mic often outperforms a pricey gaming or conferencing headset here, precisely because it does less to the signal. If you must use a processing headset, try turning off its "enhancements" or "signal processing" in your operating system's sound settings, though not all of them expose that option.

If you're not sure whether your mic is the problem, turn on **Show detection diagnostics** (Settings → Developer tools) and sing a note: a `best-dip` value near 0 means the mic is delivering a clean, periodic signal, while a value near 1 means the signal reaching the app has no usable pitch — a strong sign the mic is processing it.

## Recording environment

**Quiet room** (default) uses your raw microphone signal, which reads pitch most accurately, and asks the browser to disable its echo cancellation, noise suppression, and gain control (including vendor-specific processing) so nothing alters the waveform. Note that this only controls the *browser's* processing — a microphone that processes the signal in its own hardware or in the operating system (see **Choosing a microphone**) is not affected by this setting. **Noisy room** turns the browser processing on — it reduces background noise while recording your voice, but can slightly affect pitch accuracy. Switching this re-acquires the microphone, since the setting is applied when the mic starts.

## Mobile notes

The app works on phones, with a few browser-imposed limits worth knowing:

- **Silent / ring mode mutes the speaker.** If the phone's hardware silent switch (iPhone) or ring/mute mode (Android) is on, the browser cannot play the reference tone through the **speaker** — you'll see the note "play" but hear nothing. **Headphones play even in silent mode**, so plugging them in is the quickest fix (and better for singing practice anyway); otherwise turn silent mode off. A web page has no way to *detect* the switch, so the app shows a one-time reminder on phones. (Your singing is still recorded fine either way — this only affects playback.)
- **On iOS every browser is Safari underneath**, so Safari's rules apply everywhere. Audio only starts from a tap (the Play buttons handle this).
- **The microphone requires https.** Opening `index.html` from local storage on a phone will not work — use the GitHub Pages URL. The app says so explicitly if it detects an insecure context.
- **On iOS the mic is released between takes**, because iOS routes playback to the quiet earpiece for as long as a mic stream is live — so the reference tone would be inaudible otherwise. That means iOS may ask about the mic more than once. On desktop the stream is held for the session and muted between takes instead, so permission is asked for once.
- **Backgrounding the app** stops a recording in progress; the app detects this and aborts cleanly rather than grading noise. On desktop it also releases the mic, so the browser's recording indicator doesn't stay lit while you're in another tab.
- Pitch detection runs at ~30Hz to keep CPU use reasonable on phones.

## When the microphone won't start

Different failures get different messages, so you can tell them apart:

- **Microphone blocked** — permission was refused. Allow mic access for the site.
- **No microphone found** — nothing is connected.
- **Audio didn't start** — permission was granted but the browser's audio engine didn't come up. Press Play to try again. (This is a real Chromium/Edge behaviour: the request to start audio can hang rather than fail. The app now gives up after 1.5 seconds and tells you, instead of sitting on "starting the microphone…" forever.)
- **Microphone needs https** — you've opened the file locally rather than over a URL.

In every case the app re-enables itself and Play retries. It should never get stuck in a listening or loading state.

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


## Tuning

A few constants near the top of their sections in `index.html` control how forgiving the app is. All were chosen against synthetic test signals, not real voices, so they're the first things to adjust if the app disagrees with your ears. `test.html` will tell you if you push one far enough to break something else.

| constant | default | raise it if… | lower it if… |
|---|---|---|---|
| `CONF_MIN` | 0.5 | room noise is being scored as notes | quiet singing reads as "no sound" |
| `MAX_UNSTABLE` | 45¢ | steady singing is rejected as "pitch kept moving" | slides and wobbles are being scored as held tones |
| `GAP_BRIDGE_MS` | 150 ms | one held tone keeps splitting into two | two deliberate notes get merged into one |
| `MIN_HOLD` | 500 ms | brief blips are counted as notes | short deliberate notes are being missed |
| `BEG_MAX_C` | 1200¢ | — | the Beginner needle feels too lively when far off |
| `SEG_TOL` | 60¢ | legato notes aren't being separated | vibrato is splitting one note into several |

The measured margins behind two of these: a sung vowel scores 0.83–1.00 confidence while pure noise tops out around 0.11, so `CONF_MIN` sits in a wide empty gap rather than near an edge. Heavy vibrato measures ~32¢ of wobble and a slide through the target ~85¢, so `MAX_UNSTABLE` separates them with room to spare.

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

- Pitch detection uses the **YIN algorithm**, which finds the period at which the signal best repeats and is robust against the octave errors that simpler autocorrelation is prone to. It reports a **confidence** value (how cleanly the signal repeats) alongside each reading; readings below the confidence threshold are discarded rather than graded, which is what lets quiet singing be read accurately instead of written off as silence. The **Forgiving / Strict** setting adjusts that threshold. Very high input sample rates (96/192 kHz) are decimated toward 48 kHz first, and the mic is band-limited to roughly 60–1400 Hz before analysis to keep hum and high harmonics from misleading it.
- The lowest **target** is E2 (~82 Hz), but the detector reaches a whole tone lower so it can tell you how far below E2 a flat note landed. Below that, or for genuinely aperiodic input, it reports "no sound" rather than guessing. Mains hum (50/60 Hz) sits below the detection floor.
- **Follow me** matches your held tones to the expected sequence with an order-preserving dynamic-programming alignment, so a dropped or added note stays a local error instead of shifting every note after it. **On the clock** still uses fixed time windows, so sing at a steady pace in that mode.
- Grading measures the pitch you **settled on**, not your closest instant: it finds the steadiest stretch of the take, reports the median deviation there, and measures steadiness separately. A slow slide through the target doesn't score as a held tone, and an initial scoop is ignored without a hard grace period.
- Works in any modern browser. Requires microphone permission.

## Developer tools

At the bottom of Settings, a collapsed **Developer tools** section holds diagnostics that ordinary use never needs:

- **Show detection diagnostics** — adds a technical line under each take: the detected pitch, sample rate, confidence, periodicity, harmonic profile, and why a note wasn't scored. Useful for troubleshooting a mic or a "no sound" report.
- **Use legacy detector** — swaps YIN for the older, simpler autocorrelation detector, for comparison.
- **Bypass audio filters** — feeds the raw mic signal to the detector, skipping the band-pass filters (takes effect the next time the mic starts).

## Self-check

`test.html` sits next to `index.html` and checks the grading maths — 148 checks covering pitch detection (including sample-rate independence and forgiving/strict modes), single-tone scoring, phrase alignment, microphone startup, below-E2 feedback, and the dial scale. Open it and press **Run the checks**. (Opening it from a folder rather than a URL may stop the browser reading `index.html` on its own; drag the file onto the drop box if so.)

It does **not** test the app — no microphone, no audio, no interface. It tells you whether an edit broke the scoring. Green means any problem you're seeing is the browser, mic, or audio rather than the maths. The page explains all of this in more detail when you open it.

## Changelog

### v2.2
Layout update (Tone section)

### v2.1

Detection, feedback, and mobile improvements. All backward-compatible; existing behaviour is preserved, with smarter defaults.

- **New pitch detection (YIN).** Replaced the earlier autocorrelation detector with the YIN algorithm.
- **Forgiving / Strict detection modes** (Settings → Note detection). 
- **Below-E2 feedback.** A note sung below the lowest target (E2) is now reported by how far below it landed.
- **Sample-rate independence.** Detection no longer degrades on audio hardware running at 96/192 kHz, and the mic signal is band-limited (~60–1400 Hz) before analysis.
- **Improved detection robustness**
- **Microphone guidance** (README). Added a "Choosing a microphone" section: some headsets and call-optimised mics process the signal in ways that hide sung pitch, and an inexpensive clean mic (a phone, a plain wired earbud, a built-in laptop mic) often works far better than a pricey processing headset.
- **Silent-mode reminder on phones.**
- **Developer tools** (Settings, collapsed): detection diagnostics, a legacy-detector toggle, and a filter-bypass toggle, for troubleshooting.
- **Self-check** grew to 148 checks, now covering sample-rate independence, the detection modes, and below-E2 feedback.

### v2.0

Grading and detection overhaul: order-preserving phrase alignment, steadiness-aware scoring, confidence-based detection, the Beginner/Advanced pitch dial, and the `test.html` self-check.

## Third-party content

The piano samples in `samples/piano/` are **Salamander Grand Piano V3**, recorded by Alexander Holm and packaged as MP3 by darosh (Jan Forst), used under the MIT license. They are not covered by this project's own license — see [NOTICES.md](NOTICES.md) for the full notice and attribution.

## License

MIT — see [LICENSE](LICENSE).
