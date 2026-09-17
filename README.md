# diffbox

A very light GitKraken: the files you changed, a side-by-side diff, and the
three discard buttons that matter. Nothing else — no staging, no committing,
no branch graph.

Single-file Python (stdlib only), served on `127.0.0.1` and opened in your
browser. Works against any git repo.

```bash
diffbox                 # the repo you are standing in
diffbox ~/code/myproject  # a specific repo
diffbox -p 3090         # a specific port (default 3085, walks up if taken)
diffbox --no-open       # do not open a browser
```

## Install

No dependencies beyond `git` and Python 3.9+. Clone it and put the script on
your `PATH`:

```bash
git clone https://github.com/saadnvd1/diffbox.git
cd diffbox
ln -s "$PWD/diffbox" ~/.local/bin/diffbox
ln -s "$PWD/diffbox" ~/.local/bin/db      # optional short alias
```

## What it shows

Working tree **vs HEAD** — staged and unstaged changes together, the way a WIP
node reads in GitKraken. A blue dot on a file means part of it is staged.

- Sidebar: every changed file with `M`/`U`/`A`/`D`/`R`, `+adds −dels`, and an
  `✕` to discard it.
- Diff: side-by-side, per hunk, with **char-level highlighting** inside a
  changed line pair.
- Untracked files render as an all-additions diff.

## Discarding

| Action | What it runs |
|---|---|
| Discard hunk | `git apply --reverse` of just that hunk |
| Discard file (tracked) | `git restore --source=HEAD --staged --worktree` |
| Discard file (untracked) | deletes the file |
| Discard all | `git reset --hard HEAD` (+ `git clean -fd` if you tick the box) |

Every one asks first. None of them can be undone — that is the point of the
tool, so it is worth reading the modal.

**Partially staged files:** discarding a hunk unstages the file first
(`git restore --staged`), because reverse-applying to the worktree alone would
desync the index. Your other changes to that file survive; only the staging
goes away.

## Keys

`j` / `k` move between files · `d` discard the current file · `r` refresh ·
`w` toggle wrap · `Esc` / `Enter` in a confirm dialog.

The file list polls every 2s, so edits in your editor appear without a
refresh.

## Notes

- Binds `127.0.0.1` only, deliberately: this app deletes work, and it is not
  something to expose on a network.
- Diffs use `-U3` context. Two nearby edits will render as one hunk.
- Requires `git` and Python 3.9+. No pip install, no node, no build step.

## License

MIT — see [LICENSE](LICENSE).
