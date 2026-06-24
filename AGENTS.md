# AGENTS.md - Wedding Website Vũ & Nhung

## Project Overview

Single-page wedding invitation website with Firebase backend for guest management and image storage.

## Tech Stack

- **Frontend**: HTML, Tailwind CSS (CDN), WOW.js, Animate.css
- **Backend**: Firebase Firestore (guest data) + Firebase Storage (images)
- **No build tool required**: Plain HTML file

## Firebase Config

✅ **Đã cấu hình Firebase project thật** (trong `index.html`):
- **Project ID**: `vu-nhung-wedding`
- **Firestore DB**: collection `guests`
- **Storage**: images + music files
- Sử dụng `firebase-app-compat.js` + `firebase-firestore-compat.js` + `firebase-storage-compat.js`

## Data Structure

### Firestore Collection: `guests`
```json
{
  "name": "Nguyen Van A",
  "attending": true,
  "guestCount": 2,
  "message": "Chúc cô dâu chú rể hạnh phúc!",
  "guestOf": "bride",
  "createdAt": timestamp
}
```

## Sections Order

| # | Section | ID | Notes |
|---|---------|----|-------|
| 1 | **Header** | `#home` | Hero image + "Chúng mình cưới!" |
| 2 | **Invitation** | `#invitation` | Lời mời + ảnh cặp đôi |
| 3 | **Family** | `#family` | Nhà Trai / Nhà Gái + họ tên bố mẹ |
| 4 | **Event Party** | `#event-party` | Card động: Tiệc cưới |
| 5 | **Event Ceremony** | `#event-ceremony` | Card động: Lễ Vu Quy |
| 6 | **Event Marriage** | `#event-marriage` | Card động: Lễ Thành Hôn |
| 7 | **Story** | `#story` | Ảnh + quote lãng mạn |
| 8 | **Story 2** | `#story-2` | Ảnh ghép + text |
| 9 | **Couple** | `#couple` | Groom & Bride intro |
| 10 | **Gallery** | `#gallery` | Ảnh cưới collage |
| 11 | **Countdown** | `#countdown` | Đếm ngược đến giờ cưới |
| 12 | **Timeline** | `#timeline` | 3 cột ngang: lịch trình |
| 13 | **Dress Code** | `#dresscode` | Gợi ý trang phục |
| 14 | **RSVP** | `#rsvp` | Form xác nhận tham dự |
| 15 | **Guestbook** | `#guestbook` | Sổ lưu bút real-time |
| 16 | **Footer** | `footer` | Ảnh cảm ơn + copyright |

## Features

### Invitation Card Overlay
- 2 cánh fullscreen (trái kem `#f4f2ea`, phải burgundy `#7f0505`)
- Chốt giữa hình tròn 囍
- Tự động hiện choice dialog sau 1s: chọn "Chú Rể" / "Cô Dâu"
- Sau khi chọn: flaps trượt 2 bên (4s), play nhạc, slow scroll 180s

### Dynamic Event Cards (`#event-party`, `#event-ceremony`, `#event-marriage`)
- Render động theo `selectedGuest` (groom/bride)
- Mỗi card: title band, giờ, ngày-tháng-năm, thứ, âm lịch, địa chỉ, nút Chỉ đường
- Groom: Tiệc cưới nhà trai (29/11 CN) + Lễ Thành Hôn (13:30)
- Bride: Tiệc cưới nhà gái (28/11 T7) + Lễ Vu Quy (11:30)

### Timeline
- 3 cột ngang, đường kẻ đỏ ở giữa
- Icon ảnh tròn 64px cho mỗi mốc
- Cập nhật động theo guest: giờ, label, venue, Google Maps link

### Countdown Timer
- Dùng `timelineData[selectedGuest].countdown` làm mốc
- Tự động cập nhật mỗi giây

### Music Player
- Nút tròn 48px, fixed góc trên phải
- Icon quay khi phát nhạc, hover tooltip
- Auto-play với volume 50% sau khi chọn guest
- File: `music/wedding-song.mp3` (upload Firebase Storage)

### Navigation Bar
- 6 items fixed bottom: Home, Events, Couple, Gallery, Timeline, Confirm
- SVG icons, frosted glass background
- Auto-hide sau 1.5s idle scroll
- Smooth scroll khi click

### Slow Scroll
- Tự động scroll từ đầu đến cuối trang trong 180s
- Linear speed, dùng `requestAnimationFrame`
- Cancel khi user scroll/wheel/touch/keypress

### RSVP Form
- **Firebase integration**: Lưu vào collection `guests`
- Fields: name, decision (yes/no), guest_of (bride/groom), number_of_guests, message
- Pre-select `guest_of` theo lựa chọn từ overlay
- Validation + success/error message

### Guestbook (Sổ lưu bút)
- Real-time listener: `onSnapshot` với `orderBy('createdAt', 'desc')` limit 11
- Lọc message rỗng bằng JavaScript
- Load More: `startAfter(lastDoc)` + limit 11, append vào container
- PAGE_SIZE = 10, dùng limit(PAGE_SIZE+1) để phát hiện còn message
- WOW.js animation cho mỗi item

## Timeline Data

