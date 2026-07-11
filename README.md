# Ward Rounds — Pediatrics

A single-file, offline medical quiz game styled as a **patient monitor**. Answer
multiple-choice clinical questions across a large pediatric bank, keep the ECG line
green, and track which subjects you're strong on and which need work.

Everything — questions, graphics, and synthesised audio — is embedded in one
`ward-round-quiz.html` file. No build step, no server, no external dependencies, and
it runs fully offline in any modern browser.

> **Disclaimer:** This is a self-testing revision aid built from flashcards, **not a
> clinical reference.** Do not use it for real patient-care decisions.

---

## Features

- **1,000+ questions** across 40+ topics, organised into themed **bundles**
  (Infectious disease, Neonatology, Gastroenterology, Respiratory, Rheumatic, Genetic
  Syndromes, Endocrinology, Nephrology, Hematology, Immunology, Oncology, and more).
- **Two modes:**
  - **Challenge** — 3 strikes end the round. The ECG monitor deteriorates with each
    mistake (sinus → tachycardia → V-fib → asystole), with a flatline failure, a
    one-shot **"Adrenaline and shock"** revive, and a **Return Of Spontaneous
    Circulation** comeback if you string together five correct answers after a shock.
  - **Free play** — no strikes; steady rhythm; a simple right/wrong tally at the end.
- **Three ways to play:** *All topics* (balanced shuffle of everything), *Bundles*
  (themed sets), or *Choose a topic* (pick one or several).
- **Flexible session length** — 10 / 20 / 30 presets, a full-bank option, and a
  **Custom** number entry for larger pools.
- **Help tokens** — a one-use **50:50** and a one-use **"Ask the clinician"** hint.
- **Synthesised audio (Web Audio, no files):** a chill 8-bit background track and a
  rhythm-synced ECG **beep** overlay, each toggled independently.
- **Progress tracking that persists between sessions** — per-topic accuracy, questions
  flagged to revisit, and a no-repeat rule so mastered questions stop reappearing.
- **On-demand report** (from the main page or after a round) showing your strongest and
  weakest subjects, with a **text export** you can download.
- **Installable on iPhone** via Safari's *Add to Home Screen* — runs full-screen like
  an app, with its own icon.

---

## Getting started

### Play locally
Download `ward-round-quiz.html` and open it in any browser (double-click, or drag it
into a browser window). That's it — it works offline.

### Host it (recommended for phone use)
Serve the single file from any static host so you can open it on your phone:

- **GitHub Pages:** enable Pages for this repo, then visit
  `https://<your-username>.github.io/<repo>/ward-round-quiz.html`
- **Netlify Drop / Vercel / Cloudflare Pages:** drag the file in and use the given link.

### Install on iPhone
1. Open the hosted link in **Safari** (Add to Home Screen is Safari-only on iOS).
2. Tap the **Share** button → **Add to Home Screen** → **Add**.
3. Launch it from the new icon — it opens full-screen.

> First launch needs a connection to fetch two web fonts; the questions and all game
> logic are baked into the file. For audio, make sure the phone isn't on silent.

---

## How progress is saved

Progress is stored in your browser's **local storage on that device** — it is private,
never uploaded anywhere, and survives closing or reopening the game (including the
home-screen app). Because it's device-local:

- it does **not** sync across different phones or browsers;
- clearing site data or using private browsing will remove it;
- if storage is unavailable, the game still runs and simply tracks progress for the
  current session only.

You can wipe your data anytime with **Reset progress** inside the report.

---

## Adding or editing questions

Questions live in a `TOPICS` array embedded near the top of the `<script>` block in
`ward-round-quiz.html`. Each topic looks like:

```js
{ id: "nec", name: "Necrotizing Enterocolitis (NEC)", short: "NEC", cards: [ /* ... */ ] }
```

and each card looks like:

```js
{
  qid: "nec-01",
  q: "Question text?",
  options: ["Option A", "Option B", "Option C", "Option D"],
  a: "Option B",                 // must exactly match one of the options
  hint: "A nudge toward the answer.",
  rationale: "Why the correct answer is correct."
}
```

Notes:
- **Stable IDs:** each card's `qid` is `<topicId>-NN`. The ID is shown on screen during
  play so you can note any question you'd like to fix.
- **True/False** questions simply use two options instead of four.
- **Bundles** are defined in a `BUNDLES` array; each lists the topic `id`s it groups.

The questions were originally authored as CSVs with the columns
`#, Question, Hint, Option A–D, Correct Answer, Rationale` and parsed into the embedded
data. If you keep those CSVs, that's the easiest format to extend from.

---

## Tech

- Plain **HTML + CSS + vanilla JavaScript** in one file.
- **Web Audio API** for all synthesised sound (no audio assets).
- **localStorage** for cross-session progress.
- Inline **SVG** for the scrolling ECG waveforms.
- iOS web-app meta tags + an embedded base64 icon for home-screen install.

No frameworks, no bundler, no npm install.

---

## License

Add a license of your choice (e.g. MIT) here. Note that the **question content** is
your own material — set its terms of use separately from the game code if needed.
