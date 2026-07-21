# UPSTREAM.md — Estado del pin de upstream

Fuente de verdad sobre qué versión de `microsoft/vscode` tiene VStudy como base y cómo se sincroniza. Referencia: PLAN.md §1 (pin de versión) y §9 (riesgos 1 y 3).

## Pin actual

| Campo | Valor |
|---|---|
| Tag pineado | `1.128.1` |
| SHA del tag | `5264f2156cbcd7aea5fd004d29eaa10209155d66` |
| Node (según `.nvmrc` del tag) | `24.17.0` |
| Fecha del pin | 2026-07-21 |

> El Node de `.nvmrc` es el **único** Node válido para builds (vía `fnm`, nunca el global). Un Node equivocado envenena los módulos nativos (riesgo 3).

## Regla de pin

- Se pinea siempre la **última release semanal estable con ≥ 7 días sin hotfix**.
- Merge de upstream **mensual**; nunca más de **2 meses** de rezago.
- Tras cada merge: regresión G0 + G2 abreviado + G3 abreviado antes de mergear a `main` (PLAN.md §10).
- Usar `git rerere` para no re-resolver los mismos conflictos; todo archivo upstream tocado va al ledger `vstudy/UPSTREAM-TOUCHED.md`.

## Remotes y fetch

- `origin` = repo del fundador: `https://github.com/brunogo25/vstudy`
- `upstream` = `https://github.com/microsoft/vscode.git`

```sh
git remote add upstream https://github.com/microsoft/vscode.git
# Refspec exacto usado (main + tags):
git fetch upstream +refs/heads/main:refs/remotes/upstream/main --tags
```

## Procedimiento al actualizar el pin

1. Elegir el nuevo tag según la regla de pin.
2. `git fetch upstream +refs/heads/main:refs/remotes/upstream/main --tags`
3. Merge del tag en una rama de integración; resolver conflictos consultando el ledger.
4. Actualizar esta tabla (tag, SHA, Node de `.nvmrc`, fecha) en el mismo commit del merge.
5. Correr la regresión post-merge antes de tocar `main`.
