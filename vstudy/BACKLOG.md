# BACKLOG.md — Roadmap público y parking lot

Toda idea nueva aterriza aquí, no en el MVP (anti scope-creep, PLAN.md §9 riesgo 7). El MVP está congelado: marca + chat de enseñanza (Learn/Build) + Bubbles + Biblioteca + Context Receipt + `.vstudyignore`.

## Post-MVP (Fase 5)

- **Discord Rich Presence** (T5.1): extensión `vscord` en `builtInExtensions`, opt-in, sin publicar nombres de archivos/proyectos/ejercicios.
- **Panel "Home"** (T5.2): webview con widget oficial de servidor Discord + botón de invitación. Sin bot, sin backend.
- **Música lo-fi** (T5.3): webview con streams lo-fi de YouTube con **reproductor completo visible** (línea dura: nada de audio-only ni video oculto), y/o play/pause del Spotify local vía AppleScript detrás de un setting (sin API de Spotify).

## Ideas aparcadas

- Homebrew cask para distribución post-MVP.
- Auto-update **sin servidor propio** (p. ej. chequeo del feed público de GitHub Releases desde el editor; v1 se distribuye solo por GitHub Releases). Un servidor de updates propio queda descartado por la línea dura "sin backend" (PLAN.md §1 y §11).
- Windows/Linux por comunidad, con `vstudy/build/BUILDING.md` como guía de build reproducible.
- Export enriquecido de bubbles (formatos adicionales; hoy el export = zipear la carpeta de bubbles).

## Rechazado (ver PLAN.md §11)

Estas son líneas duras, no ideas pendientes de debate:

- Gateway / auth / cuentas / cuotas / billing — de cualquier tipo.
- Sync cifrado de bubbles (el export = zipear la carpeta; el usuario la versiona con git si quiere).
- Multi-repo (un backend dentro del fork complica cada merge upstream).
- Matriz Windows/Linux en el MVP (la comunidad portará con `BUILDING.md`).
- Telemetría de cualquier tipo, incluida "metadata-only".
- Prompts firmados, canary, doble aprobación, prompt registry server.
- Dashboards docentes, scoring académico, detección de trampas.
- Bubbles anidadas en UI (profundidad solo vía `rootSessionId`).
- Spotify integrado, YouTube audio-only/oculto, Marketplace de Microsoft.
