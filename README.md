# Telegram Download Manager — Array as Stack (LIFO) Demo

A single-file HTML demo showing how an **array used as a stack** models a
preemptive, Last-In-First-Out (LIFO) download queue, similar to how Telegram
(or any download manager) can bump a newly tapped file to the front while
pausing the one that was running.

## Files
- `telegram-lifo-demo.html` — the full demo (HTML + CSS + JS, no dependencies, open directly in a browser)

## What it demonstrates
- **The array**: `let downloadStack = []` is the core data structure — every
  download's name, progress, and state (`RUNNING` / `WAITING`) lives in one
  contiguous JS array.
- **Push (`.push()`)**: tapping a season button preempts whatever is currently
  `RUNNING` (flips it to `WAITING`) and pushes the new download onto the top
  of the array — O(1).
- **Pop (`.pop()`)**: "Finish Top" removes the item at `array[length-1]` and
  promotes the new top back to `RUNNING` — also O(1), and this unwind is what
  prevents starvation (every waiting item eventually becomes top and finishes).
- **LIFO order**: the on-screen list is rendered top-to-bottom from
  `array.length-1` down to `0`, so the most recently tapped download is always
  shown — and processed — first, not the one tapped earliest (that would be
  FCFS/a queue, not a stack).

## How to use it
1. Open `telegram-lifo-demo.html` in any browser.
2. Tap "Season 1", "Season 2", "Season 3" in any order — watch each new tap
   preempt the previous one (its card dims to `WAITING`).
3. Tap "Finish Top (Pop)" to complete the current top item; the next one down
   resumes as `RUNNING`.
4. The monospace readout above the buttons shows the live array contents,
   its length, and which index is the current top.

## Why an array specifically
- **O(1) access to the top** via `array[array.length-1]` — no searching needed.
- **O(1) push/pop** — preemption and completion are both constant-time.
- **Contiguous memory** — all download states sit together, which is cache
  friendly and mirrors how a real OS keeps process control blocks for a
  call/recursion-style stack.
