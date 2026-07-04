# Bare Metal — Assistant-Free Interview Trainer

An interview simulator that uses AI to enforce *not* using AI. You solve an
LC-medium problem in a deliberately bare `<textarea>` — no syntax highlighting,
no bracket matching, no autocomplete, no execution — while a Claude-powered
interviewer watches your code and narration in real time and interrupts you
the way a real interviewer would.

Built for engineers whose daily Python is written with Claude Code / Cursor /
Copilot and who need to rebuild "code author" muscle memory for a shared-notebook
interview.

## Two modes

- **Coach mode (prepare).** Untimed. The AI becomes a coach: it can name the
  pattern family, explain *why* an approach fits, review your code line by
  line, and give unlimited guidance — but it makes you attempt first, and it
  drills the meta-skills (state your bounds, trace by hand, say the complexity
  out loud). It tells you when you look ready. Ending a practice session
  produces a **study report** with a readiness verdict and drill prescriptions.
- **Interview mode (test).** The strict simulation: countdown clock, 3-hint
  budget, Socratic probes only, Staff-bar debrief with a hire verdict. Flip
  into it any time from Coach mode with the **"I'm ready — start the test"**
  button — same topic, fresh problem, real clock, no warm-up.

## What it does

- **Plain-text editor, on purpose.** The one mercy is that Tab inserts four
  spaces. Everything else — bounds, brackets, spelling — is on you.
- **Think-aloud enforcement.** Type your narration (or use the mic in
  Chrome/Edge). Go quiet for ~35 seconds and the interviewer interrupts and
  asks what you're thinking.
- **Mid-thought probes on code patterns.** Client-side triggers watch for
  nested loops, `range(len(...))`, `while True:`, recursion, sorting, and
  mutation-while-iterating — each fires the interviewer once with a targeted
  Socratic probe. The interviewer never writes code for you.
- **Adaptive difficulty.** The interviewer calibrates probe depth to your
  narration quality, not just your code — crisp reasoning gets pushed harder.
- **Stateful session.** Timer (default 25 min), a 3-hint budget (each hint
  caps your ceiling), timestamped transcript of everything you said and typed.
- **Staff-bar debrief.** On submit, Claude hand-traces your final code against
  the examples, grades correctness / complexity / communication, calls out
  "autocomplete scars," gives a hire verdict, and prescribes your next drill.
- **Problem bank tuned to a real prep plan:** intervals (weighted as the cold
  spot), graphs & grids, hash tables, strings & sliding window, sorting,
  heaps. All LC-medium, ~25–30 line solutions.

## Syntax feedback (without breaking the point)

The editor stays bare while you type — that's the training. But real CPython
syntax checking is available via Pyodide (Python compiled to WASM, lazy-loaded
from CDN on first use):

- **Coach mode:** the "Check syntax" button is free. Use it liberally while
  drilling.
- **Interview mode:** a syntax check costs one hint — an interviewer catching
  your typo costs you credibility too.
- **At submit:** the code is always compile-checked automatically and the
  objective result (`PASSES` / `SyntaxError line N: ...`) is fed to the
  grader, so the debrief never has to guess whether your code parses.

## Where problems come from

- **Built-in bank:** 15 curated LC-mediums tagged by topic, with intervals
  weighted as the cold spot. Problems you've seen are tracked in localStorage
  and not repeated until the topic's pool is exhausted.
- **Fresh AI-authored problems:** check "Author a fresh problem" on setup and
  Claude writes an original LC-medium variant for your topic via structured
  outputs — a disguised classic pattern in a fresh framing, so you can't
  pattern-match on a memorized title. It's told which problems you've seen
  recently and avoids resembling them. Falls back to the bank on any failure.

## Pattern library (recognize the patterns)

The setup screen has a card library. "Distill a new pattern card" takes any
of: a **YouTube URL** (e.g. a NeetCode video), pasted source material (a
transcript, article, or your notes), or nothing at all — and produces a
compact study card: recognition triggers (statement phrase → pattern), a
≤15-line archetype to memorize, why each line exists, classic pitfalls, one
hand-trace, and three drill problems. Cards persist in localStorage. After
any session, the report screen has a one-click "Distill this into a pattern
card" that builds the card from the problem you just faced, your code, and
the grader's observations — your personal weaknesses become the emphasized
pitfalls.

**YouTube via Gemini:** the Gemini API accepts YouTube URLs natively as a
`file_data` part in `generateContent` — the model watches the video directly
(code on screen included, not just audio), extracts dense study notes, and
Claude distills them into the card. Uses the `gemini-flash-latest` alias so
the model doesn't rot. Constraints per Google's docs: public videos only
(no private/unlisted), and the free tier allows ~8 hours of YouTube per day.
The Gemini key field only appears once you enter a YouTube URL, and is only
ever used for that; restrict the key to the Generative Language API in AI
Studio since it lives in your browser.

## Session history

Every completed session (verdict or readiness call, mode, problem, date) is
recorded locally and shown on the setup screen — the streak that keeps you
coming back.

## Debugging the app itself

Open with `index.html?debug=1` (or press **Ctrl+Shift+D** any time) for a live
debug panel: every API call with latency and token usage (input / cached /
output), every trigger firing (silence, code patterns), problem selection,
Pyodide load state, and network failures. Everything is also mirrored to the
browser DevTools console via `console.debug`.

## Run it

No build, no dependencies, no server:

1. Get an Anthropic API key from <https://console.anthropic.com>.
2. Open `index.html` in a browser (double-click, or `python3 -m http.server`
   and visit `http://localhost:8000`).
3. Paste your key (stored only in your browser's `localStorage`; requests go
   straight from your browser to `api.anthropic.com`).
4. Pick a topic and start. Talk out loud. Suffer productively.

Voice narration uses the Web Speech API and works in Chrome/Edge; typed
narration works everywhere.

## How it's wired

Single self-contained `index.html`. The interviewer is `claude-opus-4-8` via
the Messages API called directly from the browser
(`anthropic-dangerous-direct-browser-access`). Probes run at low effort for
snappy interruptions; the final debrief runs with adaptive thinking at high
effort. The system prompt (interviewer persona + rubric) carries a prompt-cache
breakpoint so repeated event-driven calls within a session stay cheap.

The client decides **when** the interviewer speaks (silence timer, regex
pattern triggers, periodic check-ins, hints, direct questions, submit); Claude
decides **what** to say, given the elapsed time, a code snapshot, and your
narration since its last message.
