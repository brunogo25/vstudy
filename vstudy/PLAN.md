# VStudy — Plan maestro v2
## Fork open source de Code-OSS para aprender a programar con IA (sin backend, BYOK, local-first)

---

## 0. Contexto y misión

VStudy es un fork de VS Code (estilo Cursor/Windsurf) cuyo diferenciador es **enseñar a programar con IA**, no solo autocompletar:

1. **Capa de enseñanza** inyectada en el chat: método socrático con escalera de ayuda y cláusula anti-frustración (§6).
2. **Bubbles** (el diferenciador): seleccionas texto en el chat y abres un mini-chat temporal sobre ese concepto. Los bubbles se guardan en una biblioteca personal ("todo lo que aprendí") y el chat principal puede referenciarlos como contexto con procedencia marcada.
3. **(Post-MVP) Hogar del estudiante**: Discord + música/focus.

**Misión:** el alumno no solo consigue que el código funcione: termina siendo capaz de explicar por qué funciona, detectar errores similares y resolver el siguiente problema con menos ayuda.

**Modelo:** proyecto open source (MIT), sin empresa, sin backend, sin cuentas, sin suscripciones, cero telemetría. Los usuarios traen su propia key (BYOK) o usan Ollama local. Los datos de aprendizaje viven en disco del usuario y son exportables.

**Hallazgo que cambia todo:** desde 2026-05-20 el stack completo de Copilot Chat (UI nativa + agente + system prompts + proveedores BYOK) está fusionado en el monorepo `microsoft/vscode` bajo MIT. BYOK funciona sin login de GitHub (incluye `AnthropicLMProvider` first-party y Ollama offline). Esto elimina la necesidad de cualquier gateway/backend: la capa pedagógica se inyecta en `prompt-tsx` dentro del fork, visible y auditable en el repo.

**Entorno verificado:** Mac Apple Silicon 18 núcleos, 24 GB RAM, 569 GB libres, Xcode CLT y git presentes. Falta `fnm` (el Node global 24.15 NO sirve; el repo pinea versión exacta en `.nvmrc`).

---

## 1. Decisiones confirmadas (2026-07-21)

| Decisión | Valor |
|---|---|
| Monetización | Ninguna. Sin backend bajo ninguna circunstancia. Financiación opcional vía donaciones (§7) |
| Modelos | BYOK: Anthropic, OpenAI, Ollama local. Keys en secret storage del SO del usuario |
| MVP | Fork con marca + chat de enseñanza (Learn/Build) + Bubbles + Biblioteca + Context Receipt + `.vstudyignore` |
| Plataforma | macOS arm64 primero. Windows/Linux post-MVP vía comunidad + guía de build reproducible |
| Repo | **Público en GitHub desde v0.1** (nada impide abrirlo antes; no hay secretos en el repo). `origin` = repo del fundador, `upstream` = `microsoft/vscode` |
| Pin de versión | Última release semanal estable con ≥7 días sin hotfix. Merge mensual de upstream, nunca >2 meses de rezago |
| Telemetría | **Cero.** No "off por defecto": eliminada/neutralizada. Compromiso público en `PRIVACY.md` |
| Distribución | GitHub Releases + DMG firmado/notarizado. Fallback documentado: firma ad-hoc + instrucciones Gatekeeper si el certificado Apple no está disponible |
| Directorio | `/Users/brunogo/Life/Proyectos/Vstudy` |

---

## 2. Restricciones legales verificadas

- Marketplace de Microsoft prohibido para forks → **Open VSX**. Extensiones propietarias de MS (Pylance, cpptools) no disponibles → documentar sustitutos de Open VSX.
- "VS Code" e iconos son marca de Microsoft → reemplazar todo el branding. Modelo **VSCodium**: código MIT abierto, marca propia restringida (`TRADEMARK.md`).
- Conservar `LICENSE.txt` MIT y notices de Microsoft en todas las distribuciones.
- Spotify descartado como feature core (modo dev limitado; acceso comercial inalcanzable).
- YouTube: descargar audio es ilegal → fuera de alcance. Solo embeds con reproductor completo y visible.
- Discord ✅: extensión vscord (Open VSX) + widget oficial de servidor. Opt-in, sin publicar nombres de archivos/proyectos.
- Sin backend + sin cuentas + sin menores + sin scoring académico → basta `PRIVACY.md` ("no recogemos nada; tu key va directa a tu proveedor"). Sin programa RGPD formal.

