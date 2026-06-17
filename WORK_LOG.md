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