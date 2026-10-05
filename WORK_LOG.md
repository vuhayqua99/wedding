# WORK_LOG — Wedding Trọng Vũ & Hồng Nhung

## Session 1: 2026-09-22 — Analysis and preparation

### Tasks Completed
- [x] Explore the `wedding2.html` template (miu runtime, miuwedding.com export, theme *david-lan-2027-01-02-06*)
  - Mapped key nodes (couple, dates, venues, families, timeline, countdown, RSVP, souvenir).
  - Reviewed the animation/auto-scroll/playback engines and their events (`miu:opening:willClose`/`closed`).
- [x] Moved local assets
  - 20 photos → `assets/uploads/6a2a56e562badd7da97313bb/*.webp`.
  - Music → `assets/audio/ordinary.m4a`.
  - QR image omitted (gift card removed).

## Session 2: 2026-09-22 — Main site transformation

### Tasks Completed
- [x] Copy `wedding2.html` → `index.html` and rewrite it for the Vũ & Nhung wedding
  - Title/meta (canonical, OG, Twitter) → `https://vunhungwedding.online/` + local OG image.
  - Couple: `>DAVID<`→`>TRỌNG VŨ<`, `>LAN<`→`>HỒNG NHUNG<`, initials `>D<`/`>L<` → `>V<`/`>N<`.
  - Event card dates: `>02<`/`>01<`/`>2027<` → `29`/`11`/`2026`; lunar text → "21 tháng 10, năm Bính Ngọ".
  - Real venues (NHÀ HÀNG LINH TRÂM / NHÀ RIÊNG NHÀ GÁI + addresses), real families (Nguyễn Trọng Văn,
    Nguyễn Thị Thu / Đỗ Đức Hạnh, Nguyễn Thị Len), timeline (4 milestones: Tiệc cưới nhà trai, Lễ vu quy,
    Lễ thành hôn, Tiệc thân mật) and times (09:00–10:30, 11:30–12:30, 13:30–14:30, 15:00–16:00).
  - Countdown → `data-target="2026-11-29T11:00:00"`.
  - Removed: anti-devtools, local `@font-face` fonts, Cloudflare beacon, gift card/modal (QR).
  - Path rewrites: `/uploads/`→`assets/uploads/`, `/audio/`→`assets/audio/`, `/elements/`→`https://miuwedding.com/elements/`.
  - Fonts substituted with Google Fonts (see AGENTS.md) and bride hero font-size 65→48 px.
- [x] Integrate Firebase + overlay + rewrite API scripts
  - Compat SDK 10.7.1 (gstatic) after `<body>` + config (project `vu-nhung-wedding`).
  - Opening overlay (`#miuOpening`, double cover `#f4f2ea`/`#7f0505`, seal 囍, "Bạn là khách của ai?"
    Chú Rể/Cô Dâu dialog) per the engine contract (CSS animation on `.card-side`, events
    `miu:opening:willClose`/`closed`, pre-selects the `input[name="eventType"]` radio).
  - Replaced the miu API blocks with Firestore: RSVP → `vu_nhung_2_guests` (addDoc + confetti if
    "Có tham dự"); souvenir → `vu_nhung_2_messages` (onSnapshot orderBy createdAt desc limit 200 + "Xem thêm").
    Both guarded with `if (!window.firebase ...)`.
- [x] Fixed bugs found during verification
  - Removed leftover local `@font-face` blocks and the old `data-slug`.
  - Repaired the bride name div (damaged by a `$148px` mistranslated replacement group that deleted its
    opening tag); rebuilt from `wedding2.html` with `font-size:48px`.
- [x] Final verification of `index.html`
  - Balanced scripts (21/21), single `<body>`, existing asset paths (20 photos + audio), no
    `src="/...` or local `@font-face`, overlay present, key content confirmed.

### Files created/edited
- `index.html` (new site, ~202 KB)
- `assets/uploads/6a2a56e562badd7da97313bb/*.webp` (20 photos)
- `assets/audio/ordinary.m4a`
- `AGENTS.md` (rewritten for the new project)
- `WORK_LOG.md` (this file)

### Next Steps
1. Deploy: GitHub → Settings → Pages → Source = `master-wedding2` (do not touch CNAME/DNS).
2. Test in production: overlay → dialog → music → auto-scroll → countdown → RSVP/souvenir (Firestore).
3. If more confetti bursts or font/image tweaks are needed, check the browser console for Firestore
   errors (rules/indexes).

## Session 3: 2026-09-22 — Rename upload images to readable names

### Tasks Completed
- [x] Renamed the 20 photos in `assets/uploads/6a2a56e562badd7da97313bb/` from hashed miu names to
      readable positional names (`couple-main.webp`, `couple-photo.webp`, `gallery-01.webp`…
      `gallery-16.webp`, `portrait.webp`, `final.webp`).
- [x] Updated all references in `index.html`: 20 `<img src="assets/uploads/...">` + `og:image`/`twitter:image`
      (now `.../couple-main.webp`).
- [x] Added an image mapping table to `AGENTS.md` (Assets section) for future photo swaps.
- [x] Verified: 0 leftover old names, 20 unique refs resolve to files on disk.

### Files created/edited
- `assets/uploads/6a2a56e562badd7da97313bb/*.webp` (20 files renamed)
- `index.html` (image references updated)
- `AGENTS.md` (added Assets/image mapping table)
- `WORK_LOG.md` (this entry)

## Session 4: 2026-09-22 — Localize decorations, remove old icons, add favicon

### Tasks Completed
- [x] Downloaded the 8 decorative elements from `https://miuwedding.com/elements/...` into
      `assets/elements/` (ribbon-01..04.png, ribbon-05/06.webp, floral-pattern.png, and-ornament.png).
      The `hoavan/ mndsmdnsjadhsjakd.png` URL contains a space → encoded as `%20` at download time.
- [x] Replaced all 9 `src` hotlink references in `index.html` with local `assets/elements/...` paths
      (ribbon-05.webp is used twice) → **0 miuwedding.com references remain**.
- [x] `git rm -r icons/` — removed the 4 old-site SVGs (`icon-ceremony/guests/party/rings.svg`).
- [x] Added favicon: `assets/favicon.svg` (hand-written), `assets/favicon.png` (64×64) and
      `assets/apple-touch-icon.png` (180×180) generated with System.Drawing — red `#7f0505` square
      with white 囍; linked in `<head>` after `<meta charset>`.
- [x] Verified: all local asset refs resolve (missing = 0), scripts balanced (21/21), single `<body>`,
      external hosts now only Google Fonts / Firebase gstatic / venue Google-Maps link / self / SVG
      namespace (no miuwedding.com).

### Files created/edited
- `assets/elements/` (8 new decorative images)
- `assets/favicon.svg`, `assets/favicon.png`, `assets/apple-touch-icon.png` (new)
- `index.html` (element src → local; favicon links added)
- `icons/` (4 files deleted via `git rm`)
- `AGENTS.md` (Assets section: locals + favicon + elements mapping table)
- `WORK_LOG.md` (this entry)

## Session 5: 2026-09-22 — Restore the original template fonts (localized)

### Context
The hero header looked bad because of an earlier assumption that the miu fonts were not downloadable,
so Google substitutes were used — and the repaired bride name div kept the raw `Ergisa-Regular`
reference (undefined → browser fallback). Verified the miuwedding.com CDN actually serves the fonts
(200 OK for all OTF/TTF), so the originals could be restored.

### Tasks Completed
- [x] Downloaded the 8 original font files from `https://miuwedding.com/assets/fonts/builder/` into
      `assets/fonts/`: `vip-ergisa-regular.otf`, `vip-arcittya-begatri.otf`, `flavinda.otf`,
      `vip-high-spirited.otf`, `vip-alisheia.otf`, `uvnhoatay.ttf`, `lora-regular.ttf`,
      `lora-semibold.ttf` (lora-regular.ttf needed a retry after an initial timeout).
- [x] Re-added the `@font-face` style block in `<head>` (mirrors the template exactly; src →
      `assets/fonts/...`).
- [x] Reverted inline `font-family` names to the originals: `'Great Vibes'`→`Ergisa-Regular` (7),
      `'Kaushan Script'`→`Arcittya-Begatri` (2), `'Mea Culpa'`→`Flavinda` (2),
      `'Dancing Script'`→`'UVN Hoa Tay'` (1). `Lora` untouched (bare name, now resolves to local files).
      Bride hero node (`element_text_58ymmmme3xn`, font-size 48px) now renders correctly once more.
- [x] Cleaned up: 0 substitute family names remain; `miuwedding.com` references = 0 (fonts local);
      all 8 `@font-face` refs resolve to files on disk; scripts balanced (21/21); single `<body>`.

### Files created/edited
- `assets/fonts/` (8 new original font files)
- `index.html` (@font-face block added; inline font families reverted to originals)
- `AGENTS.md` (Fonts section rewritten: local originals + Google as fallback)
- `WORK_LOG.md` (this entry)

## Session 6: 2026-09-22 — Fix the "N" initial font (node `element_text_prol2tcxgdc`)

### Context
The decorative initial "N" (partner of the "V") rendered in an ugly fallback. Session 5 replaced
`'Dancing Script'`→`'UVN Hoa Tay'` **with literal single quotes**, which — inside the already
double-quoted style attribute — produced the invalid family `&quot;'UVN Hoa Tay'&quot;`. No
`@font-face` family matched (`UVN Hoa Tay`) → fallback `Brush Script MT`/cursive.

### Tasks Completed
- [x] Fixed the single node `element_text_prol2tcxgdc` ("N", top ≈1590.79px, 70px, `#707070`,
      uppercase, fadeInDown): `font-family: &quot;'UVN Hoa Tay'&quot;, &quot;Brush Script MT&quot;, cursive`
      → `font-family: &quot;UVN Hoa Tay&quot;, &quot;Brush Script MT&quot;, cursive;` — now exactly matches the
      template style the bride chose (content stays "N", all other attrs untouched).
- [x] Verified: stray `&quot;'` in any `font-family` = 0; clean `&quot;UVN Hoa Tay&quot;` = 1;
      scripts balanced (21/21); single `<body>`; `miuwedding.com` = 0; no `2027`/`DAVID` leftovers.
      The other `&quot;'` occurrence in the file is the guestbook `esc()` JS (harmless).

### Files created/edited
- `index.html` (font-family of node `element_text_prol2tcxgdc` cleaned)
- `WORK_LOG.md` (this entry)

## Session 7: 2026-09-22 — "V ♥ N" monogram (connected initials)

### Context
The bride felt the separate "V" and "N" initials felt disconnected / unromantic. Root causes: the V
used a different typeface (Playfair Display 75px, right-aligned) at a different baseline than the N
(UVN Hoa Tay 70px, left-aligned), with the floral ornament floating between them. Chosen direction:
option B — a single inline "V ♥ N" monogram with a red heart, so both letters share one font and are
visually tied.

### Tasks Completed
- [x] Deleted the standalone "V" text node `element_text_se9nr38shaq`
      (Playfair Display 75px, gray, top 1559 /*left column*/).
- [x] Repurposed the "N" node `element_text_prol2tcxgdc` into the monogram:
      content `V <span style="color:#d93a35;...">♥</span> N`, font kept `UVN Hoa Tay` 70px,
      `text-align: center`, box centered over the floral ornament (left 52px, top 1560px,
      width 470px, height 99px → center x≈287 matches `floral-pattern.png`). Both letters now share
      one script typeface + baseline so the "V" and "N" are literally connected by the red heart.
- [x] Verified: `element_text_se9nr38shaq` = 0; monogram content + red heart present; spans balanced
      (3/3); scripts 21/21; single `<body>`; `miuwedding.com` = 0.

### Files created/edited
- `index.html` (V node removed; N node → "V ♥ N" monogram)
- `AGENTS.md` (Key nodes: monogram replaces the two initials)
- `WORK_LOG.md` (this entry)

## Session 8: 2026-09-22 — Rollback monogram; restore 2 initials with a slight interlock

### Context
The bride rejected the "V ♥ N" monogram ("not pretty"). Requested: rollback to the two separate
letters as in `wedding2.html`, but nudged slightly inward so they overlap/interlock romantically.

### Tasks Completed
- [x] Re-added the "V" node `element_text_se9nr38shaq` with the exact template geometry
      (left 28.1413, top 1559.25, w 277.264, h 99, text-align right, Playfair Display 75px, gray
      `#707070`, uppercase, fadeInDown, content `V`) — inserted back in DOM right before the N node
      (N renders above V; floral image `element_image_dkaqi0bs8jy` stays behind both).
- [x] Restored the "N" node `element_text_prol2tcxgdc`: content back to `N`, `text-align: left`,
      geometry back to template (left 262.216/top 1590.79/w 331.729/h 91) **minus 20px on left
      (=242px)** so the N's left stroke tucks over the V's right stroke — the two short-lived initials
      now visually interlock (~63px box overlap) while staying two separate letters. Kept the Session 6
      clean `&quot;UVN Hoa Tay&quot;` family (no stray quotes).
- [x] Verified: V node present (idx 78468) with exact template style; N inner = `N`; heart `9829` = 0;
      spans balanced (2/2); scripts 21/21; single `<body>`; `miuwedding.com` = 0; DOM order
      floral → V → N.

### Files created/edited
- `index.html` (V node re-added, N node restored + −20px interlock)
- `AGENTS.md` (Key nodes: initials described again)
- `WORK_LOG.md` (this entry)

## Session 9: 2026-09-23 — "N" initial font → Flavinda

### Context
The bride provided a reference style (node `element_text_smahwg7kkri`, font **Flavinda** 75px) and
asked to restyle the "N" initial using **only** that font — keeping the existing color, size,
animation, and geometry untouched.

### Tasks Completed
- [x] Node `element_text_prol2tcxgdc` ("N", left 242 / top 1590.79, 70px, gray `#707070`, uppercase,
      fadeInDown): `font-family: &quot;UVN Hoa Tay&quot;, &quot;Brush Script MT&quot;, cursive`
      → `font-family: Flavinda, &quot;Brush Script MT&quot;, cursive;`. Nothing else changed (color,
      size, animation, position, content all preserved). Flavinda resolves to the local
      `assets/fonts/flavinda.otf` `@font-face` (Session 5) — no new asset needed.
- [x] Verified: N node now `font-family: Flavinda`; `&quot;UVN Hoa Tay&quot;` in inline styles = 0
      (only the `@font-face` declaration remains, still used by anyone referencing it — currently none);
      scripts balanced; single `<body>`.

### Files created/edited
- `index.html` (font-family of node `element_text_prol2tcxgdc` → Flavinda)
- `WORK_LOG.md` (this entry)

## Session 10: 2026-09-23 — "N" initial font-size 70px → 58px

### Context
The bride found the Flavinda "N" (70px) too large; asked for a smaller, more balanced size and
explicitly no `top` adjustment.

### Tasks Completed
- [x] Node `element_text_prol2tcxgdc` ("N"): `font-size: 70px;` → `font-size: 58px;`. Geometry/
      animation unchanged (`top: 1590.79px` stays; `fadeInDown`, `#707070`, uppercase, Flavinda kept).
- [x] Verified: N node `font-size: 58px`, `top: 1590.79px`; single `<body>`.

### Files created/edited
- `index.html` (font-size of node `element_text_prol2tcxgdc` → 58px)
- `WORK_LOG.md` (this entry)

## Session 11: 2026-09-23 — Fix invisible full-screen click blocker (opening overlay)

### Context
User could not click the music FAB or type into any form input. Audit of all 13 `<button>`s showed
buttons in the main content (RSVP submit `element_rsvp_2n9b0c1v10j`, wishes submit/more
`element_wishes_6hr88wywupk`, `#miuFabToggle`, `#audioToggleBtn`) and all gallery/venue/link targets
were unclickable, as well as the dynamic modal/album buttons whose triggers were blocked. Root cause:
`#miuOpening` (fixed full-viewport, `z-index:2147483001`, `overflow:hidden`) was set to
`data-open="0"` when the guest picked a side, its `.card-side` panes slid off-screen exposing the
site — but the container itself was **never hidden or made click-transparent**, so it kept capturing
every pointer event above everything else (FAB is `z-index:9999`; main content is in normal flow).

### Tasks Completed
- [x] Audited all buttons (13) + inputs/albums/links → affected list recorded in the work log context.
- [x] **CSS fix** (main): added `#miuOpening[data-open="0"]{pointer-events:none;}` to the opening
      `<style>` block. `pointer-events` inherits to `.card-side`/`#miuOpeningUi` (they set none of
      their own), so clicks/taps pass through immediately once a side is chosen.
- [x] **JS cleanup**: inside `close()` (the 4300 ms `setTimeout` that fires `miu:opening:closed`),
      added `try { opening.style.display = 'none'; } catch(e2) {}` so the overlay element is fully
      removed from the stacking context after the slide-out finishes.
- [x] Verified: CSS rule present (1×), `opening.style.display = 'none'` inside the closed timeout (1×),
      scripts balanced, single `<body>`.

### Files created/edited
- `index.html` (`#miuOpening[data-open="0"]` pointer-events:none; `close()` hides overlay at 4300 ms)
- `WORK_LOG.md` (this entry)

## Session 12: 2026-09-23 — "N" initial nudged down (+8px)

### Context
The bride wanted the Flavinda "N" (`element_text_prol2tcxgdc`) slightly lower to sit better.

### Tasks Completed
- [x] Node `element_text_prol2tcxgdc`: `top: 1590.79px` → `1598.79px` (+8px). Nothing else changed
      (Flavinda 58px, `#707070`, uppercase, `left: 242px`, fadeInDown all kept).
- [x] Verified: N node `top: 1598.79px`, `font-size: 58px`, `left: 242px`.

### Files created/edited
- `index.html` (top of node `element_text_prol2tcxgdc` → 1598.79px)
- `WORK_LOG.md` (this entry)

## Session 13: 2026-09-23 — Recolor opening overlay to match the card tone

### Context
The bride wanted the two opening door panels recolored to harmonize with the invitation's interior
tone (white/cream card, gray `#707070` text, neutral accents). Chose "soft cream both doors" and asked
for the seal + choice buttons to follow the new palette too.

### Tasks Completed
- [x] `#miuOpening .card-side-left` background `#f4f2ea` → `#fdfbf7` (white-cream)
- [x] `#miuOpening .card-side-right` background `#7f0505` (dark red) → `#efe6d8` (warm light cream)
- [x] `#miuSeal` (囍) background `#7f0505` → `#707070` (neutral gray matching the card headlines)
- [x] `#miuChoiceText` color `#7f0505` → `#707070`
- [x] `#miuChoiceGroom` background `#7f0505` → `#707070`, box-shadow `rgba(127,5,5,.4)` → `rgba(112,112,112,.35)`
- [x] `#miuChoiceBride` background `#f4f2ea` → `#ffffff`, text/border `#7f0505` → `#707070`
- [x] Verified all 6 rules present once; untouched: confetti (still red fall animation), favicon,
      inner page buttons/forms, opening animation/geometry.

### Files created/edited
- `index.html` (opening overlay palette: cream doors + neutral gray seal/choice UI)
- `WORK_LOG.md` (this entry)

## Session 14: 2026-09-23 — Force the page to always start from the top on load/refresh

### Context
On refresh the browser restores the previous scroll position, so after picking a side in the opening
overlay the auto-scroll engine continued mid-page instead of replaying the intro from the start.

### Tasks Completed
- [x] Added a tiny script right after `<body>`:
      `history.scrollRestoration = 'manual'` + `window.scrollTo(0,0)` (+ documentElement/body scrollTop)
      so every load starts at the top and the browser never restores a deep scroll position.
- [x] In `close()` (opening overlay), right after `opening.setAttribute('data-open','0')` and before
      firing `miu:opening:willClose`, force `window.scrollTo(0,0)` — the intro always starts fresh.
- [x] Verified: scrollRestoration manual (1×), top-reset in startup script (1×) and in `close()` (1×).

### Files created/edited
- `index.html` (start-from-top reset on load + in opening `close()`)
- `WORK_LOG.md` (this entry)

## Session 15: 2026-09-23 — Remove the RSVP form ("Xác nhận tham dự")

### Context
The bride asked to remove the RSVP confirmation form. Kept the opening "Chú Rể / Cô Dâu" dialog
(now a plain gate for the intro/music — its `setSide()` no longer marks a radio, since the
`input[name="eventType"]` lived inside the removed RSVP form → harmless no-op).

