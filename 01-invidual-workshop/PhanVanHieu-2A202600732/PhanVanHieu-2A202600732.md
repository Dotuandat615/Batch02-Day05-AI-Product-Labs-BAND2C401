# App Teardown Report — MoMo Moni (Trợ lý AI)

**Thời gian thực hiện:** 03/06/2026  
**Hình thức:** Cá nhân — dùng thử thực tế trên app MoMo  
**Sản phẩm được chọn:** MoMo — Moni (Trợ thủ tài chính & AI chatbot)

---

## 1. Sản phẩm được chọn

**MoMo — Moni** là trợ lý AI tích hợp trong ứng dụng ví điện tử MoMo, được quảng bá với khả năng:

- Quản lý chi tiêu, thu nhập, ngân sách cá nhân
- Tư vấn và đặt vé xem phim, du lịch, tìm nhà hàng
- Trả lời FAQ về ví MoMo
- Tư vấn gói data, cước internet di động
- Hỗ trợ mua MoMo SoundBox

**AI feature được kiểm tra:** Chatbot hỗ trợ đặt vé xem phim theo ngữ cảnh (intent-based conversation)

---

## 2. Promise vs Reality

### Product hứa gì?
Moni hứa hẹn là một trợ lý thông minh, hiểu ngữ cảnh, có thể dẫn dắt user từ intent ban đầu ("tôi muốn xem phim") đến kết quả cụ thể (suất chiếu + đặt vé) trong một cuộc hội thoại mượt mà.

### User nào được hứa sẽ được giúp?
User bận, muốn tiết kiệm thời gian, không muốn mở nhiều app — chỉ cần chat và được hỗ trợ đặt vé từ đầu đến cuối.

### Kỳ vọng task AI làm được:
- Hiểu "tôi muốn xem phim" → hỏi thêm chi tiết hợp lý
- Nhớ context đã cung cấp (vị trí Hà Nội, phim Doraemon)
- Chọn đúng rạp theo yêu cầu (CGV Vincom Bà Triệu)
- Dẫn đến màn hình đặt vé thực tế

### Điểm gãy quan sát được (từ các screenshot):

| Bước | Kỳ vọng | Thực tế |
|---|---|---|
| User nói "tôi muốn tìm vé xem phim" | Hỏi thêm: phim gì / ở đâu | Hỏi phim gì hoặc rạp nào — OK |
| User nói "tôi ở Hà Nội" | Nhớ ngữ cảnh, tiếp tục task đặt phim | **Reset context** — hỏi lại "Bạn muốn tìm quán ăn, xem phim, đặt vé du lịch...?" |
| User khẳng định lại "tôi đã nói muốn tìm vé" | Tiếp tục flow | Hỏi thêm "Bạn muốn xem phim nào / rạp nào?" — phải bắt đầu lại |
| User chọn CGV Vincom + Doraemon | Hiển thị suất chiếu tại CGV Vincom Bà Triệu | **Hiển thị suất chiếu tại Galaxy Mipec Long Biên** — sai rạp |
| User nói ngữ cảnh có giá rẻ + trung tâm HN | Lọc kết quả phù hợp | **Crash: "Trợ lí Moni đang gặp một số vấn đề, xin thử lại sau nhé"** |

**Evidence:**
- Screenshot 3: Moni bị reset context khi user cung cấp vị trí, phải lặp lại intent "tôi muốn đặt phim"
- Screenshot 4: Crash khi nhận query phức hợp ("xem phim hay nhưng giá rẻ quanh khu vực trung tâm Hà Nội")
- Screenshot 1 & 2: Kết quả trả về đúng task nhưng sai rạp (Galaxy Mipec Long Biên thay vì CGV Vincom Bà Triệu đã chọn)

---

## 3. Bốn Paths

### ✅ Happy Path — Khi AI đúng và tự tin
**Điều kiện:** User chọn đúng phim từ danh sách gợi ý, không có thêm điều kiện lọc phức tạp.

**Trải nghiệm user:** Moni hiển thị danh sách phim đang chiếu tại một rạp mặc định (CGV Vincom Bà Triệu), user chọn Doraemon, Moni hiển thị lịch chiếu với các slot giờ rõ ràng (12:15, 13:00, 13:30), có thể nhấn để xem sơ đồ ghế và đặt vé.

**Kết quả:** Flow thành công nếu user không cần đổi rạp và chấp nhận rạp mặc định.

---

### ⚠️ Low-Confidence Path — Khi AI không chắc
**Điều kiện:** User cung cấp thêm ngữ cảnh vị trí (Hà Nội) — thông tin không đủ cụ thể để chọn rạp.

