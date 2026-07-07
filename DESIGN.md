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

## Curriculum map — Season 1: Signal Lost (algorithms)

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

## Curriculum map — Season 2: Cold Storage (data structures)

The ship flies; the crew is still frozen. Season 1 taught *computing on*
data — Season 2 teaches *holding* it. Deliberate framing for career-switchers:
the JavaScript they already know (arrays and objects) turns out to secretly be
every structure they need. No robot this season — the player works ship
systems directly through per-chapter APIs.

| Ch | Mission | Concept | Mechanic that forces the concept | Trials |
|----|---------|---------|----------------------------------|--------|
| 8 | Pressure Seals | Stack (LIFO) | One-way weld-log stream must be replayed in reverse; wrong cut order buckles the hatch | 3 |
| 9 | Wake Order | Queue (FIFO) | Alerts arrive faster than pods can wake; the 3-slot comm relay overflows unless drained into an unbounded queue every cycle | 2 |
| 10 | The Conveyor Chain | Linked list | No master list — only `head()`/`nextOf(id)`; corroded nodes removed by re-pointing (`prev`/`cur` two-pointer walk) | 3 |
| 11 | The Manifest | Hash map | 8 lookups over 24 records with a 30-read budget; wake-list targets are placed deep so per-query rescans always exceed it | 2 |
| 12 | The Critical List | Priority queue | Highest-criticality pod must always wake next; first two arrivals ascend in criticality so FIFO fails immediately | 2 |

Season 2's paired arc mirrors Season 1's: 8↔9 contrast LIFO with FIFO, 11
echoes chapter 5 (an index beats rescanning, without even needing sorted
data), and 12 is a capstone that composes chapter 4's linear max-scan with
chapter 9's buffering — then names the result (priority queue) and points at
heaps and Dijkstra.

## Curriculum map — Season 3: The Relay Network (graphs)

The payoff season: every chapter is a graph problem whose solution is
assembled from parts the player already owns. The recurring line — "a queue
walking a graph", "swap the queue for a priority queue" — is the actual
pedagogy: graph algorithms are not new magic, they are Seasons 1+2 composed.

| Ch | Mission | Concept | Mechanic that forces the concept | Trials |
|----|---------|---------|----------------------------------|--------|
| 13 | The Debris Field | BFS | Fuel for exactly the shortest hop-count; any longer route fails. Queue (ch 9) + cameFrom hash map (ch 11) + string keys | 3 |
| 14 | The Hot Zone | Dijkstra | Per-tile radiation costs; generators verify that the cheapest fewest-hops route is strictly hotter than optimal, so reusing BFS fails on every trial. Priority pick = ch 12's min-scan | 2 |
| 15 | Restart Sequence | Topological sort | Random dependency DAG; display order is regenerated until it is NOT a valid restart order, so list-order restarting always cascades. Kahn's-style sweep with a hash map | 3 |

Chapter 13→14 is the season's thesis in miniature: change one decision
(oldest-first → cheapest-first) and BFS becomes Dijkstra. Chapter 15
generalizes "graphs" beyond space: arrows can mean *needs*, not *adjacent*.

## Curriculum map — Season 4: First Contact (techniques)

The finale season teaches ways of *thinking* rather than machines: recursion,
dynamic programming, parsing. Narratively it closes the loop — the debris
storm from chapter 1 turns out to have been the wake of something arriving,
and the last chapter literally decodes its apology.

| Ch | Mission | Concept | Mechanic that forces the concept | Trials |
|----|---------|---------|----------------------------------|--------|
| 16 | The Ice Moon | Recursion / divide & conquer | Beacon hidden in an 8×8 field; rectangle probes with a 14-charge budget and a single dig. Cell sweeps drain the sounder; quartering (2-D binary search) always fits | 3 |
| 17 | The Toll Road | Dynamic programming | 26 gates, jumps of 1–3, toll per landing. Generators regenerate until greedy is strictly worse than optimal; brute-force recursion trips the (new) function-entry guard. Bottom-up table wins | 2 |
| 18 | The Message | Parsing (recursive descent) | Run-length grammar with nested blocks (`2[VU3[T]]`). Trial 2 is depth-2, so flat find-and-replace fails; the reference decoder validates exact expansion. Trial 3 decodes the story's ending | 3 |

Engine note: Season 4 required extending the loop-guard injector to function
bodies and arrow-function blocks, so *unbounded recursion and exponential
brute force* halt with a friendly message instead of freezing the tab — which
is itself the chapter 17 teaching gate.

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

## Shipped polish

- **Hint system:** two-tier HALCYON "recovered fragments" per chapter,
  transmitted automatically after 2 and 4 failed runs (strategy first, then
  near-solution); the counter resets on completion so replays stay clean.
- **Sound:** a ~30-line WebAudio bleep engine (no assets) — moves, pings,
  reveals, error buzz, win fanfare — with a persisted header toggle.
- **Ship log:** a syllabus modal mapping all 15 chapters to their concepts
  with completion state; designed to be screenshot as a "what I learned" map.
- Efficiency stars were considered and deliberately skipped: the trial gates
  already force the efficient algorithm where it matters (battery, budget,
  fuel), so stars would add chrome without teaching.
- **Feedback upgrade:** the editor gained line numbers and a dependency-free
  syntax-highlight overlay, and every error — runtime, game-thrown, or
  syntax — now reports the offending line with a snippet (`[line 7: cut(...)]`).
  Syntax errors additionally get a string/comment-aware bracket-balance hint
  ("'{' opened on line 3 is never closed").
