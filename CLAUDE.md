# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Static HTML/template files for **오에이존 (OA Zone)**, a Korean printer/copier/ink e-commerce shop (www.ipctm.com) running on the **MakeShop** platform. No build system, framework, or package manager.

## Repository structure

```
desktop/                  # Active MakeShop template files (desktop only — ignore mobile/)
  common.css              # Global base CSS: reset, design tokens, product grid, paging, layout
  상단/                   # Site header
    header.1.html/.css    # Active header (new design — same as header.2)
    header.2.html/.css    # Reference copy (identical to header.1)
  하단/                   # Site footer
    footer.1.html/.css    # Active footer (new design — same as footer.2)
    footer.2.html/.css    # Reference copy (identical to footer.1)
  메인/                   # Main page (main.html + main.css)
  측면/                   # Side menus (menu.1, menu.2)
  부가기능/               # Platform widgets (scroll, shopping_tab.1)
  상품관련/               # Product pages
    brand.html/.css           # Brand listing
    shopsearch.html/.css      # Search results
    review_list.html/.css     # All reviews
    product_preview.html/.css # Quick-view popup widget
    pop_soldout_alarm.html/.css # Restock alarm popup
    pop_shopdetail.html/.css  # (platform popup)
    상품분류페이지/           # shopbrand — product category listing
    상품상세페이지/           # shopdetail — product detail + shopdetail_addinfo (고시 정보)
    기획전관련/기본기획전/    # plan_list, plan.1 — special promotion pages
    상품프로모션/             # best_cate, best_product, best_review, bigmatch
    사은품관련/               # gift_list, gift_choice, pop_gift_choice
    도매사입업체별화면/       # supply — wholesale vendor page
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

---

## New design system (전체 리디자인 완료)

All `desktop/` pages have been fully redesigned. The old Nanum Gothic / absolute-positioned / table-layout / GIF-button design has been replaced with the following system.

### Design tokens (`common.css` `:root`)

```css
--primary: #2563eb
--primary-dark: #1d4ed8
--primary-light: #eff6ff
--dark: #1e293b
--gray: #64748b
--gray-light: #e2e8f0
--bg: #f8fafc
--white: #ffffff
--radius: 12px
--sidebar-w: 240px
--max-w: 1200px
```

### Typography

- **Font**: Noto Sans KR (Google Fonts, 400/500/600/700) — replaces Nanum Gothic
- **Base size**: 14px (was 12px)
- Loaded via `@import` in `common.css`, `header.1.css`, `footer.1.css`

### Component class prefix

All new component classes use the `oaz-` prefix to avoid MakeShop platform conflicts.

### Page layout pattern

```html
<div id="contentWrapper">     <!-- max-width: 1200px, centered -->
  <div id="contentWrap">      <!-- flexbox row, gap: 24px -->
    <!--/include_menu(1)/-->  <!-- #aside: 240px sidebar -->
    <div id="content">        <!-- flex: 1; min-width: 0 -->
      ...
    </div>
  </div>
