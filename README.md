# Clínica Miró — Landing MIRO.DX

Landing single-file con la consola MIRO.DX embebida como widget (iframe).

## Estructura
```
clinicamiro-landing/
├── index.html         # Landing completo, single-file
├── vercel.json        # Headers de seguridad y permisos cámara/mic para el iframe
└── README.md
```

## Stack y decisiones
- **HTML estático single-file.** Sin frameworks, sin build step. Carga rápida, deploy instantáneo.
- **Skin Golden Bronze** aplicado: `#090B0B` / `#1C2121` / `#FFF` / `#F5F5F5` / `#D3A436` (bronce solo en CTAs y acentos).
- **Tipografía:** Montserrat 900 display + Outfit body (vía Google Fonts).
- **Logo:** SVG inline con la M angular en bronce (sin separador vertical en versión nav, con separador en versión completa).
- **Consola embebida:** iframe a `https://miro-dx-console.vercel.app/` con `allow="camera; microphone; autoplay"`.
- **UTMs:** capturados del query string del landing y propagados al iframe vía `postMessage`. La consola tiene listener para `miro_utm`.
- **Tracking:** evento `visit` se envía a la Edge Function `miro-dx-track` cuando el usuario llega a la sección de la consola por primera vez.

## Despliegue en Vercel

```bash
# Desde la carpeta clinicamiro-landing/
vercel --prod
```

Configurar dominio custom: `clinicamiro.cl` (o `landing.clinicamiro.cl` si quieres validar antes de switch).

## Configuración paralela necesaria

### 1. Supabase — tablas y Edge Function

Aplicar el SQL de `/supabase/sql/001_miro_dx_tables.sql`:
```bash
SUPABASE_ACCESS_TOKEN=<tu_token> supabase db push --project-ref jipldlklzobiytkvxokf
# o ejecutar el SQL directamente en el panel de Supabase
```

Desplegar la Edge Function:
```bash
SUPABASE_ACCESS_TOKEN=<tu_token> supabase functions deploy miro-dx-track --project-ref jipldlklzobiytkvxokf
```

### 2. Consola `miro-dx-console`

El repo `CACO1972/miro-dx-console` ya contiene los cambios:
- 4 campos obligatorios (nombre + RUT + email + WhatsApp)
- Flow.cl arrancado completo
- Banner "GRATIS · Piloto 30 días" en el bloque final
- Matching inteligente Motivo → Especialista preservado
- Listener `postMessage` para recibir UTMs del landing

```bash
cd miro-dx-console
git push origin main      # Vercel auto-deploya
```

## Métricas

Vista operacional `miro_dx_funnel` (Supabase):
```sql
select * from miro_dx_funnel where dia >= now() - interval '7 days';
```

Devuelve por día y fuente UTM:
- Visitas únicas
- Leads capturados (datos completos)
- Análisis completados
- Clicks en cada CTA (Dentalink / WhatsApp / Asistencia humana)
- Conversión visita→lead (%)

## URLs en producción

| Pieza | URL |
|---|---|
| Landing | `https://clinicamiro.cl/` |
| Consola embebida (iframe) | `https://miro-dx-console.vercel.app/` |
| Edge Function tracking | `https://jipldlklzobiytkvxokf.supabase.co/functions/v1/miro-dx-track` |
| Dentalink genérico | `https://ff.healthatom.io/41knMr` |
| WhatsApp principal | `+56974157966` |

## Notas

- El landing y la consola viven en proyectos Vercel separados pero se ven como una sola experiencia gracias al iframe.
- Si en el futuro quieres subir la consola al subdominio `consola.clinicamiro.cl`, basta con configurar el custom domain en el proyecto Vercel `miro-dx-console` y cambiar la URL del iframe en este landing.
