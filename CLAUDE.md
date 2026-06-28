# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Static HTML/template files for **오에이존 (OA Zone)**, a Korean printer/copier/ink e-commerce shop (www.ipctm.com) running on the **MakeShop** platform. No build system, framework, or package manager.

## File categories

**Root-level design files** — standalone, self-contained HTML with full `<head>` blocks. Opened directly in a browser for design iteration.

| File | Purpose |
|---|---|
| `index.html` | Original legacy site — table-based layout, 굴림 font, early-2000s style. Source of content/structure reference. |
| `index-new.html` | Modern redesign v1 — sidebar + main layout, blue (`#2563eb`) primary, Noto Sans KR, CSS custom properties. |
| `index-new2.html` | Modern redesign v2 — orange (`#f97316`) primary, dark header, full-width layout. |
| `shopbrand-008.html` | Product listing page redesign — standalone modern layout. |

**MakeShop template files** — fragments without `<html>`/`<head>`. Uploaded to MakeShop admin and rendered server-side. Organized by section in Korean-named directories.

| Directory | MakeShop section |
|---|---|
| `메인/` | Main page (`main.html`) |
| `상단/` | Site header (`header.1.html`) |
| `하단/` | Site footer (`footer.1.html`) |
| `카테고리/` | Category listing page |
| `상품관련/` | Product pages (detail, brand, search, reviews, promotions) |
| `개별페이지/`, `공지사항/`, `마이페이지/`, `주문관련/`, `회원관련/`, `커뮤니티/`, `파워리뷰/`, `이벤트팝업/`, `모바일전용팝업/` | Other shop sections |

## Development

Open root-level design files directly in a browser — no server or build step required:
```bash
open index-new.html   # macOS
```

MakeShop template files must be uploaded via the MakeShop admin panel to preview with real data.

## MakeShop template syntax

Templates use MakeShop's proprietary server-side tag syntax:

```html
<!--/include_header(1)/-->          <!-- include another template -->
<!--/shop_name/-->                  <!-- scalar variable -->
<!--/if_login/-->...<!--/end_if/--> <!-- conditional block -->
<!--/loop_category1/-->...<!--/end_loop/--> <!-- loop -->
<!--/block_event_banner/-->...<!--/end_block/--> <!-- optional block -->
<!--/category1@link/-->             <!-- loop variable attribute -->
<!--/makeshop{product_list_horizontal(...)}/-->  <!-- built-in widget -->
```

Templates reference MakeShop JS via absolute paths (`/js/...`) and jQuery is provided by the platform.

## Architecture notes

- All CSS lives in `<style>` blocks; no external stylesheets.
- All JavaScript is inline `<script>` at the bottom of each file.
- Images load from the live production server (`http://www.ipctm.com/shopimages/...`) — no local assets.
- CSS custom properties (`--primary`, `--radius`, `--shadow`, etc.) in `:root` control the entire theme of the design files.
- `index-new.html` uses a sticky sidebar with JS-driven accordion (`sb-toggle`/`sb-submenu`) and a mobile overlay drawer.
- The `<base href="http://www.ipctm.com/">` tag in `index.html` causes all relative links to resolve against the production domain.
- `sitemap.xml` lists the shop's URL structure and can be used as a reference for page types.