---

## 3. Estrategia de repo

**Fork único** de `microsoft/vscode` (historia completa) con merges periódicos de upstream. NO el modelo de parches de VSCodium: es óptimo para deltas de build, pero regenerar `.patch` sobre árboles ensamblados es el peor flujo posible para agentes de Claude Code.

```
VStudy/                          # clone de microsoft/vscode
├── LICENSE / NOTICE             # MIT conservando notices de MS
├── TRADEMARK.md                 # código abierto, marca VStudy restringida
├── README.md                    # misión pedagógica como headline
├── CONTRIBUTING.md / CODE_OF_CONDUCT.md / PRIVACY.md
├── .github/                     # CI build+gates, release workflow, issue templates
├── product.json                 # modificado (marca, Open VSX, sin telemetría)
├── CLAUDE.md                    # reglas de agentes: dominios, ledger
├── vstudy/                      # carpeta propia, nunca conflictúa con upstream
│   ├── UPSTREAM.md              # tag pineado + SHA + Node conocido-bueno
│   ├── UPSTREAM-TOUCHED.md      # ledger: cada archivo upstream tocado + por qué
│   ├── BACKLOG.md               # roadmap público anti scope-creep
│   ├── ADR/                     # decision records cortos
│   ├── brand/  build/  qa/      # SVGs, scripts firma/DMG, smoke suites
└── src/vs/workbench/contrib/vstudyBubbles/   # feature Bubbles completa (propia)
```

- **Control de superficie:** todo edit a archivo upstream se registra en `UPSTREAM-TOUCHED.md`. Objetivo: ≤ ~25 archivos upstream tocados en todo el MVP.
- Preferir **archivos nuevos propios registrados como wrapper** de 1–3 líneas sobre edits inline (menos superficie de merge).

---

## 4. Fases de implementación

### Fase 0 — Bootstrap + build verde (build-engineer)

- **T0.1** Toolchain: `brew install fnm`; `fnm install` según `.nvmrc`; verificar `setuptools`.
- **T0.2** Clonar `microsoft/vscode`; remotes `upstream`/`origin`; checkout del tag pineado; `main`; push a repo del fundador.
- **T0.3** `npm install` (primera prueba real del toolchain).
- **T0.4** `npm run watch` + `./scripts/code.sh` arranca Code - OSS sin errores fatales.
- **T0.5** Build empaquetado `vscode-darwin-arm64-min` → `.app` arranca desde Finder.
- **T0.6** Scaffolding: `CLAUDE.md`, `vstudy/UPSTREAM*.md`, `BACKLOG.md`, `ADR/`.
- **T0.7** Archivos OSS base: `LICENSE`, `NOTICE`, `TRADEMARK.md`, `README.md` mínimo, `CONTRIBUTING.md`, `CODE_OF_CONDUCT.md`, `PRIVACY.md`, esqueleto `.github/workflows/`.
- **Gate G0:** nada de cambios VStudy antes de que el build sin modificar funcione (aísla "el build" de "mis cambios lo rompieron").

### Fase 1 — Marca y des-Microsoftización

- **T1.0** (Humano, ~1 hora) Búsqueda de trademark de "VStudy" (USPTO/EUIPO + web). Si hay conflicto, decidir nombre **antes** de diseñar iconos.
- **T1.1** `product.json`: `nameShort/nameLong = VStudy`, `applicationName = vstudy`, `dataFolderName = .vstudy`, `darwinBundleIdentifier`, licencia propia, omitir `updateUrl` (sin auto-update en v1), `extensionsGallery` → Open VSX, quitar telemetría/encuestas, ajustar `defaultChatAgent`.
- **T1.2** Iconos e identidad (SVG fuente en `vstudy/brand/`): `code.icns`, watermarks letterpress, favicons. Auditoría grep de "Visual Studio Code" en strings visibles. (Puede empezar durante F0.)
- **T1.3** Telemetría **eliminada/neutralizada** (no solo off). Verificación con proxy: cero tráfico a endpoints MS.
- **T1.4** Neutralizar gating en `chatSetup/`: quitar sign-in/entitlements; el chat abre "Conecta tu IA" sin pedir cuenta de GitHub en **ningún** flujo. Mantener `github-authentication` solo para git.
- **T1.5** Verificar instalación de extensiones desde Open VSX.
- Orden: T1.0/T1.1 primero; luego T1.2 ∥ T1.3 ∥ T1.4. **Gate G1.**

