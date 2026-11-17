# fisiocorb3

Web de Fisio Vall del Corb: Astro 7, Tailwind 4, estática, desplegada en Cloudflare (Workers Static Assets). Bilingüe ca/es.

Guía para editar contenidos (para el cliente): **[LLEGEIX-ME.md](LLEGEIX-ME.md)**.

## Estructura

| Ruta | Qué es |
|---|---|
| `textos.json` | **Todos** los textos ca/es + rutas + datos del negocio. Única fuente de i18n. |
| `src/i18n/index.ts` | Lector de `textos.json` (`useT(lang)` → `t`, `h`, `hf`, `p`, `raw`, `r`) y mini-marcado. |
| `src/pages/[...path].astro` | Ruta única: genera todas las páginas desde `rutes` + colección `blog`. |
| `src/lib/pagines.ts` | Registro clave de ruta → vista (`src/views/*.astro`). |
| `src/content/blog/{ca,es}/*.md` | Artículos (URL = nombre del fichero, igual que en WordPress). |
| `src/content/legal/{ca,es}/*.md` | Textos legales. |
| `public/_redirects` | 301 de la web antigua (formato Cloudflare). |
| `scripts/check-redirects.mjs` | Se ejecuta tras `astro build`: cada URL de `scripts/urls-web-antiga.txt` debe ser página o 301 válido. |

Catalán sin prefijo (por defecto, `x-default`), castellano bajo `/es/`. hreflang en todas las páginas; los artículos sin traducción solo declaran su propio idioma.

## Añadir una página

1. Ruta en `textos.json → rutes` (`{ "ca": "/…/", "es": "/es/…/" }`).
2. Textos en `textos.json → <clave>`.
3. Vista en `src/views/` (props `lang`, `alternates`) y registro en `src/lib/pagines.ts`.

## Comandos (pnpm)

```bash
pnpm install
pnpm dev
pnpm build        # astro build + comprobación de redirecciones
pnpm preview      # wrangler dev sobre dist/
pnpm run deploy   # wrangler deploy
```

Efectos: todos respetan `prefers-reduced-motion`. Sin cookies ni scripts de terceros.
