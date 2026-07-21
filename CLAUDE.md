# CLAUDE.md — Reglas de operación para agentes en VStudy

## 1. Qué es este repo

Fork de `microsoft/vscode` (Code - OSS) para **aprender a programar con IA**: chat de enseñanza socrático, Bubbles, Biblioteca personal. Sin backend, sin cuentas, sin telemetría, BYOK (Anthropic/OpenAI/Ollama), todo local.

- **El plan maestro vive en `vstudy/PLAN.md`. Es la fuente de verdad.** Ante cualquier duda de alcance o diseño, léelo antes de actuar.
- Tag de upstream pineado y Node conocido-bueno: ver `vstudy/UPSTREAM.md`.
- Producto: VStudy · `applicationName: vstudy` · bundle id `online.guio.vstudy` · datos en `.vstudy`.

## 2. Node y builds (regla dura)

El Node global NUNCA sirve. El repo pinea la versión exacta en `.nvmrc` y el estado del shell **no persiste** entre comandos. Por tanto, TODO comando de build/instalación/test se invoca así, en una sola línea:

```sh
eval "$(fnm env)" && fnm use --install-if-missing && npm <comando>
```

- Nunca invoques `npm`/`node` a pelo con el Node global: envenena `node_modules` y los módulos nativos.
- **Un solo `npm run watch` a la vez** en todo el checkout. Antes de lanzar uno, verifica que no haya otro corriendo.
- **Commit legal solo con `watch` en verde** (sin errores de compilación).
- Builds empaquetados (`vscode-darwin-arm64-min`) **solo en gates y días de merge**, nunca en sesiones normales.

## 3. Propiedad por dominio

Cada agente posee rutas explícitas. Editar fuera de tu dominio = **pasarle la tarea al dueño**, no editar tú.

| Agente | Posee | Fases |
|---|---|---|
| build-engineer | toolchain, watch, builds gulp, `vstudy/build/`, CI `.github/`, días de merge, coordinación en F0/F4 | 0, 4, merges |
| branding-agent | `product.json`, `resources/`, `vstudy/brand/`, letterpress SVGs | 1 |
| chat-core-agent | `chat/browser/chatSetup/`, plumbing BYOK/modos en `extensions/copilot/` (código no-prompt), `.vstudyignore` | 1 (T1.4), 2 |
| prompts-author | `extensions/copilot/src/extension/prompts/`, CHANGELOG de prompts, suites QA de prompts, semántica `saved_bubble` | 2, 3 |
| bubbles-agent | `src/vs/workbench/contrib/vstudyBubbles/` completo (incl. Context Receipt) + su línea de registro coordinada | 3 |
| qa-verifier | ejecuta gates; escribe solo en `vstudy/qa/`; nunca edita código de producto | cada gate |

**Puntos de coordinación:** `src/vs/workbench/workbench.common.main.ts` lo edita **un solo agente designado por fase** (el que indique el plan de la fase). Si necesitas una línea de registro ahí y no eres el designado, pídesela.

## 4. Ledger de archivos upstream

- **Todo edit a un archivo de upstream exige una entrada en `vstudy/UPSTREAM-TOUCHED.md`** (archivo + por qué), en el mismo commit.
- Preferir siempre **archivos nuevos propios** registrados mediante un **wrapper de 1–3 líneas** sobre edits inline: minimiza la superficie de merge mensual.
- Objetivo: **≤ ~25 archivos upstream tocados** en todo el MVP. Si tu cambio empuja el contador, busca antes la vía wrapper/contrib.
- La carpeta `vstudy/` y `src/vs/workbench/contrib/vstudyBubbles/` son nuestras: ahí no hay ledger.

## 5. Commits y ramas

- Commits **pequeños y verdes** (watch compilando, tests relevantes pasando).
- Ramas cortas, fusionadas a `main` **en la misma sesión**. Nada de ramas de larga vida.
- Merges de upstream: mensuales, solo build-engineer, con regresión post-merge (G0 + G2/G3 abreviados).

## 6. Rechazos duros

Sin backend/cuentas/billing, sin telemetría (ni "metadata-only"), sin sync cifrado, sin multi-repo, sin Windows/Linux en MVP, sin prompts firmados/registry, sin dashboards docentes/scoring, sin bubbles anidadas en UI, sin Spotify integrado ni YouTube oculto, sin Marketplace de MS — lista completa y vinculante en `vstudy/PLAN.md` §11.