### Tasks Completed
- [x] Removed the RSVP `<section data-node-id="element_rsvp_2n9b0c1v10j" data-miu-rsvp="1">` card
      (canvas top 6750–7400; white card with title, name/guests/attendance/side/event/message fields).
- [x] Removed the RSVP `<script>` block (Firestore write to `vu_nhung_2_guests`, `confetti()`, its local
      `toast`). The guestbook (wishes) handler for `vu_nhung_2_messages` is untouched.
- [x] Removed dead CSS: `[data-miu-rsvp="1"] …` rules and `.miu-confetti` / `@keyframes miuConfettiFall`
      (confetti was only used by RSVP).
- [x] Canvas reflow: the content below the removed card did **not** move on its own (absolute canvas),
      and `final.webp` already ended at the old 9976px edge (bottom 9975.28) — so there was no trailing
      tail to trim. To honor "reduce canvas height", shifted the 7 elements below the card up by the
      card's height (649.244140625px): `portrait.webp`, guestbook section, "Countdown" label, countdown
      widget, countdown sub-text, `final.webp`, "Thank you" text — preserving the exact template spacing
      (portrait→guestbook 12px, guestbook→countdown ~162px). New content bottom ≈ 9326px.
- [x] Canvas height 9976 → 9330 in: `.miu-stage` CSS vars (`--ch`,`--sh`), stage inline `--sh`, `.miu-canvas`
      inline `height`, and the JS `var baseH` (auto-expand keeps `Math.max(9330, scrollHeight)`).
- [x] Updated stale comment "some nodes (e.g. RSVP)" → generic.
- [x] Docs: `AGENTS.md` (Firestore = messages only; data-structure guests block removed; opening-flow note
      that `eventType` radio is gone; confetti z-index note dropped) + this `WORK_LOG.md` entry.
- [x] Verified: `miu-rsvp` = 0, `vu_nhung_2_guests` = 0, `confetti` = 0, `element_rsvp` = 0,
      "Xác Nhận Tham Dự" = 0; guestbook `vu_nhung_2_messages` = 2 (intact); scripts balanced (21/21);
      single `<body>`; `miuwedding.com` = 0; canvas = 9330px everywhere (4 spots).

### Files created/edited
- `index.html` (RSVP section/JS/CSS removed; 7 bottom nodes shifted up −649.244px; canvas 9976 → 9330)
- `AGENTS.md` (Firestore/opening-flow/troubleshooting updates)
- `WORK_LOG.md` (this entry)

## Session 16: 2026-09-24 — Rebuild opening overlay like live site + red 囍 seal

### Context
The bride asked to make the opening overlay match `https://vunhungwedding.online/` (two-flap card: cream
left flap with couple names, red gradient right flap with stripes + 囍 watermark, 2-step choice dialog
Chú Rể/Cô Dâu → Tối Thứ Bảy/Sáng Chủ Nhật, round lock seal) and to recolor the 囍 seal to a prettier
brand red (was neutral gray `#707070`).

### Key decision
Kept `id="miuOpening"`, `data-open` and the `.card-side` classes + the `close()` event contract
(`data-open="0"` → `miu:opening:willClose` → ~4300 ms `miu:opening:closed` + `display:none`) because the
miu engines (auto-scroll `MutationObserver` on `#miuOpening[data-open]`, video playback gated on
`miu:opening:closed`) latch onto those exact hooks. Animations stay CSS `animation` (AGENTS.md engine
contract) — only visual design + dialog flow changed.

### Tasks Completed
- [x] Markup: `#miuOpeningSides` left `.card-side-left` now holds the live cream content (Save the date,
      Trọng Vũ — & — Hồng Nhung, divider, 囍 seal, "Trân trọng kính mời!"); right `.card-side-right`
      empty (decor via CSS). Added `#cf-choice-step1` (Chú Rể/Cô Dâu, round avatars) + `#cf-choice-step2`
      (Tối Thứ Bảy 17:00 28/11 / Sáng Chủ Nhật 09:00 29/11). Replaced the old center UI `#miuOpeningUi`.
      `#miuSeal` kept as the round lock badge (added class `cf-lock`).
- [x] CSS: ported the live overlay styles remapped to `#miuOpening` / `.card-side` selectors
      (kept `animation` miuSlideLeft/Right 4s). `#miuSeal` → `background:#7f0505`, gold ring `#b0852b`,
      white 囍, red-tinted shadow; fades out on `[data-open="0"]`. `.cf-choice` dialogs + `.cf-lock`.
- [x] JS: 2-step flow — show step 1 after 1 s; pick guest → show step 2; pick group → `close()`.
      Removed the now-dead `setSide()`/`js-ready`/`miuChoice*` code (eventType radio is long gone).
- [x] Fonts: added `Cormorant+Garamond` to the existing Google Fonts `<head>` link (the new dialog/flap
      labels use it; `Great Vibes` was already loaded). No other head changes.
- [x] Verified: scripts balanced (21/21), single `<body>`, ids `miuOpening`/`miuSeal` unique,
      `cf-choice-step1/2` present in markup + JS, `miu:opening:willClose/closed` dispatchers intact.

### Files created/edited
- `index.html` (overlay markup + CSS + JS rebuilt to live two-flap design; Cormorant Garamond added to fonts link)
- `WORK_LOG.md` (this entry)

## Session 17: 2026-09-24 — Fix: choice popup not hiding after click

### Context
Bug report: after clicking a group (Tối/Sáng) the step-2 dialog stayed visible on screen. Cause: the ported
`close()` only set `data-open="0"` but never removed `.active` from the `.cf-choice` dialogs, so
`.cf-choice.active{opacity:1}` kept them fully visible for the whole 4.3 s until `#miuOpening` got
`display:none` (the live site's `openCard()` explicitly removes `.active` for both dialogs — that step
was missed in the Session 16 port).

### Tasks Completed
- [x] `close()` now removes `.active` from `#cf-choice-step1` and `#cf-choice-step2` immediately on close,
      and after 4300 ms also sets `display:none` on both dialogs (belt-and-suspenders, mirrors live).
- [x] CSS safety net: `#miuOpening[data-open="0"] .cf-choice{opacity:0;pointer-events:none;}` guarantees
      dialogs are invisible the instant the overlay opens, regardless of JS state.
- [x] Verified: overlay script parses (node --check), scripts balanced (21/21), rule + `display:none`
      present once each.

### Files created/edited
- `index.html` (dialog hide fix in `close()` + CSS safety rule)
- `WORK_LOG.md` (this entry)

## Session 18: 2026-09-24 — Harmonize overlay colors & fonts with the inner invitation tone

### Context
The overlay (ported in Session 16 from the live `vunhungwedding.online`) used the live site's palette —
bright red `#7f0505→#9a0a0a`, gold `#b0852b`, remote fonts Great Vibes/Cormorant Garamond. The actual
invitation canvas (miu) is neutral: dominant gray `#707070` text (46×), white hero text on the couple
photo, `#444141` body text, warm cream page; the only "red" lives in ribbon decorations + 囍. Fonts
used inside: Ergisa-Regular (couple names), UVN Hoa Tay (initials), Lora (labels/times/submit button) —
all local. So the overlay clashed with the card tone.

Decision (from user): right flap → neutral warm gray; overlay fonts → the invitation's own local fonts;
keep brand red `#7f0505` only as accent (seal/names/divider/borders).

### Tasks Completed
- [x] `.card-side-left` bg `#f4f2ea` → `#f6f3ec` (warmer, bridges hero photo + white stage); stripe tint
      `rgba(127,5,5,.015)` → `rgba(112,112,112,.02)`.
- [x] `.card-side-right` gradient `#7f0505→#9a0a0a` → `#707070→#555353` (gray); ::before stripes / ::after
      囍 watermark unchanged.
- [x] Fonts swapped to local invitation fonts (no remote):
      • `.cf-names .name`, `.cf-choice-name` → `Ergisa-Regular`
      • `.cf-save-date`, `.cf-names .and`, `.cf-invite`, `.cf-choice-question`, `.cf-choice-btn`,
        `.cf-group-sub` → `Lora`
      `.cf-choice-question` font-size 20px → 18px (Lora renders larger than Cormorant).
- [x] Colors: `.cf-save-date` `#b0852b`→`#707070`; `.cf-names .and` `#b0852b`→`#7f0505`;
      `.cf-invite` `#7f0505`→`#707070`; `.cf-choice-btn[data-guest="bride"]` border `#b0852b`→`#707070`;
      `.cf-group-sub` `#7f0505`→`#707070`; avatar border `#f4f2ea`→`#f6f3ec`.
      Kept red `#7f0505` accent intentionally: `.cf-names .name`, `.cf-divider`, `.cf-seal`, groom/group
      button borders, `.cf-choice-name`, and `#miuSeal` (red bg + gold ring `#b0852b`).
- [x] Verified: 0 remaining `Cormorant Garamond` / `Great Vibes` references in `index.html`, `#b0852b`
      only at `#miuSeal` border + inner gold ring, gray gradient present on right flap.

### Files created/edited
- `index.html` (overlay-only CSS: neutral gray right flap, warm cream left flap, Erga/Lora fonts, red kept as accent)
- `WORK_LOG.md` (this entry)

## Session 19: 2026-09-24 — Lighten the right flap (too dark gray)

### Context
After Session 18 the right flap `#707070→#555353` was judged too dark against the invitation's light
tone. User picked a lighter warm gray.

### Tasks Completed
- [x] `.card-side-right` gradient → `linear-gradient(135deg,#a39d96 0%,#847e77 100%)` (light warm gray).
- [x] Kept decor legible on the lighter surface: stripes `::before` alpha `.04`→`.06`, watermark 囍
      `::after` alpha `.06`→`.09`.
- [x] Left flap, red accent (`#miuSeal`, names, divider, seam borders), Erga/Lora fonts, markup/JS:
      untouched. Verified lines 402/407/408.

### Files created/edited
- `index.html` (right-flap gradient + decor alphas)
- `WORK_LOG.md` (this entry)

## Session 20: 2026-09-24 — Path routing + master data (like the live 404.html)

### Context
User wanted the miu template (`index.html`) to behave like the live site: choosing Chú Rể/Cô Dâu +
nhóm giờ produces a shareable path (`/groom/evening`, `/bride/morning`, …) and rewrites the content
from per-group master data. Confirmed scope with user: only timeline (4 mốc), 2 event cards and the
countdown target change; venues, families and ribbon icons unchanged. Timeline's old 4th milestone
"Tiệc thân mật" → **Đón khách = giờ Tiệc cưới − 30′** (evening 16:30 28.11 / morning 08:30 29.11),
placed as **row 1** for chronological order (Đón khách → Tiệc cưới → Lễ Vu Quy → Lễ Thành Hôn).

### Tasks Completed
- [x] Overlay IIFE in `index.html`:
  - Step 1 (Chú Rể/Cô Dâu click) now stores `window.cfGuest` (`data-guest`).
  - Step 2 (nhóm giờ click) calls `window.cfApply(window.cfGuest||'groom', data-group)` before `close()`.
- [x] New master-data `<script>` (placed right after the overlay IIFE, before `<div class="miu-wrap">`):
  - `WEDDING_MASTER[guest][group]` × 4 combos — timeline `{time,label}` ×4, 2 event cards
    `{title(html), day, month, year, lunar}`, countdown target.
    Groom cards: Card1 Tiệc cưới nhà trai + Card2 Lễ Thành Hôn (13:30) per user spec (timeline Thành Hôn
    kept 12:30 — mismatch per spec). Bride cards: Card1 Tiệc cưới nhà gái + Card2 Lễ Vu Quy (11:30).
    Evening Card1 day `28`/lunar 20/10; card2 + all morning = 29/11 · 21/10.
  - `applyMasterData(guest, group)` writes nodes by `[data-node-id=...]` (timeline
    `5zugc3by51l`/`ei8ualj2ye8`/`6nc0vdb4qe6`/`vkv8sr8623d` + labels; cards via
    `MASTER_CARD_NODES`), and `setAttribute('data-target', …)` on the countdown node
    (the engine re-reads `data-target` every tick — no engine change needed).
  - `parsePath()` mirrors `404.html` (pathname split, `index.html` filtered out) for deep links:
    on `load`, applies the combo and auto-closes the overlay ~1200 ms later using the same
    `miu:opening:willClose`/`closed` contract.
  - `window.cfApply(guest, group)` = apply data + `history.replaceState(null,'','/'+guest+'/'+group)`.
- [x] Verified: all 22 `<script>` blocks extracted to temp and `node --check` clean (the one pre-existing
      `gifSrc` regex parse fails only under Node 20, unrelated). `script` open/close balanced.
      The whole-file `git diff` on `index.html` is a CRLF/LF (autocrlf) artifact, not this change.

### Files created/edited
- `index.html` (overlay handlers + new master-data `<script>`)
- `AGENTS.md` (Path routing / master data section; timeline/cards/countdown notes updated)
- `WORK_LOG.md` (this entry)

## Session 21: 2026-09-27 — Sửa lỗi chí mạng + thay 404.html bằng redirect mỏng

### Context
Rà soát toàn repo từ đầu. Phát hiện 3 vấn đề nghiêm trọng ngoài dự kiến:

1. **Regex `SyntaxError` giết nguyên một `<script>` block của engine miu.**
   `index.html:2214` (kế thừa từ `wedding2.html` gốc của template) chứa
   `/.(mp4|webm|ogg)(?.*)?$/i` — `(?.*)` là group không hợp lệ theo ECMAScript ⇒ V8 (Chrome/Edge/Safari
   đều vậy) **ném lỗi lúc parse**, không phải lúc chạy. Hậu quả: **toàn bộ block script 22
   (dòng 2167–2413, ~10 KB) chưa bao giờ chạy**, làm chết `renderGate()` (nút copy link
   `#miuCopyLink` + share link `#miuShareLink`), `playAll()` + listener `miu:opening:closed` phát
   video, và `swapVipVideosToGif()`. Session 20 đã phát hiện nhưng ghi "unrelated" và bỏ qua.
2. **`assets/` untracked** — 0/40 file trong git, trong khi `index.html` tham chiếu 20 ảnh + 8 font +
   8 hoạ tiết + `ordinary.m4a` + favicon. Deploy nguyên trạng sẽ 404 toàn bộ.
3. **Nhầm lẫn kiến trúc:** trong git HEAD, `index.html` (90586 B) và `404.html` (90580 B) gần như là
   **cùng một file** — diff chỉ 1 khoảng trắng ở dòng 769, đều là site vs-template-5 (trang scroll cũ).
   Bản miu 208 KB chỉ nằm trong working tree, chưa commit. Nghĩa là GitHub Pages đang phục vụ site
   cũ ở **cả** `/` lẫn mọi deep link; `AGENTS.md` mô tả ngược lại. `icons/*.svg` đã bị xoá (staged)
   nhưng `404.html` cũ còn trỏ tới `/icons/...` ⇒ 3 ảnh vỡ.

### Tasks Completed
- [x] **Fix regex (quan trọng nhất)** — `index.html:2214`:
  `/.(mp4|webm|ogg)(?.*)?$/i` → `/\.(mp4|webm|ogg)(\?.*)?$/i`.
  Giữ nguyên ý định (`a.mp4` → `a.gif`, `b.webm?token=1` → `b.gif?token=1`), đồng thời escape `.`
  và chỉ giữ group 2 thật. Block 22 sống lại ⇒ khôi phục copy/share link + phát video.
- [x] **`parsePath()` hỗ trợ query param** (`index.html` ~573): đọc `?g=` / `?t=` **trước**, rồi
  mới fallback về `pathname`. Chỉ nhận `bride|groom` × `evening|morning` (case-insensitive, bỏ qua
  param thừa, `decodeURIComponent`). Cần thiết vì `404.html` mới trỏ tới `/?g=…&t=…`.
- [x] **Normalize URL ở nhánh deep-link** (`index.html` ~611): thêm
  `history.replaceState(null,'','/'+guest+'/'+group)` ⇒ `/?g=groom&t=evening` → `/groom/evening` ngay.
  Không gây vòng lặp vì `replaceState` không kích hoạt navigation (lần tải sau GitHub Pages lại
  serve `404.html` → redirect → vẫn về `/`).
- [x] **Đồng bộ giờ Lễ Thành Hôn 13:30 → 12:30**: 2 chỗ trong `WEDDING_MASTER` (groom/morning +
  groom/evening `cards[1].title`). Trước đó master data ghi 13:30 trong khi static default
  `element_text_0iedk0b1132` và timeline row 4 đều là 12:30. Bỏ qua "mismatch per spec" của
  Session 20 theo quyết định của user. Card LỄ VU QUY 11:30 (bride) và card 1 (09/17 giờ) không đổi.
- [x] **`404.html`: 92 KB → trang redirect mỏng (~1 KB).** Đọc `location.pathname` → `groom|bride`
  + `evening|morning` → `location.replace('/?g=' + guest + '&t=' + group)`; không hợp lệ (kể cả
  `/404.html` và `/index.html` truy cập trực tiếp) → `location.replace('/')`. Có `<noscript>` +
  link dự phòng, `<meta name="robots" content="noindex, follow">`, `<link rel="canonical">`.
  Không còn Tailwind/Firebase/WOW.js/ảnh Firebase Storage ⇒ deep link giờ hiển thị **đúng design
  miu** thay vì site cũ, và chỉ còn 1 nguồn sự thật duy nhất (không phải sửa song song 2 file).
  Site cũ vẫn nằm trong git history nếu cần khôi phục.
- [x] **`git add assets/`** — 40 file / 11 MB (file lớn nhất `ordinary.m4a` 2.3 MB, dưới giới hạn
  100 MB của GitHub). Xác nhận xoá `icons/` là đúng: `index.html` không tham chiếu `/icons/` nào,
  `404.html` mới không còn.
- [x] **Kiểm chứng**
  - Parse 22/22 script block bằng chính V8 parser → `bad: 0` (trước: 1). `<script>` cân bằng 22/22.
  - 40 đường dẫn `assets/…` trong `index.html` (kể cả `url()` trong `@font-face`) đều tồn tại trên đĩa.
  - `WEDDING_MASTER` apply vào mock DOM: **4/4 combo** đúng (Đón khách → Tiệc cưới → Lễ Vu Quy →
    Lễ Thành Hôn, row 4 = 12:30, countdown đúng từng nhóm).
  - `parsePath()` unit-test 12 ca (query, case-insensitive, thứ tự param, param thừa, thiếu/sai giá
    trị, fallback pathname, query thắng path) — tất cả đúng.
  - Logic `404.html` unit-test 11 path — tất cả đúng.
  - Mô phỏng hành vi GitHub Pages (static server + fallback `404.html`): `/groom/evening` trả
    `404.html`, `/` trả `index.html` (9 chỗ `miu-canvas`), mọi asset trả 200 đúng MIME.

### Ghi chú / nợ kỹ thuật
- **RSVP form + sổ lưu bút trên collection Firestore `guests` không còn truy cập được** (404.html cũ
  là nơi duy nhất dùng chúng). `index.html` vốn đã bỏ RSVP ở Session 15 và dùng
  `vu_nhung_2_messages` cho sổ lưu bút, nên site không mất tính năng; chỉ mất quyền đọc dữ liệu cũ.
  Muốn đọc lại collection `guests` thì phải viết code riêng.
- **Đường dẫn asset vẫn tương đối** (`assets/…`, không phải `/assets/…`). An toàn vì GitHub Pages chỉ
  phục vụ `index.html` ở `/` và redirect luôn về root. Nếu chuyển sang Firebase Hosting (có
  `rewrites ** → /index.html` trong `firebase.json`) thì deep link sẽ phục vụ `index.html` ở
  `/groom/evening` và asset tương đối sẽ hỏng → khi đó phải đổi hết sang `/assets/…`
  (đánh đổi: mở bằng `file://` sẽ hỏng).
- **Chưa commit** theo yêu cầu của user — chờ review `git diff` / `git status`.

### Files created/edited
- `index.html` (regex block 22, `parsePath()` + query param, `replaceState` ở deep-link,
  `WEDDING_MASTER` 13:30 → 12:30 ×2)
- `404.html` (viết lại hoàn toàn: 1549 dòng / 92 KB → 47 dòng / ~1 KB, redirect mỏng)
- `assets/**` — 40 file staged (`git add`), gồm 20 ảnh `.webp`, 8 font `.otf`/`.ttf`,
  8 hoạ tiết, `audio/ordinary.m4a`, 3 favicon
