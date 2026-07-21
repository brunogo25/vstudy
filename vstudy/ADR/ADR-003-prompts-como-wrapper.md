# ADR-003 — Prompts pedagógicos como wrapper, no edits inline

**Estado:** aceptada · **Referencia:** PLAN.md §6 (también §4 T2.2 y §9 riesgo 2)

## Contexto

La capa de enseñanza (escalera de ayuda, cláusula anti-frustración, semántica `saved_bubble`) debe inyectarse en los prompts del chat. Los internals del chat, fusionados al monorepo en mayo de 2026, son la zona de mayor churn de upstream: editar sus archivos inline multiplicaría los conflictos en cada merge mensual.

## Decisión

La capa vive en un archivo propio `vstudyTeachingInstructions.tsx` (prompt-tsx), registrado como **wrapper de 1–3 líneas** sobre `agentInstructions.tsx`. Es **no bypasseable**; la capa editable por el usuario (`teaching.instructions.md`, T2.4) se suma sin sustituirla.

## Consecuencias

- Superficie de merge mínima: una línea de registro en vez de un archivo upstream reescrito.
- Los prompts viven en el repo, versionados por git, con `CHANGELOG.md` en su directorio; el Context Receipt muestra la versión (SHA corto).
- `teaching-smoke.md` corre en CI (modelo barato o mock determinista); G2 y G3 se repiten tras cada merge.
- Rechazado explícitamente: firmas de prompts, canary, doble aprobación, prompt registry server (PLAN.md §11).
