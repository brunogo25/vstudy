# ADR-005 — Sin backend bajo ninguna circunstancia

**Estado:** aceptada · **Referencia:** PLAN.md §7 (también §0, §1 y §11)

## Contexto

Desde 2026-05-20 el stack completo de Copilot Chat (UI + agente + system prompts + proveedores BYOK) está en el monorepo `microsoft/vscode` bajo MIT, con BYOK funcionando sin login de GitHub (incluye Anthropic first-party y Ollama offline). Eso elimina la necesidad técnica de cualquier gateway. VStudy además no tiene empresa ni ingresos: cada servidor sería coste, superficie de fallo y obligación de privacidad.

## Decisión

**Sin backend, sin cuentas, sin suscripciones.** BYOK puro: keys de Anthropic/OpenAI en el secret storage del SO, u Ollama local. Todos los datos de aprendizaje viven en disco del usuario y son exportables. Financiación opcional solo vía donaciones (GitHub Sponsors / Open Collective), nunca paywall.

## Consecuencias

- La capa pedagógica se inyecta en prompt-tsx dentro del fork: visible y auditable en el repo.
- Gateway/auth/cuentas/cuotas/billing y multi-repo con backend: rechazos explícitos (PLAN.md §11).
- Sin cuentas ni menores ni scoring: basta `PRIVACY.md`, sin programa RGPD formal.
- Si las donaciones no cubren los $99/año de Apple: fallback documentado de firma ad-hoc + instrucciones Gatekeeper.