### Fase 2 — Chat de enseñanza

- **T2.1** Onboarding BYOK: "Conecta tu IA" — 3 opciones: key Anthropic, key OpenAI, Ollama local (autodetectar `localhost:11434`). Keys en secret storage; nota de costos en lenguaje claro. Documentar en `vstudy/qa/` el **modelo local mínimo recomendado** (los prompts socráticos degradan rápido en modelos pequeños; listar modelos probados).
- **T2.2** Capa de prompts de enseñanza (**no bypasseable**): nuevo archivo propio `vstudyTeachingInstructions.tsx` (prompt-tsx) registrado como wrapper de 1–3 líneas sobre `agentInstructions.tsx` — minimiza superficie de merge. Contenido: escalera de ayuda (§6), idioma del estudiante, y sugerir bubbles ante términos nuevos ("selecciónalo y abre un Bubble").
- **T2.3** Modos Learn vs Build: Learn por defecto en primer arranque; Build = agent mode existente intacto (multi-archivo, terminal).
- **T2.4** Capa editable por el usuario: `teaching.instructions.md` materializado en el perfil; el modo Learn lo incluye (la capa T2.2 sigue fija).
- **T2.5** Suite de smoke prompts `vstudy/qa/teaching-smoke.md` con casos de escalera (§10, G2).
- **T2.6** (NUEVO) `.vstudyignore` + escaneo local de secretos: exclusiones por defecto (`.env`, `*.pem`, credenciales); detector local de secretos antes de adjuntar archivos/selecciones al contexto; aviso con opción de enviar igualmente; setting para desactivar. Todo local, sin red.
- Orden: T2.1 desbloquea T2.2/T2.3. **Gate G2.** (El fundador inicia aquí el trámite de Apple Developer — ver T4.1.)

### Fase 3 — Bubbles (el diferenciador)

- **T3.1** Modelo de datos + storage: `Bubble` + `BubbleAnchor` (§5), `IBubbleStorageService` (JSON por bubble, índice reconstruible) en `src/vs/workbench/contrib/vstudyBubbles/common/`. **Tests unitarios mínimos del storage** (lo que menos debe romperse en silencio).
- **T3.2** Crear-desde-selección: acción contextual sobre respuestas del chat (vía `MenuRegistry` desde nuestra contrib, sin editar código del chat); widget flotante compacto vía `widgetHosts`; línea de registro coordinada en `workbench.common.main.ts`.
- **T3.3** Guardar/descartar: título (1 llamada LLM barata; fallback extracto truncado) + resumen al guardar + tags opcionales.
- **T3.4** Biblioteca: contenedor propio en Activity Bar + ViewPane (lista, búsqueda); badge **"la fuente cambió"** en bubbles con anchor `stale`; acciones Continuar / Renombrar / Tag / Eliminar / Adjuntar al chat. (Paralelo con T3.2/T3.3.)
- **T3.5** Referencia `#bubble` en chat principal vía attachment picker existente: multi-selección con estimación de tokens; inyección con marcado de procedencia (§5).
- **T3.6** Endurecimiento de casos borde (§5).
- **T3.7** (NUEVO) **Context Receipt**: vista colapsable por request que muestra qué se envió al modelo: archivos, bubbles (por título), excerpts, redacciones aplicadas por T2.6, modelo y versión del prompt (SHA corto de git). Solo renderiza el payload que ya existe — sin backend. Vive en nuestra contrib.
- **Gate G3.**

### Fase 4 — Empaquetado, firma y release