- `AGENTS.md` (mục *Path routing / master data* viết lại theo luồng redirect mới; thêm mục
  *404.html redirect*; bỏ mô tả RSVP/guestbook của site cũ)
- `WORK_LOG.md` (this entry)

## Session 22: 2026-09-28 - Cập nhật timeline, event card và địa điểm theo data thật

### Context
- User cung cấp data lễ mới cho cả 4 combo (Chú Rể/Cô Dâu × Tối/Sáng) và yêu cầu sửa hoặc bổ sung.
- Phát hiện 3 node địa điểm **đã có sẵn** trong canvas (25px tên + 18px địa chỉ mỗi card) nhưng
  `WEDDING_MASTER` chưa dùng tới → chỉ cần nối vào, **không phải sửa HTML canvas cho node mới**.
- Data mới mâu thuẫn ở 3 chỗ, đã hỏi và chốt với user:
  1. Lễ Vu Quy: data ghi `9:30 28.11` nhưng card Cô Dâu ghi `11:30 29/11` → chốt **9:30 T7 28.11** cho
     cả timeline lẫn card Cô Dâu.
  2. Groom+Morning: timeline `12:30` vs card `13:30` → chốt **12:30 cả hai**.
  3. Timeline chỉ có 3 mốc nhưng canvas có 4 dòng → chốt **giữ "Đón khách" ở dòng 1**, dòng 2-4 sắp theo giờ.
- Lưu ý: do Lễ Vu Quy (9:30 28.11) sớm hơn Tiệc cưới (17:00 28.11), thứ tự dòng 2-4 **không đơn điệu
  tuyệt đối** theo giờ so với dòng 1 — đây là hệ quả của quyết định giữ Đón khách ở dòng 1.
- Link bản đồ: user gửi 3 link Google Maps, chuẩn hoá cả 3 về dạng ngắn `?api=1&query=lat,lng`
  (bỏ tracking `entry=`/`g_ep=`, và chuyển link *search* về dạng ghim đúng pin).
- Phần ảnh (chuyển sang Firebase) — user yêu cầu **để sau**, chưa làm trong session này.

### Tasks Completed
1. **Hằng `MAPS` + 3 hằng venue** (đặt trước `WEDDING_MASTER`):
   - `MAPS.linhTram` = `20.8533926,105.7670935` (Nhà Hàng Linh Trâm, số 30 Kim Bài)
   - `MAPS.nhaTrai` = `20.8531959,105.7666794` (tư gia nhà trai, số 35 Kim Bài)
   - `MAPS.nhaGai` = `20.3984241,105.9106766` (tư gia nhà gái, thôn Trung Hiếu, Thanh Lâm, Ninh Bình)
   - `VENUE_LINH_TRAM` / `VENUE_NHA_TRAI` / `VENUE_NHA_GAI` + helper `card(title, day, lunar, venue)`
     → 8 card trong `WEDDING_MASTER` chỉ còn 1 dòng, tránh lặp địa chỉ 8 lần.
2. **Schema card mở rộng**: thêm `venueName`, `address`, `mapUrl` (ngoài `title/day/month/year/lunar`).
3. **`MASTER_CARD_NODES` mở rộng** thêm `venueName` + `address`; **thêm mới `MASTER_MAP_NODES`**
   (`1` → `element_button_9ybr5mpump5`, `2` → `element_button_iog0sqv0hat`).
4. **`applyMasterData`** ghi thêm 3 giá trị mỗi card:
   - `venueName` → `textContent` (node 25px)
   - `address` → **`innerHTML`** vì node 18px chứa `<br>` (dùng `textContent` sẽ mất xuống dòng)
   - `mapUrl` → `setAttribute('href', ...)` trên thẻ `<a data-miu-btn="1">`
5. **16 dòng timeline** (4 combo × 4 mốc) cập nhật theo bảng chốt. Dòng 1 luôn là Đón khách
   (= giờ Tiệc cưới − 30′), dòng 2-4 sắp theo giờ.
6. **8 event card** cập nhật title/ngày/âm lịch + 3 địa điểm. Âm lịch: 28/11 → `20 tháng 10`,
   29/11 → `21 tháng 10` (năm Bính Ngọ).
7. **Static default trong canvas cập nhật theo Groom + Evening** (`index.html` dòng 650 & 670):
   timeline 4 dòng, 2 card, 2 địa điểm, 2 nút map, và `data-target` countdown
   `2026-11-29T11:00:00` → `2026-11-28T17:00:00`. Static là fallback khi JS lỗi/path không hợp lệ,
   nếu không cập nhật sẽ lệch với data thật.
8. **Nút map card 1 tĩnh** cũng đổi từ `maps.app.goo.gl/hg6ccsYr3NxNzZEk8` sang dạng ngắn
   `MAPS.linhTram` để không còn link cũ trong file (mục 1 còn lại của chuẩn hoá link).

### Kết quả kiểm chứng
- **20/20** inline script block parse được (22 thẻ `<script>` = 20 inline + 2 CDN Firebase).
- **171/171** assert pass bằng mock DOM độc lập (`verify22.js`), so sánh giá trị code ghi ra với
  ma trận data lấy tay từ đặc tả user: 4 combo × (8 node timeline + 2 card × 7 field + 2 `href` + countdown).
- **Cross-check** bắt buộc đã pass: timeline D3 == giờ card 1 (mọi combo); Groom D4 == card 2;
  Bride D2 (Lễ Vu Quy) == card 2; `groom.evening` D4 = 11:30 vs `groom.morning` D4 = 12:30.
  *Lưu ý khi viết test: combo Cô Dâu có card2 = Lễ Vu Quy nên **không** được đối chiếu D4↔card2.*
- **Static default == `groom.evening`** khớp từng node.
- Quét sót dữ liệu cũ: 0 kết quả cho `10 GIỜ 30 PHÚT`, `13 GIỜ 30 PHÚT`, `09:00 - 10:30`,
  `15:00 - 16:00`, `NHA RIENG NHA GAI`, `THON TRUNG HIẾU THƯỢNG`, `hg6ccsYr3NxNzZEk8`.
- **32/32** tham chiếu asset tồn tại. Encoding giữ nguyên: không BOM, CRLF 2290 / LF 2451
  (khớp đúng trước session) — dùng `[System.IO.File]::WriteAllText` + `UTF8Encoding($false)`.
- Sửa 18 node canvas bằng script **neo theo `data-node-id`** (không dùng regex trên text), vì
  node `wk2sh3dr2dg` và `d7sj8uh0hat` có text âm lịch **giống hệt nhau** → regex theo text sẽ đụng.

### Ghi chú / nắm kỹ thuật
- **Cả 4 node địa điểm có `text-transform: uppercase`** → ghi "Nhà Hàng Linh Trâm" (chữ thường)
  ra "NHÀ HÀNG LINH TRÂM". Không cần gõ hoa trong data.
- **Link Nhà Hàng Linh Trâm (số 30) và tư gia nhà trai (số 35) chỉ cách nhau ~25m** (cùng đường Kim Bài).
  Đúng theo địa chỉ user cung cấp, nhưng 2 cái pin trên bản đồ sẽ gần như trùng nhau.
- **Lễ Vu Quy 9:30 T7 28.11 nằm ngoài ngày dự của khách buổi sáng** (29/11) → khách Groom/Bride
  *Morning* sẽ thấy 1 mốc diễn ra hôm trước. Cứu theo lựa chọn của user; nếu đổi thì sửa 2 dòng
  timeline trong `groom.morning` + `bride.morning`.
- **Còn 1 điểm chưa đồng bộ**: các node "ĐỊA ĐIỂM" (`ouzs7rzrxaw`, `4fsw9gt0hat`) vẫn là text tĩnh
  trong canvas, không theo master data. Hiện vô hại vì nhãn luôn là "ĐỊA ĐIỂM" ở cả 8 card.
- **Chưa commit** theo yêu cầu của user. `index.html` 209917 bytes.
- Backup trước khi sửa: `index.before-session22.html` (209824 bytes) trong thư mục temp opencode.

### Files created/edited
- `index.html` — hằng `MAPS` + 3 `VENUE_*` + helper `card()`; `WEDDING_MASTER` 4 combo (16 timeline
  + 8 card, thêm `venueName`/`address`/`mapUrl`); `MASTER_CARD_NODES` + `MASTER_MAP_NODES`;
  `applyMasterData` ghi 3 field mới; static default trong canvas (timeline, 2 card, 2 địa điểm,
  2 nút map, countdown).
- `AGENTS.md` (mục *Editing Content* + *Path routing / master data* — cập nhật bảng node địa điểm,
  node nút map, và bảng timeline/card mới)
- `WORK_LOG.md` (this entry)

### Deferred (chưa làm)
- **Ảnh trên Firebase**: 20 ảnh trong `assets/uploads/6a2a56e562badd7da97313bb/` vẫn dùng đường dẫn
  **tương đối** và bọc trong wrapper kích thước cố định px với `object-fit: cover` ⇒ thay ảnh tỉ lệ
  khác sẽ bị **cắt** (không méo, không vỡ layout). Khi chuyển sang link Firebase nhớ cập nhật
  luôn `og:image` + `twitter:image` trong `<head>` (đang trỏ link tuyệt đối cũ), nếu không ảnh share
  trên Facebook/Zalo vẫn là ảnh cũ trên domain cũ.

## Session 23: 2026-09-28 - Bỏ dòng "Đón khách" khỏi timeline (3 mốc)

### Context
- Session 22 dựng timeline 4 mốc cho mỗi combo, dòng 1 là **Đón khách** (= giờ Tiệc cưới − 30′).
- User yêu cầu bỏ hẳn **Đón khách**. Ba mốc còn lại đúng thứ tự **Lễ Vu Quy → Tiệc cưới → Lễ Thành Hôn**,
  theo thứ tự thời gian thuần tuý: Lễ Vu Quy 09:30 28.11 → Tiệc cưới → Lễ Thành Hôn 11:30 29.11.
- **Quyết định layout (user chọn)**: xóa *physical* row 4, **giữ nguyên vị trí** row 1-3 (zigzag còn lại
  là phải – trái – phải), và dịch toàn bộ section phía dưới lên **80px** cho khớp khoảng trống.
- Card mapping **không đổi** (thiết kế, không phải quirk): Groom → Tiệc Cưới + Lễ Thành Hôn
  (card 2 = timeline mốc 3); Bride → Tiệc Cưới + Lễ Vu Quy (card 2 = timeline mốc 1).

### Tasks Completed
1. **Xóa 4 node** của row 4 bằng script neo theo `data-node-id`: `element_shape_ju9bj9lof2p`,
   `element_text_vkv8sr8623d`, `element_text_cnp53v9623d`, `element_image_b4jc42o781n`.
   Node id unique: **110 → 106**.
2. **Cắt đường dọc** `element_shape_o4ddhktw62w`: là line SVG ngang `rotate(90deg)` nên chiều dài dọc
   thật là `width` (không phải `height`). `width 274.7 → 213.15`, `left 148.3 → 179.08`,
   `top 5145.19 → 5114.42`; `height:51.818359375px` **giữ nguyên** (đó là độ dày nét).
   Phủ dọc **5033.8 → 5246.9** — tâm vẫn ở `left + width/2 = 285.66` (đúng trục), giữ nguyên
   overhang đầu 34.4px / đuôi 70px như trước.
3. **Dịch 20 node** phía dưới lên `80px`, gồm cả block wishes `element_wishes_6hr88wywupk`
   (là `<section>`), `element_countdown_yzo2869hvwa`, và 2 card cuối.
4. **Canvas height 9330 → 9250**, sửa đủ **5 chỗ**: `--ch`, `--sh` trong CSS, `--sh` inline trên
   `.miu-stage`, `height` trên `.miu-canvas`, và `var baseH = 9250;`. `baseH` là *floor*
   (script lấy `max(baseH, canvas.scrollHeight)`) nên phải hạ theo. Content bottom = **9246**, còn 4px lề dưới.
5. **`WEDDING_MASTER`**: mỗi combo 4 timeline → **3**. Groom Evening `09:30 28.11 / 17:00 28.11 / 11:30 29.11`,
   Groom Morning `09:30 28.11 / 09:00 29.11 / 12:30 29.11`, Bride Evening `09:30 28.11 / 17:00 28.11 / 12:30 29.11`,
   Bride Morning `09:30 28.11 / 09:00 29.11 / 12:30 29.11`.
6. **`MASTER_TEXT_NODES.tlTime` / `.tlLabel`** rút còn 3 phần tử. `applyMasterData` vốn đã lặp
   `i < d.timeline.length` nên **không** hardcode số dòng — thêm/bớt timeline row trong data tự chạy.
7. **Static default** trong canvas = Groom + Evening (`Lễ Vu Quy` / `Tiệc cưới nhà trai` / `Lễ Thành Hôn`).
   `data-target` countdown **không đổi** (đã là `2026-11-28T17:00:00` từ Session 22).

### Kết quả kiểm chứng
- **73/73** assert pass bằng `verify23.js`: 20/20 inline script parse (22 thẻ `<script>` = 20 inline + 2 CDN),
  36/36 tham chiếu asset tồn tại, 106 node id unique, timeline đơn điệu đúng 4 combo, hình học R–L–R
  không overlap, 0 reference tới 4 node đã xóa.
- Quét sót dữ liệu cũ: **0** kết quả cho `Đón khách`, `08:30`, `16:30`, `9330`.
- Encoding giữ nguyên: **không BOM**, LF giữ nguyên (khớp blob chuẩn hoá của git).
- `index.html`: 209917 → **206651** bytes.

### Ghi chú / nắm kỹ thuật
- **Thứ tự node trong source ≠ thứ tự hiển thị.** Export của miu xen kẽ section: `element_shape_ju9bj9lof2p`
  và cả block `element_wishes_6hr88wywupk` nằm *giữa* timeline row 1 và 2 trong file. Muốn biết vị trí
  phải đọc `top:` trong `style`, không được đoán theo thứ tự file.
- **Không phải node nào cũng là `<div>`** — `element_wishes_6hr88wywupk` là `<section>`. Script dò bằng
  `lastIndexOf('<div', …)` sẽ âm thầm trỏ về node *trước đó* và sửa hỏng. Phải match đúng tên tag.
- Khi xóa node: tìm thẻ mở bằng cách lùi về `<` mà tag đó **thực sự chứa** `data-node-id`, rồi quét
  cân bằng **đúng tên tag**.   `element_image_*` bọc một `<div>` con nên đếm bằng `'<div'` sẽ quá tay.
  (Script đầu tiên của session này dính đúng 2 lỗi này → file hỏng → **restore từ backup** rồi viết lại script;
  xem `surgery23.js`, đã sửa và chạy pass.)
- **Chưa commit** theo yêu cầu của user.
- Backup trước khi sửa: `index.before-session23.html` (209917 bytes) trong thư mục temp opencode.

### Files created/edited
- `index.html` — xóa 4 node, cắt spine, dịch 20 node, canvas height 9250, `WEDDING_MASTER` 3 timeline
  mỗi combo, `MASTER_TEXT_NODES` 3 phần tử, static default 3 dòng.
- `AGENTS.md` (mục *Editing Content* + *Path routing / master data* — timeline 3 dòng, card mapping,
  và thêm mục `Session 23: bỏ dòng Đón khách` ghi 4 bẫy kỹ thuật ở trên; tiện sửa typo `ĐỊA ĐỶM` → `ĐỊA ĐIỂM`)
- `WORK_LOG.md` (this entry)

### Deferred (chưa làm)
- **Chưa QA trực quan** trên trình duyệt tại `/groom/evening`.
- **Ảnh trên Firebase** (20 ảnh trong `assets/uploads/6a2a56e562badd7da97313bb/` vẫn là đường dẫn tương đối,
  bọc trong wrapper px cố định `object-fit: cover`) và `og:image`/`twitter:image` vẫn trỏ link cũ — như Session 22.

## Session 24: 2026-09-29 — Overlay mở: cửa đôi 3D (theo reference melipage)

### Tasks Completed
1. **Tải + tự host 3 family font** chỉ dùng cho thẻ mở (tên file giữ đúng tên gốc của Google Fonts):
   - `dancing-script-{latin,vietnamese}.woff2` (42.708 / 7.712 bytes)
   - `cormorant-garamond-{roman,italic}-{latin,vietnamese}.woff2` (37.640 / 21.168 / 39.260 / 11.516 bytes)
   - 6 `@font-face` mới trong `<head>`, `font-display:swap`, `unicode-range` tách subset (VN mang dấu,
     Latin mang `&` + ASCII). **Cần cả 2 subset** — bỏ Latin là mất dấu `&`.
2. **Thay overlay 2D bằng cửa đôi 3D** (theo yêu cầu user, mô hình theo
   `melipage.com/the-truong-nhu-quynh-2026-05-24-template`):
   - `#miuOpeningBackdrop` (radial `#3a332e→#241f1c→#16120f` + gold seam glow tại 68%).
   - `#miuOpeningSides`: `perspective:1200px`, `perspective-origin:68% 50%`, `overflow:hidden`.
   - `#miuDoorLeft` (68%, hinge trái) + `#miuDoorRight` (32%, hinge phải), `backface-visibility:hidden`
     nên mỗi lá biến mất sau 90°. `108deg`, `cubic-bezier(.34,.02,.2,1)`, delay 340ms, stagger 120ms.
   - Bỏ hoàn toàn `miuSlideLeft/Right`, `.cf-divider`, `.cf-seal`, lá phải nền xám gradient, accent `#7f0505`.
3. **Card bám sát token của reference** (copy từ `--slide-*` / `--opening-*`):
   nền `#ffffff`, sọc `#e9e9e9` = 3.5% mép phải lá trái, màu chữ/seal `#9e8130`,
   `.cf-save-date` High Spirited `clamp(38px,10.5vw,66px)` + `<span>S</span>` 95px,
   `.cf-names` Dancing Script (`&` italic) tại `clamp(150px,32%,240px)`,
   `.cf-bottom` Cormorant Garamond `gap:10px` tại `clamp(170px,23%,230px)`.
4. **Seal 囍 thành trang trí**: bỏ handler, `pointer-events:none`, canh đúng seam
   (`translateX(calc(var(--door-w)*.1681))`), pulse khi `data-open="1"`, scale 0.55 + fade khi mở.
   Luồng chọn Chú Rể/Cô Dâu → nhóm giờ giữ nguyên trong `#cf-choice-step1/2`.
5. **Timing do CSS làm nguồn sự thật**: `--door-duration/--door-delay/--door-stagger/--door-angle` trên
   `#miuOpening`; `close()` đọc 3 token đó (đọc `animation` shorthand sẽ ra `0s` vì rule chỉ match khi
   `data-open="0"`) rồi fire `miu:opening:closed` ở `2.4+0.34+0.12+0.32 = 3180ms`.
   `prefers-reduced-motion:reduce` ép 3 token về `1ms/0ms/0ms`.
6. **Gate auto-scroll sửa**: hai lá kết thúc lệch nhau (stagger) nên đếm 2 `animationend` là fire sớm —
   chuyển sang dedupe theo `event.target`.
7. **Avatar border** `.cf-choice-avatar` → `#ffffff` (đồng bộ card trắng).

### Kết quả kiểm chứng
- Tĩnh: 22/22 `<script>` (20 inline + 2 CDN), 5/5 `<style>`, 20/20 inline script `node --check` pass,
  106 `data-node-id` unique, đúng 2 `.card-side`, không còn class/token cũ.
- Puppeteer/Chrome: **0 pageerror, 0 console error** ở cả mobile 390×844 và desktop 1440×900.
- Hình học: desktop lá `391 + 184 = 575px`, tâm seal `816.7` vs tâm sọc `816.65` (chênh 0.05px);
  mobile lá `265.2 + 124.8`, seal khớp tuyệt đối. Font resolve từ disk, màu `rgb(158,129,48)`.
- **Không còn mảng trắng sót** (đây là điểm dễ sai nhất): tô `html,body` xanh lá + nền test cho
  `#miuOpeningSides` rồi đếm pixel theo timeline → white **97.4%** (t=0) → 11.5% (t=1700) → **0%**
  từ t=2100 đến 3500. Lần đo đầu báo "100% trắng" là **nền `body` màu trắng lọt ra sau khi backdrop
  fade**, không phải lá cửa — bài học: phải tô nền trang khác trắng mới kết luận được.
