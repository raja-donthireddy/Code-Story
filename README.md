# Code Story

A story-driven browser game that teaches **algorithms, data structures, and
core CS techniques** by making you program a robot to save a crippled
starship. Built for adult career-switchers: real JavaScript from chapter one,
no drag-and-drop blocks. Four seasons, eighteen chapters.

**Play it:** open `index.html` in any browser. No build step, no dependencies,
no network — the whole game is one self-contained file.

## Premise

The deep-space freighter *Meridian* hit a debris storm. The ship's AI is
fragmented, the crew is in cryo, and you — the newest engineer aboard, first
job after switching careers — are awake. Your one tool is **Byte**, a
maintenance robot that executes any JavaScript you upload.

## The curriculum

Each chapter is a mission that quietly *is* a classic algorithm. Later
chapters run your program against **randomized trials**, so memorized command
lists fail and only general algorithms pass — the core lesson of the game.

**Season 1 — Signal Lost** (algorithms):

| Ch | Mission | Concept | The teeth |
|----|-------------------|--------------------------|-----------|
| 1 | Wake-Up Call | Sequences | Byte does exactly what you wrote — nothing else |
| 2 | The Long Walk | Loops | 10-line uplink buffer: copy-paste won't fit |
| 3 | The Shifting Deck | Conditionals | 3 randomized corridors, one program |
| 4 | The Dead Cell | Linear search — O(n) | The dead cell moves every trial |
| 5 | The Archive | Binary search — O(log n) | 15 cabinets, only 4 scanner charges |
| 6 | Cargo Shuffle | Sorting — O(n²) | Order is verified when your program ends |
| 7 | Escape Route | Pathfinding | Unmapped maze, local sensors only |

**Season 2 — Cold Storage** (data structures — wake the frozen crew):

| Ch | Mission | Concept | The teeth |
|----|--------------------|--------------------------|-----------|
| 8 | Pressure Seals | Stacks — LIFO | One-way weld log; seals come off newest-first |
| 9 | Wake Order | Queues — FIFO | 3-slot comm relay overflows unless you buffer |
| 10 | The Conveyor Chain | Linked lists | No master list — the chain IS the data; splice around corrosion |
| 11 | The Manifest | Hash maps — O(1) | 8 lookups, 24 records, 30-read budget: build an index |
| 12 | The Critical List | Priority queues | Most-critical pod always wakes first, alerts keep arriving |

**Season 3 — The Relay Network** (graphs — restore the sector):

| Ch | Mission | Concept | The teeth |
|----|------------------|--------------------------|-----------|
| 13 | The Debris Field | Breadth-first search | Fuel for exactly the shortest route — chapter 7's wall-follower won't cut it |
| 14 | The Hot Zone | Dijkstra — weighted paths | Fewest tiles ≠ least radiation; grids are built so plain BFS always fails |
| 15 | Restart Sequence | Topological sort | Dependency-locked systems; list order is never a valid restart order |

**Season 4 — First Contact** (techniques — answer the signal):

| Ch | Mission | Concept | The teeth |
|----|-------------|-----------------------------|-----------|
| 16 | The Ice Moon | Recursion — divide & conquer | 64 cells, 14 rectangle probes, one dig: quarter or die |
| 17 | The Toll Road | Dynamic programming | Greedy provably overpays; brute-force recursion trips the guard |
| 18 | The Message | Parsing — recursive descent | Nested repeat-blocks; flat find-and-replace shatters on trial 2 |

After each chapter a **concept card** names what you just did, gives the
big-O intuition, and maps it to real-world engineering (and interviews).

## For developers

- Everything lives in `index.html`: level definitions, a tiny sandboxed
  interpreter (`new Function` + loop-guard injection so infinite loops halt
  gracefully), a deterministic seeded RNG for fair trials, and the renderer.
- User programs run headlessly against a simulated world; the recorded action
  log is then replayed as animation. Sensors work because simulation is
  synchronous.
- Debug/test hooks are exposed on `window.CS` (`load(i)`, `runCode(src)`,
  `instant`). The Playwright test suite drives every chapter's intended
  solution and asserts the failure gates (line budget, battery limit,
  infinite-loop guard) fire correctly.
- Progress and per-chapter code persist in `localStorage`.

See [DESIGN.md](DESIGN.md) for the full game design document, including the
learning-design principles and the roadmap for future seasons.
