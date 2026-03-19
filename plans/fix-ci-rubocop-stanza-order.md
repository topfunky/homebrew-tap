# Fix CI RuboCop Stanza Order Errors

## Context

GitHub Actions run [`test-bot`](https://github.com/topfunky/homebrew-tap/actions/runs/23271681702/job/67665645320) is failing due to RuboCop style violations in three cask files. All offenses are marked `[Correctable]`.

**Failing files:**
- `Casks/countdown.rb`
- `Casks/og-image-generator.rb`
- `Casks/tiny-timer.rb`

**Error categories:**
- `Cask/StanzaOrder` — `name`, `desc`, `homepage`, `version`, `url`, `sha256` out of order
- `Cask/StanzaGrouping` — stanza groups not separated by a single empty line
- `Layout/ArgumentAlignment` — multi-line method args not aligned
- `Style/IfUnlessModifier` — single-line `if` body should use modifier form (countdown.rb only)

---

## Plan

### Step 1 — Run `brew style --fix` on all three casks

```sh
brew style --fix Casks/countdown.rb Casks/og-image-generator.rb Casks/tiny-timer.rb
```

This auto-corrects all `[Correctable]` offenses in one command.

### Step 2 — Verify no remaining offenses

```sh
brew style Casks/countdown.rb Casks/og-image-generator.rb Casks/tiny-timer.rb
```

Expected output: `No offenses detected.`

### Step 3 — Review diffs and commit

```sh
git diff Casks/
```

Confirm changes look correct (stanza reordering, alignment, grouping), then commit.

---

## Expected Outcome

CI `test-bot` passes on next push.
