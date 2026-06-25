# Work Log - Wedding Website Vũ & Nhung

## Session 1: 2026-04-25

### Tasks Completed
- [x] Analyzed existing template structure (`vu-nhung-wedding-cr.html`)
- [x] Created `AGENTS.md` with project instructions
- [x] Created `WORK_LOG.md` to track progress
- [x] Integrated Firebase SDK into HTML (Firestore)
- [x] Added RSVP form handler with Firestore integration
- [x] Added countdown timer functionality

### Next Steps
1. Create Firebase project (user task)
2. Update Firebase config in HTML with real credentials
3. Update wedding date in countdown timer
4. Update images to use Firebase Storage (optional)
5. Test the complete flow

---

## Session 2: 2026-06-03

### Tasks Completed
- [x] Removed all dependencies on `api.vesey.vn`
  - Removed `<base>` tag
  - Inlined vesey.css (đã 404, copy nội dung vào `<style>`)
  - Removed font-loader.js (đã 404, fonts đã load qua Google Fonts)
  - Downloaded 23 wedding images from vesey CDN to `vs-template-5/`
- [x] Added Music Toggle button
  - CSS: nút tròn 48px, góc trên phải, icon local
  - Icon play: `vs-template-5/image-play.png`
  - Icon playing: `vs-template-5/image-playing.png` (quay vòng khi phát)
  - HTML: `<audio>` element + toggle button
  - JS: `toggleMusic()` function (play/pause + class toggle)
  - Created `music/` directory
- [x] Created `.opencode/instructions.md` for auto-logging

### Next Steps
1. Place `wedding-song.mp3` in `music/` folder
2. Create Firebase project and update config (`firebaseConfig` in HTML)
3. Upload 23 images from `vs-template-5/` to Firebase Storage (optional)

## Session 3: 2026-06-03

### Tasks Completed
- [x] Added smooth scroll navigation bar (fixed bottom)
  - 5 items: Trang chủ, Đôi uyên, Hình ảnh, Sự kiện, Xác nhận
  - Inline SVG icons, fixed bottom, white frosted glass background
- [x] Added `id` attributes to all 11 sections for anchor linking
- [x] Added CSS `scroll-behavior: smooth` + JS fallback for Safari
- [x] Enhanced WOW.js animations for individual images & text
  - Added `wow animate__*` classes + `data-wow-delay` to key elements
  - Staggered animations (zoomIn, fadeInLeft, fadeInRight, fadeInUp)
  - Sections: invitation, story, couple, gallery, countdown, timeline, rsvp
- [x] Updated WORK_LOG.md with Session 3

### Next Steps
1. Place `wedding-song.mp3` in `music/` folder
2. Create Firebase project and update config (`firebaseConfig` in HTML)
3. Upload images from `vs-template-5/` to Firebase Storage (optional)

## Session 4: 2026-06-12

### Tasks Completed
- [x] Sửa lỗi Guestbook không cần composite index
  - Bỏ `.where('message', '>', '')` khỏi query
  - Thay bằng lọc message rỗng bằng JavaScript
  - Tăng limit từ 50 → 100 (để bù cho việc filter JS)
  - Update AGENTS.md + WORK_LOG.md

### Next Steps
1. Place `wedding-song.mp3` in `music/` folder
2. Create Firebase project and update config (`firebaseConfig` in HTML)
3. Upload images from `vs-template-5/` to Firebase Storage (optional)

---

## Session 5: 2026-06-16

