# ADR-002 — Storage de bubbles global a nivel de perfil

**Estado:** aceptada · **Referencia:** PLAN.md §5

## Contexto

Los bubbles son la biblioteca personal del estudiante ("todo lo que aprendí"). Había que decidir dónde persisten: por workspace, en un formato interno de VS Code, o en almacenamiento propio global.

## Decisión

Storage **global a nivel de perfil**, un archivo JSON por bubble:
`~/.vstudy/User/globalStorage/vstudy.bubbles/<id>.json` + `index.json` reconstruible por escaneo.

## Consecuencias

- Los bubbles siguen al estudiante entre proyectos (no quedan atados a un workspace).
- Son legibles y exportables: "exportar todo lo que aprendí" = zipear la carpeta. Eso **es** la sincronización; el usuario puede versionarla con git si quiere (sync cifrado rechazado, PLAN.md §11).
- Desacopla del churn de formatos internos de upstream.
- El índice es reconstruible: perder `index.json` no pierde datos.
- Los adjuntos al chat llevan snapshot, nunca puntero vivo; el excerpt es inmutable aunque el anchor quede `stale`.