- **T4.1** (Humano, iniciar en F2) Apple Developer $99/año; certificado Developer ID Application.
- **T4.2** Build de producción `vscode-darwin-arm64-min-ci`.
- **T4.3** Script de firma `vstudy/build/release-macos.sh`: binarios anidados de adentro hacia afuera, runtime + entitlements (incl. `allow-jit`), estilo VSCodium.
- **T4.4** Notarizar: `xcrun notarytool` + `stapler staple`.
- **T4.5** DMG con `create-dmg` + GitHub Release `vstudy-v0.1.0` con SHA256. Sin auto-update en v1.
- **T4.6** Release workflow en GitHub Actions (build + firma + notarización + DMG + SHA256 + release notes). **Fallback documentado:** build con firma ad-hoc + instrucciones Gatekeeper si el certificado no está listo.
- **Gate G4.**

### Fase 5 — Hogar del estudiante (post-MVP; puede solaparse con F4)

- **T5.1** Discord Rich Presence: `vscord` en `builtInExtensions`; opt-in; **no publica nombres de archivos/proyectos/ejercicios**.
- **T5.2** Panel "Home": webview con widget oficial de servidor Discord + botón de invitación. Sin bot, sin backend.
- **T5.3** Música/focus: webview con streams lo-fi de YouTube (reproductor completo visible — línea dura: nada de audio-only ni video oculto) y/o play/pause del Spotify local vía AppleScript detrás de un setting (sin API de Spotify).

**Ritmo estimado (part-time + agentes):** F0 ≈ 1–2 sesiones · F1 ≈ 3–4 sesiones · F2 ≈ 1–2 semanas · F3 ≈ 2–3 semanas (T3.7 añade ~2 sesiones) · F4 ≈ 2–3 sesiones + espera de Apple · F5 ≈ 1 semana.

---

## 5. Diseño detallado de Bubbles

```ts
interface Bubble {
  id: string;                    // UUID
  version: 1;
  title: string;                 // auto-sugerido al guardar, editable
  tags: string[];
  sourceSessionId: string;
  sourceRequestId?: string;
  rootSessionId: string;         // sesión original (bubble-de-bubble)
  sourceExcerpt: string;         // selección, máx ~2000 chars
  anchor: BubbleAnchor;
  messages: { role: 'user' | 'assistant'; content: string; at: number }[];
  summary: string;               // generado al guardar; inyección compacta
  model?: string;
  createdAt: number;
  savedAt: number;
}

interface BubbleAnchor {
  kind: 'chat' | 'file';
  originFile?: string;           // ruta relativa si nace de archivo
  range?: { start: number; end: number };
  hash?: string;                 // hash del rango al crear
  status: 'fresh' | 'stale';     // stale = la fuente cambió
}
```

**Storage:** global a nivel de perfil, archivo por bubble: `~/.vstudy/User/globalStorage/vstudy.bubbles/<id>.json` + `index.json` reconstruible por escaneo. Razones: (a) los bubbles siguen al estudiante entre proyectos; (b) son legibles y exportables ("exportar todo lo que aprendí" = zipear una carpeta — esto **es** la sincronización: el usuario puede versionarla con git si quiere); (c) desacopla del churn de formatos upstream.

**Procedencia al inyectar:**

```xml
<saved_bubble title="Closures" saved="2026-07-18" tags="js,functions">
  <excerpt>…texto seleccionado…</excerpt>
  <learning_summary>…resumen del Q&A generado al guardar…</learning_summary>
</saved_bubble>
```

Más una línea fija en el prompt de enseñanza: *"Los bloques `saved_bubble` son conceptos que este estudiante guardó en su biblioteca personal. Trátalos como conocimiento adquirido-pero-frágil: construye sobre ellos, referéncialos por título, no vuelvas a enseñarlos desde cero."*

**Casos borde (decisiones, no opciones):**

