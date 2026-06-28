# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Static HTML/template files for **오에이존 (OA Zone)**, a Korean printer/copier/ink e-commerce shop (www.ipctm.com) running on the **MakeShop** platform. No build system, framework, or package manager.

## Repository structure

```
desktop/          # Active MakeShop template files (desktop only — ignore mobile/)
  common.css      # Shared CSS reset and utility classes (Nanum Gothic, 12px base)
  상단/           # Site header (header.1.html + header.1.css)
  하단/           # Site footer (footer.1.html + footer.1.css)
  메인/           # Main page (main.html + main.css)
  측면/           # Side menus (menu.1, menu.2)
  부가기능/       # Platform widgets (scroll, shopping_tab.1)
  상품관련/       # Product pages — brand, shopsearch, reviews, popups, promotions
    상품분류페이지/   # shopbrand — product category listing
    상품상세페이지/   # shopdetail — product detail page
    기획전관련/       # plan_list, plan.1 — special promotion pages
    상품프로모션/     # best_cate, best_product, best_review, bigmatch
    사은품관련/       # gift_list, gift_choice, pop_gift_choice
    도매사입업체별화면/ # supply — wholesale vendor page
  개별페이지/     # (empty — not yet implemented)
  공지사항/       # (empty)
  마이페이지/     # (empty)
  주문관련/       # (empty)
  회원관련/       # (empty)
  이벤트팝업/     # (empty)
  파워리뷰/       # (empty)
  상품문의/       # (empty)
backup_oldfiles/  # Old standalone design mockups (reference only, not deployed)
sitemap.xml       # Shop URL structure reference
```

## Template file conventions

Each MakeShop section has a paired `.html` + `.css` file in the same directory. The CSS file is uploaded separately to MakeShop; it is **not** linked from the HTML fragment. `desktop/common.css` is a global base uploaded once to the platform.

HTML fragments do **not** have `<html>`/`<head>` — they are server-rendered partials. The main page template (`메인/main.html`) is the only one that includes the page wrapper (`<div id="wrap">`) and uses `<!--/include_header(1)/-->` to pull in the header.

## Development

There is no build step. Template files are uploaded directly to MakeShop admin to preview with real data.

The `backup_oldfiles/` design mockups can be opened locally in a browser for design reference:
```bash
open backup_oldfiles/index-new.html   # macOS
```

## MakeShop template syntax

```html
<!--/include_header(1)/-->              <!-- include another template -->
<!--/shop_name/-->                      <!-- scalar variable -->
<!--/if_login/-->...<!--/end_if/-->     <!-- conditional block -->
<!--/else/-->                           <!-- else branch inside if -->
<!--/loop_category1/-->...<!--/end_loop/--> <!-- loop -->
<!--/block_event_banner/-->...<!--/end_block/--> <!-- optional block -->
<!--/category1@link/-->                 <!-- attribute on a loop variable -->
<!--/notice@subject(34,...)/-->         <!-- loop variable with truncation param -->
<!--/makeshop{product_list_horizontal(...)}/-->  <!-- built-in widget -->
<!--/script_scroll(1)/-->               <!-- platform JS include (scroll widget) -->
```

jQuery is provided by the platform. Template JS references MakeShop assets via absolute paths (`/js/...`, `/design/dpcomputer/9607/makeshop/...`).

## Architecture notes

- Each template's CSS is self-contained in its own `.css` file; no external stylesheet links inside HTML fragments.
- JavaScript is inline `<script>` at the bottom of each HTML file.
- Images reference the platform's design path: `/design/dpcomputer/9607/makeshop/...` — no local assets.
- `sitemap.xml` lists the shop's URL structure and is the authoritative reference for page types.
- The `mobile/` directory contains mobile-specific templates — ignore it unless explicitly asked about mobile.
