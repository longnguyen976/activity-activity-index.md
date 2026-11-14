# UI Conventions: MySen Platform

## 1. Language & Tone

* **Interface Language:** Vietnamese (Default and sole language for Release 1; English localization is deferred to Release 2).
* **User Address:** Direct, friendly, and youthful tone suitable for Gen Z park visitors using **"bạn" / "của bạn"** (e.g., *"Hồ sơ của bạn"*, *"Bạn đã mở khóa huy hiệu mới!"*).
* **Button Labels:** Verb-first action descriptions in Vietnamese Sentence case (e.g., *"Đặt vé ngay"*, *"Xác nhận thanh toán"*, *"Đổi ưu đãi"*, *"Lưu thay đổi"*, *"Chỉ đường"*, *"Đăng ảnh"*). Never use isolated generic words like *"OK"* or *"Gửi"*.

---

## 2. Standard Formats

| Item | Format | Example | Notes |
|---|---|---|---|
| Date | `DD/MM/YYYY` | 16/10/2026 | Standard Vietnamese date format |
| Time | `HH:mm` (24-hour) | 14:30 | 24-hour clock |
| Date and Time | `DD/MM/YYYY HH:mm` | 16/10/2026 14:30 | Timestamp format |
| Currency | `#,### VNĐ` | 120.000 VNĐ | Thousands separator is dot (`.`) |
| Coin / Points | `#,### xu` | 1.250 xu | Platform loyalty currency |
| Numbers | Thousands separator is dot (`.`) | 1.200 lượt khách | Standard Vietnamese numeric display |
| Phone Number | `0xxx xxx xxx` | 0912 345 678 | 10 digits starting with 0 |
| Personal Names | Full name field, Vietnamese diacritics supported | Nguyễn Hải Long | Displayed as typed |

---

## 3. Standard Component Rules

| Component | When to Use | Usage Rules & Notes |
|---|---|---|
| **Primary Button** | Main call-to-action per screen/panel | Maximum 1 per screen/tab; filled with primary brand color; disabled state clearly shaded (e.g., *"Đặt vé ngay"*, *"Xác nhận đổi quà"*). |
| **Secondary Button** | Alternative or supporting actions | Outlined or text style (e.g., *"Hủy"*, *"Chia sẻ thành tích"*, *"Thử lại"*). |
| **Text Input** | Data entry fields | Field label on top in Vietnamese, inline validation error text displayed below field. |
| **Modal / Dialog** | Confirm or cancel critical single actions | Used for critical confirmations (e.g., *"Đăng xuất"*, *"Xác nhận đổi ưu đãi"*); never used for main page navigation. |
| **Bottom Sheet** | Display contextual details or complex inputs | Slides up from bottom (e.g., Voucher Code display `M-F04-04`, Attraction Details `M-F03-01`, Create Post `M-F05-01`). |
| **Toast Notification** | Non-blocking brief feedback | Auto-dismisses after 3 seconds (e.g., *"Đã lưu hồ sơ."*, *"Đã gửi báo cáo."*). |
| **Banner** | Persistent situational information | Includes icon + Vietnamese text (e.g., Lock-out countdown, Offline alert, Streak ended alert). |
| **Segment Control / Tabs** | Switch sections within the same screen | Uses concise Vietnamese noun labels (e.g., *"Nhiệm vụ"*, *"Quét mã QR"*); active tab indicated by font weight and underline indicator, not color alone. |
| **Badge / Status Chip** | Display state or counter | Always includes Vietnamese text label + icon (e.g., *"Đang mở"*, *"Đã đóng"*, *"Khả dụng"*, *"Đã đổi"*, *"Đã đặt"*). |
| **Map Marker** | Point of interest on 2.5D map | Accessible Vietnamese text alternative provided for screen readers. |

---

## 4. Message & Feedback Patterns

| Situation | Design Pattern | Standard Example (Vietnamese UI Copy) |
|---|---|---|
| **Validation Error** | Explicit explanation + how to fix | *"Vui lòng nhập số điện thoại hoặc địa chỉ email hợp lệ."* |
| **System / Network Error** | Actionable cause + recovery button | *"Không thể tải hồ sơ của bạn. Vui lòng kiểm tra kết nối mạng và thử lại."* |
| **Empty State** | Clear reason + primary call-to-action | *"Thư viện ảnh hiện chưa có bài đăng nào. Hãy là người đầu tiên chia sẻ khoảnh khắc!"* |
| **Success Confirmation** | Brief positive confirmation | *"Đổi ưu đãi thành công! Bạn có thể xem mã trong mục Ưu đãi của tôi."* |
| **Permission Denied** | Explanation + fallback guidance | *"Cần quyền truy cập máy ảnh để quét mã QR. Bạn cũng có thể nhập mã thủ công."* |

---

## 5. Accessibility & Responsive Guidelines (WCAG 2.2)

* **Mobile Baseline:** Minimum target width 360px. Single-column responsive layout.
* **Touch Target Size:** Minimum 24x24 CSS px (recommended 48x48 dp on mobile touchscreens).
* **Color Contrast:** Minimum 4.5:1 contrast ratio for body text against background.
* **Color Independence:** Status and state information must never be conveyed by color alone (always pair color with icons, text badges, or labels).
* **Screen Reader Support:** All images/icons provide Vietnamese alt text; interactive components use proper ARIA labels and focus trap inside modals.