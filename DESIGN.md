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

## Curriculum map — Season 5: The Deep Archive (interview-staple techniques)

Narratively a gift, not a threat: the visitors who spent Season 4 apologizing
hand over an archive built from the same fundamentals the player already
owns. Mechanically, five of the most-asked technical-interview topics that
Seasons 1–4 hadn't yet covered explicitly, each still assembled from parts
already in the player's toolkit.

| Ch | Mission | Concept | Mechanic that forces the concept | Trials |
|----|--------------------|--------------------------|-----------|--------|
| 19 | The Index Tree | Binary search trees | 15-node array-indexed tree built by IN-ORDER assignment (not index order, which would silently violate the BST property) — `keyOf`/`leftOf`/`rightOf` walk, wrong comparison direction fails immediately | 3 |
| 20 | The Sift | Binary heaps | A valid min-heap array; must extract-min and sift-down repeatedly until output is fully sorted — skipping the sift-down is checked by validating the exact reported sequence, not just the final state | 2 |
| 21 | The Broadcast Window | Greedy algorithms | 7 overlapping windows; generator regenerates until greedy-by-end-time strictly beats BOTH greedy-by-duration and greedy-by-start-time, so the "obviously reasonable" alternatives are provably wrong on every trial | 2 |
| 22 | The Shortest Span | Two pointers / sliding window | Shortest sum-threshold span; generator verifies the true answer is short (2–5) so "report the first span that works" is distinguishable from "report the shortest" | 2 |
| 23 | The Sentry Grid | Backtracking | 5×5 N-queens with 2 damaged cells; generator verifies solvability first. `isSafe`/`place`/`remove` API makes the commit-discover-undo shape explicit rather than implicit in a recursive return value | 2 |

Deliberate choices: heaps make explicit the machine chapter 12's priority
queue left implicit (`O(n)` linear-scan vs. `O(log n)` sift), and the BST
chapter's build function is a case study in a subtle correctness bug —
assigning sorted values by ARRAY INDEX rather than by IN-ORDER TRAVERSAL
looks plausible but produces a tree with no valid ordering at all; caught by
running the chapter's own intended solution against it in testing, not by
inspection.

## Curriculum map — Season 6: The Substrate (the primitives underneath)

