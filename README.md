# Bare Metal

**Code and learn data structures & algorithms without your AI crutches.**

A single-file web app: a deliberately bare Python editor (no autocomplete, no
syntax highlighting, no execution while you type), a Claude-powered coach /
interviewer that watches your code and thinking in real time and interrupts
you mid-thought, a step-through debugger for practice, and rapid
pattern-recognition drills. Built for engineers whose daily code is written
with Claude Code / Cursor / Copilot and who need to rebuild "code author"
muscle memory for whiteboard-style interviews.

No build. No server. No dependencies. One `index.html`.

---

## Quick start (3 minutes)

1. **Get the code**
   ```bash
   git clone https://github.com/demart78/Template.git
   cd Template
   ```
2. **Open the app** — double-click `index.html`, or serve it:
   ```bash
   python3 -m http.server   # then visit http://localhost:8000
   ```
   Use **Chrome or Edge** for the full experience (voice narration needs them).
3. **Paste your Anthropic API key** (get one at
   [console.anthropic.com](https://console.anthropic.com)). It's stored only
   in your browser's localStorage; every request goes straight from your
   browser to `api.anthropic.com` — there is no middleman server.
4. **Pick Coach mode + a topic** and press **Start session**.
5. **Talk out loud.** Type your thinking into the narration box (Enter to
   log) or click 🎙 for voice. Go silent too long and you'll be interrupted —
   that's the product working, not a bug.

---

## Your first session, step by step

A good first loop takes ~30 minutes and touches everything:

1. **Warm up with a Pattern Sprint** (setup screen). 8 rapid rounds: read a
   disguised problem statement, name the pattern before you'd write any code.
   Wrong answers show you the "tell" you missed. Score lands in your history.
2. **Start Coach mode → Intervals** (or your weak topic). The coach greets
   you and asks for your first-instinct classification. Answer in the
   narration box. Write your solution in the editor.
3. **Predict, then trace.** When your code looks done, click **Step through**
   (top of the editor). The call box is pre-filled from the problem's first
   example. Before you hit Trace, say what you expect — then scrub through
   the execution with **← / →** and watch every variable change (changed
   values highlighted). This is where bugs become visible without a single
   `print`.
4. **Check syntax** (free in coach mode) until it compiles clean.
5. **End session → study report.** You get a readiness verdict and drill
   prescriptions. If it says you're ready —
6. **"I'm ready — start the test."** One click flips you into a timed, graded
   interview on a *fresh* problem in the same topic. No tracer, 3 hints, real
   clock. Submit (or run out of time) and get a Staff-bar debrief with a hire
   verdict.
7. **"Distill this into a pattern card"** on the debrief screen. Your
   session — the problem, your code, the grader's observations — becomes a
   study card whose pitfalls are *your* pitfalls. Review it before your next
   run.

---

## The two modes

| | **Coach** (learn) | **Interview** (prove it) |
|---|---|---|
| Clock | Counts up, untimed | Counts down: 20 / 25 / 35 min |
| Teaching | Allowed — names patterns, explains, reviews your code | Never — Socratic probes only |
| Guidance | Unlimited, free | 3 hints total; each caps your verdict ceiling |
| Step-through debugger | ✅ | ❌ (the real interview has none) |
| Syntax check | Free | Costs 1 hint |
| Silence tolerance | ~90 seconds | ~35 seconds |
| Ends with | Study report + readiness verdict | Debrief + hire verdict (Strong Hire → No Hire) |
| Mode switch | **"I'm ready — start the test"** button any time | — |

In both modes the AI interrupts on **code patterns** it sees you type —
nested loops, `range(len(...))`, `while True:`, recursion, `.sort()`,
mutating-while-iterating — each fires once, as a question about *your* code.

---

## Controls reference

### Setup screen

| Control | What it does |
|---|---|
| Mode | Coach (untimed practice) or Interview (timed test) |
| Topic | Intervals · Graphs & grids · Hash tables · Strings & sliding window · Sorting & pointers · Heaps — or "Surprise me" (weights intervals, the cold spot) |
| Clock | Test-mode length; ignored in coach mode |
| Fresh problem ☑ | Claude authors an original LC-medium variant instead of using the built-in bank — you can't pattern-match on a memorized title. Applies to the coach→test switch too. Falls back to the bank on failure. |
| Pattern Sprint | 8 rapid classify-the-problem rounds, freshly generated each time |
| Recent sessions | Your history: date, mode, problem, verdict/score |
| Pattern library | Saved study cards ("Study" to view, ✕ to delete) + the distiller |

### During a session (top bar)

| Control | What it does |
|---|---|
| Timer | Counts down in Interview, up in Coach; goes orange at 5 min, red at 2 |
| 🔕 / 🔔 | Opt-in chime when the interviewer speaks (visual cues — chat pulse + tab-title flash — are always on) |
| Request hint / Ask for guidance | Spends a hint (test) / free coaching (coach) |
| I'm ready — start the test | Coach only: ends practice, starts a timed test on a fresh problem |
| Submit solution / End session | Triggers the graded debrief / study report |

### Editor (right pane)

| Control | What it does |
|---|---|
| The editor itself | A bare `<textarea>`, on purpose. **Tab** inserts 4 spaces — the only mercy. |
| Step through *(coach only)* | Opens the tracer. Enter a call (pre-seeded from the problem's example), hit **Trace**: your code runs under real CPython, recording every line. **⏮ ◀ ▶ ⏭** or **← / →** to scrub; current line highlighted; changed variables in orange; stdout + return value at the end. Infinite loops stop at 500 steps. |
| Check syntax | Compiles your code under real CPython (Pyodide, ~10s first load then cached). Free in coach; **costs 1 hint** in interview. Always auto-runs at submit and feeds the grader an objective result. |

### Narration box (bottom left)

| Input | What it does |
|---|---|
| **Enter** | Log a thought (this is your think-aloud — it's graded) |
| **Shift+Enter** | Newline |
| `?` prefix | Ask the interviewer/coach a direct question (e.g. `? are inputs sorted?`) |
| 🎙 | Voice narration (Chrome/Edge; final phrases logged automatically) |

### Debrief / report screen

| Control | What it does |
|---|---|
| Distill this into a pattern card | Turns the problem + your code + the grader's observations into a study card |
| Copy report | Copies the raw markdown |
| New session | Back to setup |

### Debug panel (for watching the machinery)

| Input | What it does |
|---|---|
| `index.html?debug=1` or **Ctrl+Shift+D** | Toggles a live log: every API call with latency + token usage (input / cached / output), every trigger firing, problem selection, Pyodide state, network errors. Also mirrored to the DevTools console. |

---

## The pattern library & YouTube distillation

**Distill a new pattern card** (setup screen) accepts any combination of:

- **A pattern name only** — Claude writes the canonical card from scratch.
- **Pasted material** — a transcript, article, or your own notes.
- **A YouTube URL** (e.g. a NeetCode video) — **Gemini watches the video
  natively** (the Gemini API accepts YouTube URLs directly; it sees the code
  on screen, not just the audio), extracts dense notes, and Claude distills
  them into the card. Requires a Gemini API key
  ([aistudio.google.com/apikey](https://aistudio.google.com/apikey)) — the
  key field only appears once you enter a URL, and is only used for that.
  **Public videos only** (no private/unlisted); the free tier covers ~8 hours
  of YouTube per day.

Every card has the same anatomy, built for assistant-free recall:
**Recognition triggers** (statement phrase → pattern) · **The archetype**
(a ≤15-line skeleton to write cold in under 5 minutes) · **Why each line
exists** · **Classic pitfalls** · **Trace it once** · **Drill next**.

---

## API keys, cost, and privacy

**Keys.** Two, both optional beyond the first: an Anthropic key (required —
powers the interviewer, grading, problem generation, sprints, and card
distillation, all on `claude-opus-4-8`) and a Gemini key (only if you use
YouTube distillation). Both live in your browser's localStorage and are sent
only to their own vendor's API. Since the keys sit in a browser, **restrict
them**: in AI Studio, limit the Gemini key to the Generative Language API;
treat the Anthropic key as personal and rotate it if you ever share your
machine.

**Rough cost per activity** (Claude Opus 4.8, $5/$25 per MTok; the app uses
prompt caching so repeated in-session calls are mostly cached-input at ~10%
price — these are honest ballparks, not guarantees):

| Activity | Typical cost |
|---|---|
| One full session (greeting + interruptions + debrief) | ~$0.25–0.75; the debrief is the biggest single call |
| Pattern Sprint (one generation call) | ~$0.05–0.15 |
| Distilling a card | ~$0.10–0.30 |
| Fresh AI-authored problem | ~$0.05 |
| Gemini watching a ~20-min video | Free-tier for most usage |

Watch real numbers live in the debug panel (`?debug=1`).

**What's stored, and where.** Everything is localStorage in your browser —
nothing leaves your machine except the API calls themselves:

| Key | Contents |
|---|---|
| `baremetal_api_key` / `baremetal_gemini_key` | Your keys |
| `baremetal_cards` | Pattern cards |
| `baremetal_history` | Session results |
| `baremetal_seen` | Problems already used (no-repeat tracking) |
| `baremetal_sound` | Chime preference |

To wipe everything: DevTools console → `localStorage.clear()` (or your
browser's "clear site data").

---

## Troubleshooting

| Symptom | Cause & fix |
|---|---|
| "API error — 401" | Wrong/expired Anthropic key. Re-paste it on the setup screen. |
| "API error — Failed to fetch" | The browser couldn't reach `api.anthropic.com`: corporate proxy, VPN, an ad-blocker/privacy extension blocking the request, or you're offline. Check the debug panel and DevTools console. |
| "Syntax checker unavailable" / "tracer unavailable" | Pyodide loads ~10 MB from jsDelivr CDN on first use — blocked network or offline. Retry once you're online; it's cached afterwards. |
| Trace shows "stopped at 500 steps" | Working as intended — likely an infinite loop, or trace a smaller input. |
| Gemini "returned no notes" or 4xx | Video must be **public** (not private/unlisted/age-gated); free tier caps ~8h/day; check the key is valid and unrestricted-enough. |
| 🎙 does nothing | Voice needs Chrome/Edge + mic permission. Typed narration works everywhere. |
| Interviewer never interrupts | It only speaks on triggers (silence >35s/90s, code patterns, check-ins, hints, `?` questions). Type code and go quiet — it'll come. Watch triggers arm in `?debug=1`. |
| Debrief fails at the end | Your session is preserved — press the submit button again to retry. |
| Chime doesn't play | It's opt-in: click 🔕 → 🔔. Some browsers also block audio until you've interacted with the page. |

---

## Under the hood (for tinkering)

Everything is in `index.html` (~1,600 lines, vanilla JS, no framework):

- **Division of labor:** the client decides **when** the interviewer speaks
  (silence timer, regex code-pattern triggers, periodic check-ins, hints,
  `?` questions, submit); Claude decides **what** to say, given the trigger,
  elapsed time, a code snapshot, and your narration since its last message.
- **Models:** `claude-opus-4-8` everywhere — interruptions at low effort for
  snap, debriefs/distills with adaptive thinking at high effort, problem
  generation and sprints via structured outputs (guaranteed-valid JSON).
  YouTube uses `gemini-flash-latest` (tracks Google's current GA Flash model).
- **Caching:** the system prompt (persona + rubric + problem) carries a
  prompt-cache breakpoint, so the many small event-driven calls within a
  session mostly hit cache.
- **Execution:** Pyodide (CPython 3.12 in WASM) powers both the syntax check
  (`compile()`) and the tracer (`sys.settrace` recording line events +
  `repr`'d locals, 500-step cap). Nothing you write ever leaves the browser
  to run.
- **Sessions are isolated** by a session id — API replies from an ended
  session can't leak into the next one.
- **Key browser APIs:** Web Speech (voice narration), WebAudio (chime),
  localStorage (all persistence).

Extending it: the problem bank (`PROBLEMS`), code triggers (`CODE_TRIGGERS`),
and sprint pattern list (`SPRINT_PATTERNS`) are plain arrays near the top of
the script — add entries and they just work.
