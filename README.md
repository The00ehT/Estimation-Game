# 📏 Schätzmeister / Estimaster

**An estimation game inspired by fermi.gg. Guess numbers you can't possibly know: the closer you get, the more points you score.**

### ▶ [Play now](https://the00eht.github.io/Fermi-Estimation-game/)

No download, no account, works in any browser.

Schätzmeister asks questions like *"How many hairs are on a human head?"* or *"How many days would it take to drive to the Moon at 100 km/h?"* and scores your estimate in **orders of magnitude**, not exact digits. Being off by a factor of 2 still earns you most of the points, while being off by a factor of 100 earns nothing.

It runs entirely in the browser and **works offline** once loaded. On a phone, use "Add to Home Screen" to install it like an app.

## Features

- **705 questions** in 11 categories: space, geography, people, the human body, nature, structures, transport, economy, tech, maths and everyday life. Roughly half are well-known facts. The other half are calculated questions that come with all the numbers you need to work out the answer.
- **German and English**: every question, unit and note is translated. Number input follows the language, so `1,5 Mio` works in German and `1.5m` works in English.
- **Six game modes**
  - **Daily puzzle**: the same 5 questions for everyone each day. It also tracks your daily streak.
  - **Free play**: 5, 10, 15 or 20 questions. You can tick any combination of categories (this also applies to endless, survival and blitz mode).
  - **Endless mode ∞**: questions keep coming until you stop. When you stop, you get statistics for that session.
  - **Survival mode ❤️**: you have 3 lives. Scoring below 50 points on a question (roughly 6× off) costs one life.
  - **Blitz mode ⚡**: 10 questions against the clock (10, 15, 20 or 30 seconds each). Whatever is in the box when time runs out gets scored. After each round you see how long each answer took. The stats tab compares your precision with and without the clock and shows your points by answer time.
  - **Review mistakes ↺**: every question you score below 50 on goes into a flashcard-style deck. It leaves the deck once you answer it with 75+ points twice in a row.
- **Statistics**: a radar chart of your strengths per category. It also shows your averages, your hit rate within 2×, your bullseyes, whether you tend to guess too high or too low, and your survival record.
- **Distribution per category**: tap a category in the radar chart or the table to see every estimate plotted against the correct answer, a histogram of your misses (finer the more estimates you have), your typical miss, your spread and any systematic bias.
- **Learn tab**: six short lessons on estimation techniques (orders of magnitude, setting bounds with the geometric mean, Fermi decomposition, anchor numbers, comparing, checking units), each with a worked example and an interactive practice question.
- **52 anchor numbers**: useful reference values such as the Earth's circumference, Germany's population or seconds per year, each with a tip on how to use it. You unlock one by answering the matching question with 80+ points in any regular game mode.
- **55 achievements**: for streaks of good guesses (3, 5, 10 in a row with 80+ points), bullseyes, flawless rounds, daily streaks, survival runs, blitz rounds and lightning-fast answers, playing every category, mastering mistakes, collecting anchor numbers and a few silly ones. Locked achievements show your progress.
- **Settings**: language, light or dark theme, the number of questions per round and the time limit in blitz mode.
- **CSV export and import**: back up all your progress as a CSV file or move it to another device.
- **Shareable results**: emoji squares in the style of Wordle (🟩🟨🟥).
- **Private by design**: all progress is stored only in your browser's localStorage.

## Scoring

```
points = 100 × (1 − |log₁₀(guess / answer)| / 2)^1.5
```

| Off by | Points |
|---|---|
| exact | 100 |
| 1.5× | ≈ 86 |
| 2× | ≈ 78 |
| 10× | ≈ 35 |
| 100× | 0 |

## Run it yourself

It's a plain static site with no build step, so any static web server works:

```bash
python3 -m http.server
```

Then visit `http://localhost:8000`. The offline mode only works over `http(s)`, not when you open the file directly with `file://`.

## Adding questions

Questions are stored in two files that must stay in the **same order**:

```js
// questions.js — [category, German question, answer, German unit, optional German note]
["Natur", "Wie schnell kann ein Gepard sprinten?", 100, "km/h"],

// questions_en.js — [English question, English unit, optional English note]
["How fast can a cheetah sprint?", "km/h"],
```

Categories use their German names (`Weltraum`, `Geografie`, `Menschen`, `Körper`, `Natur`, `Bauwerke`, `Verkehr`, `Wirtschaft`, `Technik`, `Mathe`, `Alltag`). They are translated automatically.

After changing any file, increase the cache version in `sw.js` (for example `schaetzmeister-v18` → `v19`) so that installed copies pick up the update.

## Files

| File | Purpose |
|---|---|
| `index.html` | the whole app: UI, logic and styles |
| `questions.js` | questions in German, with answers and categories |
| `questions_en.js` | English translations |
| `sw.js` | service worker for offline play |
| `manifest.webmanifest`, `icon*` | settings that make the game installable as an app, plus its icons |

## A note on accuracy

Answers are rounded reference values. Some of them, like populations or numbers of websites, change over time. Because scoring works on orders of magnitude, small differences between sources hardly matter. Found a wrong answer? Please open an issue.