The Archive's own index queries turn out not to be running on the Archive —
narratively, one layer lower than the "final" gift, and the true bottom:
after this, there is nothing left underneath to discover. Mechanically, the
five raw primitives that even Season 5's "smart" structures were quietly
built from, chosen from the roadmap's own suggestion (tries, union-find,
bit manipulation) plus two natural pairings (a monotonic stack rounds out
the technique-family set; Kruskal's algorithm is a capstone that reuses
chapter 25's exact union-find primitives as its cycle check).

| Ch | Mission | Concept | Mechanic that forces the concept | Trials |
|----|--------------------------|--------------------------|-----------|--------|
| 24 | The Prefix Index | Tries | 12 call-signs over a 3-letter, 4-character alphabet (guarantees real prefix overlap by pigeonhole); 4 queries include one prefix proven not to exist via an exhaustive length-1-to-3 search over the alphabet | 3 |
| 25 | The Wreckage Map | Union-Find | A random spanning tree (guaranteed cycle-free) plus exactly one extra edge, shuffled — the extra edge is provably the only possible redundant connection regardless of shuffle order, verified by real union-find simulation at generation time | 2 |
| 26 | The Binary Vault | Bit manipulation — XOR | 15 fragment IDs, one unpaired; `valueAt()` is capped at exactly 15 total calls, ruling out any approach that re-reads a value (nested-loop duplicate checks, building a count table) | 2 |
| 27 | The Watch List | Monotonic stack | 12 readings, generated with a retry loop guaranteeing at least one duplicate value (to catch a `<=`-vs-`<` off-by-one specifically); `readingAt()` throws on a second read of the same index | 2 |
| 28 | The Minimum Spanning Relay | Kruskal's algorithm | 8 stations; generator regenerates until processing edges in original (unsorted) order strictly overpays versus the true minimum — reuses chapter 25's `parentOf`/`setParent` API verbatim | 2 |

Engine note: this season's build functions lean harder on generate-and-verify
than any prior one — every chapter's random instance is checked against a
real reference implementation (a second, independent union-find simulation,
a real Kruskal's run, a brute-force prefix count) before being accepted,
after the BST lesson from Season 5 that a plausible-looking generator can
silently produce an unsolvable or meaningless instance.

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

- **Accessibility pass:** prompted by a general "make the UI friendlier to
  all types of users" request, audited with axe-core (automated) plus manual
  review, then fixed everything found rather than picking a narrow slice:
  - *Technical (axe-core, 0 violations remaining across the game view,
    reference library, and every modal/overlay state)*: resizers now report
    `aria-valuenow`/`min`/`max` (updated live on drag, keyboard, and
    double-click reset); three near-miss contrast failures fixed (`--muted`
    from 4.47:1 to 5.17:1+, the editor gutter from 2.17:1 to 4.66:1+, the
    reference library's current-item badge from 2.91:1 to 9.75:1); the
    reference library's section headings changed from `h3` to `h2` (they
    sat directly under an `h1` with no `h2` between, an invalid skip); the
    modal dialog gained `aria-labelledby` pointing at its own title.
  - *Colorblind-safety*: every color-only pass/fail signal (tile marks,
    reference-library "good"/"bad" cells, cabinet/crate success states)
    now pairs its color with a shape — a ✓/✗ overlay, or a dashed vs. solid
    border — so meaning survives red-green confusion. Caught a real,
    unrelated bug in the process: `.vgc.bad` (the recursion "missing base
    case" demo's failure state) had no CSS rule at all and rendered with
    zero visual distinction; it now gets a dashed red treatment matching
    the rest of the "bad" family.
  - *Keyboard*: the guided tour's div-based first target (a plain `<div>`,
    not natively focusable) gets a temporary `tabindex="0"` + `role="button"`
    + Enter/Space handler for the duration of that step only, cleaned up on
    advance/skip/end — a bug of the tour's own making, caught by testing
    keyboard-only use rather than assuming a click handler was enough.
    Escape now closes the tour from anywhere. Chapter chips gained a full
    `aria-label` (title + concept + completion/current status) instead of
    relying on a `title` attribute alone, which screen readers don't
    reliably announce.
  - *Focus containment*: opening the modal or reference library now sets
    `inert` on `<header>`/`<main>` (removing the dimmed background from
    both the tab order and the accessibility tree — a real behavioral
    effect, not just visual: `inert` blocks pointer events too, which
    surfaced a test-harness shortcut that bypassed the real close path and
    left the background permanently uninteractive for the rest of that
    run, since it never called the function that resets `inert`). The
    guided tour can't use blanket `inert` the same way — its whole premise
    is that ONE background element must stay interactive per step — so it
    instead sweeps every other focusable element to `tabindex="-1"`
    (recording and restoring each one's original value) and excludes only
    the current step's target and the tour dialog's own controls.
  - *Motion*: `prefers-reduced-motion` now also covers the tour's
    transitions, the goal-cell pulse, the error shake, and the
    correct/incorrect cell pop, on top of what it already handled.
  - *Screen reader cleanup*: icon-prefixed buttons (Run, Share, Next, Tour,
    Learn) gained an `aria-label` matching their existing tooltip text so
    the emoji doesn't become part of the announced name; the sound and
    step toggles gained `aria-pressed`, updated everywhere their state
    changes (click handler, the `CS.stepMode` setter, and initial boot).
  - *Zoom/mobile*: verified no horizontal overflow at 375px, 700px, and
    1024px widths; chip and header-button tap targets bumped from 24px to
    32px tall on narrow viewports.
  - Scope note: a light/high-contrast theme toggle was considered and
    deliberately deferred — it's a genuine visual-identity trade-off against
    the single committed dark "terminal" theme (see Visual & narrative
    identity, above), not a fix with no downside, so it's left as a future
    option rather than decided unilaterally.
- **Reference-library legend:** each visualization now shows an
  always-visible, plain-text color key (in the style of
  [visualgo.net](https://visualgo.net/)) listing only the states the
  current example actually uses — computed by scanning every frame's cell/
  grid classes up front (`collectVizClasses`), not hand-maintained per
  concept, so it can't drift out of sync with what's actually rendered.
- **Guided tour:** a first-visit onboarding overlay addressing blank-page
  syndrome — four steps (Objective panel, editor, ▶ Run, console), each
  requiring a genuine interaction with the real element (click the objective
  box, focus the editor, click Run) rather than a passive "Next" click,
  so the first few actions double as muscle memory instead of a slideshow.
  Engine: a single reusable spotlight built from a fixed backdrop `<div>`
  whose `clip-path` is recomputed every step as an evenodd polygon with a
  "keyhole" cutout around the live target's `getBoundingClientRect()` (the
  standard technique for a functional, not just visual, cutout — clip-path
  affects hit-testing in modern browsers, so clicks pass through to the
  real element everywhere the cutout is, and nowhere else) plus a
  separately positioned glow border and tooltip card, all transitioned via
  CSS. Each step attaches a `{once:true}` listener for its required event
  (`click` or `focus`) directly to the real target element — the tour reacts
  to genuine use of the UI, not a parallel simulated one — and tears the
  listener down on Back/Skip/completion so nothing leaks. Gated by a
  `cs_seen_tour` localStorage flag set on completion or skip, triggered
  after the intro modal closes (from any of its three exit paths: straight
  in, or via either JS Primer exit) and replayable anytime via a header
  button, which is safe to fire from any chapter since none of the four
  target elements are chapter-specific.
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
  - A **dedicated page per concept** (28 total, one per chapter — every
    algorithm, data structure, graph technique, and Season 5/6 topic in the
    curriculum), each with an explanation, a complexity table, runnable
    pseudocode, and an **interactive visualization** with Play/Step/Reset
    controls. Binary
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
- **A second example + a mistakes list, per concept.** Each `CONCEPTS[id]`
  gained an `examples: [{label, demoType, frames}, ...]` array (was a
  single `frames` function) rendered as a tab strip above the player, plus
  a `mistakes: [...]` array rendered as a warning-accented bullet list.
  Every second example deliberately puts a real failure mode on screen
  rather than a second happy path: binary search's "not found" termination,
  a fully sealed maze loop that traps wall-following forever, selection
  sort silently reordering two tied crates (`[20A,20B,5C]` — the textbook
  minimal instability counter-example, since naive selection sort *is*
  usually accidentally stable on small inputs and a careless example would
  undercut its own point), a hash-map lookup on a key that was never
  inserted, a linked-list head deletion (the one case with no `prev` to
  splice from), Kahn's algorithm stalling on a 3-node cycle, and a
  hand-verified 3-node directed graph (`A→B(4), A→C(1), B→C(−10)`) where
  Dijkstra provably finalizes C too early and never reconsiders it once B
  reveals the cheaper route — traced by hand against the actual `finalized`
  Set logic before shipping, since a wrong "the algorithm fails" demo would
  be worse than none. Every mistake example still *computes* its outcome
  live (e.g. the sort-stability caption branches on the real post-sort
  order) rather than asserting a scripted one, so it can't drift out of
  sync with the code that produces it.
- **A third example for the six flagship concepts.** "More the better, at
  least two per algorithm" was the brief; binary search, sorting, BFS,
  Dijkstra, dynamic programming, and recursion/divide-and-conquer each
  picked up a genuine third angle rather than a third re-skin of the same
  idea: binary search on a rotated sorted array, merge sort as the
  O(n log n) counterpoint to selection sort, multi-source BFS, a uniform-
  weight Dijkstra run that visibly reduces to BFS (same visited order, same
  distances — "BFS is Dijkstra with every weight fixed at 1"), House
  Robber as a second DP recurrence next to the toll-road table, and fast
  exponentiation as a non-grid divide-and-conquer example. Every one is a
  real simulation of the actual algorithm on fixed sample data, same as the
  first two examples — none of these are hand-authored animations.
- **Sample code in five languages, per concept.** Each `CONCEPTS[id]`
  gained a `code: {js, ts, python, java, csharp}` field alongside
  `pseudocode` — every language snippet implements the same algorithm as
  that concept's pseudocode, not an independently-reinvented approach.
  Rendered as a language-tab strip (`.lv-lang-tab`, deliberately a distinct
  class from the visualization's `.lv-ex-tab` so the two tab strips don't
  collide under a shared `querySelectorAll`) above a `.lv-code` block; the
  selected language persists across concepts via `localStorage`
  (`cs_code_lang`), same persistence pattern as sound/step-mode/panel
  sizes. The 140 snippets (28 concepts × 5 languages) were drafted by
  parallel subagents fed the exact existing pseudocode as ground truth,
  then syntax-verified (`node --check` for JS, `py_compile` for Python) and
  scrubbed for two recurring subagent failure modes before being merged in:
  HTML-entity-escaped output (`&lt;` instead of `<`) that would have
  double-escaped once the render pipeline's own `escapeHtml()` ran over it,
  and JSON responses wrapped in ```` ```json ```` fences despite
  instructions not to.
- **MIT 6.006 further-reading notes.** Eight concepts with strong,
  confirmed correspondence to MIT's [6.006 Introduction to
  Algorithms](https://ocw.mit.edu/courses/6-006-introduction-to-algorithms-spring-2020/pages/lecture-notes/)
  (Spring 2020) syllabus — binary search, sorting, hash maps, BSTs, heaps,
  BFS, Dijkstra, and dynamic programming — gained a `furtherReading` field
  rendered as a short synthesized note plus a link to that course's lecture
  notes. Only one MIT OCW URL was ever confirmed reachable (the lecture-
  notes index — the per-lecture pages returned 403s to automated fetches);
  every note links to that single verified URL rather than a guessed
  per-lecture one, and the note text paraphrases the course's known
  content instead of quoting pages that were never actually fetched.
- **Resizable panels:** the fixed 310px/430px mission and editor columns
  became `var(--mission-w,310px)`/`var(--game-w,430px)` grid tracks with
  a thin `.resizer` div dropped between them (and between the editor and
  console, and the Reference Library's nav and detail panes) — one
  `makeResizer(handle, {axis, cssVar, min, max, def, direction})` utility
  drives all four via pointer events (`setPointerCapture` for robust
  dragging past the handle's own bounds), persists to `localStorage`, and
  supports keyboard nudging (`role="separator"` + arrow keys) and
  double-click-to-reset, matching the WAI-ARIA separator pattern. No
  re-render is needed on resize — the editor overlay, gutter, and game
  grid are all pure CSS flex/grid, so they reflow for free.
- **Layout: code center, game right.** Originally mission | game | editor.
  Swapped to mission | editor | game purely via CSS `order` on the five
  grid children (`#mission`,`#rzMission`,`#right`,`#rzRight`,`#viewport`)
  — zero DOM restructuring, so every `#right`/`#viewport` reference
  elsewhere in the script, and every resizer's drag semantics, kept
  working unchanged; the resizer between the editor and game viewport
  just resized whatever now sits in its fixed-width track. `order` is
  reset back to the original mission → game → editor sequence inside the
  narrow-viewport media query, so stacked mobile layout is unaffected.
  Renamed `--right-w` to `--game-w` for clarity since it now sizes the
  viewport, not the editor. Redundant `border-left`/`border-right` on the
  panels themselves were dropped in the same pass — the resizers already
  draw the divider line, so the old borders were a latent double-line risk.

## Roadmap

The curriculum, tooling, and reference library are all complete: six
seasons (algorithms, data structures, graphs, techniques, a fifth covering
trees/heaps/greedy/two-pointers/backtracking, and a sixth covering
tries/union-find/bit-manipulation/monotonic-stacks/Kruskal's algorithm),
five practice side missions, a JS primer, step-through debugging, share
links, a first-visit guided tour, an accessibility-audited UI (0 axe-core
violations), and a 28-page visualized reference library where every concept
has at least two worked examples, six flagship concepts have three, and
every concept now ships runnable sample code in five languages alongside
its pseudocode. Remaining ideas are smaller polish:

- Localization of story text (the level logic is already data-driven).
- Efficiency stars, revisited only if playtesting shows the trial gates
  aren't sufficient signal on their own (see above — currently judged
  unnecessary).
- A printable/exportable "ship's certificate" summarizing mastered concepts,
  for players who want a portfolio artifact from finishing the game.
- A third (or further) example for the remaining concepts beyond the six
  flagship ones, if playtesting shows two worked examples per concept still
  leaves gaps for a given topic.
- A light/high-contrast theme toggle, deliberately deferred during the
  accessibility pass as a visual-identity trade-off rather than decided
  unilaterally — worth revisiting if players actually ask for it.
- A Season 7, if the curriculum needs to grow further — segment trees,
  KMP/string matching, or graph coloring would extend the "one chapter, one
  concept, randomized trials" template past the primitives Season 6 bottomed
  out at.