### Tasks Completed
- [x] Added auto-open 2-flap invitation card overlay (style miuwedding)
  - 2 cánh trái-phải: trái kem (#f4f2ea), phải đỏ burgundy (#7f0505 gradient)
  - Nội dung cánh trái: "Save the date", tên Trọng Vũ & Hồng Nhung, dấu 囍, lời mời
  - Cánh phải: chữ 囍 watermark mờ + radial gradient decoration
  - Animation trượt sang 2 bên 1.2s easing cubic-bezier(0.77,0,0.18,1)
  - Tự động mở sau 2.5s, không cần click
- [x] Changed desktop background to dark (#2c1810) for card contrast
- [x] Auto-scroll chậm xuống #invitation sau khi thiệp mở
- [x] Added Work Logging Rule to AGENTS.md (tự động ghi chép sau mỗi session)
  - File đã sửa: vu-nhung-wedding-cr.html (CSS ~83 dòng, HTML ~13 dòng, JS ~8 dòng), AGENTS.md, WORK_LOG.md

---

## Session 6: 2026-06-16

### Tasks Completed
- [x] Refined overlay to match miuwedding card proportions
  - Card container max-width 575px, height 72vh (giống tỉ lệ miuwedding)
  - Thêm `.cf-card` wrapper bao quanh 2 cánh + chốt
  - Stripe pattern trên 2 cánh (repeating-linear-gradient -45°, 3.5%)
- [x] Added chốt cài thiệp (seal/lock) at center seam
  - Hình tròn 68px, viền vàng gold (#b0852b), nền burgundy (#5a0303)
  - Chữ 囍 vàng, bóng đổ, tự động mờ dần khi thiệp mở
  - Có vòng tròn trang trí bên ngoài (inset -6px)
- [x] Replaced auto-scroll → slow scroll from top to bottom
  - Sau overlay đóng: scroll về #home (page đầu), sau đó chạy slow scroll 40s
  - Easing ease-in-out, hủy nếu user scroll/touch/keypress
- [x] Animation timing: flaps 1.5s (chậm hơn 0.3s so với phiên bản trước)
- [x] Fixed flaps not sliding visibly (overflow:hidden trên .cf-card)
  - Xóa `overflow: hidden` — flaps có thể trượt ra ngoài khung card
  - Tăng max-width 575px → 640px (bằng kích thước thiệp bên trong)
  - File đã sửa: vu-nhung-wedding-cr.html (CSS ~110 dòng, HTML ~15 dòng, JS ~40 dòng), WORK_LOG.md

---

## Session 7: 2026-06-16

### Tasks Completed
- [x] Card overlay height 95vh (full màn hình, chỉ chừa 2.5% trên/dưới)
  - Bỏ `max-height: 520px`, bỏ `height: 72vh` → `height: 95vh`
- [x] Flap animation chậm hơn: 1.5s → 2.5s
- [x] Adjusted JS timing chain cho animation mới:
  - 4.0s: add `.closed` → flaps bắt đầu trượt (2.5s)
  - 6.0s: scroll về đầu trang
  - 6.8s: bắt đầu slow scroll 40s
  - File đã sửa: vu-nhung-wedding-cr.html (CSS: dòng 214-218, dòng 224; JS: dòng 1116-1122)

---

## Session 8: 2026-06-16

### Tasks Completed
- [x] Flaps chuyển từ card container → fullscreen overlay (50vw × 100vh mỗi bên)
  - Bỏ `.cf-card` wrapper, flaps là `position: fixed`
  - Overlay chỉ giữ `pointer-events`, không còn background/opacity
  - Lock chuyển từ `position: absolute` → `fixed`, ở chính giữa màn hình
- [x] Animation flap chậm hơn: 2.5s → 4s transition
- [x] Auto-play nhạc khi mở thiệp với volume 50%
  - `audio.volume = 0.5; audio.play()`
- [x] Timing chain mới:
  - 3.0s: add `.closed` + play nhạc
  - 3.0-7.0s: flaps trượt sang 2 bên (4s)
  - 6.5s: scroll về đầu trang
  - 7.3s: bắt đầu slow scroll 40s
  - File đã sửa: vu-nhung-wedding-cr.html (CSS, HTML, JS), WORK_LOG.md

---

## Session 9: 2026-06-16

### Tasks Completed
- [x] Slow scroll bị giật — đã fix
  - Nguyên nhân: ease function (quadratic) + `window.scrollTo` mỗi frame gây tốc độ không đều
  - Fix: dùng linear scroll với `window.scrollBy(0, delta)` — mỗi frame scroll 1 delta nhỏ, tốc độ hằng số
  - Nav bar không còn respond khi auto-scroll: thêm `if (autoScrollId) return;` trong scroll listener
  - File đã sửa: vu-nhung-wedding-cr.html (dòng 1075, dòng 1119-1134)

---

## Session 10: 2026-06-16

### Tasks Completed
- [x] Slow scroll giật — fix triệt để
  - Root cause: `html { scroll-behavior: smooth; }` (dòng 156) — khiến `scrollTo`/`scrollBy` animate smooth mỗi lần gọi, xung đột với rAF 60fps
  - Fix: xóa `scroll-behavior: smooth` (nav bar đã dùng `scrollIntoView({ behavior: 'smooth' })` nên không bị ảnh hưởng)
  - Đổi từ `scrollBy` → `scrollTo` tuyệt đối (không drift, không sai số)
  - Duration 40s → 50s
  - File đã sửa: vu-nhung-wedding-cr.html (dòng 156, dòng 1119-1133), WORK_LOG.md

---

## Session 11: 2026-06-16

### Tasks Completed
- [x] Slow scroll duration 50s → 60s (theo yêu cầu chậm hơn)
  - File đã sửa: vu-nhung-wedding-cr.html (dòng 1122)

---

## Session 12: 2026-06-16

### Tasks Completed
- [x] Remove auto-open timer → click to open + choice dialog
  - Khi click vào thiệp (overlay), hiển thị dialog 2 lựa chọn: "Chú Rể" / "Cô Dâu"
  - Sau khi chọn: flaps mở, cập nhật địa điểm venue, play nhạc, scroll 60s
- [x] Choice dialog UI (theme wedding)
  - Backdrop mờ 55% đen, box cream 360px, 2 button bo góc (groom viền đỏ, bride viền vàng)
- [x] Venue data — 2 địa chỉ placeholder cho groom & bride
  - Thêm `id="venue-address"` + `id="gmaps-link"` cho JS update
- [x] Pre-select RSVP `guest_of` dựa theo lựa chọn
- [x] Xóa gift/QR section (Mừng cưới sớm) và CSS/inline script liên quan
- [x] Xóa `html { scroll-behavior: smooth }` (đã xóa từ trước, fix scroll giật)
- [x] Slow scroll duration: 60s
  - File đã sửa: vu-nhung-wedding-cr.html (CSS, HTML, JS), WORK_LOG.md

---

## Session 13: 2026-06-16

### Tasks Completed
- [x] Popup choice dialog background → ảnh cưới (img-content-3-1.webp)
  - Thay `background: rgba(0,0,0,0.55)` → gradient overlay #2c1810 + ảnh nền
- [x] Music icon tự động quay khi auto-play qua choice
  - Root cause: choice handler gọi `audio.play()` nhưng không update CSS class trên button
  - Fix: thêm `classList.remove('music-paused')` + `.add('music-playing')` cho cả btn và tooltip
  - File đã sửa: vu-nhung-wedding-cr.html (CSS dòng 253-256, JS dòng 1123-1132), WORK_LOG.md

---

## Session 14: 2026-06-16

### Tasks Completed
- [x] Revert background rgba(0,0,0,0.55) + redesign popup
  - 2 button ngang (flex row, 46% width each)
  - Avatar ảnh tròn 80px: groom (img-content-4-1) + bride (img-content-4-3)
  - Box rộng hơn: max-width 360→400px, border-radius 20→24px
- [x] Auto-show popup sau 1s khi load trang (thay vì chờ click)
  - Bỏ click handler overlay → setTimeout 1000ms
- [x] Music icon tự động quay khi auto-play (fix từ session 13)
  - File đã sửa: vu-nhung-wedding-cr.html (CSS, HTML, JS), WORK_LOG.md

---

## Session 15: 2026-06-16

### Tasks Completed
- [x] Fix auto-scroll không cancel được trên mobile & click desktop
  - Root cause: race condition giữa `cancelAnimationFrame` và `requestAnimationFrame` — scroll function set `autoScrollId` mới sau khi event listener cancel
  - Fix: thêm flag `scrolling`, kiểm tra `if (!scrolling) return;` ở đầu mỗi frame + `if (progress < 1 && scrolling)` trước khi set frame mới
  - Bỏ `{ once: true }` khỏi event listeners để listeners tồn tại vĩnh viễn (không mất sau 1 lần kích hoạt)
  - File đã sửa: `vu-nhung-wedding-cr.html` (dòng 1097, 1147-1171), WORK_LOG.md

---

## Session 16: 2026-06-18

### Tasks Completed
- [x] Thêm timeline động theo URL parameter (?slot=evening / ?slot=morning)
  - Tạo object `slotData` chứa dữ liệu 2 buổi (tối 28/11 + sáng 29/11)
  - Đọc `?slot=` từ URL, fallback về `morning` nếu không có hoặc sai
  - Thêm `id` vào 8 phần tử trong timeline HTML để cập nhật động
  - Hàm `updateTimeline(slot)` cập nhật ngày tháng, âm lịch, 3 mốc thời gian
  - Countdown timer tự động đổi mốc đếm ngược theo slot
  - Màn hình mở đầu (chọn chú rể/cô dâu) giữ nguyên, không ảnh hưởng
  - File đã sửa: `index.html` (timeline HTML thêm 8 id, JS thêm ~45 dòng), WORK_LOG.md

## Session 17: 2026-06-19

### Tasks Completed
- [x] Tải 4 SVG icon timeline từ UXWing (free, không cần attribution)
  - `icons/icon-guests.svg` — khách đến
  - `icons/icon-ceremony.svg` — lễ vu quy
  - `icons/icon-rings.svg` — lễ thành hôn / nhẫn cưới
  - `icons/icon-party.svg` — tiệc cưới / ly sâm banh
- [x] Thay thế timeline 3 cột ngang bằng timeline dọc ảnh nền (kiểu miuwedding)
  - Background ảnh động theo lựa chọn nhà trai/gái
  - Overlay gradient tối để text dễ đọc
  - Mỗi mốc: icon tròn 64px → thời gian + label trắng
  - Đường kẻ dọc nối các mốc (bên trái icon)
  - Địa điểm + nút "Chỉ đường" ở cuối
- [x] Tích hợp timeline với lựa chọn Chú Rể/Cô Dâu
  - `timelineData` keyed by `groom`/`bride` thay vì `evening`/`morning`
  - `updateTimeline(guest)` render động HTML từ JS
  - Countdown timer tự động đổi theo selectedGuest
  - Xoá `slotData`, `venueData`, các ID cũ không cần thiết
- [x] Thêm CSS cho timeline dọc (~80 dòng)
  - File đã sửa: `index.html`, `WORK_LOG.md`

---

## Session 29: 2026-06-24

### Tasks Completed
- [x] Thêm path-based routing cho 4 tổ hợp khách (groom/bride × morning/evening)
  - Mở rộng `timelineData` từ `[guest]` → `[guest][group]` với 4 tổ hợp:
    - groom/morning: 09:00 CN 29/11 (default)
    - groom/evening: 17:00 T7 28/11
    - bride/morning: 09:00 CN 29/11
    - bride/evening: 17:00 T7 28/11
  - Revert Lễ Thành Hôn từ 14:30 → **13:30**
  - Thêm JS `parsePath()` đọc `window.location.pathname`
  - Nếu path đầy đủ (`/groom/evening`) → auto-select, bỏ qua dialog
  - Nếu không → choice dialog 2 bước: Chú Rể/Cô Dâu → Tối T7/Sáng CN
  - Thêm `404.html` (copy từ `index.html`) cho GitHub Pages routing
  - Thêm CSS cho group dialog (`.cf-group-btn`, `.cf-group-sub`)
  - Cập nhật `updateTimeline`, `updateEventSections`, `updateCountdown` dùng `data()`
  - File đã tạo: `404.html`
  - File đã sửa: `index.html`, `AGENTS.md`, `WORK_LOG.md`

### Tasks Completed (follow-up)
- [x] Fix null path khi chọn dialog từ trang chủ
  - Thêm kiểm tra null trong `openCard()`: `'/' + (selectedGuest || 'groom') + '/' + (selectedGroup || 'morning')`
  - Xử lý `index.html` trong `parsePath()`: nếu `parts[0] === 'index.html'` thì coi như root
  - Cập nhật `parsePath()`: kiểm tra `parts[0]` hợp lệ trước khi gán guest
  - File đã sửa: `index.html`, `404.html`, `WORK_LOG.md`

## Session 19: 2026-06-19

### Tasks Completed
- [x] Restructure page layout to match miuwedding.com reference
  - Added 3 event section containers (`#event-party`, `#event-ceremony`, `#event-marriage`) with card-style CSS (border-left, large time, venue, map button)
  - Moved Countdown + Timeline sections from after Gallery to after event sections, before Story
  - Updated `timelineData` JS: removed "Khách đến" events, added `sections` array per guest, updated countdown times to first event (groom 10:30, bride 17:30), added Lễ Thành Hôn to bride's timeline
  - Added `updateEventSections(guest)` function to dynamically render 2 section cards per guest
  - Called `updateEventSections('groom')` on init and `updateEventSections(selectedGuest)` in choice handler
  - Added "Events" nav-bar link pointing to `#event-party`
  - Fixed TDZ bug: moved `let selectedGuest = 'groom'` before `updateCountdown()` and `updateTimeline('groom')`
  - File đã sửa: `index.html`, `WORK_LOG.md`

## Session 20: 2026-06-19

### Tasks Completed
- [x] Cách điệu timeline title
  - Split title thành 3 phần: "WEDDING TIMELINE" (small uppercase label), decorative "✦ ✦ ✦", và divider line
  - Thêm CSS cho `.tl-decoration`, `.tl-divider`, `.tl-title` (chuyển thành label nhỏ)
  - File đã sửa: `index.html`
- [x] Đổi ảnh nền timeline thành ảnh tĩnh
  - `updateTimeline()` giờ dùng URL cố định thay vì `data.bgImage` (không còn phụ thuộc guest)
  - File đã sửa: `index.html`
- [x] Fix dresscode bị đè lên story
  - Nguyên nhân: gallery section dùng `absolute bottom-[-65%]` tràn xuống section dưới
  - Fix: thêm `pb-[25%]` vào gallery section
  - File đã sửa: `index.html`
- [x] Xác nhận thứ tự event sections: Tiệc cưới (party) luôn là card đầu tiên cho cả groom và bride — không cần sửa

---

## Session 21: 2026-06-23

### Tasks Completed
- [x] Rollback timeline UI từ dọc (vertical) → 3 cột ngang (horizontal)
  - Xoá toàn bộ CSS vertical timeline (`#timeline-section`, `.tl-overlay`, `.tl-inner`, `.tl-title`, `.tl-decoration`, `.tl-divider`, `.tl-subtitle`, `.tl-timeline`, `.tl-item`, `.tl-item-icon`, `.tl-item-content`, `.tl-item-time`, `.tl-item-label`, `.tl-venue`, `.tl-venue-address`, `.tl-map-btn`)
  - Giữ nguyên CSS event cards (`event-section`, `.event-card`, ...)
  - Thay thế HTML `#timeline` section bằng layout 3 cột ngang từ code cũ (VS Code Local History `tTUI.html`)
  - Thêm `id` cho các phần tử để JS update động: `tl-date-label`, `tl-lunar-label`, `tl-event1-time/label`, `tl-event2-time/label`, `tl-event3-time/label`, `venue-address`, `gmaps-link`
  - Đơn giản hoá `timelineData`: xoá `bgImage` và `icon` khỏi `events`
  - Viết lại `updateTimeline(guest)`: cập nhật text các element trong grid 3 cột thay vì render vertical items
  - File đã sửa: `index.html`, `WORK_LOG.md`

## Session 22: 2026-06-23

### Tasks Completed
- [x] Restore to clean pre-Session21 state + re-apply timeline rollback, Plan B header
  - Restore từ VS Code Local History `mocb.html` (trạng thái trước Session 21)
  - Xoá CSS vertical timeline + HTML cũ
  - Thay HTML `#timeline` bằng layout 3 cột ngang + Plan B header (font-allura text-5xl)
  - Viết lại `updateTimeline(guest)` — cập nhật element 3 cột trực tiếp
  - File đã sửa: `index.html`
- [x] Scale text sizes toàn trang (largest → smallest, tránh cascade)
  - `text-3xl` → `text-4xl`
  - `text-2xl` → `text-3xl`
  - `text-xl` → `text-2xl`
  - `text-lg` → `text-xl`
  - `text-[16px]` → `text-lg`
  - `text-sm` → `text-base`
  - `text-[18px]` → `text-2xl` (chỉ có 1 instance: "WEDDING INVITATION" heading)
  - Lý do scale từ lớn→nhỏ: mỗi step tạo value LỚN hơn, không bị step sau bắt
  - File đã sửa: `index.html`
- [x] Sửa ngày cưới trong lời mời (09/06 → 28/11) + âm lịch
  - File đã sửa: `index.html`

### Next Steps
- [ ] Kiểm tra trực quan layout + text sizes trên browser
- [ ] Verify guestbook, RSVP form, countdown timer hoạt động

## Session 25: 2026-06-23

### Tasks Completed
- [x] Scale text guestbook (subtitle 15px → text-lg, loading/empty text-base → text-xl)
  - File đã sửa: `index.html`
- [x] Thêm Load More button cho guestbook (10 message/lần)
  - Real-time listener cho page 1: `onSnapshot` `.limit(11)`, luôn cập nhật message mới nhất
  - Load More: `get()` với `startAfter(lastDoc)` `.limit(11)`, append vào `#guestbook-more`
  - Kỹ thuật `limit(PAGE_SIZE + 1)` để phát hiện còn message — nếu snapshot.size > 10, hiện nút "Xem thêm"
  - File đã sửa: `index.html`

### Next Steps
- [ ] Kiểm tra guestbook load more trên browser

## Session 26: 2026-06-23

### Tasks Completed
- [x] Chuyển story + story-2 + couple lên trước countdown
  - Cut 3 section (#story, #story-2, #couple) từ sau timeline (vị trí cũ)
  - Chèn giữa #event-marriage và #countdown
  - File đã sửa: `index.html`

### Thứ tự mới
event-party → event-ceremony → event-marriage → story → story-2 → couple → gallery → countdown → timeline → dresscode

### Tasks Completed (bổ sung)
- [x] Chuyển gallery lên sau couple
  - Cut #gallery từ sau timeline
  - Chèn giữa #couple và #countdown
  - File đã sửa: `index.html`

## Session 24: 2026-06-23

### Tasks Completed
- [x] Thêm ảnh "Cảm ơn" và Gmail vào cuối trang
  - Chèn section mới sau footer (trước `</div>` wrapper)
  - Ảnh: `img-content-8-2.webp` (giống ảnh cuối RSVP)
  - Gmail: nguyentrongvu121199@gmail.com (clickable mailto link)
  - File đã sửa: `index.html`

## Session 23: 2026-06-23

### Tasks Completed
- [x] Fix khoảng trống giữa event cards và WEDDING INVITATION
  - Countdown section dùng `mt-[60%]` tạo khoảng trống quá lớn
  - Fix: `mt-[60%]` → `mt-8`
  - File đã sửa: `index.html`
- [x] Fix dresscode bị gallery overwrite
  - Gallery section có absolute element `bottom-[-65%]` tràn xuống dresscode
  - `pb-[25%]` không đủ để chắn — chỉ tương đương ~25% width
  - Fix: `pb-[25%]` → `pb-[70%]`
  - File đã sửa: `index.html`

---

## Session 27: 2026-06-24

### Tasks Completed
- [x] Cập nhật AGENTS.md cho khớp với trạng thái hiện tại của project
  - Xoá mục "Firebase Setup" cũ (hướng dẫn tạo project + placeholder config) → thay bằng note ✅ đã có config thật
  - Sửa tên file từ `vu-nhung-wedding-cr.html` → `index.html`
  - Thay thế "Existing Sections" (12 section cũ, còn Gift) → bảng 16 section đúng thứ tự hiện tại
  - Thêm mô tả các tính năng mới: overlay 2 cánh, choice dialog, event cards động, timeline groom/bride, music player, nav-bar, slow scroll, guestbook real-time + pagination
  - Thêm "Timeline Data" section với dữ liệu groom/bride
  - Bỏ các mục lỗi thời: Key Dates, Images (hướng dẫn upload), Animations cũ
  - File đã sửa: `AGENTS.md`, `WORK_LOG.md`

---

## Session 28: 2026-06-24

### Tasks Completed
- [x] Cập nhật thông tin nhà trai & nhà gái theo yêu cầu
  - Groom: countdown `2026-11-29T00:00:00`, Lễ Thành Hôn 13:30 → **14:30**
  - Bride: đổi ngày 28/11 (Thứ Bảy) → **29/11 (Chủ Nhật)**, venue TP.HCM → **Ninh Bình**
  - Bride venue: timeline = "nhà riêng, thôn Trung Hiếu Thượng, Thanh Lâm, Ninh Bình"
  - Bride sections: thêm `venue` riêng cho mỗi card = "nhà văn hóa Trung Hiếu Thượng, Thanh Lâm, Ninh Bình"
  - Countdown cả groom & bride: `2026-11-29T00:00:00`
  - Dress code: 4 màu cũ → 5 màu mới (Hồng nhạt, Trắng, Be, Nâu đậm, Xanh nhạt)
  - File đã sửa: `index.html`, `WORK_LOG.md`

## Session 11: 2026-06-25

### Tasks Completed
- [x] Fixed bug: music & slow scroll không hoạt động khi chọn guest từ dialog
  - Root cause: `.cf-group-btn` có cả class `.cf-choice-btn`, khi click "Evening"/"Morning" thì handler `.cf-choice-btn` chạy trước, đọc `data-guest` → null, gán `selectedGuest = null`, gây ra TypeError ở `timelineData[null][selectedGroup]`
  - Fix: đổi selector `.cf-choice-btn` → `.cf-choice-btn[data-guest]` để group buttons không match handler này
  - File đã sửa: `index.html` (dòng 1479), `404.html` (dòng 1479)

## Session 12: 2026-06-25

### Tasks Completed
- [x] Thay thế timeline icons từ Firebase Storage ảnh sang local SVG icons
  - Cột 1 (Tiệc cưới): `img-content-7-1.webp` → `icons/icon-party.svg`
  - Cột 2 (Lễ Vu Quy): `img-content-7-2.webp` → `icons/icon-ceremony.svg`
  - Cột 3 (Lễ Thành Hôn): `img-content-7-3.webp` → `icons/icon-rings.svg`
  - Thêm wrapper `div` nền tròn `bg-wedding-red` (burgundy) để SVG hiển thị rõ
  - File đã sửa: `index.html` (dòng 742-745, 756-760, 768-775), `404.html` (dòng 748-751, 762-766, 778-782)

## Session 13: 2026-06-25

### Tasks Completed
- [x] Fix SVG icons không hiển thị do relative path sai sau `history.replaceState`
  - Nguyên nhân: `src="icons/..."` → resolve thành `/groom/icons/...` → 404
  - Fix: đổi thành absolute path `src="/icons/..."`
  - File đã sửa: `index.html` (dòng 747, 762, 777), `404.html` (dòng 753, 768, 783)
- [x] Fix nhạc không play trên localhost do CORS Firebase Storage
  - Nguyên nhân: `<audio>` dùng Firebase Storage URL, browser chặn CORS trên localhost
  - Fix: chuyển `<audio src="...">` sang `<audio>` với 2 `<source>` — local `/music/wedding-song.mp3` là primary, Firebase Storage là fallback
  - File đã sửa: `index.html` (dòng 437-440), `404.html` (dòng 437-440)

## Session 14: 2026-06-25

### Tasks Completed
- [x] Bỏ nền burgundy background của icon timeline
  - Bỏ wrapper `<div>` nền tròn, SVG hiển thị trực tiếp với `w-16 h-16`
  - File đã sửa: `index.html` (dòng 745-746, 758-759, 771-772), `404.html` (dòng 751-752, 764-765, 777-778)
- [x] Fix nhạc không autoplay khi chọn guest từ dialog / auto-select URL
  - Thêm `audio.load()` trước `audio.play()`
  - Nếu play bị chặn (autoplay policy), retry trên event `click`/`touchstart` đầu tiên với `{ once: true }`
  - File đã sửa: `index.html` (dòng 1447-1471), `404.html` (dòng 1453-1477)

## Session 15: 2026-06-25

### Tasks Completed
- [x] Fix autoplay: play-pause ngay khi click dialog để capture user gesture, sau 4s play lại
  - Thêm `audio.play()` → `audio.pause()` ngay trong `openCard()` (khi user gesture còn hiệu lực)
  - Sau 4s: `audio.play()` lại (sticky activation từ browser cho phép)
  - Fallback: nếu vẫn bị chặn (URL auto-select path), retry trên click/touch với `{ once: true }`
  - File đã sửa: `index.html` (dòng 1446-1483), `404.html` (dòng 1452-1489)
- [x] Fix timing: nhạc + scroll chạy sau khi cửa mở hoàn toàn (4s)
- [x] Fix content giật: `new WOW().init()` chồng chéo
  - Move `new WOW().init()` ra ngoài `d.sections.forEach()` → gọi 1 lần duy nhất
  - File đã sửa: `index.html` (dòng 1334-1335), `404.html` (dòng 1340-1341)

## Session 16: 2026-06-25

### Tasks Completed
- [x] Bỏ WOW animation cho header image (#home)
  - Xóa class `wow animate__fadeInDownSlow` khỏi `<header id="home">`
  - File đã sửa: `index.html` (dòng 493), `404.html` (dòng 493)
- [x] Fix autoplay triệt để: unlock audio trên lần tương tác đầu tiên của user
  - Thêm `unlockAudio()` listener trên `click`/`touchstart`/`wheel` ngay khi script load
  - User click/scroll bất kỳ đâu → play-pause silent (+ volume 0.01) → unlock permission
  - File đã sửa: `index.html` (dòng 1416-1432), `404.html` (dòng 1416-1432)

## Session 17: 2026-06-25

### Tasks Completed
- [x] Fix autoplay: xóa `unlockAudio`, thêm `wheel` vào fallback listener
  - Nguyên nhân: `unlockAudio` luôn pause audio → nhạc tắt ngay sau khi mở. Fallback chỉ listen click/touch → scroll không play được
  - Fix: xóa `unlockAudio` hoàn toàn, thêm `wheel` event vào `tryPlay` trong `.catch()` của setTimeout(4000)
  - File đã sửa: `index.html`, `404.html`

## Session 18: 2026-06-25

### Tasks Completed
- [x] Thay thế timeline icons bằng hand-drawn SVG từ Temploola 24 Free Wedding Icons
  - CHAMPAGNE.svg → icon-party.svg (Tiệc cưới)
  - BRIDE AND GROOM.svg → icon-ceremony.svg (Lễ Vu Quy)
  - RINGS.svg → icon-rings.svg (Lễ Thành Hôn)
  - Tất cả icon đều style hand-drawn, fill #684C4B, 512x512px
  - Files đã sửa: `icons/icon-party.svg`, `icons/icon-ceremony.svg`, `icons/icon-rings.svg`

## Session 19: 2026-06-25

### Tasks Completed
- [x] Thêm hiệu ứng pháo hoa canvas-confetti khi xác nhận RSVP thành công
  - Thêm script canvas-confetti CDN vào `<head>` (sau animate.css)
  - Khi khách chọn "Có tham dự": bắn confetti 3 giây từ 2 góc (trái + phải)
  - Màu sắc theo theme wedding: đỏ burgundy (#7f0505), gold (#b0852b), kem (#f4f2ea)
  - Không bắn nếu khách chọn "Không thể tham dự"
  - Files đã sửa: `index.html` (dòng 16, 1029-1052), `404.html` (dòng 16, 1029-1052)

## Session 20: 2026-06-25

### Tasks Completed
- [x] Redesign header + font system overhaul
  - **Google Fonts**: Giảm từ 11 → 5 fonts (Cormorant Garamond, Lora, Great Vibes, Italianno, Tangerine)
  - **CSS classes**: Xoá 7 classes cũ (`.font-dancing`, `.font-crimson`, `.font-libre`, `.font-sacramento`, `.font-alex`, `.font-allura`, `.font-pinyon`), thêm `.font-cormorant-lining` + `.font-tangerine`; đổi `.font-playfair` → `.font-lora`
  - **Inline `'Crimson Text'`** (22 instances): thay bằng `'Cormorant Garamond', serif; font-variant-numeric: lining-nums` — áp dụng cho event cards, guestbook, dress code
  - **Header hero**: thêm `bg-black/40` overlay, fix typo "WEDDING INVATION" → "Wedding Invitation", thêm SVG branch wreaths + couple names (Great Vibes) + date (Cormorant-lining)
  - **#invitation section**: xoá couple name "Trọng Vũ & Hồng Nhung" (Italianno) — đã chuyển lên header
  - **Language**: English cho "Wedding Invitation" header label (theo design plan)
  - Files đã sửa: `index.html`, `404.html`, `WORK_LOG.md`

### Notes
- `.font-cormorant-lining`: dùng Cormorant Garamond với `font-variant-numeric: lining-nums` cho dữ liệu số (countdown, timeline, địa chỉ, guestbook entry)
- Lora font vẫn được load (4th font) nhưng không còn dùng trong element nào — `.font-lora` class tồn tại như fallback cho body text
- Tangerine font được load nhưng class `.font-tangerine` hiện không dùng — có thể dùng sau cho script accent nếu cần