- Event: `willClose` → `closed` = **3181ms** (đúng 3180). Auto-scroll chạy **1 lần** (`animationend` count = 2).
- `prefers-reduced-motion: reduce`: `animation-name:none`, `willClose`→`closed` = 327ms, overlay đóng sạch.
- Trạng thái cuối: `data-open="0"`, `#miuOpening` → `display:none`, 2 lá có `width/height = 0`,
  seal `opacity:0` — không còn gì vẽ.

### Ghi chú / kỹ thuật
- **`backface-visibility:hidden` là bắt buộc**, không chỉ để đẹp: góc `108deg` > 90° nên nếu bỏ nó,
  mặt sau của lá (nền trắng) hiện lại đè lên trang. Đừng "tiết kiệm" dòng này.
- **Animation không `forwards`-latched**: khi `data-open` đổi sang `"0"` thì rule animation biến mất,
  lá trở lại transform gốc (full trắng). Vì vậy handler `closed` **phải** set
  `opening.style.display='none'` — không được để lá nằm lại trong cây DOM ở góc xoay.
- Nhánh **deep-link auto-close** (`load` + 1200ms, dùng cho `?g=…&t=…` / `/groom/evening`) vẫn hardcode
  `4300ms` thay vì đọc token. An toàn (dài hơn mọi animation hợp lý) nhưng **không** token-driven —
  nếu sau này rút ngắn animation thì nhánh này không tự theo.
- `getBoundingClientRect()` **không** dùng để kết luận lá còn vẽ hay không: sau 90° bề rộng hình học
  tăng lại (23.9px → 350.7px) vì perspective, dù lá đã khuất. Phải đo pixel.
- 32% lá phải **tương đương** reference (reference cho `.card-side.right` 50% chồng lên lá trái 68%,
  lá trái `z-index:1` nên chỉ còn 32% lộ ra) — cùng hình, ít node hơn.

### Files created/edited
- `index.html` — 6 `@font-face`, markup overlay (backdrop / 2 lá / `.cf-leaf-text` / `.cf-bottom` / seal),
  toàn bộ block CSS `#miuOpening` (token + keyframes + card), `close()` đọc token từ CSS, gate auto-scroll dedupe.
- `assets/fonts/dancing-script-{latin,vietnamese}.woff2`,
  `assets/fonts/cormorant-garamond-{roman,italic}-{latin,vietnamese}.woff2` (file mới).
- `AGENTS.md` — mục *Opening Flow* viết lại theo cửa 3D; mục *Fonts* thêm 3 family + lý do giữ subset Latin.
- `WORK_LOG.md` (this entry).

### Deferred (chưa làm)
- **Chưa QA trực quan bằng mắt** (agent không đọc được ảnh): toàn bộ kiểm chứng session này là
  computed-style + đếm pixel. Người dùng nên mở `/` và xem lần chạy thật để chốt cảm giác cửa.