**Hành vi thực tế:** Moni **không hỏi lại** để làm rõ (ví dụ: "Bạn muốn rạp gần khu vực nào ở Hà Nội?") mà thay vào đó **reset toàn bộ context**, quay về màn hình giới thiệu năng lực ("Bạn muốn xem phim, tìm quán ăn...?").

**Thiếu:** Không có hành vi hỏi lại có cấu trúc, không show options rạp gần user, không giữ lại intent đã nói.

**Verdict:** Low-confidence path **chưa có** — Moni không nhận ra mình đang ở trạng thái thiếu thông tin và xử lý sai.

---

### ❌ Failure Path — Khi AI sai
**Điều kiện 1 — Crash:** Query có nhiều điều kiện ("xem phim hay nhưng giá rẻ quanh khu vực trung tâm Hà Nội") → Moni trả về lỗi kỹ thuật, không giải thích, không gợi ý cách thử lại.

**Điều kiện 2 — Sai kết quả:** User đã chọn rạp CGV Vincom Bà Triệu, nhưng khi chọn phim Doraemon, Moni hiển thị lịch chiếu tại Galaxy Mipec Long Biên — hoàn toàn sai rạp.

**User biết bằng cách nào?** Chỉ khi đọc kỹ tên rạp trong card kết quả. Không có cảnh báo, không có xác nhận "tôi đang hiển thị rạp X vì..."

**User sửa thế nào?** Không có nút "đổi rạp" rõ ràng trong flow. Phải quay lại từ đầu.

---

### 🔄 Correction Path — Khi user sửa
**Điều kiện:** User phủ nhận kết quả (nhấn 👎 "Không hữu ích") → Moni hỏi lý do bằng free text.

**Hành vi:** Moni có cơ chế feedback cơ bản ("Rất tiếc... bạn có thể chia sẻ lý do?") nhưng không có correction loop thực sự — user không thể chỉnh sửa trực tiếp kết quả sai.

**Correction có được lưu/học lại không?** Không rõ — không có confirmation, không có indication hệ thống đã ghi nhận để cải thiện.

**Verdict:** Correction path **tồn tại nhưng nông** — chỉ thu thập feedback, không có correction in-session (user không thể nói "không, tôi muốn rạp khác" và có kết quả mới ngay lập tức).

---

## 4. Finding thành Quyết định Product

### Finding 1 — Context Reset

```
Khi user cung cấp thông tin bổ sung giữa chừng (vị trí, điều kiện lọc),
AI reset toàn bộ conversation context và quay về màn hình giới thiệu,
hậu quả là user phải lặp lại intent ban đầu ít nhất 2-3 lần, gây frustration
và tăng drop-off trong flow đặt vé.
Lỗi thuộc layer: Intent + UX Recovery.
Nên sửa bằng: Persistent intent state — hệ thống phải lưu task context (task = "đặt vé xem phim")
trong suốt cuộc hội thoại; thông tin bổ sung chỉ được xử lý như refinement,
không phải trigger reset. Test case: sau 3 lượt trao đổi, AI vẫn giữ đúng task ban đầu.
```

### Finding 2 — Wrong Cinema Mapping

```
Khi user chọn rạp (CGV Vincom Bà Triệu) rồi chọn phim (Doraemon),
AI trả về lịch chiếu tại rạp khác (Galaxy Mipec Long Biên) mà không thông báo,
hậu quả là user có thể đặt vé sai rạp mà không biết, dẫn đến trải nghiệm tệ ngoài app.
Lỗi thuộc layer: Data-Tool (sai mapping cinema-session) + UX Recovery (không xác nhận rạp).
Nên sửa bằng: (1) Fix data pipeline — session lookup phải filter đúng cinema_id đã chọn;
(2) UX confirmation — luôn hiển thị tên rạp nổi bật trước khi show lịch chiếu
với option "Đổi rạp khác".
```

### Finding 3 — Crash on Complex Query

```
Khi user nhập query có nhiều điều kiện đồng thời ("phim hay + giá rẻ + khu vực trung tâm HN"),
AI crash và trả về thông báo lỗi kỹ thuật chung chung ("đang gặp sự cố"),
hậu quả là user mất tin tưởng vào Moni và quay về cách đặt vé thủ công.
Lỗi thuộc layer: Promise (quảng bá "trợ lý thông minh" nhưng không xử lý được query thực tế).
Nên sửa bằng: Graceful degradation — khi không parse được query phức hợp,
AI phải tự break down: "Mình chưa lọc được theo giá ngay, nhưng mình có thể show
các rạp ở trung tâm HN trước — bạn chọn rạp rồi mình check giá nhé?"
```

### Finding 4 — No In-Session Correction

