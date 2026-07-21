# ADR-001 — Fork único vs. modelo de parches

**Estado:** aceptada · **Referencia:** PLAN.md §3

## Contexto

VStudy nace de `microsoft/vscode`. Hay dos modelos conocidos: fork único con historia completa y merges periódicos, o el modelo de parches de VSCodium (árbol ensamblado + archivos `.patch`). El desarrollo lo ejecutan agentes de Claude Code, y regenerar `.patch` sobre árboles ensamblados es el peor flujo posible para ellos.

## Decisión

**Fork único** de `microsoft/vscode` (historia completa) con merges periódicos de upstream (mensuales, pin en `vstudy/UPSTREAM.md`). NO el modelo de parches: es óptimo para deltas de build, pero incompatible con nuestro flujo de agentes.

## Consecuencias

- Todo edit a archivo upstream se registra en `vstudy/UPSTREAM-TOUCHED.md`; objetivo ≤ ~25 archivos tocados en el MVP.
- Se prefieren archivos nuevos propios registrados como wrapper de 1–3 líneas sobre edits inline (menos superficie de merge).
- Lo propio vive en `vstudy/` y `src/vs/workbench/contrib/vstudyBubbles/`, rutas que nunca conflictúan con upstream.
- Coste asumido: resolver conflictos de merge cada mes (`git rerere` + ledger lo acotan).
