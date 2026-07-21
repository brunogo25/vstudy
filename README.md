# VStudy

**An editor that teaches you to program with AI — instead of programming for you.**

Most AI editors optimize for shipping code as fast as possible. VStudy optimizes for the moment *after* the code works: can you explain why it works, spot a similar bug next time, and solve the next problem with less help? That is the whole point of this project.

## What is VStudy?

VStudy is an open-source fork of [VS Code](https://github.com/microsoft/vscode) built for people who are **learning** to program with AI. It keeps everything you expect from a modern editor and adds a teaching layer on top of the AI chat:

- **Learn mode** — the default chat mode uses a Socratic *help ladder*: it first asks you to predict the problem, then gives a conceptual hint, then the next step or pseudocode, then a minimal snippet. And when you explicitly ask for the full solution, **it gives it to you** — a Socratic tutor that never yields is just frustrating — then closes with a quick transfer check ("explain it back in your own words"). A separate **Build mode** keeps the standard agent behavior intact for when you just need to ship.

- **Bubbles** — the differentiator. Select any text in a chat response and open a mini-chat about that concept, right there. When you're done, save the bubble to your personal **"everything I learned" library**. Later, reference any saved bubble from the main chat with `#bubble` — the AI treats it as knowledge you already acquired and builds on it instead of re-teaching from scratch. Your library lives on your disk as plain JSON, and exporting it is just zipping a folder.

- **Context Receipt** — every request shows exactly what was sent to the model: files, bubbles, excerpts, secret redactions, model, and prompt version. The AI's memory is inspectable.

## Status

> **Pre-alpha.** Expect rough edges and breaking changes.

- Platform: **macOS Apple Silicon (arm64) first.** Windows/Linux are planned post-MVP via community ports (a reproducible build guide will live in `vstudy/build/BUILDING.md`).
- Pinned to upstream VS Code **1.128.1**, with monthly upstream merges (never more than 2 months behind).
- Repo: <https://github.com/brunogo25/vstudy>

## Bring your own key (BYOK)

VStudy has **no backend, no accounts, and no telemetry**. You connect your own AI provider; your key is stored in your OS keychain and talks directly to the provider.

| Provider | What you need | Where it runs |
|---|---|---|
| Anthropic | Your Anthropic API key | Your machine → Anthropic's API |
| OpenAI | Your OpenAI API key | Your machine → OpenAI's API |
| Ollama | Ollama installed locally (auto-detected at `localhost:11434`) | 100% local, works offline |

Your key, your machine, your data. There are no VStudy servers — none exist.

## Privacy

VStudy collects nothing. Not "telemetry off by default" — telemetry is **removed**. All learning data (bubbles, library, settings) lives on your local disk and is exportable. See [PRIVACY.md](PRIVACY.md).

## Building from source

```sh
# Node is pinned by .nvmrc — use fnm, never your global Node
brew install fnm
eval "$(fnm env)"  # hook fnm into this shell (add to your shell profile)
fnm install        # installs the exact version from .nvmrc
fnm use
npm install
npm run watch      # keep exactly one watch running
./scripts/code.sh  # launches VStudy from source
```

A full build guide (packaging, signing, porting) is coming in `vstudy/build/BUILDING.md`.

## Roadmap

The public roadmap lives in [`vstudy/BACKLOG.md`](vstudy/BACKLOG.md). New ideas go there first — the MVP scope is frozen.

## License & trademark

VStudy is MIT-licensed, inheriting from upstream VS Code (Microsoft copyright notices preserved — see [NOTICE.md](NOTICE.md)). The VStudy name and branding are restricted, VSCodium-style — see [TRADEMARK.md](TRADEMARK.md).

## ¿Hablas español?

Este proyecto lo lidera su fundador en español. Los issues y PRs en español son totalmente bienvenidos — no hace falta escribir en inglés para participar. Contacto: bruno@guio.online.
