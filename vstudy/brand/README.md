# vstudy/brand/ — SVGs fuente de los iconos de VStudy

## Archivos

- `vstudy-icon.svg` — **master del icono de app** (1024×1024, full-bleed): "V" geométrica cuyo contraespacio contiene un libro abierto (los trazos gruesos son las tapas, los chevrons claros las páginas). Dos tonos índigo `#4F46E5` / `#818CF8` sobre pizarra `#1E1B2E`, cuadrado redondeado r=232.

## Derivados generados desde este master

- `resources/darwin/code.icns` — icns de macOS. Para regenerar: envolver el master en `<g transform="translate(100 100) scale(0.8046875)">` (rejilla de iconos de Apple: squircle de 824px centrado en lienzo de 1024 con margen transparente), renderizar con `/opt/homebrew/bin/rsvg-convert` a PNG en tamaños 16/32/64/128/256/512/1024 con nombres `icon_16x16.png`, `icon_16x16@2x.png` … `icon_512x512@2x.png` dentro de `vstudy.iconset/`, y empaquetar con `/usr/bin/iconutil -c icns vstudy.iconset -o code.icns`. Trabajar en un dir temporal bajo `vstudy/brand/` y borrarlo al terminar.
- `src/vs/workbench/browser/parts/editor/media/letterpress-{dark,light,hcDark,hcLight}.svg` — versión outline monocroma de la marca (rejilla 260×260, trazo 11). Cada variante conserva el esquema de color del original de upstream: dark = negro con `opacity="0.3"`, light = negro con `opacity="0.1"`, hcDark = `#3C3C3C`, hcLight = `#D9D9D9`.

## PENDIENTE pre-release

Iconos de `resources/` que aún llevan branding de VS Code (ver auditoría T1.2): icns de tipos de archivo en `darwin/`, `.ico` de win32, favicons/PNGs del server, `linux/code.png`. No bloquean el MVP macOS-only pero deben reemplazarse antes de cualquier release.