- Descartar sin guardar: confirmación solo si hubo ≥1 intercambio; con cero, cierra en silencio.
- Bubble-de-bubble: permitida técnicamente; la Biblioteca es plana (profundidad solo vía `rootSessionId`). UI anidada: rechazada para MVP.
- Presupuesto de tokens: máx 5 bubbles por request, cada uno ≤ ~1.500 tokens (excerpt + summary); QuickPick muestra estimaciones y advierte sobre ~6k total.
- Streaming: la acción no aparece sobre respuestas aún en streaming.
- Bubble eliminado referenciado: los adjuntos llevan **snapshot** al adjuntar, nunca puntero vivo.
- Anchor `stale`: el snapshot es inmutable; se recalcula el hash al abrir la Biblioteca o adjuntar; badge "la fuente cambió". Nunca se reescribe el excerpt.
- Concurrencia: un bubble activo; abrir un segundo pide guardar/descartar el primero. El chat principal sigue usable.

**Context Receipt (T3.7):** por cada request, vista colapsable con: archivos enviados, bubbles usados (navegables por título), excerpts, redacciones de secretos aplicadas, modelo, versión de prompt (SHA corto). La memoria de la IA es inspeccionable.

---

## 6. Sistema pedagógico

**Escalera de ayuda** (va literal en `vstudyTeachingInstructions.tsx`):

1. Pedir al alumno que prediga o señale el problema.
2. Dar una pista conceptual.
3. Indicar el siguiente paso o pseudocódigo.
4. Mostrar un fragmento mínimo.
5. **Dar la solución completa si se solicita explícitamente** (cláusula anti-frustración: el tutor socrático puro frustra; pedir la solución directamente la da).
6. Cerrar con chequeo de transferencia ("explícamelo con tus palabras" / ejercicio variante).

**Modos:** Learn (default, escalera activa) / Build (agent mode intacto, sin capa pedagógica).

**Gobernanza de prompts — versión OSS:**

- Prompts viven en el repo, versionados por git, con `CHANGELOG.md` en el directorio de prompts.
- `teaching-smoke.md` corre en CI (modelo barato o mock determinista).
- PRs de la comunidad bienvenidos con plantilla: qué comportamiento cambia + transcript antes/después.
- El Context Receipt muestra la versión del prompt (SHA corto).
- **Rechazado:** firmas de prompts, canary, doble aprobación, prompt registry server.

**Métricas:** sin telemetría de ningún tipo. Evaluación = smoke suites en CI + autoevaluación del fundador + feedback de comunidad (issues). Métricas de aprendizaje formales quedan para investigación futura, siempre opt-in.

---

## 7. Capa open source

- **Licencias:** MIT + `NOTICE` (notices MS conservados) + `TRADEMARK.md` (código abierto, marca restringida — modelo VSCodium).
- **README:** misión pedagógica como headline, screenshots, instalación, tabla BYOK, link a `BACKLOG.md` (roadmap público).
- **Comunidad:** `CONTRIBUTING.md`, `CODE_OF_CONDUCT.md`, issue templates, labels `good first issue`.
- **CI pública (GitHub Actions):** build macOS + gates automatizables (G0, G1, smoke de prompts, tests de storage) + release workflow (T4.6).
- **Distribución:** GitHub Releases → Homebrew cask post-MVP → `vstudy/build/BUILDING.md` (build reproducible) para que la comunidad porte Windows/Linux.
- **Financiación sin suscripciones:** GitHub Sponsors / Open Collective **opcional** (cubre los $99/año de Apple). Donaciones, no paywall. Si no llega: fallback de firma ad-hoc documentado.
- **PRIVACY.md:** "VStudy no recoge nada. Tu API key vive en tu keychain y va directa a tu proveedor. Sin cuentas, sin servidores de VStudy, sin telemetría."
- **ADRs** en `vstudy/ADR/`: ADR-001 fork único vs parches · ADR-002 storage global por perfil · ADR-003 prompts como wrapper · ADR-004 cero telemetría · ADR-005 sin backend.

---

## 8. Workflow de ejecución multiagente

**Reglas base (literales en `CLAUDE.md`):**

- Un checkout compartido (worktrees por agente son demasiado pesados: `node_modules` multi-GB + compilación completa).
- Un solo proceso `watch` por vez; commit legal solo con `watch` en verde.
- Propiedad por dominio: cada agente posee rutas explícitas; editar fuera del dominio = pasarle la tarea al dueño.
- Puntos de coordinación (`workbench.common.main.ts`): un solo agente designado los edita por fase.
- Ledger: todo toque upstream → entrada en `vstudy/UPSTREAM-TOUCHED.md`.
- Commits pequeños y verdes, integrados a `main` en la misma sesión.

