# AGENTS.md - Wedding Website Vũ & Nhung

## Project Overview

Single-page wedding invitation website with Firebase backend for guest management and image storage.

## Tech Stack

- **Frontend**: HTML, Tailwind CSS (CDN), WOW.js, Animate.css
- **Backend**: Firebase Firestore (guest data) + Firebase Storage (images)
- **No build tool required**: Plain HTML file

## Firebase Setup (Required before development)

### Step 1: Create Firebase Project
1. Go to https://console.firebase.google.com
2. Create new project: `vu-nhung-wedding`
3. Enable **Firestore Database**:
   - Create database → Start in **test mode** (allows read/write for 30 days)
4. Enable **Firebase Storage**:
   - Start in **test mode** (allows read/write for 30 days)

### Step 2: Get Firebase Config
1. Go to Project Settings (gear icon)
2. Scroll down to "Your apps" → Click web icon `</>`
3. Register app → Copy the `firebaseConfig` object

### Step 3: Update HTML
Replace placeholder config in `vu-nhung-wedding-cr.html`:
```javascript
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT.appspot.com",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```

## Data Structure

### Firestore Collection: `guests`
```json
{
  "name": "Nguyen Van A",
  "phone": "0912345678",
  "attending": true,
  "guestCount": 2,
  "message": "Chúc cô dâu chú rể hạnh phúc!",
  "guestOf": "bride",  // or "groom"
  "createdAt": timestamp
}
```

## Development

### Testing
Open `vu-nhung-wedding-cr.html` directly in browser.

### Edit Content
Find elements with `data-editable="true"` attribute and modify the text/content inside.

### Key Dates to Update
- Line ~175: Wedding date (Dương lịch)
- Line ~178: Wedding date (Âm lịch)
- Line ~343-346: Event date/time
- Line ~408: Venue address
- Line ~599: QR code image URL
- Line ~604: Bank account info
- **JS Config** (line ~620): Update `weddingDate` in countdown timer code

## Existing Sections (Keep as template)

1. **Header**: Hero image with title
2. **Invitation**: Main invitation text
3. **Family**: Both family info
4. **Story**: Couple photos with romantic text
5. **Couple**: Bride & groom introduction
6. **Gallery**: Photo collection
7. **Countdown**: Days/hours/minutes/seconds to wedding
8. **Timeline**: Event schedule (guest arrive → ceremony → reception)
9. **Dress Code**: Color suggestions
10. **RSVP Form**: Guest confirmation form (⚠️ needs Firebase integration)
11. **Gift**: QR code for money gift
12. **Footer**: Closing message

## Scroll Animations

Already configured with WOW.js:
- `wow animate__fadeIn`
- `wow animate__fadeInUp`
- `wow animate__slideInRight`
- `wow animate__zoomIn`
- `wow animate__bounceIn`

Animation triggers on scroll. No changes needed unless customizing.

## RSVP Form Integration

✅ **DONE** - Form đã được tích hợp Firebase:
- SDK: `firebase-app-compat.js` + `firebase-firestore-compat.js`
- Handler: Lưu data vào collection `guests`
- Validation: Kiểm tra required fields
- Success/Error handling

### Form Fields
- `name`: Họ tên khách mời (required)
- `decision`: Xác nhận tham dự (yes/no)
- `guest_of`: Khách của cô dâu hay chú rể
- `number_of_guests`: Số người đi cùng
- `message`: Lời nhắn (optional)

### Firestore Data Saved
```javascript
{
  name: string,
  attending: boolean,
  guestOf: "bride" | "groom",
  guestCount: number,
  message: string,
  createdAt: timestamp
}
```

## Images

✅ **DONE** - Firebase Storage SDK đã được thêm và cấu hình (`firebase-storage-compat.js` + `storage = firebase.storage()`)

### Hướng dẫn upload ảnh lên Firebase Storage

✅ **DONE** - Tất cả ảnh và nhạc đã được upload và cập nhật URL vào HTML.

### Cấu trúc Storage
```
/ (root)
├── images/
│   ├── header-background.webp
│   ├── img-content-1-1.webp
│   ├── ...
│   └── img-content-8-2.webp
├── icons/
│   ├── image-play.png
│   └── image-playing.png
└── music/
    └── wedding-song.mp3
```

## Guestbook (Sổ lưu bút)

✅ **DONE** - Hiển thị danh sách lời chúc từ Firestore:
- Query collection `guests` với `message != ''`
- Sắp xếp theo `createdAt` giảm dần (mới nhất trước)
- Giới hạn 50 lời chúc gần nhất
- Real-time cập nhật qua `onSnapshot`
- Hiệu ứng WOW.js cho từng item

## Animations

✅ **DONE** - Nâng cấp animations theo phong cách miuwedding:
- Custom slow variants: `animate__fadeInUpSlow`, `animate__fadeInLeftSlow`, `animate__fadeInRightSlow`, `animate__fadeInDownSlow` (1.5s)
- `rotateInDownLeft` / `rotateInDownRight` cho ảnh cô dâu chú rể
- Giảm padding sections (py-12→py-8, py-8→py-6) cho layout gần nhau hơn

## ✅ ĐÃ SỬA: Guestbook không cần composite index nữa

Guestbook trước đây báo lỗi "Không thể tải lời chúc" do query dùng `.where('message', '>', '')` kết hợp `.orderBy('createdAt', 'desc')` yêu cầu composite index.

**Fix:** Đã bỏ `.where('message', '>', '')` — query giờ chỉ dùng `.orderBy('createdAt', 'desc')` và lọc message rỗng bằng JavaScript. Chỉ cần single-field index `createdAt DESC` (Firestore tự tạo).

### Các bước test:
- [ ] Guestbook hiển thị lời chúc từ Firestore
- [ ] RSVP form submit — kiểm tra Firestore collection `guests`
- [ ] Music button hover animation + state sync
- [ ] Nav-bar ẩn/hiện sau 1.5s idle

## Troubleshooting

- **CORS issues**: If images don't load, check Firebase Storage CORS config
- **Firestore permissions**: Use test mode or configure proper rules
- **Form not submitting**: Check browser console for Firebase errors
- **Guestbook error "Không thể tải lời chúc"**: Kiểm tra Firebase config, Firestore rules, và console log để xác định lỗi. Không cần composite index nữa.

## Work Logging Rule (Bắt buộc - Automatically enforced)

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