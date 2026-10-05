# Wedding Website — Trọng Vũ & Hồng Nhung

## Project Overview

Single-page wedding invitation (wedding 29/11/2026, ceremony 11:00) built from the miuwedding.com
runtime export (`wedding2.html` → `index.html`). Deployed on GitHub Pages under the existing domain
`vunhungwedding.online` on the `master-wedding2` branch. The old site (vu-nhung) remains on the
`master` branch.

## Tech Stack

- **Frontend**: HTML + miu runtime (absolute canvas, nodes with `data-node-id`), Tailwind CSS CDN,
  WOW.js, Animate.css, Google Fonts.
- **Backend**: Firebase Firestore (compat SDK 10.7.1 via `https://www.gstatic.com/firebasejs/10.7.1/...`).
- **No build tool**: single plain HTML file.

## Firebase Config (in `index.html`, block after `<body>`)

- **Project ID**: `vu-nhung-wedding` (reuses the old site's project).
- **Firestore**: `vu_nhung_2_messages` (souvenir/guestbook). The RSVP form was removed in Session 15.
- Firestore rules in test mode (open read/write) are sufficient.
- If you add new collections that require composite indexes, create them in the console.

## Data Structure

### Collection `vu_nhung_2_messages` (souvenir)
```json
{ "fullname": "...", "comment": "...", "createdAt": Timestamp }
```
- Real-time read: `orderBy('createdAt', 'desc')`, limit 200, empty values filtered in JS.
- No composite index needed (single-field query).

### Guestbook list — a fixed-height scroll box (Session 37)

The list is **not** paginated. All messages from the query (max 200) are rendered at once into a
fixed-height box; the "Xem thêm lời chúc ↓" button and the whole `showN` / `initial` /
`data-miu-wishes-more` machinery were **deleted**. The idea was ported from the old vs-template-5
site on branch `master` (`#guestbook-list { max-height:500px; overflow-y:auto }` +
`::-webkit-scrollbar{width:4px}`); only the scroll behaviour and the date stamp were copied — the
item styling stayed gray `#707070`.

- Geometry is **not** set in CSS. The list is a flex child (`flex:1 1 auto; min-height:0`) of
  `.miu-wishes-inner` inside `element_wishes_6hr88wywupk` (727.23px tall), so the box simply fills
  whatever height is left: measured **300.5px on screen at a 575px viewport** (canvas ~336px).
  Do not add `max-height` — to make it taller you must grow the section, and the next node starts at
  `top:8590.31` (162px of headroom).
- **`overflow` lives in the node's inline `style`** (`overflow-y:auto; overflow-x:hidden`), not in
  the stylesheet: a longhand in a stylesheet loses to the shorthand already inline. `overflow-x:hidden`
  plus `word-break:break-word` on `.miu-wishes-comment` is what keeps one unbreakable 220-char
  message from producing a horizontal scrollbar.
- **Scrollbar styling is two-track, and the `@supports` guard is mandatory.** Chrome ≥121 supports
  `scrollbar-color`; if `scrollbar-width`/`scrollbar-color` are written unconditionally, Chrome
  **discards the whole `::-webkit-scrollbar` block** and uses `thin` (~11px), losing the 4px that
  was chosen. `@supports not selector(::-webkit-scrollbar)` is true only on Firefox, so WebKit
  engines keep the exact 4px and Firefox gets a `thin` standard bar.
- **Traps when verifying the 4px:** Windows 11 uses overlay scrollbars, so
  `listEl.clientWidth - listEl.offsetWidth` is **0 even for an unstyled `overflow-y:scroll` div** —
  that assertion cannot pass on this machine and would fail for any implementation. Assert the
  CSSOM rules instead (`doortest\wishes.js` W8a–W8c). And the live collection holds too few messages
  to overflow, so `wishes.js` injects 60 synthetic items, measures, then restores `innerHTML`.
- Item markup is `<.miu-wishes-item><.miu-wishes-head><.miu-wishes-name><.miu-wishes-date>` —
  `.miu-wishes-head` is `display:flex; align-items:baseline`. Because the two font sizes differ
  (20px name / 15px date) their **box tops differ by 5px on purpose**; a correct assertion tests
  vertical *overlap*, not equal tops. The date is preformatted once per snapshot into `items[].at`
  by `fmtDate()` (port of the old site's `formatGuestDate`, `dd/MM/yyyy HH:mm`, local time).
- `.miu-wishes-head` / `.miu-wishes-date` are **new classes** — a re-export from `wedding2.html`
  loses them. `data-initial-limit="3"` and `data-loadmore-text` are still on the section but no
  script reads them.
- The old site's guestbook (collection **`guests`**, fields `name`/`message`, italic Cormorant, date
  on every item) is **still on branch `master` only** — not reachable from this site and not merged.
  To show those messages too you would need a second query and a field rename, then merge both
  sources by `createdAt`.

## Editing Content

The HTML is a huge single-line canvas (the body is minified). Edit with regex anchored on
`data-node-id`. Key nodes (inner text after `">`...`</div>`):

- Couple headlines (hero): `element_text_ghi89lrdzxt` (Trọng Vũ, 65px), `element_text_58ymmmme3xn`
  (Hồng Nhung, `font-size:48px`). **Session 36:** both are now **centred on the canvas**
  (`left:0px; width:575px; text-align:center`) instead of the old `left:180px` + shrink-to-fit box,
  and `data-anim-duration="2400"` (Session 35; was 3000). Presets `fadeInLeft` / `fadeInRight`,
  delay 0, distance 200. Centring via `text-align` rather than `left = 287.5 - w/2` so it stays
  correct if the font fallback (Ergisa → Brush Script MT) has different metrics.
  `element_text_w6mjrszfugn` (THE WEDDING OF, 3000ms) was nudged `133.952 → 136.3125` so its box
  centre is exactly 287.5. Measured ink boxes (canvas px): Trọng Vũ 110.1→464.9, Hồng Nhung
  112.7→462.3 — the two free gutters are 110.1px each.
- Initials (two separate, slightly interlocked): `element_text_se9nr38shaq` (V, Playfair Display 75px,
  right-aligned, left 28.14), `element_text_prol2tcxgdc` (N, UVN Hoa Tay 70px, left-aligned, left 242
  = template position −20px so the N tucks over the V's right stroke for a romantic closeness; both
  gray `#707070`, uppercase, fadeInDown, above `floral-pattern.png`).
- Venues (2 per event card, **written at runtime by master data** — see "Path routing / master data"):
  | Card | Tên (25px, `venueName`) | Địa chỉ (18px, `address`, has `<br>`) | Nút map |
  |---|---|---|---|
  | 1 | `element_text_69ocrv4ssvy` | `element_text_7e3dpnntik0` | `element_button_9ybr5mpump5` |
  | 2 | `element_text_ktl76jg0hat` | `element_text_w1cs2vj0hat` | `element_button_iog0sqv0hat` |

  All four text nodes have `text-transform: uppercase` → store normal case in the data.
  The "ĐỊA ĐIỂM" labels (`ouzs7rzrxaw`, `4fsw9gt0hat`) are static and always read "ĐỊA ĐIỂM".
  The two `<a>` map buttons are real anchors with `data-miu-btn="1"`; `applyMasterData` swaps
  their `href`. **Both used to point at the same `maps.app.goo.gl` short link**; Session 22 replaced
  both with the short `?api=1&query=lat,lng` form.
- Families: `element_text_wb0vq032wwe` (Ông Nguyễn Trọng Văn), `element_text_pewt62d2wwe`
  (Bà Nguyễn Thị Thu), `element_text_mqnvp852wwe` (Ông Đỗ Đức Hạnh), `element_text_82vv7as2wwf`
  (Bà Nguyễn Thị Len).
- Timeline: **3 rows** since Session 23 (the "Đón khách" row was deleted — see "Session 23: bỏ dòng Đón khách").
  labels `arvqe7k0yci`/`v1e58wj2ye8`/`lx3ongb4qe7`; times
  `5zugc3by51l`/`ei8ualj2ye8`/`6nc0vdb4qe6`.
  The order is **plain chronological**: Lễ Vu Quy 09:30 28.11 → Tiệc cưới → Lễ Thành Hôn.
  Row side pattern is right–left–right (row 1 = `element_shape_3fnx89oxlmx`, row 2 = `z1g5m6ho2j3`,
  row 3 = `28qg0f8o9yh`). Static default text = **Groom + Evening**
  (09:30 28.11 / 17:00 28.11 / 11:30 29.11).
- Event card dates: runtime master data rewrites day/month/year + lunar + title:
  card1 `njqb10iackg`/`h8uc8oajpky`/`jdrtqz9csn8`/`wk2sh3dr2dg` + title `0iedk0b1132`;
  card2 `rqma35y0hat`/`vncalha0hau`/`h8ut0rd0hau`/`d7sj8uh0hat` + title `mb26vvt0hat`.
  Both `title` and `address` are written with **`innerHTML`** (they contain `<br>`); everything else
  uses `textContent`. Card titles are uppercase in the data because the node also has
  `text-transform: uppercase`.
- Countdown: `data-target` static default `2026-11-28T17:00:00` (Session 22, = Groom + Evening);
  runtime overwritten per group (evening `2026-11-28T17:00:00`, morning `2026-11-29T09:00:00`;
  engine re-reads on each tick).

## Path routing / master data (Session 20, reworked in Session 21)

- Picking Chú Rể/Cô Dâu + nhóm giờ calls `window.cfApply(guest, group)` (defined in the master-data
  `<script>` right after the overlay IIFE, before `<div class="miu-wrap">`), which rewrites the content
  from `WEDDING_MASTER` and then does
  `history.replaceState(null, '', '/groom/evening' | '/groom/morning' | '/bride/evening' | '/bride/morning')`.
- Master data lives in `WEDDING_MASTER[guest][group]` (timeline 3 mốc, 2 event cards, countdown target).
  Session 22 extended the card schema to `title / day / month / year / lunar / venueName / address /
  mapUrl`, added a `MAPS` constant (3 short `?api=1&query=lat,lng` links), a `VENUE_*` constant per
  venue, and a `card(title, day, lunar, venue)` helper so each of the 8 cards is a single line.
  Overlay step 1 stores `window.cfGuest`; step 2 group click calls `cfApply(cfGuest, group)` then `close()`.
- **Venues ARE now touched by the master data** (Session 22) — `MASTER_CARD_NODES` carries
  `venueName` + `address` and `MASTER_MAP_NODES` carries the two map-button node ids.
  Families and the `src=` of timeline ribbon icons are still NOT touched (unchanged per design).
- **Which card is which is part of the design, not a data quirk**: groom combos show
  Tiệc Cưới + Lễ Thành Hôn (timeline rows 1–2, so card 2 == row 3), bride combos show
  Tiệc Cưới + Lễ Vu Quy (rows 1–2, so card 2 == row 1). A test asserting the row index of card 2
  must branch on the guest: **groom → row 3, bride → row 1**.
- Editing the canvas: replace node text by anchoring on `data-node-id`, not by regexing the text —
  `wk2sh3dr2dg` and `d7sj8uh0hat` currently hold identical lunar text.

### Session 23: bỏ dòng Đón khách

The "Đón khách" timeline row was removed entirely (4 canvas nodes deleted, 20 nodes shifted up,
canvas height 9330 → 9250). What matters if you touch this area again:

- **Node ids are unique (106 of them) but the canvas source order is NOT visual order.** The miu
  export interleaves sections (e.g. the whole `element_wishes_6hr88wywupk` block sits between
  timeline rows 1 and 2 in the file). Never assume
  "next node in the file = next row on screen" — resolve positions from the `top:` in the `style`
  attribute instead.
- **Not every node is a `<div>`.** `element_wishes_6hr88wywupk` is a `<section>`. Any script that
  walks nodes with `lastIndexOf('<div', …)` will silently land on the *previous* node and corrupt
  the edit. Match on the actual tag name.
- To delete a node, find the opening tag by walking back to the `<` whose tag actually contains
  `data-node-id="…"`, then scan forward balancing **that tag name** (an `element_image_*` node wraps
  a nested `<div>`, so a naive count overshoots).
- The vertical spine is `element_shape_o4ddhktw62w`: a horizontal line SVG of width 274.7 rotated
  90°, so its **on-screen vertical length is its `width`**, centred on `left + width/2`. To shorten
  it, change `width` and re-centre `left`/`top` — do not touch `height` (that is the stroke
  thickness). Current: `left:179.08px; top:5114.42px; width:213.15px; height:51.81836px` →
  covers y 5033.8 → 5246.9, with the same 34.4px head / 70px tail overhang as before.
- Canvas height is written in **5 places** and must stay in sync: the CSS `--ch:9250px` + `--sh:9250px`
  (line ~107), the inline `--sh: 9250px` on `.miu-stage`, `height: 9250px` on `.miu-canvas`, and
  `var baseH = 9250;` in the resize script. `baseH` is a *floor* — the script computes
  `max(baseH, canvas.scrollHeight)`, so if you shrink the layout, lower it too or the page keeps a
  dead 1000px of scroll. Content bottom is 9246px, leaving the same 4px bottom margin as before.
- `applyMasterData` iterates `i < d.timeline.length`, **not** a hardcoded count — add/remove timeline
  rows freely in `WEDDING_MASTER`, and keep `MASTER_TEXT_NODES.tlTime` / `.tlLabel` the same length.

### `404.html` = thin redirect (Session 21)

GitHub Pages serves `404.html` for every path it cannot resolve, so a refresh on `/groom/evening` would
land on an unrelated page. `404.html` is therefore now a ~1 KB redirect stub, **not** a second site:

```
/groom/evening  →  404.html  →  location.replace('/?g=groom&t=evening')
                                    →  index.html parsePath()  →  replaceState  →  /groom/evening
```

- `index.html`'s `parsePath()` reads `?g=` / `?t=` **first**, then falls back to `pathname`
  (query wins; only `bride|groom` × `evening|morning` accepted, case-insensitive). Both work, so the
  file is also usable if a host ever rewrites deep links to `index.html`.
- The deep-link branch normalizes the URL with `replaceState` so the address bar shows the clean
  `/groom/evening`. No redirect loop: `replaceState` does not trigger navigation, and a later refresh
  just goes through `404.html` again.
- Anything not matching a valid pair (including `/404.html` and `/index.html` opened directly) →
  `location.replace('/')`.
- **Keep `404.html` in sync with nothing** — it has no content, no assets, no Firestore. All
  invitation content lives only in `index.html` + `WEDDING_MASTER`. If you ever need the old
  vs-template-5 scroll site back, it is in git history (before Session 21), not in the working tree.

### Firestore collections

- `vu_nhung_2_messages` — active souvenir/guestbook (`fullname`, `comment`, `createdAt`).
- `guests` — legacy, used only by the old vs-template-5 site (RSVP form + guestbook). No longer
  reachable now that `404.html` is a redirect. The data still exists in Firestore; reading it again
  would need new code.


## Fonts

The original miu fonts (Ergisa-Regular, Arcittya-Begatri, Flavinda, High Spirited, Alisheia,
UVN Hoa Tay, Lora) are now downloaded locally into `assets/fonts/` and served via a re-added
`@font-face` block in `<head>` — the site uses exactly the template fonts (no remote dependency).

| File | Family |
|------|--------|
| `vip-ergisa-regular.otf` | Ergisa-Regular (hero names) |
| `vip-arcittya-begatri.otf` | Arcittya-Begatri |
| `flavinda.otf` | Flavinda |
| `vip-high-spirited.otf` | High Spirited |
| `vip-alisheia.otf` | Alisheia |
| `uvnhoatay.ttf` | UVN Hoa Tay |
| `lora-regular.ttf` / `lora-semibold.ttf` | Lora (400 / 600) |
| `dancing-script-vietnamese.woff2` / `dancing-script-latin.woff2` | Dancing Script (overlay names, `&`) |
| `cormorant-garamond-roman-vietnamese.woff2` / `…-roman-latin.woff2` | Cormorant Garamond roman (overlay bottom block) |
| `cormorant-garamond-italic-vietnamese.woff2` / `…-italic-latin.woff2` | Cormorant Garamond italic (`.cf-invite`) |

Session 24 added the last three families **only for the opening card** (the opening slide copies the
melipage reference's typography). They are served as woff2 with explicit `unicode-range` per subset:
Vietnamese files carry the diacritics (Trọng Vũ / Hồng Nhung / Trân trọng kính mời), Latin files carry
`&` and ASCII — **the Latin subset is required**, dropping it breaks the ampersand. Weights are declared
as `400 700` (Dancing Script) and `300 700` / `400 700` (Cormorant roman / italic) to match the
variable-font axes of the files; nothing in the overlay needs more than 500.

Google Fonts link is kept only as a safety fallback for these families.

## Assets

**Session 38 — 32 asset (33 chỗ tham chiếu) đã chuyển sang Firebase Storage. File local vẫn
còn nguyên trên đĩa.** Mục tiêu: đổi ảnh trên bucket là thiệp tự đổi, **không cần deploy lại**.

- **URL dạng**: `https://firebasestorage.googleapis.com/v0/b/vu-nhung-wedding.firebasestorage.app/o/<object>?alt=media&token=<download-token>`
  với object path **URL-encode** (`uploads%2Fgallery-01.webp`). Dùng `?alt=media` + token.
- **Phạm vi đã đổi**: 20 ảnh `uploads/`, 11 file `elements/`, 1 audio `audio/ordinary.m4a`, và 3
  `<link>` favicon/apple-touch-icon → **tổng 33 chỗ `src`/`href`** (riêng `ribbon-05.webp` dùng 2 lần).
  Thêm **2 `<meta>`** `og:image` + `twitter:image` cũng trỏ Firebase (trước đây là URL tuyệt đối
  `https://vunhungwedding.online/assets/uploads/couple-main.webp`).
- **Còn local**: toàn bộ 14 font trong `assets/fonts/`. `assets.js` A6 khẳng định 14 `@font-face`
  local và **0** font remote — font không được chuyển sang Firebase.
- **⚠ Điểm yếu duy nhất: download token.** Xoay/regenerate token trong Firebase console sẽ làm
  hỏng **cả 33 chỗ cùng lúc**. Nếu phải xoay token, sửa lại toàn bộ map trong `doortest\assets.js`.
- **Thứ tự thay thế là bắt buộc** (xem `s38-assets.js`): `og:image`/`twitter:image` **phải** được
  thay **trước** các `src="assets/uploads/…"`. Nếu ngược lại, thay chuỗi
  `assets/uploads/couple-main.webp` sẽ biến 2 dòng meta thành
  `https://vunhungwedding.online/https://firebasestorage…`. `assets.js` A3a chốt lại điều này.
- **Không xoá file local.** `assets/uploads/`, `assets/elements/`, `assets/audio/`,
  `assets/favicon.*`, `assets/apple-touch-icon.png` vẫn commit trong repo như **bản dự phòng /
  fallback thủ công** — nếu mạng lỗi thì chỉ cần đổi `src` về đường dẫn cũ.
- **Dung lượng**: ảnh tăng 4.8 MB → **8.5 MB** (bản Firebase là bản full-resolution, phần lớn
  4160×6240 vào ô ~288px ⇒ oversampling ~14×). Đã giảm nhẹ nhờ 29/30 ảnh có `loading="lazy"`,
  nên chỉ ảnh đang xem mới tải.
- **5 ảnh đổi tỉ lệ** khi up lại (đã đo bằng `shoot38.js`, so cả trước/sau):
  `gallery-09` 1.316→0.667, `gallery-10` 1.392→0.667, `gallery-15` 1.778→1.500,
  `gallery-16` 1.778→1.500 — **cả 4 đều khớp ô hơn bản local**.
  `gallery-12` từng là vấn đề (bản cũ 4160×6240 = dọc 0.667 vào ô ngang 1.654 ⇒ crop ~60%), nhưng
  **đã được up lại thành 6240×4160 (1.500)** nên nay **khớp đúng bản local**, không còn crop lệch.
- `assets/uploads/gallery-15.webp` thực tế là **JPEG** dù đuôi `.webp` (đã có từ trước, không đổi).
- Mọi hình học canvas / animation / audio timing **không đổi** (`assets.js` A7a: canvas vẫn 9250px).

- Images: `assets/uploads/*.webp` (20 photos) — file local còn nguyên nhưng **không còn được tham
  chiếu** từ Session 38; `index.html` trỏ thẳng Firebase. Session 25 đã flatten thư mục con cũ
  `assets/uploads/6a2a56e562badd7da97313bb/`; id đó là invitation id của miu export (vẫn còn 1 lần
  trên canvas dưới dạng `data-invitation-id`, không script nào đọc).
- Music: `assets/audio/ordinary.m4a`, **Session 38: `src` trỏ Firebase** (`audio%2Forinary.m4a`,
  2.46 MB, byte-identical bản local). **Session 33: starts on `miu:opening:closed`** — i.e. only
  once the door has finished sliding (4500 = `--door-duration` 4s + `--door-delay` 500ms +
  `--door-stagger` 0ms, + the script's 320ms pad ⇒ **4820ms after `close()`**). Before this it
  fired on the very first gesture, so the music started while the door was still moving. That
  matches the melipage reference, which gates its audio on `closed` too.
  `preload="metadata"` còn nguyên ⇒ Chrome **cố ý hủy** range request sau khi đủ metadata
  (`net::ERR_ABORTED`), nên `assets.js` A5b phải bỏ qua lỗi đó cho audio.
- Decorations: `assets/elements/` (9 files) — local còn nguyên, 8 file **đã trỏ Firebase**;
  `ribbon-01.png` không được tham chiếu (asset dư) nên giữ local.
- Favicon: `assets/favicon.svg` + `assets/favicon.png` + `assets/apple-touch-icon.png`
  (red `#7f0505` square with 囍). **Lưu ý đường dẫn lệch**: local nằm ở `assets/`, trên Firebase nằm
  ở `elements/`.

### Image files (renamed to readable names)

Tên file local giữ nguyên để đổi ảnh sau này không nhầm. **Session 38: `couple-main.webp` là
og:image/twitter:image, nhưng cả 2 `<meta>` giờ trỏ URL Firebase tuyệt đối**
`https://firebasestorage.googleapis.com/v0/b/vu-nhung-wedding.firebasestorage.app/o/uploads%2Fcouple-main.webp?alt=media&token=523941c0-3c7d-4c45-b6f7-9906c406f55d`
— nên up ảnh mới lên bucket là preview Facebook/Zalo đổi ngay, không cần deploy.

| File | Role |
|------|------|
| `couple-main.webp` | Ảnh đôi chính (also og:image/twitter:image) |
| `couple-photo.webp` | Ảnh đôi to, đầu phần couple |
| `gallery-01.webp` … `gallery-16.webp` | Ảnh trong album (tile), theo thứ tự vị trí trên canvas |
| `portrait.webp` | Ảnh chân dung to |
| `final.webp` | Ảnh đôi cuối trang |

> Đây là **tên local**. Trong `index.html` các ảnh này được tham chiếu bằng object path
> `uploads/<tên>`, không phải đường dẫn `assets/`.

### Decorative elements (localized from miuwedding.com)

| File | Original (miuwedding.com/elements) |
|------|-----|
| `ribbon-01.png` … `ribbon-04.png` | `bieutuongluudo/*.png` |
| `ribbon-05.webp`, `ribbon-06.webp` | `bieutuongluudo/*.webp` |
| `and-ornament.png` | `and.png` — **Session 36: the `&` in the hero.** Was `left:122.08px`, i.e. spanning 122→185 while Trọng Vũ's ink spans 110→465, so it sat *inside* the text and was painted over (it precedes both names in DOM order and all three are `z-index:0`). Now **`left:38.76px`**, on the **left** of the names and **tucked against them** — 8.1px from Trọng Vũ's ink. It was briefly centred in the gutter at `23.38px` (23.4px to each side) but that read as detached; a flourish only reads as one when it nearly touches the word. `top` unchanged (571.4177517361111) — it still straddles the two name lines. It has **no** `data-anim-preset`, so it is visible from the first frame and is *not* part of `HEADER_EARLY`. Both edges are real ink, not layout: the "T" has 0px left side-bearing and the PNG has ~0.1px horizontal padding. |
| `floral-pattern.png` | `hoavan/ mndsmdnsjadhsjakd.png` (URL had a space) |
| `side-card-icon.png` | `melipage.com/assets/images/side-card-icon.png` (icon ấn cửa, Session 26) |
| `ribbon-01.png` | *(Session 31: đã commit nhưng `index.html` **không** tham chiếu — asset dư, giữ cho đủ bộ)* |

## Scroll reveal system (miu engine) — already installed, do not rebuild

- Animated nodes carry `data-anim-preset` + `data-anim-duration` / `-delay` / `-easing` / `-loop` /
  `-distance` (81 nodes). Preset resolves as `'miu-' + preset` against the 58 `@keyframes miu-*`
  already in the file. Easing is `cubic-bezier(0.2, 0.8, 0.2, 1)` everywhere; duration `3000`
  (75 nodes), `2400` (2 — the hero couple names, Session 35) or `2000` (4); distance `200` (32) or
  `150` (1), keyframe default `12px`.
  `isInfinitePreset` forces `infinite` for `rotate`/`spin`/`flicker`/`pulse`/`wiggle`/`heartBeat`/
  `swayBottom` regardless of `data-anim-loop`.
- The observer script is **byte-identical (6239 chars) to the melipage reference** — do not rewrite
  it. `IntersectionObserver` `{root:null, rootMargin:'0px 0px -12% 0px', threshold:[0.08,0.15,0.22]}`,
  then `applyAnim()` + `obs.unobserve()` so each node plays **once**. It gates on the opening overlay
  via `miu:opening:willClose` / `miu:opening:closed`, plus a 2500 ms fallback that fires **only if
  the overlay was never seen open**.
- **A node without `opacity:0` does not animate correctly.** `applyAnim` writes
  `el.style.opacity='1'` and then `animation: … both`, so the *backwards* fill re-hides the node at
  the trigger instant. An unhidden node is therefore **visible from page load → snaps invisible →
  fades in** (a flicker, not a reveal). All 81 nodes need `opacity:0` as the **last** `opacity`
  declaration in their inline `style`.
- **Trap: the canvas carries two different `style` serialisations**, and the second is the warning sign.

  | Form | Marker | `opacity` declarations |
  |------|--------|------------------------|
  | Original export | `var(--miu-node-rotate,0deg)` (no space after comma) | `opacity:1;…;opacity:0` (2, last wins) |
  | Re-serialised by an earlier session's node edit | `--miu-node-rotate: 0deg; ` (space) + `position: absolute; left: …` | had only `opacity: 1` (1) — **the hiding `opacity:0` was lost** |

  Session 31 found **41 of 81** nodes in the second form with the hiding declaration missing, all
  inside the first 3100px (hero, families, the V/N initials, timeline and both event cards — i.e.
  the whole middle of the invitation). Fixed by replacing the 41 occurrences of
  `--miu-node-rotate: 0deg; transform: rotate(var(--miu-node-rotate,0deg)); opacity: 1;` with the
  same string ending `opacity: 0;`. **Any future edit that rewrites a node's `style` must re-append
  the trailing `opacity:0`**, otherwise that node silently stops animating.
  The very same 41 nodes were the ones carrying a baked `animation:` (see the next trap) — one
  broken export, two symptoms.
  Audit: count `[data-anim-preset]` nodes whose *last* `opacity` declaration is `0` — must be
  **81/81** (the reference export is 85/85).
- **Trap (Session 32): the export bakes `animation:` into 41 inline `style` attributes.** This is
  the second half of the "two serialisations" problem above, and it is *worse* than the lost
  `opacity:0`. Those 41 nodes carried a literal
  `animation: 3000ms cubic-bezier(0.2, 0.8, 0.2, 1) 0ms 1 normal both running miu-<preset>;`
  in their `style` — the author saved the export from a live DOM where the engine had already
  applied them. Consequences, all silent:
  - the CSS animation starts at **page parse**, i.e. behind the 4.5 s opening overlay, runs its
    3 s and is latched at the `to` state by `fill-mode:both`;
  - when the observer later calls `applyAnim()`, it assigns the **identical** animation string, so
    the browser does **not** restart it → the node never reveals;
  - so the whole **top third of the invitation** (hero, families, V/N initials, timeline, both
    event cards, both map buttons) was simply visible from the first frame, while the bottom two
    thirds revealed normally.
  The melipage reference has **0** such declarations. Fixed by deleting all 41 (7 distinct strings:
  26× `fadeInDown`, 4× `fadeInLeft`, 3× `fadeIn`, 2× `2000ms fadeInLeft`, 2× `2000ms fadeInRight`,
  2× `fadeInRight`, 2× `heartBeat … infinite`). **Any future edit that rewrites a node's `style`
  must not re-introduce a literal `animation:`** — the animation belongs to the observer.
  Audit (must be **0**, and all 41 hits must sit inside a `style="…"` attribute, never in the
  `<style>` block or the engine JS):
  `[regex]'animation:\s*[\d.]+ms[^;"]*miu-[A-Za-z]+'`
- The 41 nodes also keep the *whitespace* half of the re-serialisation (`--miu-node-rotate: 0deg; `
  with a space, 41 occurrences) — that is cosmetic and functionally identical to the original
  comma form (275 occurrences), so it is left alone. The tell that a node was touched is that
  marker, **not** the whitespace itself.
- **The reveal gate is `miu:opening:closed`, NOT `willClose` (Session 34).** This reverses the
  earlier "run through the gap" behaviour: animations start **only after** the door has finished
  opening (4820ms after `willClose`). The change was deliberate at user request. The previous
  `willClose` gate had caused effects to begin ~4.8s before the leaves completed. Gate remains on
  `miu:opening:closed` (fired when `#miuOpening` becomes `display:none`), with a 2500 ms fallback
  that fires only if the overlay was never seen open.
- **…with ONE exception: the 3 hero nodes start at `willClose` (Session 36).** Session 34's gate
  made the guest stare at a blank door for 4.8s and then wait another 2.4s for the names, and the
  hero is the only part of the canvas on screen from the first frame — it has no scroll reveal to
  speak of. So `element_text_w6mjrszfugn` / `ghi89lrdzxt` / `58ymmmme3xn` are started by
  `onWillCloseHeader` (a second listener on `miu:opening:willClose`, the list is `HEADER_EARLY`
  and it must stay in sync with the same list in `doortest\reveal.js`). Everything else — the
  other 78 — is still gated on `closed`.
  - Two structural consequences, both deliberate:
    - **`applyAnim` and `isInfinitePreset` were hoisted out of `start()`** to IIFE scope. They used
      to be inside it, so the early listener had no way to reach them. `start()`'s
      `if (started) return;` guard is untouched and the early handler does **not** set `started` —
      if it did, the `closed` gate would never run and 78 nodes would stay invisible.
    - **A hero node whose `getBoundingClientRect().top >= innerHeight` is skipped.** Without that
      guard a short window (a landscape phone, where the unscaled canvas puts the hero at
      y=483..683) would run the fade off-screen and the guest would scroll down to a node that
      was already at opacity 1 — the scroll reveal gone. This cannot happen on a portrait phone
      (the stage is scaled to `min(575px, 100vw-32px)`, so at 380px wide the hero is already at
      screen y≈293..362), which is why `reveal.js` PHASE 4 tests it at **640×360**.
  - `applyAnim` does **not** early-return on `__miuAnimApplied`, so when the IntersectionObserver
    later re-applies the hero's identical `animation` string the browser does not restart it (same
    mechanism as the baked-`animation:` trap). That is why no dedupe guard was needed.
- The reference has **no** `position:sticky`, no parallax, no scroll-scrub (0 occurrences in its
  279 KB) — any such effect here is an *addition*, not a port. It also has a **bug**: under
  `prefers-reduced-motion:reduce` it still plays the animation, it only drops the scroll gate.
  Its FAB dock is music + collapse only, **not** section navigation.

## Opening Flow (overlay) — contract with the miu engines

- `#miuOpening[data-open="1"]` with `#miuOpeningSides` + two `.card-side`
  (CSS `animation`, not `transition`) — the slow auto-scroll waits for `animationend` on both sides
  +350 ms (fallback 4 s...6 s). The gate **dedupes by `event.target`**. Since Session 29 the two
  leaves run on the *same* schedule, so the stagger that once made them end at different times is
  gone — but keep the dedupe, it costs nothing and the reduced-motion path can still differ.
- Visual design = **flat 2D slide** (Session 29), copied from
  `melipage.com/the-truong-nhu-quynh-2026-05-24-template`, which is a 2D `translateX` slide, **not** a
  rotating door. Our own `#miuOpeningSides` is `overflow:hidden` with **no `perspective`**, and the
  leaves carry no `backface-visibility` / `transform-origin`.
  - **Session 29 removed the whole 3D layer** that Sessions 24/27/28 had built: `perspective:1200px`,
    `perspective-origin:68% 50%`, `backface-visibility:hidden`, `transform-origin:left|right center`,
    the `rotateY` keyframes and the `--door-angle:108deg` token. **Do not reintroduce them.** History is
    in git (Session 28) and in `C:\Users\Admin\AppData\Local\Temp\opencode\index.before-session29.html`.
  - The user asked for the reference's slide because the two leaves of the 3D door were visibly out of
    sync: the 150 ms stagger meant the right leaf started late (measured first movement 911 ms vs
    804 ms, peak angle difference ≈ 9.9°). A slide with `--door-stagger:0ms` cannot desync —
    measured first movement **712 ms for both**, max progress difference **0.0000** over 48 samples.
  - **No backdrop at all** (Session 26). `#miuOpeningBackdrop` and its gold seam glow were deleted —
    the page behind shows through while the leaves move, exactly like the reference
    (`#miuOpening{background:transparent}`). Because the leaves are `#ffffff` on a `#fff` body, the
    separation comes from a shadow caster instead: `#miuOpening::after` (a `box-shadow`-only rect
    the size/position of the door, `z-index:0`, no background) carrying
    `0 0 0 1px rgba(0,0,0,.05), 0 18px 60px rgba(0,0,0,.18)`. It **must** fade with
    `#miuOpening[data-open="0"]` (`.5s ease .1s`) or a dark rectangle
    hangs in mid-air for 4.8 s after the leaves have left. `@media (max-width:480px)` drops the shadow
    entirely (`--door-w:100vw`, no white margin left to define). This is the **only** thing on the overlay
    that still fades — see the Session 28 rule below.
  - Geometry: `--door-w:min(575px,calc(100vw - 32px))` (`100vw` ≤ 480 px). Left leaf `68%`, right leaf
    `32%` — equivalent to the reference's overlapping 68% + 50% with the left leaf on top: both cards
    are white, the only visible edge is the `#e9e9e9` stripe on the right of the **left** leaf (reference
    token `--slide-card-stripe-size`, 3.5%), so 32% and 50% are indistinguishable at rest. Travel is
    `translateX(-110%)` on the left and `translateX(100%)` on the right, both copied from the reference;
    because the leaves end *outside* the door and are clipped by `overflow:hidden`, the leaf-to-leaf gap
    finishes at `1.10 × 391 + 184 = 614px` — **wider than the 575px door**, which is correct, not a bug.
  - Palette/text: card `#ffffff`, ink/seal `#9e8130`. `.cf-save-date` = High Spirited
    `clamp(38px,10.5vw,66px)` with a 95 px `<span>S</span>ave our date`; `.cf-names` = Dancing Script
    (`&` italic), `top:clamp(150px,32%,240px)`; `.cf-bottom` = Cormorant Garamond, `gap:10px`,
    `bottom:clamp(170px,23%,230px)`. All values are copies of the reference's `--slide-*`/`--opening-*`
    tokens, so re-tuning is a matter of editing the block at the `#miuOpening` rule.
  - `#miuSeal` is the reference's **icon button** — `#miuOpeningBtn > img` of
    `assets/elements/side-card-icon.png` (78 px, `object-fit:contain`, `drop-shadow(0 4px 10px rgba(0,0,0,.35))`),
    no disc/border/ring any more. **Session 28 moved it back inside `.card-side-left`**, right after
    `#miuOpeningCta`, which is the reference's structure (`.seal-icon` is a child of the card, not a
    sibling). It now sits at plain `left:98%` / `z-index:2` / `top:50%` with
    `transform:translate(-50%,-50%)` — the Session 26 "do not write `left:98%`" warning is **reversed**:
    that warning only existed because the seal was a sibling, so the percentage would have resolved
    against the viewport. As a child of the leaf, `98%` is the reference's own token and is exact:
    `0.98 × 68% = 66.64%` of the door = the old `50% + var(--door-w) * .1664` = 7.8 px left of the seam.
    Verified identical on screen (seal centre 815.7 px both before and after the move), so re-tuning the
    door geometry still means editing `--door-w` / the leaf width, never the seal.
    Because of `left:98%` + `translate(-50%,-50%)` the seal **deliberately overhangs the seam by ~31px**
    and lies over the right leaf. That is the design (it is the door knob), but it breaks naive gap
    hit-testing — see the probe trap below.
  - `#miuOpeningCta` / `#miuOpeningCtaBtn` = the reference's "Mở thiệp" pill, and it **must stay inside
    `.card-side-left`** so it slides out and is clipped with its leaf. `left:50%` is the *leaf* centre
    (34% of the door), `top:calc(50% + (var(--slide-seal-size) / 2) + 4px)`, `width:min(92%,420px)`,
    Lora 12 px/800, `letter-spacing:.06em`, uppercase, `#9e8130`. Colours are copied verbatim
    (`rgba(255,255,255,.10)` fill, `rgba(255,255,255,.26)` border) — on a white card that pill is
    deliberately almost invisible, leaving gold text + `0 10px 30px rgba(0,0,0,.25)`; the user chose to
    keep it faithful to the reference rather than tint it gold.
  - **The card is bare** (Session 26): the popup no longer auto-opens (the `setTimeout(…, 1000)` is
    gone). Both `#miuOpeningCtaBtn` and `#miuOpeningBtn` call the same `openChoice()` →
    `#cf-choice-step1` → `#cf-choice-step2` (Chú Rể/Cô Dâu → Tối Thứ Bảy 17:00 28/11 / Sáng Chủ Nhật
    09:00 29/11) → `window.cfApply()` → `close()`. `.cf-choice` keeps its `rgba(0,0,0,.55)` scrim.
  - **No fade on card content** (Session 28) — the rule that matters most here. The reference's
    `miuOpeningSlideLeft/Right` keyframes pin `opacity:1` in both `from` and `to`, so save-date, names,
    bottom block, seal and CTA all leave as **one solid object** carried by the card. Do not reintroduce
    a close-time fade: `.cf-leaf-text` has **no** `opacity`/`transform` rule at all (16/16 samples at
    `opacity:1`, `transform:none` across the whole slide), `#miuOpeningCta` only adds
    `pointer-events:none`, and `#miuSeal` has no `data-open="0"` rule (the old `scale(.55)` is gone).
    Note `.cf-save-date` legitimately computes to `opacity:0.98` — that is the reference's own
    `--slide-save-opacity` token, not a fade.
    **Session 29 removed the last remaining reason the content could not be perfect**: under 3D,
    `backface-visibility` removed the text past 90° and perspective foreshortened it over the last
    ~30% (249 px text box → ~36 px → gone). A flat slide has neither problem, so `.cf-save-date` keeps a
    **constant 249 px width for the whole 4.5 s** (measured variation 0 px over 16 samples), and the
    seal stays welded to the leaf (offset from the leaf's left edge constant to 0.18 px).
  - Old design tokens (`.cf-divider`, `.cf-seal`, `miuSlideLeft/Right`, the gray-gradient right leaf and
    the `#7f0505` accents) are **gone** — do not reintroduce them. So is the `#7f0505` 囍 disc seal
    (`.cf-lock` never had a CSS rule; the class is gone too).
- **Timing lives in CSS on `#miuOpening`**: `--door-duration:4s`, `--door-delay:500ms`,
  `--door-stagger:0ms`. The `close()` script reads those three custom properties
  (not the `animation` shorthand, which only resolves once `data-open="0"`) and fires
  `miu:opening:closed` at `duration+delay+stagger+320` ≈ 4820 ms. Change the animation? Change the
  tokens — the script follows. `prefers-reduced-motion:reduce` overrides the three timing tokens to
  `1ms/0ms/0ms` (animation effectively `none`).
  - **Session 27 slowed it 1.3×** (`2.4s`/`340ms`/`120ms` → `3s`/`500ms`/`150ms`) because 2.4 s read as
    "hơi nhanh". Speed has **two** independent causes and both must be addressed:
    - *length* → the three tokens;
    - *snap* → the easing. The old `cubic-bezier(.34,.02,.2,1)` has control-point **y = 0.02**, i.e. the
      leaf accelerated almost instantly off the hinge, which is most of why it felt fast.
  - **Session 28 matched the reference's own duration**: its leaves are `animation-duration:4s` with
    `animation-delay:500ms` and its script even carries `if (!total) total = 4500;`, so 4 s is the
    reference's real figure, not an invention.
  - **Session 29 set the easing to `cubic-bezier(.4,.05,.25,1)` on both leaves** — deliberately *not* the
    reference's `ease-out`. `ease-out` = `cubic-bezier(0,0,.58,1)` has control-point **y = 0**, the very
    property Session 27 removed as the cause of the snap, and since `y1 = x1 = 0` it is its own inverse,
    so at 15% of the window it has already covered 15% of the distance. Our curve covers **7.1%** there
    (theory 7.9%) and 98.4% at 85% of the window, i.e. gentle off the mark and a soft landing, without
    giving back the "hơi nhanh" complaint. The user picked this over `ease-out` explicitly.
  - **Session 29 removed the stagger** (`--door-stagger:150ms → 0ms`) so the two leaves cannot drift
    apart, which was the user's stated reason for the change. Total `4s + 500ms + 0ms` = **4500 ms**,
    `closed` at 4820 ms — which is also exactly the reference's own `4500`.
  - Three **fallback constants** mirror the token sum (`4500`): `doorTotal` in the overlay script and
    both `deepTotal` fallbacks in the master-data script. They only fire if reading the custom
    properties throws, but a stale value there hides the overlay mid-slide, so **retune them together
    with the tokens** (this is the same bug class as the hardcoded `4300` removed in Session 27).
  - The **deep-link auto-close** (the `load`+1200 ms branch in the master-data script, used by
    `?g=…&t=…` / `/groom/evening`) computes `deepTotal` from the same three tokens through a local copy
    of the same `parseMs` helper, using `deepTotal + 320` — identical arithmetic to `close()`. Measured
    margin over `animationend`: 284–287 ms, i.e. `closed` always lands *after* the last leaf.
  - The auto-scroll fallback timer is still a hardcoded **6000 ms** (guarded by `data-open === '0'`).
    The real gate fires at `delay+stagger+duration+350` = 4850 ms, so there is ~1.1 s of slack; past
    `--door-duration ≈ 5.6 s` the fallback becomes a no-op and only `animationend` can start the scroll.
- On choosing a group: sets `data-open="0"`, fires `miu:opening:willClose`, then `miu:opening:closed`
  which also sets `opening.style.display='none'` (the animation is not `forwards`-latched, so the leaves
  must be taken out of the tree, not left translated). (The `eventType` radio used to be pre-selected by
  the old RSVP form and is gone — the choice now only gates the intro/music.)
- The overlay covers `#miuBootLoading` (z-index 2147483000 → overlay uses 2147483001). That boot screen
  is `#ffffff`, so the transparent overlay does not expose anything odd while the page loads.
- Verifying a door change: no test suite exists — drive it with `puppeteer-core` against
  `file:///D:/lean_AI/wedding/index.html`   (`C:\Users\Admin\AppData\Local\Temp\opencode\doortest\s26.js` = 35 assertions: bare card, seal/CTA
  geometry vs the reference formula, click flow, event timing, reduced motion, deep links; **`s29.js` =
  35 assertions that replaced the 3D `s27.js`**: it parses the 2D matrix (`matrix(a,b,c,d,e,f)`, `e` is
  the translate) and asserts `perspective:none`, `backface-visibility:visible`, `transform-origin` = the
  leaf centre, **no `matrix3d` in any of 48 samples**, both leaves starting within 100 ms and staying
  within 2% progress at every sample, full travel `-430.1px` / `+184.0px`, the gentle-start and
  soft-landing percentages, `.cf-save-date` width constant, the seal welded to the leaf, and the
  `closed`-after-`animationend` ordering; `probe29.js` = 6 non-visual assertions on the opening gap via
  `elementFromPoint`; `s26b.js` = overlap matrix at 5 viewports).
  **`s27.js` is obsolete and was deleted** — it asserted the final angle was 108°, a design Session 29
  deliberately removed. Do not resurrect it; port new ideas into `s29.js`.
- **Verifying the scroll reveal: `doortest\reveal.js` = 37 assertions, run it after any change to a
  node's `style` or to the observer.** It is the only test that would have caught the baked
  `animation:` trap. What it does: at `load`, with the overlay still up, assert **0** nodes have a
  running `CSSAnimation` (this is the assertion that fails if anyone re-bakes one); ~40ms after
  `willClose` assert the two reveal populations are still apart (78 anonymous nodes at
  `opacity 0`, the 3 hero nodes already animating — Session 36); after the deep link closes,
  assert every off-screen node has `getAnimations().length === 0` **and** `opacity 0` (76 nodes
  qualify); walk the canvas in ~40 jumps and assert all 81 end with exactly one animation whose
  `animationName` is `miu-<preset>`, `--miu-anim-distance` matches `data-anim-distance`, and
  `startTime` falls inside that node's own window (hero: `[willClose, closed)`, the rest
  `≥ closed`); measure the reveal latency with one clean jump per node; in a **640×360
  viewport** prove a below-the-fold hero node is handed back to the IntersectionObserver (K0–K2);
  and measure the **hero geometry** (H1–H4) — see the next trap.
  **Traps in this script, each of which cost a run:**
  - **P4 asserts `getAnimations()`, not opacity.** The pre-gate sample is taken ~40ms after
    `willClose`, so the hero nodes are mid-ramp at opacity ≈0. An earlier version of this
    assertion demanded `op=1` ("finished inside the gap") and failed for exactly that reason —
    the far end of the window is C6's job, not P4's.
  - **H1–H4 must measure INK, never the node's `getBoundingClientRect()`.** Since Session 36 the
    two names are `left:0; width:575px` boxes, so their *rect* is the whole canvas and a box-based
    overlap test would pass forever. Use `Range.selectNodeContents()` + `getClientRects()` and
    divide by the stage scale to get canvas px. `element_image_e9ddhxndm6z` (the `&`) has **no**
    `data-anim-preset`, so every other phase in the script skips it — that is how it sat buried
    behind the text for this long. **H3 has teeth: verified it returns `false` for the old
    `122.1→185.4` position.** `H4` asserts the gap is **4..16px** ("tucked, not touching"), not
    that the ornament is centred in the gutter — `38.76px` is a deliberate tuck, and a
    gutter-centring assertion would fail on it.
  - **D1 must sample opacity in the same task as the gate dispatch.** It used to read it 120ms
    later, which made it a race: the reference curve `cubic-bezier(0.2, 0.8, 0.2, 1)` is heavily
    front-loaded (`y1 = 0.8`), so opacity is already **0.41 at 250ms** of a 2400ms ramp and the
    old `< 0.3` threshold failed on nothing but timing. The IntersectionObserver callback is
    async, so a same-task read still sees the hidden value.
  - a *new* `CSSAnimation` reports `startTime === null`, then `0`, before the timeline resolves it.
    Polling for `!== null` measures Chrome's pending→ready bookkeeping (up to ~230 ms), not the
    reveal. Wait for `startTime > 1000` (the page has been open seconds by then).
  - the last ~3 nodes sit at the clamped page end: a jump there requests `target > scrollHeight -
    innerHeight` and costs extra frames. Exclude clamped groups from the latency metric — measured
    12–31 ms on a legal jump vs ~220 ms on a clamped one.
  - a node 120 px below the jump target is already inside the trigger band, so a *lower* node can be
    revealed by the *previous* jump and report a **negative** latency. Record how many ids in the
    group already had an animation before scrolling and do not charge them to this jump.
  - `Animation.startTime` is the time the **delay** phase began, not the first painted frame.
  - `effect.getTiming().easing` reads `linear` for any CSS-eased animation — the timing function
    lives in the shorthand, so read `getComputedStyle(el).animationTimingFunction` instead.
  - `history.replaceState` to a different path is refused on `file://`, so use `?g=groom&t=evening`
    and prove `cfApply` ran instead of reading the address bar.
- **Verifying the music timing: `doortest\music.js` = 17 assertions in 4 browser modes**, run it
  after any change to the audio script or to the opening events. No mode covers everything, which
  is why there are four:
  | Mode | Chrome | What it proves |
  |---|---|---|
  | A | `--autoplay-policy=no-user-gesture-required`, deep link | the happy path: held mid-slide, starts at `closed`, FAB syncs, `currentTime` advances |
  | B | default policy, injected refusal, auto-scroll killed | a refused `play()` stays paused and the **next guest gesture** recovers it (the iPhone path) |
  | C | default policy, real click flow | the actual guest journey: `unlock()` primes without a blip, music starts 4831ms after `willClose` |
  | D | default policy, injected refusal **once**, auto-scroll running | on Chrome the auto-scroll's own `scroll` event repairs the refusal by itself |

  Three traps in this script, each of which cost a run:
  - **Chrome will not refuse autoplay for you.** Neither the default policy nor
    `--autoplay-policy=user-gesture-required` blocks `play()` on a `file://` page, so Safari's
    refusal has to be injected by wrapping `HTMLMediaElement.prototype.play` in
    `evaluateOnNewDocument`. Aim it with `window.__evts['miu:opening:closed'] != null`, **not**
    with a call counter: the first post-`closed` `play()` is not always `start()`'s — the
    auto-scroll's `scroll` is one of `onIntent`'s listeners, so `unlock()` can get there first.
  - **The auto-scroll fires ~30ms after `closed`** (its `setTimeout(start, 350)` runs at
    `close()+4850`, after `closed` at `close()+4820`) and its very first `scroll` drives
    `onIntent` → `start()`. Any assertion of the form "still paused after `closed`" is therefore
    a race unless the auto-scroll is suppressed. `wheel` is one of the auto-scroll's stop events
    and is **not** an audio gesture, so `setInterval(dispatchEvent('wheel'), 50)` kills it
    without standing in for a guest gesture. A wheel dispatched once is not enough — the 6000ms
    fallback timer calls `start()` again afterwards.
  - `KILL_SCROLL` must be installed **after** `goto(…, {waitUntil:'load'})`, since the interval
    has to outlive the auto-scroll's parse-time 6000ms fallback timer.
  - Traps that cost a run each: `history.replaceState` to a different path is **refused on
    `file://`**, so URL routing is only assertable through the `?g=&t=` deep link (prove `cfApply` ran via
    `[data-countdown="1"]` + `element_text_0iedk0b1132` instead); the smooth auto-scroll fires hundreds of
    scroll events, so count monotonic **runs**, not events; once the overlay is `display:none`,
    `getComputedStyle(...).transform` returns `'none'`, so the sampler must stop on `null` or the profile
    tail fills with fake zeros. Two of these are Session 27's and no longer apply to a 2D slide — the
    `matrix3d` column-major trap (`rotateY` = `atan2(v[8], v[0])`, **not** `atan2(v[9], v[5])` which
    silently returns 0 for every sample) only bit us because we *had* a rotateY. If a 3D transform ever
    returns, that trap returns with it.
  - `elementFromPoint` traps if you ever probe "is this painted": `.cf-save-date` / `.cf-names`
    have `pointer-events:none` (the reference's own value), so hit-testing always falls through them, and
    `.cf-save-date` computes to `opacity:0.98` via `--slide-save-opacity` — neither is a fade. **Third
    trap, found in Session 29:** `#miuSeal` overhangs the seam by ~31 px, so hit-testing the middle of
    the opening gap returns the *seal* (a left-leaf descendant) for the first ~1.4 s, and returns `PAGE`
    afterwards. A test that asserts "the gap always shows the page" will fail for a legitimate reason —
    exclude `#miuSeal`, or assert only that **nothing belonging to the right leaf** ever floats over the
    gap. Also do not assert the final gap is 575 px: both leaves end *outside* the door, so it is 614 px.
    Use `shoot29.js` + a clipped screenshot for a visual record (`s29-0000-dong.png`, `s29-0700…4500-swing.png`).

## Deployment

- GitHub Pages: **Source = `master-wedding2`** (change the branch in Settings → Pages).
- **Do not touch** `CNAME` (`vunhungwedding.online`) or the DNS (A 185.199.108...111.153).
- The old site remains on `master`; only the Pages source branch is changed.
- `.gitignore`: excludes `music/` and `vs-template-5/` (from the old site) and `wedding2.html` (template).

## Troubleshooting

- **Images 404**: verify that `assets/uploads/...` exists and is committed (exact name).
- **Souvenir not submitting**: browser console → Firestore errors (rules/indexes).
- **Music not playing**: the audio script now waits for `miu:opening:closed` (see Assets) and falls
  back to the next guest gesture if `play()` is refused. `unlock()` primes the element during the
  first gesture for iOS; if that still fails the guest can press the FAB music button.
- **Music starts while the door is still moving**: `hold` is being cleared too early, or something
  else is calling `start()` — the only thing that may clear it is the `miu:opening:closed` listener
  (`releaseMusic`). Check with `doortest\music.js`.
- **Auto-scroll not starting**: check that `.card-side` uses `animation` (not `transition`) and that
  `data-open="0"` is fired.

## Work Log Rule (mandatory)

After every code change, record it in `WORK_LOG.md` using numbered session format
(list of created/edited/deleted files and technical reasons).

## References

- Firebase Docs: https://firebase.google.com/docs/firestore
- GitHub Pages: https://docs.github.com/pages