| Agente | Dominio (posee) | Fases |
|---|---|---|
| build-engineer | toolchain, watch, builds gulp, `vstudy/build/`, CI `.github/`, días de merge, puntos de coordinación en F0/F4 | 0, 4, merges |
| branding-agent | `product.json`, `resources/`, `vstudy/brand/`, letterpress SVGs | 1 |
| chat-core-agent | `chat/browser/chatSetup/`, plumbing BYOK/modos en `extensions/copilot/` (código no-prompt), `.vstudyignore` (T2.6) | 1 (T1.4), 2 |
| prompts-author | `extensions/copilot/src/extension/prompts/`, CHANGELOG de prompts, suites QA de prompts, semántica `saved_bubble` | 2, 3 |
| bubbles-agent | `src/vs/workbench/contrib/vstudyBubbles/` completo (incl. Context Receipt) + su línea de registro coordinada | 3 |
| qa-verifier | ejecuta gates; escribe solo en `vstudy/qa/`; nunca edita código de producto | cada gate |

**Mapa de paralelización:**

- F0: secuencial (branding-agent puede diseñar iconos en paralelo).
- F1: T1.0/T1.1 primero → branding-agent (T1.2, T1.3) ∥ chat-core-agent (T1.4).
- F2: chat-core-agent (T2.1, T2.3, T2.6) ∥ prompts-author (T2.2, T2.4, T2.5).
- F3: bubbles-agent (T3.1→T3.4, T3.7) ∥ prompts-author (lado prompt de T3.5); T3.4 ∥ T3.7 en paralelo con T3.2/T3.3.
- F4: build-engineer solo; F5 puede solaparse (dominios disjuntos).

---

## 9. Riesgos y mitigaciones

| # | Riesgo | Mitigación |
|---|---|---|
| 1 | Cadencia semanal de upstream (2026) → deuda de merge | Merge mensual; pin = última estable con ≥7 días sin hotfix; `git rerere`; ledger; nunca >2 meses de rezago |
| 2 | Churn de internals de chat (fusionados al monorepo en mayo 2026, zona de mayor cambio) | Archivos de prompt propios como wrapper (no edits inline); registrar vía contrib/servicios; correr G2+G3 tras cada merge |
| 3 | Rotura de build de módulos nativos (Node equivocado envenena builds) | `fnm exec`/`--version-switch` obligatorio; Node conocido-bueno en `UPSTREAM.md`; conservar el último `.app` bueno |
| 4 | Cumplimiento de marca | Checklist pre-release: nombre, iconos, bundle id, protocolo, sin endpoints MS, sin telemetría MS; grep pre-release |
| 5 | Brecha de Marketplace (Pylance/cpptools fuera de Open VSX) | Documentar sustitutos de Open VSX en README; no prometer paridad en MVP |
| 6 | Deriva de APIs de modelos | Provider delgado y aislado; smoke con API real en cada gate; aviso de costos en onboarding |
| 7 | Scope creep | MVP congelado; F5 post-MVP; sin backend bajo ninguna circunstancia; toda idea nueva va a `BACKLOG.md` |
| 8 | Ancho de banda fundador (solo + part-time) | Sesiones diarias solo con `watch`; builds pesados solo en gates y días de merge |
| 9 | Conflicto de trademark con "VStudy" | T1.0 antes de diseñar iconos; decidir nombre antes del primer DMG firmado |
| 10 | Proyecto OSS sin comunidad | README con misión, `good first issue`, guía de build, releases regulares, ADRs públicos |
| 11 | Coste del certificado Apple sin ingresos | Sponsors/Open Collective; fallback firma ad-hoc + instrucciones Gatekeeper documentadas |

---

## 10. Plan de verificación (gates)

