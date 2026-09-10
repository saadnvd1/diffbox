# diffbox — ideas

- Stage / unstage per hunk (it is the same `git apply` plumbing, `--cached`).
- Syntax highlighting in the diff (Prism from a CDN, or a tiny tokenizer).
- Word-diff mode toggle (line-level vs char-level).
- Commit box: message + `git commit` for the staged set.
- Multi-repo picker in the sidebar, so one instance serves all of ~/dev.
- Diff against an arbitrary ref, not just HEAD (`diffbox --base main`).
- Collapse unchanged regions between hunks with an "expand 10 lines" control.
- Discard selected LINES inside a hunk (checkbox per row, synth a patch).
- Image diffs (before/after swipe) for png/jpg.
- `--watch` mode using fswatch instead of the 2s poll.
- Keyboard: `[`/`]` to jump hunk to hunk, `u` to undo the last discard by
  stashing instead of hard-resetting.
