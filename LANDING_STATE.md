# Landing State · Pivot Simulador de Sonrisa

**Fecha:** Julio 2026
**Estado actual:** consola MIRO.DX oculta del landing · placeholder de simulador activo

---

## Contexto del pivot

El análisis IA de detección de patologías (`analyze-dental` v76) presentaba **resultados clínicamente incorrectos** con cierta frecuencia (falsos positivos, hallazgos equivocados). Antes de continuar afinando ese producto, se decidió pivotar el foco del landing hacia un **Simulador de Sonrisa** (before/after estético).

Justificación:
- El análisis de patologías puede generar ansiedad en el visitante que llega a un landing dental
- El simulador de sonrisa es **aspiracional** — muestra el resultado deseado, motiva a agendar consulta
- Está alineado con proyectos existentes del ecosistema: ARMONÍA / Índice Miró / PerfectCorp
- Menor riesgo clínico y legal (no diagnostica, solo proyecta)

---

## Estado del landing (post-pivot)

### ✅ Quitado

- `<iframe>` de la consola MIRO.DX (línea ~1283 pre-pivot)
- CTA "Probar MIRO.DX · 30 días gratis" → cambiado a "Agenda tu evaluación"
- Microcopy "Análisis IA real con tu sonrisa · Sin tarjeta · Resultado 2 min" → cambiado a "Simulador de sonrisa · Próximamente · Evaluación presencial disponible"
- `console-helpers` reformulado hacia agenda presencial + WhatsApp

### ✅ Reemplazado por

Placeholder `.simulator-placeholder` con:
- Icon (estrella bronce animada pulsante)
- Status badge "EN DESARROLLO · Q3 2026"
- Título "MIRO · Simulador de Sonrisa"
- Descripción de la propuesta
- 3 features numeradas del flujo esperado
- CTA WhatsApp "Avísame cuando esté listo"

### ⚠️ Sin tocar (pendiente revisar cuando llegue el simulador)

Estas secciones aún hablan de análisis IA / detección. Pueden revisarse en la próxima iteración:
- Sección `#pipeline` — describe 4 pasos técnicos del análisis (GPT-4o Vision, Gemini)
- Sección `.pipeline-trust` — "Lo que MIRO.DX NO ES" (habla de análisis vs quiz marketing)
- Demo IA del hero (SVG + foto + markers) — muestra detección de patologías
- Counter live "N análisis IA realizados" — cuenta uso del backend (podría seguir siendo relevante)

**Decisión:** dejarlas hasta tener claro qué tono/copy tendrá el simulador. Muchas siguen aplicando si el simulador también usa IA (que probablemente sí).

### ✅ Preservado intacto

- Repo `miro-dx-console` (no eliminado, solo desconectado del landing)
- URL `miro-dx-console.vercel.app` sigue viva y accesible directamente
- Backend `analyze-dental` v76 + `validate-dental-view` + `miro-dx-stats` siguen deployed
- El JS `postMessage` UTM al iframe está preservado (con nota de re-activación)

---

## Cuando llegue el widget del simulador

El widget del simulador debe integrarse en el mismo slot donde estaba la consola (sección `#consola` → placeholder actual). Opciones técnicas conocidas:

- **PerfectCorp API** (CACO tiene ID `356643601268084883`)
- **Banuba SDK** (token en `App.jsx` de otro repo, activo)
- **GPT-4o image gen / Nano Banana** (Google) con prompt de simulación estética
- **HuggingFace** (Face parsing + face manipulation)

Al integrar el widget nuevo, revisar:

1. Reactivar tracking UTM (código preservado en el JS)
2. Revisar sección `#pipeline` para reformular a "cómo funciona el simulador"
3. Revisar demo IA del hero — el actual apunta a caries, el nuevo debería mostrar antes/después
4. Reactivar CTAs específicos ("Simular mi sonrisa" en lugar de "Agenda tu evaluación")

---

## Commits relevantes de este pivot

- Sesión anterior: sprint IA-visible (`fff7380`, `2212604`, `f6b34a0`, `2cef42c`, `a5fbe37`, `31283df`, `32d93c8`)
- Este pivot: (commit actual)

## Referencias

- `BRAND_GUIDELINES.md` — sigue vigente, el simulador debe seguirlas
- `humana-tokens.css` — sigue vigente
- Backend: `https://jipldlklzobiytkvxokf.supabase.co/functions/v1/`