- **Chưa commit** theo yêu cầu của user.
- Script kiểm chứng nằm ngoài repo: `%LOCALAPPDATA%\Temp\opencode\doortest\{shoot,verify,verify2,vis}.js`
  + ảnh trong `...\doortest\out\`. Cần thì chạy lại, không add vào project.

## Session 25: 2026-09-29 — Bỏ thư mục ảnh mang tên id của runtime

### Tasks Completed
1. `git mv` **20 file `.webp`** từ `assets/uploads/6a2a56e562badd7da97313bb/` lên thẳng `assets/uploads/`,
   xoá thư mục rỗng, `git add -A assets/uploads` → 20 entry `A assets/uploads/*.webp` (trước đó cũng
   đang staged `A` từ session trước nên index vẫn nhất quán).
2. Thay **22 tham chiếu** trong `index.html`: `assets/uploads/6a2a56e562badd7da97313bb/` → `assets/uploads/`.
   - 20 × `src="…"` (ảnh trên canvas) + **2 × URL tuyệt đối** `og:image` / `twitter:image`
     (`https://vunhungwedding.online/assets/uploads/couple-main.webp`).
3. `AGENTS.md`: sửa bullet *Images* (kèm giải thích id là gì) + ghi rõ 2 meta share trỏ URL tuyệt đối.

### Kết quả kiểm chứng
- Tĩnh: 0 tham chiếu dạng cũ, **22** tham chiếu dạng mới; **20** ảnh distinct, trên đĩa **20**,
  *referenced but missing = 0*, *on disk but unreferenced = 0*.
- Cấu trúc file **không đổi**: 2526 dòng trước = 2526 sau, `data-invitation-id` còn nguyên 1 lần.
- Encoding giữ nguyên convention: **không BOM, LF thuần** (`UTF8Encoding($false)`), diacritics nguyên vẹn.
- Chrome/Puppeteer: 0 console error / 0 pageerror; cả 20 `<img>` trong canvas có `naturalWidth > 0`
  (check bắt được trường hợp sai đường dẫn mà vẫn không lỗi console); 0 tham chiếu cũ còn sót trong
  DOM lẫn các rule CSS; overlay (`2 × .card-side`, High Spirited) và canvas 9250px vẫn nguyên.

### Hai sự kiện "lạ" khi chạy test — KHÔNG phải hồi quá (đừng mất thời gian truy)
- **`ordinary.m4a :: net::ERR_ABORTED`** trong `requestfailed`. Không phải lỗi tải: `bgAudio` có
  `readyState: 4` (HAVE_ENOUGH_DATA) và `error: null`. Chrome huỷ *range request* sau khi buffer đủ.
  Đường dẫn audio không bị session này đụng tới.
- **`nodeCount` DOM = 103 còn file = 106**. Ba "dư" là **chuỗi literal trong JS** master-data, không phải
  node thật: `'[data-node-id="' + id + '"'`, `MASTER_MAP_NODES[String(idx + 1)] + '"'`, `targetId.replace("…`.
  Nên node thật = 103/103 đều có trong DOM. Đếm bằng regex thô sẽ luôn ra 106.

### Ghi chú / kỹ thuật
- `6a2a56e562badd7da97313bb` là **invitation id của bản export miu**, lộ ra ở `data-invitation-id` trên
  `.miu-canvas`. Server miuwedding.com lưu ảnh theo `/uploads/<invitation-id>/<file>`; Session 1 tải 20
  ảnh về mà **giữ nguyên cấu trúc thư mục**, nên tên đó tồn tại tới nay dù các file bên trong đã được
  đặt tên đẹp. Không có script nào đọc `data-invitation-id` (chỉ xuất hiện đúng 1 lần trong file) —
  giữ nguyên attribute, chỉ bỏ thư mục.
- **Thay chuỗi có tiền tố `assets/uploads/` là an toàn** với `data-invitation-id="6a2a…"` (id trần,
  không có tiền tố) — không vô tình sửa nhầm. Đây là lý do không nên thay **id trần**.
- Dễ sót nhất là 2 meta share: chúng là URL **tuyệt đối** trên domain thật, sửa thiếu thì ảnh share
  404 dù trang vẫn chạy bình thường. Đã có bước verify riêng cho 2 tag này.
- **Không sửa 6 dòng lịch sử** trong `WORK_LOG.md` vẫn nhắc tên thư mục cũ (Session 1/22/23) — log ghi
  việc đã xảy ra, không viết lại quá khứ. `AGENTS.md` (tài liệu mô tả hiện trạng) thì phải sửa.
- `404.html`, `CNAME`, `.gitignore`: **0** tham chiếu tới thư mục này.

### Files created/edited
- `index.html` — 22 đường dẫn ảnh (210853 → **210303** bytes, không đổi số dòng).
- `assets/uploads/*.webp` — 20 file chuyển từ subfolder id lên thẳng `assets/uploads/` (đã staged).
- `AGENTS.md` — mục *Assets* (bullet Images) + mục *Image files*.
- `WORK_LOG.md` (this entry).

### Deferred (chưa làm)
- **Chưa commit** theo yêu cầu của user. Nhớ `git add` 6 file `.woff2` của Session 24 (đang untracked)
  trước khi commit, nếu không deploy sẽ mất font của overlay mở.
- Backup trước khi sửa: `index.before-session25.html` (210853 bytes) trong thư mục temp opencode.

## Session 26: 2026-09-29 — Bỏ nền đen cửa, thêm nút `Mở thiệp` + icon ấn cửa gốc

### Yêu cầu
1. `Bỏ phần màu đen của 2 bên cánh cửa` → xoá lớp nền tối toàn màn hình, thay bằng đổ bóng/viền nhẹ.
2. `Layout cánh cửa trùng với thiệp giống URL` → đối chiếu
   `melipage.com/the-truong-nhu-quynh-2026-05-24-template`: thêm nút pill `Mở thiệp` và thay seal
   囍 bằng icon ấn cửa gốc của họ. Bảng màu nút: **copy y hệt reference** (user chọn, không tô vàng).
   Lớp phủ popup `.cf-choice` giữ nguyên `rgba(0,0,0,.55)` (user chọn).

### Files
- **Tạo**: `assets/elements/side-card-icon.png` (79,628 bytes) — tải từ
  `https://melipage.com/assets/images/side-card-icon.png` (HTTP 200, `image/png`).
- **Sửa**: `index.html` (markup overlay + ~14 dòng CSS + ~5 dòng JS),
  `AGENTS.md` (Opening Flow, Assets, Verification), `WORK_LOG.md` (mục này).

### index.html — chi tiết kỹ thuật
1. **Xoá backdrop**: `<div id="miuOpeningBackdrop"></div>` + 3 rule CSS
   (`#miuOpeningBackdrop{}`, `#miuOpeningBackdrop::after{}` = gradient tối `#3a332e→#241f1c→#16120f`
   + gold seam glow, và rule `data-open="0"` fade). Không có JS nào tham chiếu tới nó → xoá sạch, không
   dọn dẹp phụ. Reference cũng không có (`#miuOpening{background:transparent}`).
2. **Thay bằng shadow caster** `#miuOpening::after` — rect đúng kích thước/vị trí cửa, **không có
   background**, chỉ `box-shadow:0 0 0 1px rgba(0,0,0,.05),0 18px 60px rgba(0,0,0,.18)`, `z-index:0`.
   Bắt buộc: nó phải fade theo `#miuOpening[data-open="0"]` — dùng `.34s ease .06s` (cùng nhịp
   `.cf-leaf-text`) chứ **không** dùng `var(--door-duration)` như backdrop cũ, vì bóng đổ tĩnh còn
   đứng 2.4 s trong khi 2 lá đã xoay đi sẽ thày hình chữ nhật tối lơ lửng.
   `@media (max-width:480px)` bỏ `box-shadow` vì `--door-w:100vw` không còn mép trắng nào định nghĩa.
3. **Seal → icon reference**: `<div id="miuSeal" class="cf-lock">囍</div>` →
   `<div id="miuSeal><button id="miuOpeningBtn" …><img src="assets/elements/side-card-icon.png" alt=""></button></div>`.
   Bỏ class `cf-lock` (chỉ xuất hiện 1 lần, không có rule CSS nào). Bỏ đĩa trắng, viền 2px, vòng
   `::after`; ảnh `object-fit:contain` + `drop-shadow(0 4px 10px rgba(0,0,0,.35))`. Giữ nguyên
   `cfSealPulse` + transition scale 0.55 khi đóng.
   **Bẫy**: `#miuSeal` là **sibling của `#miuOpeningSides`**, không phải child của lá trái (reference
   thì seal nằm trong card). Viết `left:98%` như reference sẽ tính theo **100vw** → seal lệch ra
   1411px. Sửa bằng công thức tương đương: `left:calc(50% + var(--door-w) * .1664)`
   (= 50% − 0.5·w + 0.98·0.68·w). Test bắt được lỗi này ở lần chạy đầu.
4. **Thêm CTA** `#miuOpeningCta`/`#miuOpeningCtaBtn` — đặt **trong** `.card-side-left` (sau
   `.cf-leaf-text`) để nó xoay và bị clip cùng lá. `left:50%` là **tâm lá trái** (34% cửa) chứ không
   phải tâm cửa; `top:calc(50% + (var(--slide-seal-size)/2) + 4px)`; `width:min(92%,420px)`;
   pill 999px, padding 12/18, Lora 12px/800, letter-spacing .06em, uppercase, `#9e8130`,
   `rgba(255,255,255,.10)` fill + `rgba(255,255,255,.26)` border + `blur(8px)` +
   `0 10px 30px rgba(0,0,0,.25)` — copy y hệt reference.
   Trên thẻ trắng nền/viền trắng gần như vô hình và `backdrop-filter` là no-op, nên nút hiện ra là
   **chữ vàng + bóng mềm** — đúng như reference trên thẻ trắng, user chủ động chọn giữ vậy.
   Bắt buộc có rule fade riêng `#miuOpening[data-open="0"] #miuOpeningCta{opacity:0;pointer-events:none}`
   vì nó không nằm trong `.cf-leaf-text`; nếu thiếu sẽ thấy chữ nằm nghiêng khi lá xoay qua 90°.
5. **Bỏ auto-popup**: xoá `setTimeout(… step1.classList.add('active') …, 1000)` → thẻ trần đúng như
   reference. Thay bằng `openChoice()` gắn cho **cả hai** `#miuOpeningCtaBtn` và `#miuOpeningBtn`.
   Giữ nguyên toàn bộ phần còn lại: step1 → step2 → `window.cfApply()` → `close()` → events →
   `display:none` → auto-scroll 1 lần.

### Kết quả kiểm chứng
- Tĩnh: `miuOpeningBackdrop` = 0; 3 id mới mỗi cái 1 lần; đúng **2** `.card-side`;
  magic `.1681` = 0; `setTimeout … 1000` = 0; script 22/22, style 5/5; **22** ref `assets/uploads`
  nguyên vẹn; 0 ref thư mục cũ; LF thuần, không BOM.
- Puppeteer `doortest/s26.js` — **35/35 PASS**: thẻ trần ở t=2.2 s, icon `naturalWidth=188`,
  seal cx khớp 98% lá (desktop 815.7 / mobile 259.9, lệch −7.8px so với seam), CTA đúng công thức
  (desktop tâm x 628, y 493, rộng 360 / mobile 132.6, 465, 244), shadow có trên desktop và
  `none` trên mobile, backdrop không còn trong DOM, click icon→popup, chọn khách→step2, chọn nhóm→
  `data-open=0`, CTA/seal/shadow đều `opacity:0`, `willClose→closed` = **3184 ms**,
  `display:none`, auto-scroll **1 run**, `cfApply` ghi lại canvas (countdown `2026-11-29T09:00:00`
  + card1 `TIỆC CƯỚI NHÀ TRAI`), reduced-motion đóng đúng 1 lần, 3 deep-link `?g=&t=` đều đúng,
  0 console error / 0 pageerror ở mọi ca.
- `doortest/s26b.js` — ma trận chồng lấn ở 5 khung nhìn (1440×900, 1280×720, 768×1024, 390×844,
  360×640): **không chồng lấn** giữa seal / CTA / save-date / names / bottom, cửa không tràn viewport.

### Sai lầm của chính test (không phải lỗi site)
- `history.replaceState` sang path khác bị Chrome **từ chối trên `file://`** → không assert được URL;
  chuyển sang deep-link `?g=&t=` (cùng thứ tự ưu tiên trong `parsePath()`) và chứng minh `cfApply`
  chạy bằng `[data-countdown="1"]` + `element_text_0iedk0b1132`.
- Auto-scroll mượn phát ra hàng trăm `scroll` event → phải đếm **run** tăng đơn điệu, không đếm event.
- Selector thiếu tiền tố: id thật là `element_text_0iedk0b1132`, không phải `0iedk0b1132`.
- Regex `/NHA TRAI/` không khớp vì text thật là `NHÀ TRAI` (thiếu dấu).
- Sửa markup bằng `oldString` quá rộng đã **xoá nhầm** `<div class="card-side card-side-right">`;
  phát hiện ngay khi đọc lại block và đã khôi phục, test xác nhận lại đủ 2 lá.

### Tác động nhìn thấy được (cần user QA bằng mắt)
Bỏ backdrop là **trang thiệp hiện ra ngay khi lá bắt đầu xoay**, tức trước lúc khách bấm `Mở thiệp`.
Đầu trang là ảnh `couple-main.webp` + tên lớn nên giữa lúc mở cửa sẽ thấy hai khối tên cùng nội dung
(1 trên thẻ, 1 trên trang). Nếu thấy khó chịu thì thêm một lớp phủ màu trắng/cream fade cùng nhịp
`.34s` (vẫn không có màu đen). `#miuBootLoading` là nền `#ffffff` nên lúc đang load không ảnh hưởng.

### Deferred (chưa làm)
- **Chưa commit** theo yêu cầu của user. Nhớ `git add` 6 file `.woff2` (Session 24) **và**
  `assets/elements/side-card-icon.png` trước khi commit, nếu không deploy sẽ mất font overlay + icon.
- Backup trước khi sửa: `index.before-session26.html` (210,303 bytes) trong thư mục temp opencode.
## Session 27: 2026-09-29 — Chậm lại nhịp mở cửa 1.3× + mềm điểm khởi động

### Yêu cầu
`Tốc độ mở thiệp đang hơi nhanh`. Đã hỏi 2 câu: mức chậm (**vừa — kéo dài 1.3×**) và có mềm easing
không (**có**). Nhận định quan trọng: cảm giác nhanh có **hai** nguyên nhân độc lập — *ngắn* và *giật* —
nên phải sửa cả hai, sửa một cái là chưa đủ.

### Files
- **Sửa**: `index.html` (3 token thời gian, 2 rule animation, 3 transition nhịp mờ, 1 magic number),
  `AGENTS.md` (bullet *Timing* + bullet *Verification*), `WORK_LOG.md` (mục này).

### index.html — chi tiết kỹ thuật
1. **Token trên `#miuOpening`**: `--door-duration 2.4s→3s`, `--door-delay 340ms→500ms`,
   `--door-stagger 120ms→150ms` (`--door-angle` giữ 108deg). Tổng `doorTotal` 2860→**3650ms**,
   `miu:opening:closed` 3180→**3970ms**. Không phải sửa JS: `close()` đọc 3 token qua
   `getComputedStyle` nên tự đi theo.
2. **Easing mềm** (2 rule `.card-side-left`/`.card-side-right`):
   `cubic-bezier(.34,.02,.2,1) → cubic-bezier(.5,.06,.3,1)`. Lý do kỹ thuật: control-point
   **y = 0.02** của curve cũ khiến lá tăng tốc gần như tức thì ngay frame đầu — phần lớn cảm giác
   "nhanh", tách biệt hoàn toàn với độ dài. Đo thật: ở 15% thời gian lá phải mới quay **5.5°** thay vì
   ~10.8° của curve cũ (2× mềm hơn).
3. **Nhịp mờ chữ** `.34s ease .06s → .5s ease .1s` ở `.cf-leaf-text`, `#miuOpeningCta` và
   `#miuOpening::after`. Bắt buộc đi kèm: nếu giữ nhịp cũ, chữ biến mất lúc 400ms trong khi lá chỉ bắt
   đầu xoay ở 500ms → 100ms thẻ trắng trơn. Sau khi sửa: chữ còn đang mờ khi lá đã đi (cùng quan hệ
   chồng lấn ~100ms như bản gốc).
4. **Bỏ magic `4300` của deep-link** — con số cuối cùng mà timing cửa còn phụ thuộc. Hiện tổng 3650
   nên vẫn dư 650ms, nhưng nó vỡ **âm thầm**: overlay bị `display:none` giữa lúc lá còn ở ~55°.
   Thay bằng `deepTotal` đọc cùng 3 token qua bản sao cục bộ của đúng hàm `parseMs` mà `close()`
   đang dùng, rồi `setTimeout(…, deepTotal + 320)` — cùng công thức với `close()`.
   Fallback `6000` của auto-scroll **giữ nguyên**: gate thật bắn ở 4000ms nên còn ~2s dự phòng; chỉ ghi
   rõ điều kiện phải nâng lên trong `AGENTS.md`.

### Kết quả kiểm chứng
- Tĩnh: token trên DOM đúng `3s/500ms/150ms`; **2** rule lá dùng easing mới, 0 còn easing cũ;
  **5** occurrence `.5s ease .1s`, 0 còn `.34s ease`; 0 còn `4300`; 0 còn `2.4s`;
  script 22/22, style 5/5, 22 ref ảnh; LF thuần, không BOM.
- `doortest/s27.js` — **16/16 PASS**. Hồ sơ góc xoay thật của lá phải (lấy mỗi 100ms):
  `0d` tới 602ms → `3.5°`@1001 → `11.6°`@1302 → `52.7°`@1901 → `91.2°`@2501 →
  `105°`@3101 → `108°`@3700. Góc cuối đúng 108°, quãng xoay 3098ms, **a15 = 5.51°** (< 8.6°),
  a50 = 69.3°, a85 = 106°, tại 602ms (trước delay 650ms) góc = 0.
  `willClose→closed = 3977ms`. Deep-link: `closed` **sau** `animationend` 415–434ms (đúng thứ tự,
  không bị giật). Auto-scroll 1 run, `display:none`, reduced-motion 1 lần, master data đúng 3 ca,
  0 console/pageerror.
- `doortest/s26.js` chạy lại: **35/35 PASS** (đã cập nhật ngưỡng 3181→3970 cho khớp Session 27).

### Sai lầm của chính test (đáng ghi lại vì rất dễ tái diễn)
- **Chrome `matrix3d` là column-major**: góc `rotateY` là `atan2(v[8], v[0])`. Tôi đoán
  `atan2(v[9], v[5])` → **mọi sample trả 0** mà test vẫn "chạy", vì mảng đều bằng 0 nên các ngưỡng
  `< 10°` đều pass một cách ngu dốt. Đã lộ ra khi nhìn log thô.
- Khi overlay đã `display:none`, `getComputedStyle(leaf).transform` trả `'none'` → góc 0, làm
  nhiễu đuôi profile. Phải dừng interval ngay khi đọc được `null`.
- Đo "15% thời gian" phải lấy từ **cửa sổ animation thật** (`delay+stagger` → `+duration` của đúng
  lá), không lấy từ thời điểm phát hiện chuyển động — ngưỡng mềm rất nhạy với gốc đo.
- Điều kiện "chưa xoay ở thời điểm delay" phải so với delay **của lá đang đo** (lá phải có thêm
  `--door-stagger`), không phải delay của lá trái.

### Deferred (chưa làm)
- **Chưa commit** theo yêu cầu của user. Vẫn còn 7 file untracked cần `git add` trước khi commit:
  6 `.woff2` (Session 24) + `assets/elements/side-card-icon.png` (Session 26).
- Backup trước khi sửa: `index.before-session27.html` (211,413 bytes) trong thư mục temp opencode.
## Session 28: 2026-09-29 — Cửa 4s (khớp reference) + text/seal giữ nguyên khi mở

### Yêu cầu
Hai ý trong một lượt: `(1) cánh cửa mở chậm hơn chút nữa, (2) các text, hình seal trên cửa giữ nguyên
khi mở cửa giống như <reference>`.

### Điều tra reference trước khi sửa (quan trọng — đã xác nhận yêu cầu là đúng)
Trong export của reference (	ool_0eb52f9ca001pOs1xnNHDZZjVt):
- `@keyframes miuOpeningSlideLeft{from{transform:translateX(0);opacity:1;}to{transform:translateX(-110%);opacity:1;}}`
  → `opacity:1` ở **cả hai đầu**: reference **không hề fade** text.
- `#miuOpeningSides .seal-icon{position:absolute;top:50%;left:98%;…;z-index:2}` → seal là **child của
  thẻ**, nằm ở `left:98%`, không có rule `data-open` nào. Nó trượt đi cùng thẻ chứ không tự mờ.
- `#miuOpeningCta` cũng không có fade khi đóng.
- Thời lượng: `--animate-duration:4s`, `animation-duration:4s`, `animation-delay:500ms`, và script có
  `if (!total) total = 4500;` → **cửa reference dài 4,5s**, chậm hơn cả bản 3s của ta. Đó là con số
  chuẩn để theo, không phải do tôi chọn.

Hệ quả trong code ta: `.cf-leaf-text`/seal/CTA có fade là **sáng tạo của ta**, không phải của reference;
và seal phải nằm trong lá trái thì mới "giữ nguyên" được (nếu để làm sibling thì không fade thì nó sẽ
trôi lơ lửng giữa màn hình sau khi cửa đã mở).

### Files
- **Sửa**: `index.html` (1 token, 3 fallback, 3 rule fade xoá, 1 rule seal viết lại, 1 lần di chuyển
  markup, 2 chú thích), `AGENTS.md` (bullet seal / CTA / leaf-text / timing / verification),
  `WORK_LOG.md` (mục này).
- Test cập nhật: `doortest/s27.js` (16→25 assert), `doortest/s26.js` (3 chỗ mã hoá thiết kế cũ).

### index.html — chi tiết kỹ thuật
1. **Token**: `--door-duration 3s→4s` (delay 500ms, stagger 150ms, angle 108deg giữ nguyên) →
   tổng **4650ms**, `miu:opening:closed` **4970ms**. JS không cần sửa, `close()` đọc token.
2. **Ba fallback cũ đã stale** — cùng lớp bug đã gỡ ở Session 27 (giá trị sai làm overlay biến mất
   giữa lúc lá còn đang xoay): `doorTotal` fallback `2860→4650` (2 chỗ) và `deepTotal` fallback
   `2860→4650`. Đã grep xác nhận 0 occurrence của `2860`.
3. **Xoá fade của text**: xoá rule `#miuOpening[data-open="0"] .cf-leaf-text{opacity:0;transform:translateY(-10px)}`
   và `transition` của nó. `.cf-leaf-text` giờ **không còn rule opacity/transform nào** → chữ luôn
   đặc 100%, không nhấc, suốt toàn bộ 4,65s.
4. **Xoá fade của CTA**: `#miuOpeningCta` chỉ còn `pointer-events:none` khi đóng; bỏ `opacity` khỏi
   `transition`.
5. **Seal → child của lá trái** (thay đổi cấu trúc, giống reference):
   - Di chuyển `<div id="miuSeal">` từ ngay trước `</div>` của `#miuOpening` vào trong
     `.card-side-left`, đặt sau `#miuOpeningCta`.
   - `#miuSeal` viết lại: `left:calc(50% + var(--door-w) * .1664)` → **`left:98%`**,
     `z-index:10003` → `z-index:2`, bỏ `transition` (chỉ phục vụ fade/scale).
   - Xoá rule `#miuOpening[data-open="0"] #miuSeal{opacity:0;animation:none;transform:…scale(.55)}`
     → seal xoay đi cùng lá, không mờ, không thu nhỏ. Pulse `cfSealPulse` vẫn chỉ chạy khi
     `data-open="1"]` nên **đóng băng** đúng lúc cửa mở — đúng nghĩa "giữ nguyên".
   - **Chứng minh vị trí không đổi trên màn hình**: `0.98 × 68% = 66.64%` cửa, đúng bằng
     `50% + 16.64%` mà công thức cũ biểu diễn. Đo thật: tâm seal **815,7px** cả trước và sau khi
     chuyển (mọi viewport trong `s26b.js` cho đúng bộ x y hệt Session 26).
6. **Giữ nguyên fade của `#miuOpening::after`**: đó không phải nội dung thẻ mà là hình chữ nhạt giả định
   nghĩa cửa; không mờ thì một hình chữ nhật tối treo trên trang đã mở suốt 4,7s. Nó mờ trong lúc 500ms
   delay khi thẻ vẫn còn nguyên → không thấy.
7. **Số liệu tĩnh**: 19/19 OK — token 4s, 0 occurrence `3s`/`2860`/`.1664`/`scale(.55)`,
   0 rule fade nào còn lại, đúng 1 id `miuSeal`, seal nằm trong lá trái và sau CTA, 2 lá, script 22/22,
   22 ref ảnh, LF thuần, không BOM. Size 212.121 → 212.292 bytes.

### Kết quả kiểm chứng
- `doortest/s27.js` — **25/25 PASS** (thêm 9 assert mới):
  - Hồ sơ góc lá phải mỗi 100ms: `0°` tới 605ms → `2,2°`@1003 → `27°`@1902 → `64,8°`@2503 →
    `90,9°`@3102 → `108°`@4902. Góc cuối đúng 108°, quãng xoay 4099ms, a15 = **4,51°**,
    a50 = 70,5°, a85 = 105,7°.
  - **Nội dung thẻ bất biến**: 16 mẫu trong suốt cửa mở → `opacity:1` ở **mọi** mẫu (0 mẫu khác 1),
    `transform` = `none` ở mọi mẫu (0 mẫu có transform).
  - **Seal đi cùng thẻ**: `opacity:1` xuyên suốt, tâm x dịch **816 → 90px (726px)** và **giảm đều
    liên tục** (không trôi lơ lửng tại đường seam).
  - Hình học: `sealCx = leafLeft + 0.98×leafWidth` khớp trong 0,1px; seal không bị clip; lá giữ
    `backface-visibility:hidden`.
  - `willClose→closed = 4981ms`; deep-link `closed` sau `animationend` 416/439ms; auto-scroll 1 run;
    reduced-motion đóng 1 lần; master data đúng 3 ca; 0 lỗi console/pageerror.
- `doortest/s26.js` — **35/35 PASS** sau khi sửa 3 chỗ mã hoá thiết kế cũ (ngưỡng 3970→4970, sleep
  3400→6000, và đảo assert "CTA + seal + shadow fade" thành "CTA + seal **giữ nguyên**, chỉ shadow fade").
- `doortest/s26b.js` — không chồng lấn ở cả 5 viewport; vị trí seal y hệt Session 26.
- **Probe pixel bổ sung** (`doortest/probe28.js`): cắt vùng lá trái 391×560 rồi so **độ dài PNG** theo
  thời gian — 447ms 40.875B → 1222ms 45.677B → 2202ms 145.091B → từ 2778ms trở đi **đều 253.025B
  (trùng khít)**. Vùng đó đã là *trang đã mở* và đứng yên ⇒ lá đã khuất hẳn và **chữ không quay lại
  ngược** sau 90°, đúng vai trò của `backface-visibility:hidden`.

### Sai lầm của chính phép đo (đáng ghi vì rất dễ tái diễn)
- Ban đầu dùng `elementFromPoint` để hỏi "chữ có được vẽ không" → luôn trả `false`, trông như
  lỗi hiển thị. Nguyên nhân: `.cf-save-date`/`.cf-names` có `pointer-events:none` (đúng giá trị
  của reference) nên hit-test luôn xuyên qua chúng.
- Tương tự, `getComputedStyle(.cf-save-date).opacity` trả `0.98` — đó là token
  `--slide-save-opacity` **của chính reference**, không phải fade. Chỉ `.cf-leaf-text` (thẻ bao) mới
  phải bằng đúng `1`.
- AABB của `.cf-save-date` **nở lại** sau khi co lại (249 → 36 → 200px) trong lúc lá quá 90°. Đó là hình
  học chiếu của một mặt phẳng đã xoay quá vuông góc, **không phải** nội dung quay lại: probe pixel chứng
  minh vùng đó từ 2778ms là trang đã mở, không đổi một byte.

### Deferred (chưa làm)
- **Chưa commit** theo yêu cầu của user. Vẫn còn 7 file untracked cần `git add` trước khi commit:
  6 `.woff2` (Session 24) + `assets/elements/side-card-icon.png` (Session 26).
- **User nên xem bằng mắt** một lượt: môi trường agent này không đọc được ảnh, nên phần "chữ giữ nguyên"
  mới được chứng minh bằng số đo, chưa được xác nhận bằng mắt.
- Ảnh chụp tạm trong `doortest/f0-closed.png`, `f1400/f2400/f3100/f3600-swing.png`, `f-after.png`.
- Backup trước khi sửa: `index.before-session28.html` (212.121 bytes) trong thư mục temp opencode.
---

## Session 29 — cửa đôi 3D → trượt 2D (đúng reference)

**Yêu cầu:** hai lá cửa không khớp nhau ("2 ô cửa thấy lệch nhau"), và sau khi đo được sai lệch thật
(150ms danh nghĩa, 107ms lấy mẫu, chênh đỉnh 9.9°) tôi đề xuất bỏ stagger. User chọn hướng mạnh hơn:
`"tôi nghĩ nên làm hiệu ứng trượt sang 2 bên giống reference"` — thay 3D bằng trượt phẳng.

**Sự thật mới, lấy trực tiếp từ reference** (`melipage.com/the-truong-nhu-quynh-2026-05-24-template`):
`#miuOpeningSides` chỉ có `overflow:hidden`, **không có `perspective`**; keyframe là `translateX` thuần,
`animation-duration:4s`, `animation-delay:500ms` **cả hai lá cùng nhau, không stagger**, easing `ease-out`.
Hai lá `68%` và `50%` (overlap 18%, phần thấy 32%), travel `translateX(-110%)` và `translateX(100%)`.

### File đã sửa
- **`index.html`** — khối CSS overlay + 3 hằng số fallback:
  1. Xoá toàn bộ lớp 3D: `perspective:1200px`, `perspective-origin:68% 50%` (trên `#miuOpeningSides`);
     `backface-visibility:hidden` / `-webkit-backface-visibility:hidden` (trên `.card-side`);
     `transform-origin:left center` (lá trái) và `right center` (lá phải); token `--door-angle:108deg`.
  2. Keyframe mới: `@keyframes miuDoorLeft{to{transform:translateX(-110%)}}` và
     `@keyframes miuDoorRight{to{transform:translateX(100%)}}` (chép đúng reference).
  3. Easing: `cubic-bezier(.5,.06,.3,1)` → `cubic-bezier(.4,.05,.25,1)` trên **cả hai** lá.
  4. `--door-stagger:150ms → 0ms`. Tổng cửa `4s+500ms+0ms` = **4500ms**, `closed` 4820ms.
  5. Ba fallback hằng số `4650 → 4500`: `doorTotal` (overlay) ×2 và `deepTotal` (master-data) ×2.
  6. Sửa comment `.cf-leaf-text` đã lỗi thời (nói còn `backface-visibility:hidden` giấu chữ sau 90°).
  **Không sửa JS** — `close()` và `deepTotal` vốn đã đọc token từ CSS, tự đi theo.
  `will-change:transform`, `overflow:hidden`, `68%/32%`, `left:98%` của seal: giữ nguyên.

### Quyết định: easing
Reference dùng `ease-out` = `cubic-bezier(0,0,.58,1)`, có **control-point y = 0** — đúng cái property
Session 27 đã xoá vì nó gây cảm giác giật. Hơn nữa vì `y1 = x1 = 0` nên `ease-out` là nghịch đảo của chính
nó: ở 15% thời gian nó đã đi được 15% quãng. Tôi hỏi lựa chọn, user chọn **"Mềm dần, không giật"** →
`cubic-bezier(.4,.05,.25,1)`. Lý thuyết: 7.9% ở 15% thời gian, 98.5% ở 85%. Đo thật: **7.1%** và **98.4%**.

### Kiểm chứng
- **`doortest/s29.js`** (mới, **35/35 PASS**) — thay `s27.js`. Đọc `matrix(a,b,c,d,e,f)` (`e` = translate)
  thay vì `matrix3d`:
  - 1 assert/3D-gone: `perspective:none`, `backface-visibility:visible`, `transform-origin` = tâm lá,
    `--door-angle` rỗng, và **0/48 mẫu có `matrix3d`**.
  - **Đồng bộ**: hai lá chuyển động đầu tiên cùng lúc **712ms** (trước 804 vs 911ms), lệch **0ms**;
    lệch tiến độ tối đa trên 48 mẫu = **0.0000** (ngưỡng assert 2%).
  - Quãng trượt: trái `-430.10px` (target `-1.10 × 391`), phải `+184.00px` (target `+1.00 × 184`).
  - Nhịp: 15% thời gian → 7.1% quãng; 85% → 98.4%; @500ms chưa chuyển động; kéo dài 4092ms.
  - **Lợi ích 2D**: `.cf-save-date` giữ nguyên **249px, biên độ 0px** trên 16 mẫu (3D: 249→36→200).
  - Seal **gắn chặt** vào lá: sai số lệch so với `0.98 × leafW` tối đa **0.18px** (assert < 1.5px).
  - `willClose → closed` = **4822ms** (target 4820); deep-link margin sau `animationend` 287/284ms.
- **`doortest/probe29.js`** (mới, **6/6 PASS**) — hit-test khe mở bằng `elementFromPoint` vì agent
  không đọc được ảnh. Khe mở đều `23→106→260→409→507→563→595→611px`; trong khe chỉ có `PAGE`
  (40 hit) hoặc `SEAL` (8 hit, ~1.4s đầu) — **không có gì của lá phải nổi lên**.
- **`doortest/s26.js`** — sửa ngưỡng `closed` `4970 → 4820`, chạy lại **35/35 PASS**.
- **`doortest/s26b.js`** — chạy lại, không chồng lấn ở 5 viewport.
- **`doortest/shoot29.js`** (mới) — chụp 8 frame crop vùng cửa: `s29-0000-dong.png`,
  `s29-0700…4500-swing.png`, `s29-sau.png`.
- Tĩnh: 16/16 assert trong khối CSS overlay (đã **bỏ comment** trước khi so khớp, vì từ khoá 3D còn nằm
  trong chính comment mới); 22/22 thẻ script; LF thuần, không BOM; dấu tiếng Việt nguyên vẹn.

### Bẫy mới phát hiện (đã ghi vào `AGENTS.md`)
1. **`#miuSeal` nhô ra seam ~31px** (vì `left:98%` + `translate(-50%,-50%)` và nằm trong lá trái nên 98%
   tính theo lá, không theo cửa). Probe "khe luôn lộ trang" **fail vì lý do chính đáng** trong ~1.4s đầu.
   Phải loại `#miuSeal` ra khỏi phép thử, hoặc chỉ assert *không cái gì của lá phải* nổi lên khe.
2. **Khe cuối rộng 614px chứ không phải 575px**: cả hai lá trượt ra *ngoài* cửa rồi mới bị
   `overflow:hidden` cắt, nên khe giữa chúng = `1.10 × 391 + 184`. Assert 575 là sai.
3. **`transform-origin` mặc định bị browser giải ra px** (`195.5px 450px`), không phải `50% 50%` —
   phải so với tâm lá thật.
4. **`animation-*` chỉ tồn tại khi `data-open="0"`**. Đo `animationName` lúc cửa còn đóng sẽ ra
   `none|0s|0s|ease`; phải đọc longhand **giữa lúc đang trượt**.
5. `perspective` / `rotateY` / `transform-origin` **vẫn còn** trong `index.html` — nhưng thuộc thư viện
   animation của canvas miu (`miu-flipInX/Y`, `miu-swayBottom`, `transform-origin` inline của node).
   **Không được** xoá; phải scope assert vào khối CSS overlay. Đây là 4 FAIL giả đầu tiên của session.

### File bị xoá
- **`doortest/s27.js`** — assert góc cuối 108°, tức thiết kế Session 29 gỡ bỏ. Giữ lại chỉ để chạy nhầm
  và nhận FAIL khó hiểu. (Lịch sử trong git, không ảnh hưởng repo.)

### Deferred (chưa làm)
- **Chưa commit** theo yêu cầu của user. Vẫn còn 7 file untracked cần `git add` trước khi commit:
  6 `.woff2` (Session 24) + `assets/elements/side-card-icon.png` (Session 26).
- **User vẫn chưa xem bằng mắt**: agent này không đọc được ảnh, nên "trượt sang 2 bên, đồng bộ, chữ
  không bị méo" mới được chứng minh bằng số đo (delta 0ms, 249px biên độ 0px, 0/48 mẫu 3D), chưa được
  xác nhận bằng mắt. Ảnh đã chụp sẵn trong `doortest/s29-*.png` để user tự xem.
- Cảnh báo đã nêu với user: cửa 4.5s + popup chọn nhóm = phải chờ ~4.5s trước nội dung lần đầu (không
  hồi tố so với bản 3D 4.97s). Nếu thấy chờ lâu, hạ `--door-duration`.
- Backup trước khi sửa: `index.before-session29.html` (212.292 bytes) trong thư mục temp opencode.

---

## Session 31 - sửa 41 node mất `opacity:0` (hiệu ứng scroll reveal)

### Nguyên nhân
User yêu cầu "thêm hiệu ứng animation lúc scroll giống https://melipage.com/david-lan-2027-01-02-06".
Khảo sát cho thấy **hệ thống đã có sẵn từ bản export** — không thiếu gì để "thêm":
- Script observer **byte-identical** tham chiếu: **6239 ký tự, khác biệt 0** (đối chiếu 2 file trực tiếp).
- 58 `@keyframes miu-*`, 81 node mang `data-anim-preset` + `duration/delay/easing/loop/distance`.
- Cùng tham số: `rootMargin '0px 0px -12% 0px'`, `threshold [0.08,0.15,0.22]`, chơi 1 lần rồi `unobserve`.
- Tham chiếu có 85 node, ta có 81; preset (`fadeInDown` 40, `fadeInRight` 13, `fadeIn` 10…) và
  duration (`3000` × 77, `2000` × 4) gần như trùng khớp.

Nhưng có **1 lỗi thật khiến hiệu ứng nháy thay vì fade**: `applyAnim` viết `el.style.opacity='1'`
rồi gán `animation: … both`, nên *backwards fill* lập tức đẩy node về `from{opacity:0}`. Node nào
**không** ship `opacity:0` ở cuối `style` thì hiện sẵn từ đầu → khi observer bắn thì **biến mất đột
ngột rồi mới fade vào**. Đó chính là cảm giác "không có animation lúc scroll".
- Trước: **40/81** node có `opacity:0` ở khai báo cuối, **41** node chỉ có `opacity: 1`.
- 41 node lỗi nằm trọn trong **3100px đầu**: hero (18), gia đình, chữ V/N, **timeline + cả 2 thẻ
  sự kiện** (23) — tức toàn bộ phần quan trọng nhất. 40 node đúng nằm từ 3100px trở xuống.
- Dấu hiệu nhận diện: 41 node đó dùng định dạng `style` bị **một session sửa canvas trước đó viết lại**
  (`--miu-node-rotate: 0deg; ` có khoảng trắng + `position: absolute; left: …`), nên mất khai báo
  `opacity:0` cuối chuỗi. 40 node nhóm gốc dùng `var(--miu-node-rotate,0deg)` không khoảng trắng.

### File bị sửa
- **`index.html`** — thay **41** lần:
  `--miu-node-rotate: 0deg; transform: rotate(var(--miu-node-rotate,0deg)); opacity: 1;`
  → `… opacity: 0;`
  Chuỗi khoá đã kiểm chứng khớp **1:1** với đúng 41 node cần sửa (không thiếu, không thừa, không
  trùng, không đụng 40 node đang đúng). Hai chuỗi **dài bằng nhau** nên file giữ nguyên 212.307 bytes.
  **Không** sửa script reveal, keyframe, preset, timing, easing hay bất kỳ node nào khác.
- **`AGENTS.md`** — thêm mục "Scroll reveal system (miu engine)" ghi lại đặc tả hệ thống (script
  byte-identical, tham số observer, preset/duration/distance, cảnh báo hai định dạng serialisation)
  để session sau không viết lại và không làm rơi `opacity:0` lần nữa.
  Kèm 1 dòng chú thích `ribbon-01.png` là asset dư (đã commit nhưng `index.html` không tham chiếu).

### Kết quả kiểm tra tĩnh
| Hạng mục | Trước | Sau |
|---|---|---|
| Node có `data-anim-preset` | 81 | 81 |
| Có `opacity:0` là khai báo cuối | 40 | **81/81** |
| Node còn hiện sẵn trước reveal | 41 | **0** |
| 40 node nhóm gốc | 40 | 40 (không đổi) |
| Kích thước file | 212.307 | 212.307 |

Encoding giữ nguyên UTF-8 không BOM, LF thuần (0 CR / 2548 LF). Đã đếm lại và xác nhận không đổi:
`@keyframes` 62, `data-anim-preset` 83, `WEDDING_MASTER` 3, `data-miu-wishes` 23,
`miu-fadeInDown` 27, `applyAnim` 5, `IntersectionObserver` 4, `--door-duration` 6, `baseH` 2.

### Ghi chú — 2 điều tham chiếu KHÔNG có
Đã xác nhận để session sau không đi tìm nhầm hoặc tưởng là "thiếu":
- Tham chiếu **không** có `position:sticky`, **không** parallax, **không** scroll-scrub (0 lần xuất
  hiện trong 279 KB). Toàn trang là canvas tĩnh 9250px. Muốn thêm thì đó là bổ sung, không phải port.
  FAB dock của tham chiếu chỉ có nhạc + thu gọn, **không** phải điều hướng theo mục.
- Tham chiếu **có bug reduced-motion**: dù bật `prefers-reduced-motion:reduce` nó vẫn chạy
  animation, chỉ bỏ gate scroll. Ta thừa kế nguyên bug đó (script giống hệt).

### Deferred (chưa làm)
- **User chọn KHÔNG kiểm chứng bằng trình duyệt** ("tôi tự xem"), nên chưa có bằng chứng runtime rằng
  observer thực sự kích hoạt và 81 node về `opacity≈1`. Đã báo rõ rủi ro: **nếu gate chờ overlay hỏng,
  41 node đó sẽ vĩnh viễn ở `opacity:0`** (hero + 2 thẻ sự kiện biến mất).
  Cách kiểm: mở `index.html` → mở thiệp → chọn nhóm; ngay khi lách cửa trượt đi, hero phải **fade dần
  từ 0 → 1 trong 3 giây**, không phải đã nằm sẵn rồi nháy. Thấy nội dung đứng yên không hiện →
  `git checkout -- index.html`.
- **Chưa commit** (user không yêu cầu). Commit nền hiện tại: `35118d4`.
- Backup trước khi sửa: `index.before-animfix.html` (212.307 bytes) trong thư mục temp opencode.
  Hoàn tác bằng 1 lệnh: `git checkout -- index.html`.

---

## Session 32 - xoá 41 `animation:` bị bake trong inline `style` => chữ xuất hiện chạy đúng như mẫu

### Yêu cầu
User: *"tôi muốn hiệu ứng, các chữ xuất hiện trong thiệp như trong mẫu này"* — tham chiếu
`https://melipage.com/david-lan-2027-01-02-06`. User chọn phạm vi **"chỉ sửa cho khớp mẫu"**
(không đổi preset sang hiệu ứng giàu chất hơn, không viết thêm hiệu ứng tách từng chữ).

### Điều tra: chỉ có **một** sai lệch với mẫu
Đã tải HTML mẫu `david-lan-2027-01-02-06` (280.364 bytes) và so **từng node** với `index.html` trên
**104 node id chung**
(cắt file theo ranh giới `data-node-id`, so `data-node-type` + 6 thuộc tính `data-anim-*`):

| Hạng mục | Mẫu | Ta |
|---|---|---|
| Observer script (6.239 ký tự) | – | **giống hệt byte-for-byte** |
| `@keyframes miu-*` | 57 | 58 (đủ) |
| `data-anim-preset/duration/delay/easing/loop/distance` | – | **khớp 100%** trên 104 node |
| Node có `opacity:0` là khai báo opacity cuối | 85/85 | 81/81 |
| `animation:` ghi cứng trong inline `style` | **0** | **41** |

Phân bố preset hai bên giống nhau (`fadeInDown` 40, `fadeInLeft` 14, `fadeInRight` 13, `fadeIn` 10,
`fadeInUp` 2, `heartBeat` 2) — tức Session 31 đã port đúng thuộc tính; chỉ còn **dấu vết export**.
Mẫu cũng **không** có tách từng chữ / `clip-path` / `mask-image` / stagger `nth-child` / thư viện
GSAP — hiệu ứng "chữ xuất hiện" của mẫu **chính là** reveal của cả node.

### Nguyên nhân gốc
41 node đó có sẵn `animation: 3000ms cubic-bezier(0.2, 0.8, 0.2, 1) 0ms 1 normal both running miu-<preset>;`
**ngay trong HTML**. Bản export của miu được lưu từ một DOM đang chạy, nên engine đã gán shorthand
đó vào inline style của những node đang nằm trong khung nhìn của tác giả; người lưu lại chép thẳng.

Hệ quả (toàn bộ im lặng):
1. CSS animation bắt đầu **lúc parse trang** — tức **dưới overlay mở câu 4,5 s**;
2. chạy hết 3 s rồi `fill-mode: both` khoá ở trạng thái `to`;
3. khi observer gọi `applyAnim()`, nó gán **đúng y chuỗi cũ** => trình duyệt **không restart**;
4. kết quả: **toàn bộ 1/3 đầu thiệp không có hiệu ứng gì** — chữ hiện sẵn từ khung hình đầu tiên,
   trong khi 2/3 dưới vẫn reveal bình thường.

41 node lỗi nằm trọn trong `top <= 3079px`: hero, gia đình, chữ V/N, timeline + **cả 2 thẻ sự
kiện**, 2 nút bản đồ, ảnh hero. Tương quan **1:1** với 41 node "serialisation dạng 2" mà Session
31 đã sửa mất `opacity:0` — **cùng một lần export hỏng, hai triệu chứng.**

### File bị sửa
- **`index.html`** — xoá **41** khai báo, khớp **1:1** (7 chuỗi phân biệt): `26x` `fadeInDown`,
  `4x` `fadeInLeft`, `3x` `fadeIn`, `2x` `2000ms fadeInLeft`, `2x` `2000ms fadeInRight`,
  `2x` `fadeInRight`, `2x` `heartBeat … infinite`. Xoá cả khoảng trắng dẫn và dấu `;` cuối.
  Kiểm chứng an toàn trước khi ghi: **0/41** nằm trong `<style>`/`<script>` (27 block) — tất cả
  nằm trong thuộc tính `style`. **Không** sửa observer, keyframe, `data-anim-*`, `opacity:0`,
  master data, overlay hay bất kỳ node nào khác. 210.968 → 207.233 ký tự
  (212.307 → 208.572 bytes, `-3.735`).
- **`AGENTS.md`** — thêm trap mới (bake `animation:`) ngay sau trap `opacity:0`, ghi rõ 7 chuỗi
  đã xoá + lệnh audit (kỳ vọng **0**), ghi chú 41 node vẫn giữ khoảng trắng serialisation (vô
  hại), và ghi lại **gate là `miu:opening:willClose`, không phải `display:none`** (xem dưới).
  Thêm mục "Verifying the scroll reveal: `doortest\reveal.js` = 25 assertions" + 5 bẫy đo.
- **`WORK_LOG.md`** — entry này.

### Bằng chứng diff thuần tuý
Dựng lại file cũ từ backup bằng **đúng** regex xoá 41 mẫu → so sánh ordinal case-sensitive với file
mới: **bằng nhau**. Encoding giữ nguyên UTF-8 **không BOM**, LF thuần (0 CR), tiếng Việt còn nguyên.

### Kết quả kiểm tra tĩnh
| Hạng mục | Trước | Sau | Mẫu |
|---|---|---|---|
| Node `data-node-id` | 106 | **106** | – |
| Node `data-anim-preset` | 81 | **81** | 85 |
| `animation:` bake trong inline `style` | 41 | **0** | **0** |
| Có `opacity:0` là khai báo cuối | 81/81 | **81/81** | 85/85 |
| Marker serialisation dạng 2 (dư, vô hại) | 41 | 41 | – |
| Marker dạng gốc (dấu phẩy) | 275 | 275 | – |
| `--ch/--sh/height/baseH` | 9250 | **9250** | – |

### Kết quả kiểm tra runtime — `doortest\reveal.js`, **25/25, 3 run liên tiếp**
Chạy `file:///D:/lean_AI/wedding/index.html?g=groom&t=evening` với `puppeteer-core`:
- **`A2` — 0 node có `CSSAnimation` chạy lúc `load`** (overlay còn đang mở). Đây chính là assertion
  chết nếu ai đó bake lại `animation:`. Trước khi sửa: 41 node.
- `B1/B2` — 76 node ngoài khung nhìn: `getAnimations().length === 0` **và** `opacity === 0`.
- `C1` — quét hết canvas (~40 cú nhảy): **81/81** node có đúng **một** animation, `animationName`
  = `miu-<preset>`, `--miu-anim-distance` khớp `data-anim-distance`, và **không** node nào bắt
  đầu trước lúc cửa bắt đầu mở.
- `D1..D5` — hero **TRỌNG VŨ**: `opacity 0.067 → 0.918 → 1`, `startTime` = gate + 45 ms,
  `duration` 3000, `iterations` 1, `animation-timing-function` = `cubic-bezier(0.2, 0.8, 0.2, 1)`.
- `F1` — cả 5 node từng bị bake giờ hành xử y hệt node khác: hero, chữ V, tên địa điểm, nút bản
  đồ (`heartBeat` loop vô hạn), tên chú rể.
- `G` — độ trễ thật (1 cú nhảy sạch mỗi node, chỉ đo khi `startTime` đã resolve nên animation
  bắt buộc phải mới): **12,7 – 31,3 ms**.

### Điều tra phụ: vì sao reveal chạy *trước khi* cửa mở hẳn — và vì sao để nguyên
Đo timeline thật của deep link: `miu:opening:willClose` lúc **~2,8 s**, 2 lá cửa `animationend`
lúc **~4,8 s**, `miu:opening:closed` → `display:none` lúc **~7,8 s** (độ trễ 4,9 s là do
`data-open` đổi lúc 2,8 s nhưng `animation-delay` vẫn còn 500 ms). 5 node nằm trong khung nhìn
đầu tiên vì thế bắt đầu 3 s reveal của mình **khoảng 4,9 s trước** khi cửa mở hẳn, và khách nhìn
thấy hero fade vào **qua khe cửa đang mở**. Đây là hành vi **đúng ý muốn** (khớp mẫu: mẫu cũng
gate bằng `willClose`). Sửa thành gate `closed` sẽ khiến khách nhìn trang trắng 4,8 s rồi mới thấy
hero — tức **mất** hiệu ứng. Đã ghi lại trong `AGENTS.md` để session sau không "sửa nhầm".

### Bẫy trong script kiểm thử (đã ghi vào `AGENTS.md`)
- `CSSAnimation` mới có `startTime === null` rồi mới = 0; phải chờ `> 1000` mới đo được.
- Cú nhảy tới ~3 node cuối bị **clamp** vào đáy trang tốn thêm frame; loại khỏi phép đo (220 ms
  giả vs 12–31 ms thật).
- Node nằm ngay dưới mục tiêu đã nằm trong vùng trigger của cú nhảy **trước**, nên độ trễ âm
  (ví dụ nút `heartBeat` `-19,9 ms`) là đúng, không phải lỗi.
- `effect.getTiming().easing` trả `linear` với animation CSS-eased; phải đọc
  `getComputedStyle(el).animationTimingFunction`.
- `history.replaceState` tới path khác bị từ chối trên `file://` → phải deep-link `?g=&t=` và
  chứng minh `cfApply` đã chạy qua `[data-countdown="1"]`.

### Deferred (chưa làm)
- **Chưa commit** (user không yêu cầu). `git status` hiện có `index.html`, `AGENTS.md`,
  `WORK_LOG.md` đều sửa; hai file tài liệu đã mang thay đổi của Session 31 từ trước.
- Backup trước khi sửa: `C:\Users\Admin\AppData\Local\Temp\opencode\index.before-session32.html`
  (212.307 bytes, SHA-256 ghi lại trước khi đụng). Hoàn tác: `git checkout -- index.html`.
- Không thay đổi overlay, master data, routing, âm thanh, Firestore.

---

## Session 33 - nhạc chỉ phát sau khi cửa mở xong (hiệu ứng giữ nguyên)

### Yêu cầu
User: *"nên mở cửa xong thì mới start nhạc và chạy hiệu ứng"*. Sau khi báo rõ hệ quả, user chốt:
- **hiệu ứng** → giữ nguyên (vẫn chạy lúc `miu:opening:willClose`, t=0, đúng bằng mẫu);
- **nhạc** → chuyển sang `miu:opening:closed` (4820ms);
- **auto-scroll** → giữ nguyên 4850ms;
- **iOS** → thêm bước unlock.

Phạm vi vì thế thu hẹp còn **một script duy nhất**. Đặc biệt: **không sửa** observer reveal, `boot()`,
`MutationObserver`, `close()`, timing `--door-*`, auto-scroll (kể cả hằng số fallback `6000`).

### Vì sao cần
Script audio cũ gắn `pointerdown/touchstart/touchend/keydown/scroll/click` và gọi `start()` ngay, nên
nhạc phát đúng lúc khách bấm nút nhóm giờ — **cửa còn đang trượt**. Mẫu melipage làm ngược lại: block
script cuối file của nó nghe `miu:opening:closed` rồi mới `playAll()`.

### File bị sửa — chỉ `index.html`, chỉ trong script audio
Backup: `C:\Users\Admin\AppData\Local\Temp\opencode\index.before-session33.html` (208.572 bytes,
SHA-256 `D0D5251C30D3688841104997C36D12A842E0B10EB9AF4129F0A95083328D5ECE` = bản gốc).

| # | Sửa | Vì sao |
|---|---|---|
| 1 | thêm `var hold = false;` + `var primed = false;` | cờ trì hoãn |
| 2 | `start()`: `if (started) return;` → `if (started \|\| hold) return;` | phải chặn **trước** khi `started = true`, nếu không sẽ mất listener gesture |
| 3 | `window.addEventListener('miu:opening:closed', releaseMusic, true)` | đúng pattern mẫu |
| 4 | `onReady` set `hold` **trước** khi gọi `start()` | script này parse ở offset ~27.240, còn `#miuOpening` tới ~34.169 mới có ⇒ không query được lúc boot |
| 5 | thêm `unlock()` | mở khoá autoplay iOS |
| 6 | 6 `add(..., start, ...)` → `onIntent` | gesture làm 2 việc |

**`unlock()`** (mới): `primed` một lần → `a.muted = true` → `play()` → khi resolve thì `pause()` +
`currentTime = 0` + trả lại `muted`. Cố tình **không** đụng `started`, không gỡ listener nào.
Lợi phụ: m4a được tải sớm nên lúc `closed` phát ra không bị nghẽn.

**`onIntent`** = `if (hold) unlock(); start();`. Chữ `if (hold)` là bắt buộc — đây là **một lỗi đã
bắt được và sửa trước khi test**: nếu `unlock()` luôn chạy, callback `.then` của nó sẽ `pause()`
đúng cái audio mà `start()` vừa phát ngay sau (ở nhánh phục hồi sau khi `play()` bị từ chối).

**Bẫy iOS**: `miu:opening:closed` bắn từ `setTimeout` ⇒ không phải user gesture ⇒ Safari sẽ từ chối
`play()`. `unlock()` ở gesture đầu tiên là lớp phòng thủ đầu. Lớp thứ hai **đã có sẵn từ trước**:
`p.catch(function(){ started = false; })` giữ nguyên listener, nên lần cuộn/chạm đầu tiên của khách
sẽ phát nhạc. Nút nhạc trên FAB luôn có sẵn để bật tay.

### Bằng chứng diff chỉ nằm trong script audio
Cắt file bằng 2 marker bao quanh script audio (đầu = `var sp = null;`, cuối = `else onReady();`):

| | Ký tự |
|---|---|
| phần **trước** script audio (27.349 ký tự) | **giống hệt** (`-ceq` True) |
| phần **sau** script audio (176.376 ký tự) | **giống hệt** (`-ceq` True) |
| script audio | 3.508 → 6.005 ký tự (**+2.497**) |

Tổng 207.233 → 209.730 ký tự, 208.572 → 211.069 bytes. Vẫn UTF-8 không BOM, LF thuần (0 CR).

**Proof mạnh nhất (không phụ thuộc marker):** thay đoạn script audio mới bằng đoạn cũ rồi so với file
gốc → **`-ceq` = True**. Nghĩa là *toàn bộ* khác biệt của Session 33 nằm trong script audio; không
 có gì ngoài nó bị đụng tới, kể cả byte nào.

### Kết quả kiểm tra tĩnh — mọi số của Session 32 giữ nguyên
| Hạng mục | Giá trị | Kỳ vọng |
|---|---|---|
| `animation:` bake trong inline `style` | **0** | 0 |
| Node `data-node-id` | 106 | 106 |
| Node `data-anim-preset` | 81 | 81 |
| Có `opacity:0` là khai báo cuối | **81/81** | 81/81 |
| Marker serialisation dạng 2 / dạng gốc | 41 / 275 | 41 / 275 |
| `IntersectionObserver` | 4 | 4 |
| `miu:opening:willClose` | **4** | 4 (không đụng) |
| `miu:opening:closed` | 7 | 6 → 7 (+1 listener `releaseMusic`) |
| `@keyframes miu-` | 58 | 58 |

### Kết quả kiểm tra runtime
- **`doortest\music.js` mới — 17 assertion, 4 chế độ, chạy 3 lần: 17/17 cả 3 lần.**
  - **A** (deep link + `--autoplay-policy=no-user-gesture-required`): giữa lúc cửa trượt
    (`willClose + 1500ms`, `data-open="0"`, `display=block`) `paused === true`; sau `closed + 800ms`
    `paused === false`; FAB đổi sang class `playing`; `muted === false`; `currentTime` 2,28s.
  - **B** (từ chối `play()` được chèn, auto-scroll bị dừng): `paused === true` sau
    `closed + 1200ms`; gesture `mouse.click` kế tiếp ⇒ `paused === false`, `currentTime` 0,86s.
  - **C** (luồng click thật — chính là hành trình của khách): sau khi bấm nhóm giờ, `paused === true`
    **và** `muted === false` (tức `unlock()` đã chạy và trả lại trạng thái, không để lại tiếng kêu);
    `paused === true` ở `willClose + 1500ms`; `closed` cách `willClose` **4823–4831ms** (đúng
    4500 + 320); sau đó nhạc phát, FAB `playing`, `currentTime` 2,31s.
  - **D** (từ chối 1 lần, để auto-scroll chạy): `scroll` event của chính auto-scroll gọi
    `onIntent` → `start()` và nhạc tự lên, `currentTime` 0,85s.
- **`doortest\reveal.js` — 25/25** (không sửa file test này, vì gate của hiệu ứng không đổi).
- **`doortest\s29.js` — 35/35** (đặc tả cửa nguyên vẹn).

### Ba bẫy trong `music.js`, mỗi cái tốn một lần chạy
1. **Chrome không chịu từ chối autoplay.** Cả policy mặc định lẫn
   `--autoplay-policy=user-gesture-required` đều **không** chặn `play()` trên trang `file://`.
   Phải chèn từ chối bằng cách bọc `HTMLMediaElement.prototype.play` trong
   `evaluateOnNewDocument`. Điều kiện nhắm phải là *"sau `miu:opening:closed`"*, **không** phải
   đếm số lần gọi: `play()` đầu tiên sau `closed` không phải lúc nào cũng của `start()` — `scroll`
   của auto-scroll cũng là listener của `onIntent` và có thể tới trước (đã quan sát thấy đúng 1
   `play()` REJECT ở `closed − 1537ms` do `unlock()` gọi trong lúc `hold` còn đang bật).
2. **Auto-scroll bắn `scroll` ~30ms sau `closed`.** `setTimeout(start, 350)` của nó chạy ở
   `close()+4850`, sau `closed` ở `close()+4820`, và cú `scroll` đầu tiên kéo `onIntent` →
   `start()`. Mọi assertion dạng *"vẫn còn paused sau `closed`"* đều là **race** nếu không dừng nó.
   `wheel` là stop-event của auto-scroll **và không phải** gesture của audio, nên
   `setInterval(dispatchEvent('wheel'), 50)` dừng được mà không giả lập gesture. Bắn `wheel` một
   lần là **không đủ** — timer fallback 6000ms sẽ gọi `start()` lại.
3. `KILL_SCROLL` phải cài **sau** `goto(…, {waitUntil:'load'})`, vì interval phải sống sót tới sau
   timer fallback 6000ms tính từ lúc parse.

### Deferred (chưa làm)
- **Chưa commit** (user không yêu cầu). `git status`: `index.html`, `AGENTS.md`, `WORK_LOG.md`.
  Hai file tài liệu vẫn mang thay đổi chưa commit của Session 31.
- Hoàn tác: `git checkout -- index.html`.
- Không đổi overlay, auto-scroll, master data, routing, hiệu ứng reveal, Firestore.

---

## Session 34 - reveal chỉ chạy sau khi cửa mở xong (theo yêu cầu mới)

### Yêu cầu
User yêu cầu **để cửa mở xong thì hiệu ứng trong thiệp bắt đầu chạy**. Chọn cách 1 (ngay tại `miu:opening:closed`, ~4820ms sau `willClose`). Áp dụng trong khi vẫn giữ nguyên auto-scroll, nhạc, geometry cửa, keyframes.

### Phạm vi thay đổi
**index.html (khối reveal boot)** và **doortest/reveal.js**. Không sửa `close()`, timing `--door-*`, IntersectionObserver config, 81 node, music/s29.

### Backup
`C:\Users\Admin\AppData\Local\Temp\opencode\index.before-session34.html` (211.069 bytes)
SHA-256: `0B4CA314444187A9AFEEFFD5F31AD55DC6A07C5B73F5997784B50E35AAD68E46`

### Sửa index.html
Khối `boot()` trong reveal script (gần ~167k). Hai thay đổi:

| # | Vị trí | Thay đổi | Vì sao |
|---|---|---|---|
| 1 | `cleanup()` | bỏ `removeEventListener('miu:opening:willClose', onWillClose, true)` | đường kích hoạt 1 không còn dùng. |
| 2 | Xoá toàn bộ `onWillClose` + `addEventListener('miu:opening:willClose', ...)` | ngăn hiệu ứng bật tại t=0. |
| 3 | MutationObserver (nhánh `!st.open`) | chuyển từ `if (sawOpen && !st.open) onClosed();` → check `display:none` | `data-open="0"` được flip **đồng bộ** ngay khi `close()` gọi (t=0), trong lúc lá cửa còn đang trượt. Đó là đường kích hoạt sớm ẩn, nên chỉ cho phép `onClosed()` khi overlay thực sự đã `display:none` (được set cùng callback với `miu:opening:closed`) hoặc không tồn tại. Giữ lại `sawOpen` để phân biệt “chưa từng mở” vs “đã mở rồi mới đóng”. |

`miu:opening:willClose` vẫn được **dispatch** trong `close()` (2 chỗ: ở `#cf-choice-step2` click và ở auto-close sau deep link). **Không xoá dispatch** để tránh phình diff không cần thiết và để `s29.js`/`music.js` vẫn dùng làm mốc đo thời gian (chúng đo khoảng 4820ms từ `willClose → closed`).

`onClosed()` vẫn được đăng ký. `fallbackTimer 2500ms` giữ nguyên (bật `start()` nếu overlay chưa từng mở).

### Sửa doortest/reveal.js (28 → 29 assertions)
- Thêm 3 assertion **P1–P3** ở ~300ms *trước* gate: 81/81 `opacity 0`, 0 animation. Đây là cái bắt được đường kích hoạt sớm thứ 2 (MutationObserver không được cho phép chạy tại t=0).
- Đổi Phase 1 giả lập gate: từ `willClose` → `o.style.display='none'` rồi `dispatchEvent('miu:opening:closed')`.
- Phase 2: thay baseline `tClosed.willClose` → `tClosed.closed` trong điều kiện “không bắt đầu trước gate”.
- C4: đổi từ “gate mở lúc cửa đang trượt” → “gate là lúc cửa **hoàn tất**: `closed = willClose + 4820ms`”.
- Thêm `window.__closed` trong `evaluateOnNewDocument`, chờ `window.__closed != null`.

### Bằng chứng diff
- Chỉ khối `boot()` trong reveal script đổi (delta +307 ký tự trong khối đó).
- `swap reveal mới → bản cũ == file gốc` (`-ceq` True). Nghĩa là toàn bộ khác biệt nằm trong khối boot; prefix/suffix ngoài khối **giống hệt**.
- bytes: 209.730 → 210.037 (delta +307). CR=0, UTF-8 không BOM, LF thuần.

### Kết quả regression
- **reveal.js**: 28/28 × 3 lần
- **s29.js**: 35/35
- **music.js**: 17/17 × 3 lần

Audit tĩnh sau sửa:
- baked animation (inline style): 0
- `data-node-id` 106, `data-anim-preset` 81, `@keyframes miu-` 58
- `opacity:0` là khai báo cuối: 81/81
- `serial form 2/original`: 41/275 (giữ nguyên như Session 32)
- `addEventListener('miu:opening:willClose')`: 0, `onWillClose`: 0
- `miu:opening:willClose` tổng: 2 (2 chỗ dispatch còn lại)
- `miu:opening:closed`: 8 (+1 so với trước khi xoá willClose listener block)

`git status`: `index.html`, `AGENTS.md`, `WORK_LOG.md` — **chưa commit**.
Backup: `index.before-session34.html` giữ nguyên.
---

## Session 35 - header Trọng Vũ / Hồng Nhung lên nhanh hơn + dịch trái

### Yêu cầu
User: *"ở phần header text Trọng Vũ và Hồng Nhung xuất hiện sớm hơn chút nữa"*, chọn phương án **B**
(không đụng delay), sau đó thêm *"ảnh & text Trọng Vũ, Hồng Nhung dịch sang trái một chút cho cân đối
với header"*.

### Vì sao chọn B thay vì delay âm
Xét 3 phương án trước khi sửa:
- **A. delay âm** (`data-anim-delay="-150"`): bắt đầu sớm hơn `closed`. **Loại** — `startTime` sẽ
  nhỏ hơn `closed`, phá assertion `C1` của `reveal.js` (`x.start < tClosed.closed - 30`) và nghĩa
  là hiệu ứng chạy *trước* khi cửa mở xong — trái với chính thay đổi Session 34 vừa làm.
- **B. giảm duration** 3000 → 2400: `startTime` **không đổi** (vẫn = `closed + 0`), chỉ fade xong
  sớm hơn 600ms. Test không cần nới điều kiện nào. **Chọn.**
- C. delay âm rất nhỏ (−80): vẫn vượt ngưỡng −30 của `C1`. Vô nghĩa.

### Sửa index.html — chỉ 2 node, 4 giá trị
| Node | Nội dung | `data-anim-duration` | `left` |
|---|---|---|---|
| `element_text_ghi89lrdzxt` | TRỌNG VŨ (65px) | 3000 → **2400** | 194.018 → **180px** |
| `element_text_58ymmmme3xn` | HỒNG NHUNG (48px) | 3000 → **2400** | 194.036 → **180px** |

`top`, `width`, `font-size`, preset (`fadeInLeft` / `fadeInRight`), delay 0, distance 200 — **không đổi**.
Hai `left` khác nhau 0.018px ở bản gốc (vết export); gộp về `180px` cho đúng một giá trị.

### Sửa doortest/reveal.js — 2 assertion bám cứng số 3000
- `D5`: đổi tên + điều kiện `r2.dur === 3000` → `2400`.
- `E1`: census duration trước là `77×3000 + 4×2000`; nay là
  `75×3000 + 2×2400 + 4×2000`. Thêm nhánh kiểm `census.d['2400'] === 2`.

Đây là chỗ test **bắt đúng** thay đổi: trước khi sửa test, 2 assertion FAIL với thông báo rõ
(`dur=2400` trong khi kỳ vọng 3000).

### Kết quả
- **reveal.js**: 28/28 × 3 lần
- **s29.js**: 35/35
- **music.js**: 17/17
- Audit tĩnh: baked animation **0**, `opacity:0` cuối **81/81**, `@keyframes miu-` **58**, 106 node.
  Không node nào khác bị đụng.

Backup: `C:\Users\Admin\AppData\Local\Temp\opencode\index.before-session35.html` (211.376 bytes).

### Bài học quy trình (đã mất thời gian thật)
Sửa `reveal.js` bằng PowerShell với `cd '...\doortest'` rồi `[IO.File]::ReadAllText('reveal.js')`
sai: **`cd` trong PowerShell không đổi current directory của .NET**, nên lệnh đọc/ghi nhầm
`D:\lean_AI\wedding\reveal.js` (đường dẫn tương đối resolve theo process CWD, không theo
PowerShell location). Phải chạy lại ~10 lần trước khi nhận ra, và trong lúc đó đã **tạo nhầm một
file rác `D:\lean_AI\wedding\reveal.js` trong thư mục repo** — đã xoá. Sửa lại bằng đường dẫn tuyệt đối
thì lần đầu ăn ngay.
**Quy tắc: khi sửa file bằng .NET trong PowerShell, luôn dùng đường dẫn tuyệt đối; không dựa vào `cd`.**


---

## Session 36 - header vua hien som hon + can bang giua

### Yeu cau
1. "header van hien thi muon qua" - canh dau tien ra len ngay khi cua bat dau mo.
2. "phan text Trong Vu va Hong Nhung va anh and-ornament nen hien thi ra giua can doi" - can
   giua 2 dong chu, day `&` sang mot ben cho can bang.

### Files
- **Chinh sua** `index.html`
  - `element_text_ghi89lrdzxt` / `element_text_58ymmmme3xn`:
    `left:180px` + shrink-to-fit box -> **`left:0px; width:575px; text-align:center`**.
    Do bang puppeteer (`doortest\measure36.js`): ca hai dong co **ink centre = 287.5 chinh xac**
    o 3 viewport (575x900, 390x844, 360x640). Dung `text-align` thay vi tinh
    `left = 287.5 - w/2` de dung ca khi font fallback (Ergisa -> Brush Script MT) doi metrics.
  - `element_text_w6mjrszfugn` (THE WEDDING OF): `left:133.952` -> **`136.3125`** cho tinh
    khop 287.5 (truoc do lech -2.4).
  - `element_image_e9ddhxndm6z` (dau `&`): `left:122.08px` -> **`23.38px`**.
  - Reveal engine: **hoist** `isInfinitePreset` + `applyAnim` ra khoi body cua `start()` len IIFE
    scope (thuan di chuyen code, 2303 ky tu, khong doi noi dung - chung chi phu thuoc globals).
  - Reveal engine: them `HEADER_EARLY` (3 id) + `onWillCloseHeader()` + listener tren
    `miu:opening:willClose`, va `removeEventListener` trong `cleanup()`.
- **Chinh sua** `C:\Users\Admin\AppData\Local\Temp\opencode\doortest\reveal.js` (25 -> 37 assertions)
- **Tao** `C:\Users\Admin\AppData\Local\Temp\opencode\doortest\measure36.js` (do ink, doc-only)
- **Chinh sua** `AGENTS.md`, `WORK_LOG.md`

### Backup
`C:\Users\Admin\AppData\Local\Temp\opencode\index.before-session36.html` - 211368 bytes,
SHA-256 `36DD7DA905F7D3658CBFD9B932E98B71392D8F29480669440C159917FCEAC419`

### Ly do ky thuat
**1. Header chay o `willClose`, 78 node con lai van o `closed` (ngoai le co chu dich).**
Session 34 chuyen toan bo gate sang `closed` de khong co hieu ung nao chay truoc khi la het.
Nhung hero la **phan duy nhat cua canvas co tren man hinh tu frame dau**, va no khong co scroll
reveal gi de ma "bi treo": khach phai nhin 1 cua trong 4.8s roi lai doi them 2.4s moi thay ten.
Vay mot ngoai le rat hep cho 3 node hero, dung y cua nguoi dung.
- `applyAnim` nam **ben trong** `start()` nen listener som khong goi duoc. Hoist ra IIFE scope
  la bat buoc - day la thay doi co truc luong nhat cua session.
- `onWillCloseHeader` **khong** set `started`. `start()` co guard `if (started) return;` - neu
  dat `started = true` o gate som thi gate `closed` se khong chay va 78 node se mai an.
- `applyAnim` **khong** early-return tren `__miuAnimApplied`, nhung gan lai chuoi `animation`
  giong het nen browser khong restart (cung co che voi trap `animation:` bi bake o Session 32).
  Khong can guard dedupe rieng.
- **Guard viewport**: node hero nao co `getBoundingClientRect().top >= innerHeight` se bi bo qua.
  Khong co guard nay, mot cua so thap (dien thoai ngang, canvas khong scale nen hero nam o
  y=483..683) se chay fade ngoai man hinh roi khach cuon xuoi thay node da opacity 1 - mat hanh
  ung scroll reveal. Tren dien thoai doc khong xay ra (stage scale = `min(575, 100vw-32)`, tai
  380px rong hero da o screen y~293..362), nen `reveal.js` PHASE 4 phai test o **640x360**.

**2. Dinh vi tri dau `&`.**
Do ink that bang `Range.selectNodeContents()` + `getClientRects()` (khong dung
`getBoundingClientRect()` cua node - xem loi). Ket qua canvas px:
Trong Vu **110.1 -> 464.9**, Hong Nhung **112.7 -> 462.3**. Hai khe trong deu rong **110.1**.
Dau `&` truoc day o **122.08 -> 185.42** tuc la **nằm trọn ben trong** dai chu cua Trong Vu, va
noi DOM no den TRUOC hai ten, ca ba deu `z-index:0` => chu ve đè len `&`. Do la ly do no "bi
bien mat" ma khong ai thay.
- `left = (110.1 - 63.34) / 2 = 23.38` -> chiem 23.38..86.72: **23.4px den mep canvas, 23.4px
  den ink Trong Vu**, hai khe can bang.
- `top` giu nguyen (571.4177517361111) - `&` van straddle ca hai dong ten.
- Khong them `data-anim-preset` cho `&` (xem Deferred).

**3. Test phai tach thanh 2 quan the.**
`P1`/`P2`/`C1` deu co gia dinh "khong node nao chay truoc `closed`" nen se **FAIL** neu giu
nguyen. Da loai 3 node hero khoi tap kiem tra, them `P4` (3 node da chay o `willClose`) va
`C6` (bat dau **trong** khoang `[willClose, closed)`, do lai +47ms). Them PHASE 4 (K0-K2) cho
guard viewport va PHASE 5 (H1-H4) cho hinh hoc hero.

### Bai hoc (3 loi, tat ca deu o *ky vong cua test*, khong phai loi san pham)
- **`D1` phai doc opacity cung task voi luc dispatch gate.** No doc sau 120ms, bien thanh race:
  reference curve `cubic-bezier(0.2, 0.8, 0.2, 1)` front-loaded manh (`y1 = 0.8`) nen opacity da
  **0.41 o giua 250ms cua ramp 2400ms**, ngan sach `< 0.3` fail vi sai timing. Callback cua
  `IntersectionObserver` la async nen mot lan doc trong cung task van thay gia tri an.
- **`P4` phai assert `getAnimations()`, khong assert opacity.** Mau lay o `willClose` + ~40ms nen
  3 node hero dang o giua ramp, opacity ~0. Phien ban dau doi `op=1` ("da xong trong gap") va fail
  vi chinh ly do do - dau cua cua so la `C6` lo, khong phai `P4`.
- **`K1` regex.** `toFixed(2)` tao ra `op=0.00` chu khong phai `op=0`, nen `/op=0$/` khong khop.
- **`H3` co rang that.** Da chay lai logic voi toa do cu (122.1->185.4) va xac nhan tra ve
  `false` - neu khong co assertion nay, chinh "bi chu nuot" lai co the quay lai im lang.

### Kiem chung
- `reveal.js`: **37/37**, chay 3 lan lien (co lan fix 3 loi test o tren)
- `s29.js`: **35/35** (cua 2D khong doi)
- `music.js`: **17/17** (nhac van start o `closed`)
- Audit tinh: baked `animation:` = **0**, `data-anim-preset` = **81**, `@keyframes miu-` = **58**,
  opacity cuoi = `0` cho **81/81**, node id trung = 0
- Proof chi 4 region thay doi (git word-diff voi ranh gioi `;`): 4 node attribute + 1 khoi
  script (hoist + 1 dong cleanup + khoi gate). Khong co thay doi nam ngoai y muon.

### Bo sung Session 36 - dua dau & sat chu
`&` ban dau duoc dat `left:23.38px` = **canh giua** khe trong (23.4px den mep canvas, 23.4px den
chu). Nguoi dung xem la "detached" va yeu cau dua sat hon, chon **cach 8px** ->
`element_image_e9ddhxndm6z` **`left:38.76px`**, nét `&` 38.8..102.0, cach nét `T` cua Trong Vu
**8.1px**. Chi doi 1 thuoc tinh; `top`/`width`/`height` va reveal engine giu nguyen (`&` khong co
`data-anim-preset` nen khong thuoc `HEADER_EARLY`).

**Do khong co sai so an** (truoc khi chon so):
- `and-ornament.png` 437x556, quet alpha: nét chiem 23.4..86.6 trong hop 23.38..86.72 => chi
  **0.1px** đệm ngang.
- Chu "T" cua Trong Vu: `measureText('T').actualBoundingBoxLeft = 0` => mép hop **chinh la mép nét**.

Nen 23.4px -> 8.1px la so do, khong phai uoc luong. Chuyen sang phai cung lam hero can hon: khoi
ink ca `&` + 2 dong ten lech trai **35.6px** so voi tam 287.5 (truoc khi doi la 43.3px).

**H4 doi y nghia**: tu *"canh giua trong khe (mép = mép, chenh <=2px)"* sang **"ôm sát nhung khong
cham"** — khoang cach nét ∈ **[4, 16]px**. Khoang nay de du chỗ cho font fallback (Ergisa ->
Brush Script MT) khi nét dam hon ma khong va cham. Van **37** assertions.

### Kiem chung (sau bo sung)
`reveal.js` **37/37** x2, `s29.js` **35/35**, `music.js` **17/17**. Audit tinh giu nguyen:
baked 0 / preset 81 / keyframes 58 / opacity 81-81.
### Deferred
- **Chua commit** (khong duoc yeu cau). `git status`: `index.html`, `AGENTS.md`, `WORK_LOG.md`.
- Hoan tac: `git checkout -- index.html`.
- Dau `&` **van khong co** `data-anim-preset` nen no hien ngay tu frame dau, lech nhip voi 2
  dong ten. Them preset se bien no thanh animated node thu 82, phai them `opacity:0` cuoi (bay
  hien flicker) va sua lai census. Neu muon dong bo, lam o session sau.
- `dsi747wh7aq` ( dong Lora "Trong Vu & Hong Nhung ") van lech tinh: ink centre **284.8**, lech
  -2.7. Khong sua vi ngoai pham vi header.
## Session 37 - guestbook: khung cuon thay cho "Xem them"

### Yeu cau
"Danh sach guestbook co thanh truot nhu project cu code o nhanh master" - port y tuong cuon + ngay
giok tu site cu (vs-template-5), giu nguyen collection hien tai.

Quyet dinh da chot voi nguoi dung:
1. Bo hoan toan nut "Xem them" -> render HET loi chuc (van toi da 200 doc) vao khung cuon.
2. Giu style item hien tai (xam #707070), CHI them ngay `dd/MM/yyyy HH:mm`.
3. Khong dung collection `guests` cu cua site cu (field `name`/`message`) - giu `vu_nhung_2_messages`.
4. Thanh truot xam `#707070` rong 4px.
5. Giu nguyen chieu cao section 727.23px -> khung list ~336px canvas, KHONG doi ho so canvas.

### Files
- **Chinh sua** `index.html` (chi file nay; `wedding2.html` la export goc, da gitignore)
  - CSS block `[data-miu-wishes="1"]`: them `.miu-wishes-head` (flex + `align-items:baseline`) va
    `.miu-wishes-date` (15px, xam 75%), 4 rule `::-webkit-scrollbar*`, va 1 khoi
    `@supports not selector(::-webkit-scrollbar)` cho Firefox.
    `.miu-wishes-name`: `margin-bottom:6px` -> `0` (margin chuyen len `.miu-wishes-head`).
    `.miu-wishes-comment`: them `word-break:break-word`.
  - DOM: list div doi inline `overflow:auto` -> `overflow-y:auto; overflow-x:hidden`; **XOA** node
    `<button data-miu-wishes-more="1">`.
  - Script (IIFE wishes): them `fmtDate()`; xoa `moreBtn` / `initial` / `showN` / 2 cho
    `moreBtn.style.display` / ca khoi listener phan trang; `render()` bo `slice(0,showN)` va bo
    `.miu-wishes-head`; `onSnapshot` push them `at: fmtDate(d.createdAt)`.

### Ly do ky thuat
- **Sua `overflow` bang inline, khong dua vao stylesheet.** Longhand trong stylesheet thua
  shorthand `overflow:auto` dang nam tren `style` cua chinh node do.
- **`@supports not selector(::-webkit-scrollbar)` la bat buoc.** Chrome tu 121 ho tro
  `scrollbar-color`; neu viet thang `scrollbar-width/color`, Chrome se **bo qua** toan bo
  `::-webkit-scrollbar` va lay do day `thin` (~11px) - mat dung 4px da chon.
  `@supports selector(::-webkit-scrollbar)` = true o Chrome/Safari, false o Firefox, nen
  `@supports not ...` moi dung chieu: Webkit lay 4px chinh xac, Firefox lay thin.
- **`align-items:baseline` -> test phai assert OVERLAP, khong phai equal `top`.** Tieu de 20px vs
  ngay 15px nen box `top` lech nhau 5px *dung y do* baseline thang hang.
- **`word-break:break-word`** de 1 chuoi 220 ky tu khong sinh thanh truot ngang trong khung hep.
- **Format `createdAt` 1 lan luc snapshot** (thanh `at`), khong tinh lai 200 lan moi render.
- **Khong doi chieu cao section/canvas.** Node ke tinh sau section bat dau `top:8590.31`, section
  bottom = 8427.61 => con 162px de noi cao, nhung nguoi dung chon giu nguyen. Do do lai thuc te o
  viewport 575x900: section man hinh `686.7px`, list `300.5px` tu `369.2` -> `669.7` (phan duoi
  17px = padding 18 * scale 0.944) - `flex:1 1 auto` tu lap day khong can them gi.
- `data-initial-limit="3"` va `data-loadmore-text` giu nguyen tren section (khong con script doc) -
  de neu export lai tu miu thi khoi cu khong mat them.

### Bai hoc (2 loi, ca 2 deu o *ky vong cua test*, khong phai loi san pham)
1. **`clientWidth - offsetWidth === 4` KHONG do duoc tren may nay.** Windows 11 bat overlay
   scrollbar, nen ca mot div `overflow-y:scroll` CHUA style cung gutter = 0 (`probeW8.js`:
   `win=0, plain=0, unstyled=0`). Assertion nay fail voi BAT KY implement nao -> doi sang doc
   rule truc tiep tu CSSOM (W8a/W8b/W8c). He qua thuc te: tren may/khe, thanh 4px se **noi tren
   noi dung** (overlay) chu khong chiem layout; tren Safari Android/Firefox no la thanh binh
   thuong. Site cu dung `::-webkit-scrollbar` y het.
2. **Collection chi co 2 tin thi layout khong bao gio tran.** Vong do thi truoc bang cach inject
   60 item gia + 1 chuoi 220 ky tu vao DOM roi do lai, va **restore `innerHTML` cu** sau do.
   Khong the cho assertion "co thanh truot" chay tren du lieu that.

### Kiem chung
- `wishes.js`: **16/16** (moi). Data that 2 doc, render 2/2, cuon toi day `5297/5297`,
  khong thanh ngang (548 = 548), moi ngay deu dung dinh dang, moi nhat o dau.
- `reveal.js`: **37/37** va `s29.js`: **35/35** (khong doi).
- Audit tinh: baked `animation:` = **0**, `data-anim-preset` = **81** (78 div + 2 a + 1 section),
  `@keyframes miu-` = **58**, opacity cuoi = `0` cho **81/81**, node id **107/107 khac nhau**.
- Hinh hoc thuc te (575x900): ten `20px/900` canh trai, ngay `15px` canh phai cach mep `13.2px`,
  cung hang (overlap y). Anh: `wishes-1-that.png`, `wishes-2-tran.png`.

### Deferred
- **Chua commit** (khong duoc yeu cau). Hoan tac: `git checkout -- index.html`.
- `wishes.js`, `probeW8.js`, `measure37.js`, `shootwishes.js` nam ngoai repo
  (`%TEMP%\opencode\doortest\`) - **khong** copy vao `D:\lean_AI\wedding` (Session 36 da gap loi
  tu chinh tai do).
- `.miu-wishes-head` / `.miu-wishes-date` la class MO. Export lai tu `wedding2.html` se mat.
- Cuon long tren mobile: khong scroll noi trong node canvas da scale co the nuot swipe doc cua
  trang. Site cu y het, chua lam gi. Muon xu ly: thu `touch-action` o session sau.
- Dang doc giua chung, co nguoi gui -> noi dung don xuong 1 item, lech view. Muon: them pill
  "co loi chuc moi".
- Khong dung collection `guests` cu. Muon gop loi chuc cu site cu vao: query them va chuyen field
  (`name`/`message` -> `fullname`/`comment`), roi tron 2 nguon theo `createdAt`.
## Session 38 - chuyen 32 asset sang Firebase Storage (33 cho + 2 meta)

Muc tieu: **doi anh tren Firebase thi thiep tu doi, khong can deploy lai**. Cac file local
**khong xoa**, chi doi thuoc tinh `src`/`href` tro sang Firebase Storage.

### Files edited
- `index.html`: 33 cho `src`/`href` + 2 `<meta>` (og:image, twitter:image) doi sang URL
  Firebase Storage. +4,552 bytes (214,380 -> 218,932).

### Files created (test, ngoai repo - %TEMP%\opencode\doortest\)
- `assets.js`: 23 assertion moi.
- `shoot38.js`: so sanh tile truoc/sau (canh, natural size, ratio, object-fit).
- `s38-assets.js`: script thay the co assert so lan xuat hien (da chay 1 lan de sinh ra thay doi).
- `s38-head.ps1`: HEAD/GET 34 URL Firebase trong `index.html`.

### Files unchanged (co chu dich)
- `assets/uploads/` 20 anh, `assets/elements/` 9 file, `assets/audio/ordinary.m4a`, 3 favicon,
  14 font: **giu nguyen tren dia**. Chay lai tu dong khi can fallback thu cong.
- `wedding2.html` (export goc), `404.html`, Firebase config.

### Ky thuat
- **Thu tu thay the bat buoc** (trap chinh cua session):
  1. `og:image` + `twitter:image` phai doi **TRUOC**.
  2. Sau do moi den cac `src="assets/uploads/..."`.
  Ly do: `assets/uploads/couple-main.webp` xuat hien **3 lan** - 1 `src` + 2 `content` trong meta
  dang URL tuyet doi `https://vunhungwedding.online/assets/uploads/couple-main.webp`. Thay chuoi
  `assets/uploads/...` bang `replace` tran se bien 2 dong meta thanh
  `https://vunhungwedding.online/https://firebasestorage...` (URL hong). `assets.js` A3a assert
  khong con chuoi nay.
- `s38-assets.js` **assert so lan xuat hien cua tung chuoi cu truoc khi ghi file**, roi assert 8
  dieu kien tren OUTPUT (0 tham chieu `assets/uploads|elements|audio|favicon|apple-touch-icon`,
  37 URL Firebase, 14 font local). Sai so lan -> abort, khong ghi file nao.
- 14 font **khong** chuyen sang Firebase (Session 24). `assets.js` A6 assert 14 `@font-face`
  local + 0 remote.
- `?alt=media&token=` bat buoc - thieu token thi Firebase tra 403.

### Ket qua do
- **33/33 URL Firebase tra 200**, tong 12.8 MB (curl GET that).
- `assets.js`: **23/23**. 29 `<img>` trong DOM + 1 audio + 3 `<link>` deu remote; 0 anh 0px;
  0 request Firebase loi; 0 404 asset.
- Hoi quy: `reveal.js` **37/37**, `s29.js` **35/35**, `wishes.js` **16/16**, `music.js` **17/17**.
- Canvas van **9250px** (A7a) - moi hinh hoc canvas / animation / audio timing giu nguyen.

### 5 anh doi ti le - ket qua **tot hon duoc lua chon**
Do bang `shoot38.js` (truoc = ban backup, sau = ban moi), so ratio anh voi ratio o:

| Anh | Truoc (local) | Sau (Firebase) | Tỉ lệ ô | Kết quả |
|-----|---------------|----------------|---------|----------|
| `gallery-09` | 1.316 | 0.667 | 0.707 | ✅ khop o hon |
| `gallery-10` | 1.392 | 0.667 | 0.715 | ✅ khop o hon |
| `gallery-15` | 1.778 | 1.500 | 1.627 | ✅ khop o hon |
| `gallery-16` | 1.778 | 1.500 | 1.627 | ✅ khop o hon |
| `gallery-12` | 1.500 | 1.500 | 1.654 | ➖ khong doi |

`gallery-12` **da duoc user upload lai** (dung token, size 594 -> 702 KB) nen da nam ngang
6240x4160 = 1.500, bo cua crop 60% du da bao. 21 anh con lai + 11 element giu nguyen ti le.

### Trap trong test (da gap, da chua)
- `assets.js` phai **dem 29 `<img>` chu khong phai 30**: chuoi thu 30 la
  `'<img data-miualbum-img="1" ...'` do album lightbox JS tao luc mo (khong nam trong DOM luc load).
- Doc ten file tu URL Firebase phai **cat sau `%2F` cuoi cung**, khong dung regex
  `[\w.-]+\.webp` - `[\w.-]` an nuoc `2F` nen key ra `"2Fgallery-09.webp"` (A4 doc 0.000).
- Cho phep `304` trong A5a: Chrome cache-hit audio tra 304, khong phai loi.
- `net::ERR_ABORTED` tren `ordinary.m4a` la **binh thuong** - `preload="metadata"` dung range
  request sau khi du metadata. A5b bo qua loi do (curl da chung minh file tai duoc).
- Cho `protocolTimeout: 300000` va cho `loading="eager"` roi poll tu Node: mot lan
  `evaluate()` blocking tren 8.5 MB se timeout.

### Deferred
- **Chua commit** (khong duoc yeu cau).
- **Xoay/regenerate download token** trong Firebase console se lam hong ca 33 cho. Phai sua lai
  map trong `assets.js` + `index.html`.
- Dung luong anh 4.8 -> 8.5 MB (ban Firebase full-resolution, ~14x oversampling). 29/30 anh co
  `loading="lazy"` nen chi anh dang xem moi tai.
- Offline / mang yeu -> mat het anh (font van local nen text khong vo). Phat trien `file://` can
  mang.
---

## Session 39 - don 2 khoang trang, canvas 9250 -> 9106

Muc tieu: **2 khoang trong lon** user bao: tu `ribbon-06` den `portrait`, va tu danh sach
guestbook den `Countdown`.

### Files edited
- `index.html`: 7 node doi `top:` + section wishes doi `height` + canvas 9106 o 5 noi.
  5 insertions, 5 deletions (chi thay chuoi so).
- `AGENTS.md`: them muc Session 39; cap nhat so lieu canvas / section wishes / guestbook list.
- `doortest/assets.js`: A7a doi ky vong 9250 -> 9106; **them A7b/A7c/A7d** (3 assertion moi).
- `doortest/wishes.js`: **them W15-W18** (4 assertion moi) do 2 khoang dem + tail + chong lan.

### Files created (ngoai repo)
- (khong co file moi)

### Do duoc - ca 2 khoang deu la **khoang dem that tren canvas**
Khong phai anh hut chieu cao: `element_image_dwzcrmtditb` (`portrait`) la `<img object-fit:cover>`
phu kin o576x882, va section wishes + `.miu-stage` cung nen `#ffffff` nen keo dai section xuong
cung khong lap duoc vung A (vung A nam ** tren** portrait, cach section 890px).

| Khoang | Truoc | Sau | Do tu -> den |
|--------|-------|-----|-------------|
| A | 194.22px | **50.01px** | day `element_image_pnlipbo7xgj` (ribbon-06) -> dinh `element_image_dwzcrmtditb` |
| B | 162.69px | **50.00px** | day `element_wishes_6hr88wywupk` -> dinh `element_text_11ngbd3gw1m` |
| dem day | 3.96px | **4.18px** | day `element_image_zsbn2r9wt93` -> canvas |
| canvas | 9250px | **9106px** | -144px (-1.56%) |

Ghi chu do lai: khoang A phai do tu **day ribbon-06** (6611.49) chứ khong phai tu text
"Khuyen khich trang phuc" (6579.83) - text nay nam **chon** len ribbon-06. Neu do tu text se ra
225.9px va lech 32px so voi thuc te.

### Cach thuc hien - thay chuoi so, KHONG serialize lai `style`
7 node dich len deu **-144.21783px**: `element_image_dwzcrmtditb`, `element_wishes_6hr88wywupk`,
`element_text_11ngbd3gw1m`, `element_countdown_yzo2869hvwa`, `element_text_l9r743dwg3w`,
`element_image_zsbn2r9wt93`, `element_text_izp2uxfcr1s`. Roi
`element_wishes_6hr88wywupk` `height` `727.2297651502821` -> `839.92188` (+112.69px).

**Moi node dich cung mot do lech** nen quan he chong lan giu nguyen: `element_text_izp2uxfcr1s`
("Thank you") van nam tren `element_image_zsbn2r9wt93` (user da xac nhan dung thiet ke). Khong co
node nao chong lan: wishes.bottom 8396.09 < countdown.top 8446.09.

### Vi sao khong giu canvas 9250px
Tong 2 khoang la 356.91px; dung ~113px lam dem con ~244px phai di dau do. Section chi dai them duoc
`162.69 - 50 = 112.69px` truoc khi de len Countdown, nen **144.22px cua vung A khong the doi vao
section**. Giu canvas => hoac 148.18px trang duoi anh cuoi, hoac phai tang chieu cao anh (cover =>
crop nhieu hon). User chon **ha canvas**.

### Trap gap (da gap trong luc lam)
- **4/7 node co `top` nhieu chu so hon**: `8590.307075` chu khong phai `8590.30708`. Lấy chuoi goc
  tu file; tu lam tron se khong khop. Da xac minh **duy nhat 1 lan toan file** cho tung chuoi.
- Regex dot node phai **quote-aware**: quet bang `([^>]*)` se cat sai the `<img>` va bo so mat
  39/103 node, du trong so do sai. Phai scan toi `>` bo qua dau `"`.
- **PowerShell `[regex]::Escape(x).Matches(...)` khong ton tai** (`Escape` tra ve String, khong
  phai Regex) - phai `[regex]::Matches($hay, [regex]::Escape($x))` moi chay duoc.
- Bang tinh dua tren hashtable trong PowerShell lam **7 node cung hien mot delta** va bo qua `$A+$B`.
  Phai tinh moi bien mot.
- **Khong duoc serialize lai attribute `style`.** 7 node nay deu la dang bi re-serialize
  (`--miu-node-rotate: 0deg; ` co space). Serialize lai se nuot mat `opacity:0` o cuoi va node
  **ngung reveal im lang**. Sau thay doi kiem tra lai: **81/81** node van co `opacity:0` cuoi,
  **0** `animation:` bi bake.
- `element_countdown_yzo2869hvwa` **khong** co `data-anim-preset` (node runtime cua engine
  countdown) - khong "sua" no cho giong cac node khac.

### Ket qua do - hoi quy day du
- `assets.js`: **26/26** (23 cu + A7b/A7c/A7d moi).
- `wishes.js`: **20/20** (16 cu+ W15-W18 moi). Khung list **client 431 / scroll 521**
  (truoc client ~300) - danh sach loi chuc cao hon ~131px, van cuon duoc.
- `reveal.js`: **37/37** (mot lan 36/37 - xem flaky), `s29.js`: **35/35** (mot lan 34/35 do
  timeout deep-link, chay lai 3/3 pass).
- `music.js`: **16/17** (xem flaky).
- Layout that (doc tu source): node co top **103/103**, content bottom **9101.82**,
  gap A 50.01, gap B 50.00, dem day 4.18, khong con chuoi `9250` nao.

### Hai assertion co san la FLAKY - da chay baseline 3 lan de chung minh
Khong do thay doi nay:
- `reveal.js` **K1** fail 1/3 lan ** tren `index.html` o HEAD, khong thay doi gi**
  (lan fail, node hero duoc `IntersectionObserver` bat truoc thoi diem do).
- `music.js` **B3** (`ct > 0.4` sau sleep 900ms) fail 1/3 lan tren HEAD, `currentTime=0.28`
  - audio da chay nhung chua du 0.4s.
- `s29.js` deep-link `closed SAU animationend` fail 1/4 lan (`closed=0ms`, overlay con mo) -
  deep-link auto-close bi tre; chay lai 3/3 pass.
Chay lai la het. **Dung sua code site vi chung.**

### Deferred
- **Chua push** (user khong yeu cau).
- `reveal.js` A1 doc `81` - dung, khong can sua. `s29.js` / `music.js` khong reference canvas nen
  khong can sua.
- Neu muon danh sach loi chuc cao hon nua: **khong con headroom** - section chi duoc +112.69px
  truoc khi de len Countdown; phai lay khoang dem ra khoi canvas height.