- **Side missions:** five optional practice variants unlocked by completing
  their parent chapters — lower-bound binary search, bracket matching,
  frequency counting, flood fill, and take-or-skip DP. One rep per concept is
  never enough; these add the second rep where it counts most.
- **JS Primer:** five skippable lessons (calling a function, variables,
  numbers vs. text, if/else, for loops) for players with zero prior syntax,
  offered as a fork in the first-visit intro modal ("New to JavaScript? Start
  here (5 min)") and reachable anytime via a header button. Built on the
  exact same level shape as a chapter (build/makeApi/win/card) — no new
  engine, just a plain terminal instead of a grid, so it inherits randomized
  trials, hints, and sound for free. One deliberate technique: a level's
  `win(sim)` function may set `sim.failed` as a side effect before returning
  false, giving a pure end-state check the same tailored diagnostic power as
  an in-flight `throw` (used for primer step 2's "you concatenated strings
  instead of adding numbers" message).
- **Step-through replay:** a STEP toggle that pauses after every action and
  highlights the exact source line that fired it, turning the replay into a
  debugger. Implemented via one hook point rather than touching every
  level's dozens of `sim.actions.push(...)` call sites: `opTick()` resolves
  the calling line by parsing `new Error().stack` for the `new Function`
  constructor's synthetic frame (the same trick `describeError` already used
  for uncaught exceptions — factored into a shared `stackLine()` helper),
  and `runTrial` wraps `sim.actions.push` once to tag every pushed action
  with that line. The editor's syntax-highlight overlay renders one
  `<span class="ln" data-ln="N">` per line (`display:block` — the
  `inline-block; width:100%` line-wrap trick silently fails under the
  parent's `white-space:pre`, which was caught only by inspecting actual
  paint geometry via screenshot, not by DOM-state assertions).
- **Solution share links:** a Share button base64-encodes the current
  chapter/side-mission/primer id and editor contents into the URL hash;
  loading that URL drops the player straight into that level with the code
  pre-filled. Works for locked chapters too — a share link is explicit
  consent to view that level, bypassing the chip-based progress gate without
  mutating `unlocked` state.
- **Reference library ("📚 LEARN"):** the mission panel's story/API text
  was clear on WHAT to do but thin on WHY it works. Two additions close
  that gap without touching the story:
  - A **"How it actually works" box** in the mission panel — a
    plain-language one-liner plus complexity badges, separate from
    HALCYON/BYTE/YOU's in-fiction dialogue, with a direct link into the
    matching reference page. Side missions and two primer lessons
    (if/else, for loops) link to their parent chapter's concept rather
    than duplicating content.
  - A **dedicated page per concept** (18 total, one per chapter — every
    algorithm, data structure, and technique in the curriculum), each
    with an explanation, a complexity table, runnable pseudocode, and an
    **interactive visualization** with Play/Step/Reset controls. Binary
    search's demo deliberately mirrors the lo/mid/hi-pointer,
    eliminate-the-other-half presentation style of reference sites like
    algorithm-visualizer.org, since that framing is the clearest way to
    *see* why the algorithm is O(log n).
  - Engine: one `VizPlayer` class (goto/play/pause over a plain `frames`
    array) and two renderers — `renderCellsFrame` for array/row-based
    demos (search, sort, stack, queue, linked list, hash map, priority
    queue, DP table, parsing), `renderGridFrame` for grid-based demos
    (pathfinding, BFS, Dijkstra, the quadrant-search recursion demo).
    Every concept's frames come from a small generator function that
    *runs* the (simplified) algorithm on fixed sample data and pushes a
    frame at each meaningful step — the same "record what happened, then
    replay it" pattern the game's own chapter engine already uses. The
    pathfinding/BFS generators go one step further and reuse the actual
    chapter level builders (`LEVELS.find(l=>l.id==='bfs').build(...)`)
    rather than a hand-authored maze, so the demo world is guaranteed
    solvable by construction instead of by manual verification.
- **Resizable panels:** the fixed 310px/430px mission and editor columns
  became `var(--mission-w,310px)`/`var(--right-w,430px)` grid tracks with
  a thin `.resizer` div dropped between them (and between the editor and
  console, and the Reference Library's nav and detail panes) — one
  `makeResizer(handle, {axis, cssVar, min, max, def, direction})` utility
  drives all four via pointer events (`setPointerCapture` for robust
  dragging past the handle's own bounds), persists to `localStorage`, and
  supports keyboard nudging (`role="separator"` + arrow keys) and
  double-click-to-reset, matching the WAI-ARIA separator pattern. No
  re-render is needed on resize — the editor overlay, gutter, and game
  grid are all pure CSS flex/grid, so they reflow for free.

## Roadmap

The curriculum, tooling, and reference library are all complete: four
seasons (algorithms, data structures, graphs, techniques), five practice
side missions, a JS primer, step-through debugging, share links, and an
18-page visualized reference library. Remaining ideas are smaller polish:

- Localization of story text (the level logic is already data-driven).
- Efficiency stars, revisited only if playtesting shows the trial gates
  aren't sufficient signal on their own (see above — currently judged
  unnecessary).
- A printable/exportable "ship's certificate" summarizing mastered concepts,
  for players who want a portfolio artifact from finishing the game.
- More reference-library visualizations per concept (e.g. a heap-based
  priority queue alongside the current linear-scan one) if playtesting
  shows the single canned example per concept leaves gaps.
