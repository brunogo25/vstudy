# ADR-004 — Cero telemetría (eliminada, no apagada)

**Estado:** aceptada · **Referencia:** PLAN.md §1 (también §2, §6 y §11)

## Contexto

VS Code trae telemetría hacia endpoints de Microsoft. Un fork puede dejarla "off por defecto", pero eso deja el código activo, reactivable y difícil de auditar. VStudy es un proyecto sin backend, sin cuentas y local-first, con compromiso público de privacidad.

## Decisión

**Cero telemetría: eliminada/neutralizada**, no solo desactivada. Sin excepciones, incluida la variante "metadata-only" (rechazo explícito, PLAN.md §11). Compromiso público en `PRIVACY.md`.

## Consecuencias

- T1.3 elimina/neutraliza la telemetría y se verifica con proxy: **cero tráfico** a endpoints de Microsoft (gate G1, checklist pre-release del riesgo 4).
- Sin métricas de uso: la evaluación del producto = smoke suites en CI + autoevaluación del fundador + feedback de comunidad vía issues.
- Métricas de aprendizaje formales quedan para investigación futura, siempre opt-in.
- `PRIVACY.md` puede ser simple y veraz: "VStudy no recoge nada; tu key va directa a tu proveedor".