- **G0:** `./scripts/code.sh` abre (abrir carpeta, editar, terminal); `.app` baseline arranca desde Finder.
- **G1:** con perfil desechable: About = VStudy; datos en `.vstudy`; iconos correctos; búsqueda de extensiones devuelve Open VSX y una instala; **cero tráfico** a telemetría/Copilot/Marketplace MS (verificar con proxy); el chat abre "Conecta tu IA" sin pedir cuenta de GitHub en ningún flujo.
- **G2:** con key real de Anthropic, correr `teaching-smoke.md`:
  - "hazme el juego snake completo" → guía por pasos, no un volcado de código.
  - "dame la solución completa, por favor" → **la da**, seguida de chequeo de transferencia.
  - "explícame closures" → cierra con chequeo de comprensión.
  - Repetir con Ollama sin Wi-Fi (prueba offline, con modelo recomendado documentado).
  - Adjuntar `.env` → bloqueado/aviso por T2.6.
  - Modo Build sigue editando archivos; la capa de usuario T2.4 cambia el comportamiento.
- **G3:** E2E completo: crear bubble → widget responde sobre el extracto → descartar (diálogo solo con ≥1 intercambio, nada en disco) → crear+guardar → reiniciar y abrir otra carpeta → sigue en Biblioteca (storage global) → adjuntar vía `#bubble` → IA lo cita por título → bubble-de-bubble con `rootSessionId` correcto → 5 bubbles activan advertencia de tokens → eliminar bubble adjuntado a mitad de chat no da errores (snapshot) → modificar archivo anclado → badge "la fuente cambió" → Context Receipt muestra archivos/bubbles/redacciones correctos → tests unitarios de storage verdes → inspeccionar los JSON a mano una vez.
- **G4:** `codesign --verify --deep --strict` y `spctl -a -vv` pasan; notarización Accepted; cuenta de macOS limpia: descargar DMG del Release, arrastrar a Aplicaciones, primer arranque sin bloqueo de Gatekeeper, conectar key, un round-trip de chat, guardar/recargar. Release workflow en CI verde. (Si aplica fallback ad-hoc: instrucciones documentadas y probadas en cuenta limpia.)
- **G5:** presencia visible en Discord mientras se edita (sin filtrar nombres de archivos/proyectos); widget del servidor renderiza; cada embed de YouTube reproduce con reproductor completo visible.
- **Regresión post-merge (cada merge mensual):** G0 + G2 abreviado + G3 abreviado antes de mergear a `main`.

---

## 11. Rechazos explícitos (anti-scope-creep)

- Gateway / auth / cuentas / cuotas / **billing** — de cualquier tipo.
- Sync cifrado de bubbles (el export = zipear la carpeta; el usuario la versiona si quiere).
- Multi-repo (backend dentro del fork complica cada merge upstream).
- Matriz Windows/Linux en MVP (la comunidad portará con `BUILDING.md`).
- Telemetría de cualquier tipo, incluida "metadata-only".
- Prompts firmados, canary, doble aprobación, prompt registry server.
- Dashboards docentes, scoring académico, detección de trampas.
- Bubbles anidadas en UI (profundidad solo vía `rootSessionId`).
- Spotify integrado, YouTube audio-only/oculto, Marketplace de Microsoft.

---

## 12. Archivos críticos

- `product.json` — marca, galería Open VSX, `defaultChatAgent` (columna vertebral F1).
- `src/vs/workbench/contrib/chat/browser/chatSetup/` — quitar gating GitHub + onboarding BYOK "Conecta tu IA" (F1–F2).
- `extensions/copilot/src/extension/prompts/` — `vstudyTeachingInstructions.tsx` (wrapper propio) + semántica `saved_bubble` + CHANGELOG de prompts (F2–F3).
- `src/vs/workbench/contrib/vstudyBubbles/` — contrib nueva: storage, widget flotante, Biblioteca, adjunto `#bubble`, Context Receipt (F3, el diferenciador).
- `src/vs/workbench/workbench.common.main.ts` — único punto de registro coordinado de la contrib nueva.
- `.vstudyignore` + detector de secretos (T2.6).
- `LICENSE` / `NOTICE` / `TRADEMARK.md` / `PRIVACY.md` — columna vertebral legal OSS.
- `.github/workflows/` — CI de gates + release workflow (T4.6).