Keyed by `selectedGuest` (groom/bride). Defined in JS object `timelineData`:
- `dateLabel`, `lunarDateLabel`
- `countdown`: ISO date string cho countdown timer
- `events[]`: `{ time, label }` — 3 cột timeline
- `venue`: `{ address, mapUrl }` — địa chỉ + Google Maps link
- `sections[]`: `{ id, label, time, day, month, year, weekday, lunar }` — event cards

### Groom (29/11/2026 - Chủ Nhật)
- Tiệc cưới nhà trai: 09:00
- Lễ Vu Quy: 11:30
- Lễ Thành Hôn: 13:30
- Venue: Nhà hàng Linh Trâm, 30 Đ. Kim Bài, Thanh Oai, Hà Nội

### Bride (29/11/2026 - Chủ Nhật)
- Tiệc cưới nhà gái: 09:00
- Lễ Vu Quy: 11:30
- Lễ Thành Hôn: 13:30
- Venue: nhà riêng, thôn Trung Hiếu Thượng, Thanh Lâm, Ninh Bình

## URL Path Routing (GitHub Pages)

Website hỗ trợ path-based routing cho 4 tổ hợp khách:

| Path | Guest | Group | Tiệc cưới | Lễ Vu Quy | Lễ Thành Hôn |
|------|-------|-------|-----------|-----------|--------------|
| `/groom/morning` (default) | Chú Rể | Sáng CN | 09:00 CN 29/11 | 11:30 CN 29/11 | 13:30 CN 29/11 |
| `/groom/evening` | Chú Rể | Tối T7 | 17:00 T7 28/11 | 11:30 CN 29/11 | 13:30 CN 29/11 |
| `/bride/morning` | Cô Dâu | Sáng CN | 09:00 CN 29/11 | 11:30 CN 29/11 | 13:30 CN 29/11 |
| `/bride/evening` | Cô Dâu | Tối T7 | 17:00 T7 28/11 | 11:30 CN 29/11 | 13:30 CN 29/11 |

Cơ chế: `404.html` copy từ `index.html`, JS đọc `window.location.pathname` và parse guest + group. Nếu path đầy đủ (`/groom/evening`) → auto-select, bỏ qua dialog. Nếu không → choice dialog 2 bước (Chú Rể/Cô Dâu → Tối T7/Sáng CN).

## Scroll Animations

Configured with WOW.js:
- `animate__fadeIn`, `animate__fadeInUp`, `animate__fadeInLeft`, `animate__fadeInRight`
- `animate__zoomIn`, `animate__rotateInDownLeft`, `animate__rotateInDownRight`
- Custom slow variants: `animate__fadeInUpSlow`, `animate__fadeInLeftSlow`, `animate__fadeInRightSlow`, `animate__fadeInDownSlow` (1.5s)
- `data-wow-delay` stagger cho nhiều element

## Assets

### Images (local in `vs-template-5/`, served via Firebase Storage URLs)
- `header-background.webp` — hero
- `img-content-1-1.webp` → `img-content-8-2.webp` — nội dung các section

### Icons
- `icons/icon-guests.svg`, `icons/icon-ceremony.svg`, `icons/icon-rings.svg`, `icons/icon-party.svg` — timeline icons

### Music
- `music/wedding-song.mp3` — nhạc nền

## Editing Content

Find elements with `data-editable="true"` attribute and modify the text/content inside. Key configurable data:
- Wedding dates (28-29/11/2026) — in `timelineData` JS object
- Family names (bố mẹ 2 bên)
- Venue addresses + Google Maps URLs
- Dress code colors
- All section text content

## 🚫 Guestbook — Không cần composite index

Query dùng `.orderBy('createdAt', 'desc')` + filter message rỗng bằng JavaScript.
Chỉ cần single-field index `createdAt DESC` (Firestore tự động tạo).

## Troubleshooting

- **CORS issues**: If images don't load, check Firebase Storage CORS config
- **Firestore permissions**: Test mode hoặc cấu hình rules phù hợp
- **Form not submitting**: Open browser console → check Firebase errors
- **Guestbook error**: Kiểm tra Firebase config, Firestore rules, console log
- **Slow scroll không cancel**: Check flag `scrolling` + `cancelAutoScroll()` listeners

## Work Logging Rule (Bắt buộc)

Sau mỗi lần thực hiện thay đổi code, **phải ghi chép ngay** vào `WORK_LOG.md`.

### Format
```markdown
## Session <N>: YYYY-MM-DD

### Tasks Completed
- [x] <Mô tả ngắn gọn task>
  - <Chi tiết thay đổi chính (files, dòng code, lý do)>
  - <Các file đã sửa: file A, file B>

### Next Steps (optional)
1. <Công việc còn lại nếu có>
```

### Quy tắc
1. **Ghi sau mỗi session làm việc**, trước khi kết thúc
2. **Mỗi lần ghi = 1 session mới** (tăng số thứ tự)
3. Liệt kê **tất cả file đã tạo/sửa/xóa**
4. Nếu có lý do kỹ thuật quan trọng (workaround, trade-off), ghi rõ
5. Không cần ghi lại nếu chỉ đọc/tham khảo code

## References

- Firebase Docs: https://firebase.google.com/docs/firestore
- Tailwind CDN: https://cdn.tailwindcss.com
- WOW.js: https://wowjs.uk/
- Animate.css: https://animate.style/