</div>
```

### Product grid

`common.css` provides `.oaz-prd-grid` (CSS Grid, replaces all `<table>`-based product lists):

```css
.oaz-prd-grid                /* 4-column default */
.oaz-prd-grid-3              /* 3-column */
.oaz-prd-grid-5              /* 5-column */
.oaz-prd-grid-2              /* 2-column */
```

**Important**: MakeShop renders product images with class `MS_prod_img_m`, not `oaz-prd-img`. The image sizing rule in `common.css` targets `.oaz-prd-img-wrap img` (the link wrapper) to catch the platform-injected class:

```css
.oaz-prd-img-wrap { display: block; aspect-ratio: 1; overflow: hidden; background: #f8fafc; }
.oaz-prd-img-wrap img { width: 100%; height: 100%; object-fit: cover; }
```

**Loop pattern** (no `<!--/if_idx/-->` row breaks — CSS Grid handles columns):

```html
<div class="oaz-prd-grid oaz-prd-grid-4">
  <!--/loop_product/-->
  <div class="oaz-prd-card">
    <a href="<!--/product@link/-->" class="oaz-prd-img-wrap">
      <img src="<!--/product@image/-->" alt="<!--/product@name/-->">
    </a>
    <div class="oaz-prd-info">
      <p class="oaz-prd-name"><a href="<!--/product@link/-->"><!--/product@name/--></a></p>
      <p class="oaz-prd-price"><strong class="oaz-price-sell"><!--/product@price_sell/--></strong></p>
    </div>
  </div>
  <!--/end_loop/-->
</div>
```

### Board (게시판) table pattern

`<thead>` is placed **outside** the MakeShop loop so it renders once; only `<tbody>` rows are inside the loop. The expand/collapse row uses both `.oaz-board-cnt` (new styling) and `.cnt` (kept for MakeShop platform JS compatibility):

```html
<table class="oaz-board-table">
  <thead><tr><th>...</th></tr></thead>
  <tbody>
    <!--/loop_review_board/-->
    <tr class="oaz-board-row"><td>...</td></tr>
    <tr class="oaz-board-cnt cnt" id="<!--/review_board@id_content/-->">
      <td colspan="...">...</td>
    </tr>
    <!--/end_loop/-->
  </tbody>
</table>
```

### MakeShop option widget compatibility

The platform injects `#MK_innerOptWrap`, `#MK_optAddList`, `#MK_innerOptTotal` etc. into the product detail form. These IDs must be preserved in `shopdetail.css` — do not rename or remove them.

### Size chart popup

The platform JS identifies the size chart popup by `.size-chart-box` and `.btn-close-layer`. Always keep these classes alongside any new `oaz-*` classes:

```html
<div id="sizeChart" class="oaz-size-chart-layer size-chart-box">
  ...
  <a href="#" class="oaz-scl-close btn-close-layer">✕</a>
</div>
```

### Payment button IDs

MakeShop injects payment buttons by ID — do not rename: `#nhn_btn`, `#payco_order_btn`, `#kakaopay_order_btn`.

### Header / GNB

- Sticky white header (`position: sticky; top: 0; z-index: 100`)
- GNB bar (`background: #2563eb`) uses static HTML menu items grouped into 6 main categories (`잉크/충전기`, `프린터/복합기`, `토너/카트리지`, `공급기/부품`, `용지/PC소모품`, `중고/기타`) to improve readability and prevent SEO link breakage from backend URL changes.
- Large GNB dropdowns support multi-column layouts (`.oaz-dropdown-wide`, `.oaz-dropdown-group`, `.oaz-group-title`).
- Rightmost 2 dropdowns are right-aligned (`.oaz-nav-item:nth-last-child(-n+2) .oaz-dropdown { left: auto; right: 0; }`) to prevent horizontal screen overflow.
- Search input uses `.MS_search_word` class (platform-injected)
- Cart count and parentheses are wrapped in `.oaz-cart-text` with `display: inline-flex; align-items: center; vertical-align: middle;` to fix baseline vertical alignment.

### Footer

- Dark background (`background: #1e293b`)
- Top link bar (`background: #0f172a`)
- Uses MakeShop scalar variables: `<!--/company_name/-->`, `<!--/company_owner/-->`, `<!--/company_addr/-->`, `<!--/shop_tel/-->`, `<!--/company_number/-->`, `<!--/online_sale_number/-->`, `<!--/privacy_charge/-->`, `<!--/shop_email/-->`
- **Crucial**: Footer CSS (`footer.1.css`) is merged into `desktop/common.css` because MakeShop does not reliably load the standalone footer CSS file on inner subpages (e.g. `shopdetail`, `shopbrand`). Always maintain footer styling in `common.css`.

### VS Code Integration

- Autocomplete snippets for 620+ MakeShop tags are configured in `.vscode/makeshop.code-snippets` with Korean descriptions, loop/if/block auto-closing templates, and `[CODE]` placeholders.

