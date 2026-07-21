# UPSTREAM-TOUCHED.md — Ledger de archivos upstream tocados

Este ledger existe para controlar la **superficie de merge** con `microsoft/vscode`: cada archivo de upstream que VStudy modifica es un conflicto potencial en cada merge mensual. Todo edit a un archivo upstream se registra aquí en el mismo commit que lo introduce (regla de PLAN.md §3 y §8). Objetivo del MVP: **≤ ~25 archivos upstream tocados** en total. Siempre que sea posible, preferir archivos nuevos propios registrados como wrapper de 1–3 líneas sobre edits inline — el wrapper también se anota aquí, marcando la columna correspondiente. Si un archivo deja de estar tocado (se revierte o se absorbe upstream), se elimina su fila indicándolo en el mensaje de commit.

| Archivo | Fase/Tarea | Razón | Wrapper o inline | Fecha |
|---|---|---|---|---|
| `README.md` | F0/T0.7 | Reemplazo completo: misión pedagógica de VStudy en lugar del README de Code - OSS | reemplazo | 2026-07-21 |
| `CONTRIBUTING.md` | F0/T0.7 | Reemplazo completo: guía de contribución de VStudy (ledger, PRs de prompts, build con fnm) | reemplazo | 2026-07-21 |
| `.github/pull_request_template.md` | F0/T0.7 | Reemplazo: checklist de VStudy (watch verde, ledger, transcripts de prompts) | reemplazo | 2026-07-21 |
| `.github/ISSUE_TEMPLATE/bug_report.md` | F0/T0.7 | Reemplazo: plantilla de bugs de VStudy (versión VStudy + macOS) | reemplazo | 2026-07-21 |
| `.github/ISSUE_TEMPLATE/feature_request.md` | F0/T0.7 | Reemplazo: plantilla de features con recordatorio de rechazos §11 | reemplazo | 2026-07-21 |
| `product.json` | F1/T1.1+T1.4 | identidad VStudy + Open VSX; 5 endpoints GitHub de `defaultChatAgent` vaciados (belt-and-braces de T1.4) | inline | 2026-07-21 |
| `src/vs/platform/telemetry/common/telemetryService.ts` | F1/T1.3 | default de `telemetry.telemetryLevel` cambiado de `on` a `off` (telemetría off por defecto) | inline | 2026-07-21 |
| `resources/darwin/code.icns` | F1/T1.2 | Icono de app VStudy (V-libro índigo) generado desde `vstudy/brand/vstudy-icon.svg`; mismo nombre de archivo que referencia el build | reemplazo | 2026-07-21 |
| `src/vs/workbench/browser/parts/editor/media/letterpress-dark.svg` | F1/T1.2 | Watermark del editor vacío: marca V-libro outline en vez del logo VS Code (negro, opacity 0.3 como el original) | reemplazo | 2026-07-21 |
| `src/vs/workbench/browser/parts/editor/media/letterpress-light.svg` | F1/T1.2 | Watermark del editor vacío: marca V-libro outline (negro, opacity 0.1 como el original) | reemplazo | 2026-07-21 |
| `src/vs/workbench/browser/parts/editor/media/letterpress-hcDark.svg` | F1/T1.2 | Watermark del editor vacío: marca V-libro outline (`#3C3C3C` como el original) | reemplazo | 2026-07-21 |
| `src/vs/workbench/browser/parts/editor/media/letterpress-hcLight.svg` | F1/T1.2 | Watermark del editor vacío: marca V-libro outline (`#D9D9D9` como el original) | reemplazo | 2026-07-21 |
| `src/vs/workbench/contrib/chat/browser/chatSetup/chatSetupContributions.ts` | F1/T1.4 | Gating GitHub/Copilot neutralizado: estado forzado a `completed` + entitlement `Unresolved` (nunca `signedOut`), y `ChatEntitlementRequests` se mantiene lazy para no resolver entitlements de GitHub al arrancar | inline | 2026-07-21 |
| `src/vs/workbench/contrib/chat/browser/chatSetup/chatSetupRunner.ts` | F1/T1.4 | `ChatSetup.doRun` corta tras el trust de workspace: nunca muestra el diálogo de sign-in ni ejecuta el controller (sin sign-up/instalación GitHub); guarda `VSTUDY_SKIP_PROVIDER_SETUP: boolean` para no dejar código inalcanzable | inline | 2026-07-21 |
| `src/vs/workbench/contrib/chat/browser/chatSetup/chatSetupProviders.ts` | F1/T1.4 | `doInvokeWithSetup` no construye el `ChatSetupController` (su construcción resolvería entitlements de GitHub); listener de progreso sustituido por `Disposable.None` tras `VSTUDY_SKIP_PROVIDER_SETUP` | inline | 2026-07-21 |
| `build/hygiene.ts` | F1/T1.1 | Check de higiene permite `extensionsGallery` solo si apunta a Open VSX (antes prohibido en absoluto) | inline | 2026-07-21 |
| `src/vs/workbench/contrib/extensions/browser/extensions.contribution.ts` | F1/T1.5 | `extensions.verifySignature` default a `false`: el build OSS no incluye `@vscode/vsce-sign`, la verificación fallaría y rechazaría toda instalación de Open VSX en el empaquetado | inline | 2026-07-21 |
| `src/vs/platform/extensionManagement/node/extensionManagementService.ts` | F1/T1.5 | Fallback de `verifySignature` a `false` cuando la config no está explícita (el camino CLI no registra la contribución del workbench); sin esto el empaquetado rechaza toda instalación de Open VSX con "Signature verification was not executed" | inline | 2026-07-21 |
