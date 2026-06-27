# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Static HTML website for **오에이존 (OA Zone)**, a Korean printer/copier/ink e-commerce shop (www.ipctm.com). No build system, framework, or package manager — all files are plain HTML with inline CSS and JavaScript.

## Files

| File | Purpose |
|---|---|
| `index.html` | Original legacy site — table-based layout, 굴림 font, early-2000s style. Treated as the source of content/structure to preserve. |
| `index-new.html` | Modern redesign v1 — sidebar + main layout, blue (`#2563eb`) primary, Noto Sans KR, CSS custom properties. |
| `index-new2.html` | Modern redesign v2 — orange (`#f97316`) primary, dark header, full-width layout. |

## Development

Open any `.html` file directly in a browser — no server or build step required.

To preview changes:
```bash
open index-new.html   # macOS
```

## Architecture notes

- All CSS is in `<style>` blocks within each HTML file; there are no external stylesheets.
- All JavaScript is inline `<script>` at the bottom of each file.
- Images are loaded from the live production server (`http://www.ipctm.com/shopimages/...`) — no local assets.
- `index-new.html` uses a sticky sidebar with JS-driven accordion (`sb-toggle`/`sb-submenu`) and a mobile overlay drawer.
- CSS custom properties (`--primary`, `--radius`, `--shadow`, etc.) are defined in `:root` and control the entire theme — change them to retheme globally.
- The `<base href="http://www.ipctm.com/">` tag in `index.html` causes all relative links to resolve against the production domain.