```
Khi user muốn chỉnh sửa kết quả sai (rạp sai, phim không đúng),
AI không có correction path in-session (không cho phép "đổi rạp" ngay trong chat),
hậu quả là user phải bắt đầu lại toàn bộ conversation — friction cao, mất thời gian.
Lỗi thuộc layer: UX Recovery.
Nên sửa bằng: Quick-fix affordances — mỗi kết quả cần kèm action chip
"Đổi rạp", "Xem phim khác", "Đổi giờ" để user có thể sửa tại chỗ
mà không cần restart conversation.
```

---

## 5. Sketch As-Is / To-Be

### AS-IS — Flow hiện tại (có đánh dấu điểm gãy)

```
User: "tôi muốn tìm vé xem phim"
    ↓
Moni: Hỏi phim gì hoặc rạp nào?
    ↓
User: "tôi ở Hà Nội"
    ↓
⚠️ [ĐIỂM GÃY 1 — Context Reset]
Moni: Reset → "Bạn muốn tìm quán ăn, xem phim hay...?"
    ↓
User: (frustrated) "tôi đã nói muốn đặt phim rồi"
    ↓
Moni: Hỏi lại rạp / phim
    ↓
User: "CGV Vincom Bà Triệu" + "Doraemon"
    ↓
⚠️ [ĐIỂM GÃY 2 — Sai rạp]
Moni: Hiển thị lịch chiếu tại Galaxy Mipec Long Biên (❌ sai rạp)
    ↓
Không có cơ chế sửa trong chat
    ↓
User phải thoát, bắt đầu lại ↩

(Nhánh khác)
User: "phim hay giá rẻ trung tâm HN"
    ↓
⚠️ [ĐIỂM GÃY 3 — Crash]
Moni: "Trợ lí đang gặp sự cố, thử lại sau" → Dead end ❌
```

---

### TO-BE — Flow đề xuất (đã sửa các path)

```
User: "tôi muốn tìm vé xem phim"
    ↓
Moni: Ghi nhận intent [task = book_movie] — lưu persistent
Hỏi: "Bạn muốn xem phim gì, hoặc cho mình biết khu vực để gợi ý nhé!"
    ↓
User: "tôi ở Hà Nội"
    ↓
✅ [Refinement — không reset]
Moni: "OK, mình tìm rạp ở Hà Nội cho bạn nhé. Khu vực nào tiện nhất?"
→ Show 3-4 khu vực phổ biến (Hoàn Kiếm, Cầu Giấy, Long Biên...)
    ↓
User: "trung tâm"
    ↓
Moni: Show danh sách rạp vùng trung tâm HN + rating/giá
    ↓
User: "CGV Vincom Bà Triệu"
    ↓
✅ [Lưu cinema_id = CGV_VincomBaTrieu]
Moni: Show phim đang chiếu tại CGV Vincom Bà Triệu (đúng rạp)
Confirm: "Rạp: CGV Vincom Bà Triệu | Hôm nay 03/06"
    ↓
User: "Doraemon"
    ↓
✅ [Query = Doraemon × cinema_id đã lưu]
Moni: Lịch chiếu Doraemon tại CGV Vincom Bà Triệu — đúng
+ Action chips: [Đổi rạp] [Xem phim khác] [Đổi ngày]
    ↓
User chọn suất chiếu → Đặt vé ✅

(Nhánh low-confidence)
User: "phim hay giá rẻ"
    ↓
✅ [Graceful degradation]
Moni: "Mình chưa lọc được theo giá trực tiếp, nhưng đây là các phim
đang chiếu. Bạn chọn phim rồi mình check giá vé giúp nhé!"
→ Không crash, không dead end

(Nhánh correction)
User nhấn 👎 hoặc nói "không, rạp sai rồi"
    ↓
✅ [In-session correction]
Moni: "Xin lỗi! Bạn muốn đổi sang rạp nào?"
→ [CGV Vincom] [Lotte] [BHD] [Nhập tay]
→ Tìm lại ngay trong cùng session, không restart
```

---

## 6. Tự kiểm

- [x] Có ít nhất 1 screenshot hoặc observation cụ thể — **4 screenshots từ session thực tế**
- [x] Có đủ 4 paths — Happy ✅ | Low-confidence ⚠️ (chưa có, đã mô tả) | Failure ❌ | Correction ⚠️ (có nhưng nông)
- [x] Finding được viết thành product decision, không chỉ là nhận xét
- [x] Sketch có as-is và to-be
- [x] Có câu nói rõ finding sẽ đổi gì trong SPEC:

> **SPEC impact:** Moni cần thêm persistent intent state management (task context không bị reset khi user cung cấp thêm thông tin), cinema_id binding (kết quả phim phải filter đúng cinema đã chọn), graceful degradation handler cho complex query, và in-session correction chips (Đổi rạp / Đổi phim / Đổi giờ) thay vì chỉ thu thập feedback thụ động.
