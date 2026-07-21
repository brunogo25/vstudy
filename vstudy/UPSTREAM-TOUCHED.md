# UPSTREAM-TOUCHED.md — Ledger de archivos upstream tocados

Este ledger existe para controlar la **superficie de merge** con `microsoft/vscode`: cada archivo de upstream que VStudy modifica es un conflicto potencial en cada merge mensual. Todo edit a un archivo upstream se registra aquí en el mismo commit que lo introduce (regla de PLAN.md §3 y §8). Objetivo del MVP: **≤ ~25 archivos upstream tocados** en total. Siempre que sea posible, preferir archivos nuevos propios registrados como wrapper de 1–3 líneas sobre edits inline — el wrapper también se anota aquí, marcando la columna correspondiente. Si un archivo deja de estar tocado (se revierte o se absorbe upstream), se elimina su fila indicándolo en el mensaje de commit.

| Archivo | Fase/Tarea | Razón | Wrapper o inline | Fecha |
|---|---|---|---|---|
| `README.md` | F0/T0.7 | Reemplazo completo: misión pedagógica de VStudy en lugar del README de Code - OSS | reemplazo | 2026-07-21 |
| `CONTRIBUTING.md` | F0/T0.7 | Reemplazo completo: guía de contribución de VStudy (ledger, PRs de prompts, build con fnm) | reemplazo | 2026-07-21 |
| `.github/pull_request_template.md` | F0/T0.7 | Reemplazo: checklist de VStudy (watch verde, ledger, transcripts de prompts) | reemplazo | 2026-07-21 |
| `.github/ISSUE_TEMPLATE/bug_report.md` | F0/T0.7 | Reemplazo: plantilla de bugs de VStudy (versión VStudy + macOS) | reemplazo | 2026-07-21 |
| `.github/ISSUE_TEMPLATE/feature_request.md` | F0/T0.7 | Reemplazo: plantilla de features con recordatorio de rechazos §11 | reemplazo | 2026-07-21 |
