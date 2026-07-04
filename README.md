# Bare Metal — Assistant-Free Interview Trainer

An interview simulator that uses AI to enforce *not* using AI. You solve an
LC-medium problem in a deliberately bare `<textarea>` — no syntax highlighting,
no bracket matching, no autocomplete, no execution — while a Claude-powered
interviewer watches your code and narration in real time and interrupts you
the way a real interviewer would.

Built for engineers whose daily Python is written with Claude Code / Cursor /
Copilot and who need to rebuild "code author" muscle memory for a shared-notebook
interview.

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
