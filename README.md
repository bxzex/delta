# Delta

Myers diff and three-way merge. The real O(ND) algorithm, character-level
refinement, conflict detection and unified patch output.

Live: https://bxzex.github.io/delta/

## The algorithm

Myers' difference algorithm walks diagonals `k = x - y`, tracking the furthest
`x` reachable on each one for a given edit distance `d`. The first time a
diagonal reaches the far corner, `d` is the length of the shortest edit script.
Every row of the frontier is kept so the path can be walked backwards
afterwards into a real script of insertions, deletions and matches, rather than
just a distance.

- **Character refinement.** A run of deletions immediately followed by a run of
  insertions is a change, not a removal and an addition, so the two are paired
  and diffed again at word granularity. That second pass is the same algorithm
  applied to tokens instead of lines.
- **Three-way merge** is diff3: base against A and base against B, anchored on
  lines that are unchanged on both sides. Between anchors, a region where only
  one side moved takes that side, a region where both sides made the same change
  takes it once, and a region where both moved differently is a conflict,
  reported with the usual markers.
- **Unified patch** output in the standard `@@ -a,b +c,d @@` form.

## Verification

Two properties are checked, both against references rather than by eye.

**Correctness.** 400 randomised pairs, each built by applying random insertions,
deletions and substitutions, are diffed and then reconstructed: the script is
replayed against A and the result compared to B. All 400 rebuild B exactly, and
the script's consumption of A is checked to line up index by index.

**Minimality.** For 120 random pairs the edit distance is compared against the
optimal computed independently by longest-common-subsequence dynamic
programming. The worst excess over optimal is **zero** — Myers returns the
shortest possible edit script every time, which is the entire point of it.

## Notes

One HTML file. No libraries, no build step. Edit either pane and the diff
updates as you type.

Built by [bxzex](https://bxzex.com).
