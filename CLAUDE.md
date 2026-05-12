# Kura Holdings website

Static Astro site deployed via GitHub Pages (workflow in `.github/workflows/deploy.yml`).

## Design tokens — non-negotiable

All colors and fonts MUST come from CSS custom properties defined in `src/styles/global.css`. Never hardcode hex values or alternate font families.

**Palette** (use only these, via `var(--token)`):
- `--ink` `#023047` — primary dark / background
- `--steel` `#219ebc`
- `--sky` `#8ecae6` — muted text, links
- `--ember` `#fb8500` — accent / CTA
- `--sun` `#ffb703` — CTA hover

**Typography:** Figtree only, via `var(--font-sans)`. Loaded in `src/layouts/Base.astro`. Do not introduce additional font-families or Google Fonts imports.

**Rules:**
- No raw hex values in `.astro`, `.css`, or component files. The only file permitted to contain hex literals is `src/styles/global.css`.
- No `rgb()`/`rgba()` for brand colors — use the tokens. `rgba()` is acceptable only for derived effects (shadows, translucent overlays) already patterned in `global.css`.
- No `font-family` declarations outside `global.css`.
- If a new color or weight is genuinely needed, propose adding it to `global.css` first — don't inline it.

## Stack notes

- Astro static site, no framework components
- Components live in `src/components/`, single layout in `src/layouts/Base.astro`
- Global styles only — no CSS modules, no Tailwind
