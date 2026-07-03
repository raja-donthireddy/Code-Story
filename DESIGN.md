# Code Story — Game Design Document

## One-liner

*Zachtronics-lite meets CS50*: a narrative robot-programming game where each
story mission secretly is a classic algorithm, aimed at adult career-switchers
who want real, transferable skills — not gamified flashcards.

## Audience & design goals

**Primary audience:** adults switching careers into software. They are
motivated, time-poor, comfortable reading prose, and allergic to being talked
down to. They don't need a game to trick them into learning — they need one
that makes *abstract ideas concrete* and *progress visible*.

Design consequences:

1. **Real JavaScript from minute one.** No block editor. Syntax errors are
   teaching moments with friendly messages, not failures to be prevented.
2. **The story is a scaffold, not a sticker.** Every fictional constraint is a
   real CS constraint in costume: the "10-line uplink buffer" is DRY, the
   "4-charge scanner battery" is O(log n), the "3 warped corridors" are
   generalization over inputs.
3. **Randomized trials are the antagonist.** The single most important idea in
   the game: a program is not a recording of steps, it is a *general method*.
   Chapters 3–7 run the player's code on multiple seeded layouts. Hardcoding
   fails loudly, with an in-fiction explanation of *why* it failed.
4. **Name the concept only after it's felt.** Concept cards appear *after* a
   chapter is beaten: "that thing you just did to save 11 scans — that's
   binary search, here's its big-O, here's where it shows up at work and in
   interviews."
5. **Failure is diegetic and specific.** "Byte replaced a healthy cell and the
   deck lights browned out" beats "Wrong answer." Every failure message says
   what happened and points at the fix.

## Core loop

```
read mission brief  →  write JS in the editor  →  ▶ Run
      ↑                                              │
      │            program runs headlessly on N seeded trials
      │                                              │
      │            first (failing) trial replays as animation
      │                                              │
      └── failure: specific diegetic error ←─────────┤
                                                     └── all trials pass:
                                                         concept card → next chapter
```

## Curriculum map (Season 1: Signal Lost)

| Ch | Mission | Concept | Mechanic that forces the concept | Trials |
|----|---------|---------|----------------------------------|--------|
| 1 | Wake-Up Call | Sequences | Literal-minded robot; exact steps to a console | 1 |
| 2 | The Long Walk | Loops (DRY) | 22 needed commands vs a 10-line code budget | 1 |
| 3 | The Shifting Deck | Conditionals | Corridor layout randomized per trial; must `scan()` and branch | 3 |
| 4 | The Dead Cell | Linear search, O(n) | Dead cell position randomized; check-one-by-one is the *right* tool here | 3 |
| 5 | The Archive | Binary search, O(log n) | 15 sorted cabinets, 4 scanner charges — linear search physically cannot win | 3 |
| 6 | Cargo Shuffle | Sorting, O(n²) | `weigh(i)`/`swap(i,j)` primitives; order checked at program end | 2 |
| 7 | Escape Route | Pathfinding / graphs | Unmapped randomized maze, local sensors only → wall-follower | 2 |

Deliberate arc: chapters 4→5→6 form a single argument. Linear search works
(4), sorted data makes search exponentially cheaper (5), and sorting is the
price you pay for that speed (6). Chapter 7 opens the door to graphs and
points at BFS/A* as the next thing to learn.

## Systems

### The interpreter
Player code runs via `new Function` with the chapter's API passed as
parameters — nothing else is reachable by name. Before compilation, a small
tokenizer injects `__guard()` into every loop body (string- and
comment-aware), so `while (true) {}` halts with a friendly "check your exit
condition" error instead of freezing the tab. A separate 20k-operation budget
on API calls catches runaway-but-productive programs.

### Simulate, then replay
Programs execute headlessly and synchronously against a world model, recording
an action log (`move`, `ping`, `reveal`, `swap`, `win`, …). The log is then
replayed as animation. This keeps sensors trivially correct (no async), makes
trials cheap (only one is animated), and makes the whole game testable —
the Playwright suite runs every intended solution and every failure gate.

### Determinism
Trials use fixed seeds through a seeded PRNG (mulberry32), so a chapter is
fair: what you see replayed is exactly what was judged, and a failing seed
fails the same way every run while the player debugs.

## Visual & narrative identity

Starship engineering console: deep-space navy ground, phosphor-cyan for Byte
and everything interactive, signal-amber for HALCYON's fragmented voice,
monospace-led typography (the terminal *is* the game), single committed dark
theme. Three voices carry the story: HALCYON (broken, amber, cryptic hints),
BYTE (dry, literal — the embodiment of how computers read code), YOU (the
career-switcher's inner monologue, doing the conceptual connective work).

## Roadmap

- **Season 1.5 (polish):** hint system (HALCYON fragments on repeated
  failure), efficiency stars (beat par on scans/swaps/moves), solution
  playback share-links, sound.
- **Season 2 — "Cold Storage" (data structures):** stacks (airlock cycling),
  queues (cryo-pod wake order), hash maps (crew manifest lookup), linked
  lists (repairing a broken conveyor chain).
- **Season 3 — "The Relay Network" (graphs):** BFS (shortest route through
  the wreck), Dijkstra (power routing with costs), topological sort
  (repair-dependency order).
- **Meta-progression:** a "ship systems" board that fills in as concepts are
  mastered, doubling as a syllabus map the player can screenshot.
