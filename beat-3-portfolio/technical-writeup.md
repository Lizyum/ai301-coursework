# Reproducing and Planning My First Open Source Fix in Avogadro

## Overview

Avogadro is a cross-platform C++ desktop app that chemists use to build and visualize molecules, and its `avogadrolibs` repo has an open issue (#3187) where several dialogs hardcode text colors instead of pulling them from the active Qt palette, so some text fails contrast guidelines and one error state relies on red as its only signal. I claimed the `gamesshighlighter.cpp` sub-task — the comment/number colors and the red-on-yellow error cue in the GAMESS input preview — and spent units 5 through 7 building the toolkit I'd need to work on an unfamiliar, decades-old C++ codebase responsibly with AI assistance: an issue-scout to find and grade the right first issue, a voice guard to keep my own posted comments honest, a reproduction pass that proved the bug at the pixel level, a plan graded against the issue before I touched any code, and a ship-gate manifest to keep me from claiming a check passed that I never actually ran. As of this write-up, the bug is reproduced and independently measured, the fix is planned and plan-checked, and the PR is not yet opened — the next and only remaining step is writing the two-line color fix and opening it.

## What happened

I started by building a scout (unit 5) to search GitHub for issues that fit my profile (Python, JavaScript, C++, SQL, docs) and grade candidates against a written rubric. Its first run accepted all five top candidates, but reading the verdicts showed two real gaps: it had no check for the language an issue's thread was written in, and its AI-disclosure check only failed on an outright ban, treating silence the same as permission — which mattered to me specifically because I was contributing with AI help. I added a language-fit check and tightened the AI-policy check so hedging counts as a fail too.

In unit 6 I re-graded my three shortlisted issues under a patched rubric and caught the opposite failure mode: the old scope check rejected *any* issue with an open design question, which had flipped a legitimate, well-scoped issue (Ocelot-Social#7127) to an automatic reject the week before. I replaced the hard reject with a scope-feasible check plus a difficulty note. Under the new rubric all three issues came back accepted, with avogadrolibs#3187 scored **moderate** — bounded by exact file and line numbers — which is what made me confident claiming it instead of one of the other two. Reading the issue thread directly also mattered here: two other contributors had already claimed two of its seven sub-tasks, so I deliberately claimed the unclaimed `gamesshighlighter.cpp` piece rather than duplicating their work.

Reproducing the bug (unit 6-7) meant building `avogadrolibs` standalone in a Docker container and rendering the real `GamessHighlighter` headless. Two blockers came up and both needed verification, not assumption, to resolve: a full-parallelism Ninja build got OOM-killed, fixed by limiting it to `-j2`; and `QStyleHints::setColorScheme()` turned out to be a documented no-op under the offscreen Qt platform, which I only trusted after checking Qt's own source rather than guessing why my dark-mode screenshots looked identical to light mode. With a real GTK theme driving the palette instead, I confirmed the bug and independently computed the contrast ratio:

> Independently computed the WCAG contrast ratio for `Qt::green` on white: **≈1.37:1**, matching the issue's own stated "about 1.4:1" almost exactly.

That's proof-from-real-work, not a sample run — the number came from measuring my own rendered screenshot, and it happened to confirm the issue report almost exactly.

Before writing any code, I graded my plan with `plan-check`:

> What it produced: `accept` on the first pass — all five required checks passed; only the preferred honest-unknowns check failed, for having no explicit Risks/Unknowns section in `plan.md`.

I hadn't written down what I wasn't yet sure of, so I added one: whether `highlightColor`'s red shade is visibly distinct from the preview's default background in both themes, to verify during testing instead of assuming. That's a real change driven by a tool's output, not my own second-guessing.

I also caught a stale note in my own repo memory. Back in unit 6 I'd written that the palette-aware color pattern lived as "an inline helper in `interfacescript.cpp`." Going back to verify the current file directly (instead of trusting my own prior notes) showed the maintainer had since refactored it into a shared `avogadro/qtgui/colortheme.h` helper, already reused by another contributor fixing a neighboring sub-task of this same issue. I corrected the note before it could steer my implementation plan wrong.

Last, my ship-gate manifest records what I'm **not** claiming: `ctest` is explicitly excluded, dated, with the reason (no `avogadrodata` checkout locally, and the issue itself asks for before/after screenshots instead of a new test suite run) — rather than silently omitting it and letting the manifest imply more coverage than I actually have.

## Limitations and what's next

The PR for #3187 is not open yet. Everything above — the scout, the scope-feasible patch, the reproduction, the plan, the plan-check — happened; the two-line color change itself hasn't been written. I also don't yet have an automated regression test for this fix: `gamessinput` has no existing test suite, and setting one up needs the separate `avogadrodata` checkout, which is why my ship-gate manifest currently excludes `ctest` rather than claiming it passed. Next, I'll write the fix, add a first test for `GamessHighlighter` per the project's own test convention, re-run my reproduction harness for a real before/after screenshot pair, and resolve the one open unknown from `plan.md` — whether the new red is visibly distinct from the preview background in both themes — before opening the PR.
