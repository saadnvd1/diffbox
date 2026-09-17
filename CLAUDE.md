# diffbox — for Claude

Single-file Python app at `./diffbox` (~860 lines, stdlib only). Installed by
symlinking it onto `PATH` as `diffbox` and `db`. Read `README.md` for
behaviour.

## Shape

- `Repo` — every git call lives here. `status()` (porcelain v1, `-z`),
  `diff()`, `discard_file()`, `discard_hunk()`, `discard_all()`.
- `parse_hunks()` — unified diff → side-by-side rows. Consecutive `-` and `+`
  runs are paired index-wise; a pair becomes one `mod` row with char-level
  segments from `difflib.SequenceMatcher`.
- `INDEX_HTML` — the whole frontend (CSS + JS, no framework, no CDN).
- `Handler` — `/`, `GET /api/status`, `GET /api/diff`, and three POSTs.

## Traps

- **Diff base is HEAD**, not the index. Everything downstream assumes that.
- `discard_hunk` unstages a partially staged file first, or `git apply -R`
  leaves the index describing a worktree that no longer exists.
- Untracked files have no diff — they use `git diff --no-index /dev/null <f>`,
  and their line counts come from reading the file, not from `--numstat`.
- Porcelain `-z` puts the rename SOURCE in the *next* NUL field, and
  `--numstat -z` puts the two names in the *following two* fields. Both
  parsers step the index by hand; do not naively `split("\x00")`.
- `sig` (mtime+size of changed files) drives the 2s poll. If a change is not
  showing up in the UI, that hash is the first suspect.
- Bind stays `127.0.0.1`. This app destroys work; do not expose it.

## Testing

No test suite yet. The manual loop: make a scratch repo, modify/delete/add
files, then exercise each discard path and confirm with `git status`.
A headless screenshot is the fastest UI check (macOS path shown; use whatever
`chrome`/`chromium` binary you have):

    "/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" --headless \
      --disable-gpu --window-size=1400,850 --virtual-time-budget=6000 \
      --screenshot=ui.png http://127.0.0.1:3085/

Print-to-PDF renderers come back blank here — the flex layout collapses in
paged media. Use the raster screenshot.
