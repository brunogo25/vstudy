# Contributing to VStudy

Thanks for your interest in VStudy! This project is founder-led and part-time, so small, focused contributions are the easiest to review and merge.

## Building from source

```sh
# Node is pinned by .nvmrc — use fnm, never your global Node
brew install fnm
eval "$(fnm env)"  # hook fnm into this shell (add to your shell profile)
fnm install        # installs the exact version from .nvmrc
fnm use
npm install
npm run watch      # keep exactly one watch running at a time
./scripts/code.sh  # launches VStudy from source
```

Notes:

- **Never use your global Node.** Native modules are compiled against the pinned version; the wrong Node poisons the build.
- Run **one** `npm run watch` at a time. Only commit when watch is green.
- Packaged builds (`vscode-darwin-arm64-min`) are only needed at release gates — day-to-day development uses `./scripts/code.sh`.

VStudy is pinned to upstream VS Code **1.128.1** with monthly upstream merges.

## The upstream ledger rule

VStudy is a fork of `microsoft/vscode`, and every file we change from upstream is merge-conflict surface forever. So:

> **Any change to an upstream file must be recorded in [`vstudy/UPSTREAM-TOUCHED.md`](vstudy/UPSTREAM-TOUCHED.md)** — which file, and why the change couldn't live in our own code.

Prefer creating **new files under `vstudy/` or `src/vs/workbench/contrib/vstudyBubbles/`** registered via a 1–3 line wrapper over editing upstream files inline. PRs that touch upstream files without a ledger entry will be asked to add one before review.

## Prompt-change PRs

The teaching prompts (the Socratic help ladder, `saved_bubble` semantics, etc.) define VStudy's core behavior, so changes to them get extra scrutiny. Every prompt-change PR must use the template and include:

1. **What behavior changes** — a plain-language description of the before/after behavior.
2. **A before/after transcript** — the same conversation run against the old and new prompt, showing the difference.

The prompt smoke suite (`vstudy/qa/teaching-smoke.md`) runs in CI; prompt PRs should keep it green. Prompts are versioned by git with a `CHANGELOG.md` in the prompts directory.

## What's out of scope

Some things are explicitly rejected in the master plan (§11) and PRs adding them will be closed, kindly but firmly: any backend/gateway/accounts/billing, telemetry of any kind (including "metadata-only"), encrypted bubble sync, teacher dashboards / academic scoring / cheat detection, signed prompts or prompt registries, and Microsoft Marketplace integration. When in doubt, open an issue first.

## Finding something to work on

Look for issues labeled **`good first issue`** — they're scoped to be doable without deep knowledge of the VS Code codebase. Issues and PRs in **Spanish are welcome** too.

## Code of conduct

By participating you agree to our [Code of Conduct](CODE_OF_CONDUCT.md). Be kind — many contributors here are students writing their first PRs.
