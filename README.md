# Delta

A text diff and three-way merge, using the real Myers O(ND) algorithm.

https://bxzex.github.io/delta/

When a deleted line is followed by an added one, Delta treats it as a change and diffs the two again word by word, so you can see exactly what moved. The three-way merge anchors on lines that neither side touched and marks conflicts with the usual markers. It can also export a unified patch.

To test it, I rebuilt B from A plus the diff for 400 random pairs, and every one matched. For another 120 pairs the edit distance was compared against an LCS solution, and Myers found the shortest script every time.
