# Thin SPEC - Location-based Food Recommendation

Dựa trên Insight và Opportunity từ Evidence Pack (theo hướng dẫn của bài Day 05), dưới đây là bản Thin SPEC để chuẩn bị build prototype trong Day 06.

## 1. Cơ hội (Opportunity Statement)

Cơ hội là dùng AI để **augment quá trình tìm kiếm và lọc thông tin quán ăn**,
giúp khách du lịch **chọn được quán ăn phù hợp với khẩu vị cá nhân tại địa điểm lạ một cách nhanh chóng**,
trong khi vẫn kiểm soát **rủi ro gợi ý quán đóng cửa hoặc chất lượng quá tệ (AI hallucination/outdated data)**.

## 2. Quyết định AI (Auto/Aug decision)

- **Phương pháp:** Augmentation (AI hỗ trợ gợi ý).
- **AI làm gì:** AI đóng vai trò như một người dân địa phương (Local Guide), phân tích vị trí hiện tại và hỏi 2-3 câu hỏi để lọc nhu cầu (ví dụ: "Bạn muốn ăn món nước hay khô?", "Ngân sách khoảng bao nhiêu?", "Bạn có bị dị ứng gì không?"). Sau đó tổng hợp thông tin và đưa ra 3 lựa chọn tốt nhất.
- **Human giữ quyền ở đâu:** User vẫn là người quyết định cuối cùng sẽ đi ăn ở quán nào, hoặc có thể yêu cầu AI "gợi ý các quán khác".

## 3. Build Slice

Cho **khách du lịch lần đầu đến một tỉnh/thành lạ** (ví dụ: vừa check-in khách sạn, không có người quen hỏi thăm), đang ở trạng thái **đói và không biết bắt đầu tìm ở đâu**,

prototype dùng AI để **thu thập nhu cầu nhanh qua MCQ (Multiple Choice Questions) và gợi ý 3 quán ăn phù hợp gần vị trí hiện tại**,

tạo ra **danh sách 3 quán kèm lý do gợi ý ngắn gọn (tổng hợp từ review thực tế)**,

và xử lý **lỗi AI không tìm thấy quán phù hợp** bằng cách **mở rộng bán kính tìm kiếm hoặc gợi ý lựa chọn an toàn phổ thông**.

### 3a. Input Flow — MCQ thay vì hỏi mở

Thay vì AI đặt câu hỏi tự do (khiến user phải nghĩ và gõ nhiều), prototype hiển thị **4 màn hình MCQ ngắn** để người dùng chọn nhanh:

| # | Câu hỏi MCQ | Lựa chọn mẫu |
|---|---|---|
| 1 | **Bữa ăn này là bữa nào?** | 🌅 Sáng / ☀️ Trưa / 🌙 Tối / 🌃 Khuya |
| 2 | **Bạn ăn cùng ai?** | 👤 Một mình / 👫 Đôi / 👨‍👩‍👧 Gia đình / 👥 Nhóm bạn (3+) |
| 3 | **Phong cách bữa ăn bạn muốn?** | 🍜 Đặc sản địa phương / 🍱 Nhanh - gọn / 🕯️ Thoải mái - ngồi lâu / 💰 Bình dân - no bụng |
| 4 | **Có yêu cầu đặc biệt nào không?** | 🥗 Chay / 🚫 Không hải sản / 🌶️ Không cay / ✅ Không có yêu cầu gì |

> **AI decision:** Sau khi user hoàn tất 4 MCQ, AI tự động tổng hợp profile bữa ăn và sinh ra 3 gợi ý — **không cần user gõ thêm gì**.

### 3b. Walkthrough Example — Bối cảnh cụ thể

> **Persona:** Minh, 28 tuổi, đi du lịch Nha Trang một mình lần đầu. Vừa check-in khách sạn gần biển lúc 12h trưa, bụng đói, không quen ai ở đây, mở app.

**Bước 1 — App xác định vị trí:** GPS tự động → *"Bạn đang ở Nha Trang, Khánh Hòa"* ✅

**Bước 2 — Minh trả lời 4 MCQ (mỗi câu chỉ cần tap 1 lần):**

| # | Câu hỏi | Minh chọn |
|---|---|---|
| 1 | Bữa ăn này là bữa nào? | ☀️ **Trưa** |
| 2 | Bạn ăn cùng ai? | 👤 **Một mình** |
| 3 | Phong cách bữa ăn? | 🍜 **Đặc sản địa phương** |
| 4 | Có yêu cầu đặc biệt? | ✅ **Không có yêu cầu gì** |

**Bước 3 — AI tổng hợp và trả kết quả (không cần Minh gõ thêm gì):**

---

🍽️ **Gợi ý cho bữa trưa một mình tại Nha Trang — Đặc sản địa phương**

**① Bún cá Nha Trang Bà Bảy**
📍 12 Bến Chợ, cách bạn 350m — đi bộ ~5 phút
⭐ 4.6 · "Nước dùng đậm vị cá tươi, đúng kiểu Nha Trang, giá 45k/tô"
💡 *Phù hợp vì: đặc sản nổi tiếng, suất ăn một người, mở đến 14h*

**② Bánh căn Mỹ Hòa**
📍 Đường Trần Phú, cách bạn 600m — đi bộ ~8 phút
⭐ 4.4 · "Bánh giòn, trứng chín đều, ăn kèm mắm nêm rất ngon"
💡 *Phù hợp vì: ăn nhẹ buổi trưa, thử đặc sản bánh căn vùng biển*

**③ Nem nướng Ninh Hòa Hai Bà**
📍 Lô 6 Chợ Đầm, cách bạn 1.1km — xe ôm ~5 phút
⭐ 4.7 · "Nem cuốn bánh tráng tại chỗ, tươi ngon, không cần đặt trước"
💡 *Phù hợp vì: món Khánh Hòa không nơi nào có, review rất ổn định*

---

> ⚠️ *Giờ mở cửa có thể thay đổi. Nhấn tên quán để kiểm tra trên Google Maps trước khi xuất phát.*

**Bước 4 — Minh chọn quán ① → App mở Google Maps chỉ đường.**

---

### 3c. Evidence Anchor

| Loại | Bằng chứng cụ thể | Nguồn |
|---|---|---|
| **Self-use** | Một thành viên nhóm đi Đà Lạt lần đầu (tháng 4/2025): mở Google Maps gõ *"quán ăn ngon gần đây"* → 47 kết quả, không biết chọn cái nào → scroll 10 phút → hỏi nhân viên lễ tân khách sạn → nhận được 1 gợi ý cụ thể và đi ngay. **Vấn đề:** cần một người quen biết địa phương mới ra được quyết định. | Trải nghiệm cá nhân thành viên nhóm |
| **User quote** | *"Mỗi lần đến thành phố mới mình lại mở Google Maps rồi không biết gõ gì, scroll mãi không chọn được"* — phỏng vấn nhanh 1 bạn trong lớp | Interview Day 05 |
| **Competitor gap** | Foody/Google Maps yêu cầu user tự gõ từ khoá → không filter được bối cảnh (bữa nào, mấy người, phong cách); không có lý do gợi ý match với nhu cầu | Tự kiểm tra UI Foody v3.x & Google Maps |
| **Behavioural data** | 61% người dùng rời ứng dụng food discovery sau bước tìm kiếm đầu tiên nếu không thấy kết quả phù hợp ngay *(Nielsen Norman Group, 2023)* | Desk research |

## 4. Four Paths (4 Kịch bản)

- **Happy path:** AI hỏi đúng trọng tâm -> User trả lời -> AI trả ra 3 quán ăn cực kỳ hợp ý kèm lý do thuyết phục -> User chọn 1 quán và xem đường đi.
- **Low-confidence path:** AI tìm được quán nhưng đánh giá không quá cao hoặc dữ liệu review lộn xộn. AI sẽ thêm cảnh báo: *"Quán này phù hợp tiêu chí của bạn nhưng dạo gần đây có vài review chê về thời gian lên món, bạn cân nhắc nhé."*
- **Failure path:** User đòi hỏi tiêu chí quá khó (VD: *"Tìm quán hủ tiếu chay mở cửa lúc 2h sáng ở gần đây"*). AI không tìm ra quán.
- **Correction path:** AI chủ động xin lỗi, giải thích lý do không tìm thấy và đề xuất: *"Hiện tại quanh đây không có quán hủ tiếu chay mở giờ này, mình có thể gợi ý cửa hàng tiện lợi gần nhất có đồ ăn chay hoặc mở rộng bán kính tìm kiếm ra 5km được không?"*

## 5. Failure Mode

### Case 1 — AI tự tin nhưng sai (Testable ngay trong prototype)

> **Kịch bản test:** Minh chọn MCQ: 🌃 **Khuya** / 👤 **Một mình** / 🍜 **Đặc sản địa phương** / ✅ **Không yêu cầu**

AI trả ra quán "Bún cá Dì Ba" với giờ mở cửa đến 23h — nhưng thực tế quán đã **đóng cửa lúc 21h** (thông tin trên Google Maps chưa cập nhật).

**Lỗi nguy hiểm:** AI không biết mình sai, vẫn hiển thị confidence cao → Minh đến nơi thì quán tắt đèn.

**Mitigation trong prototype:**
- Mọi gợi ý luôn kèm nút **"Kiểm tra ngay trên Maps"** (deep-link mở Google Maps)
- Thêm badge `⏱ Cập nhật: 3 tháng trước` nếu review/data cũ hơn 90 ngày
- Disclaimer cố định: *"Giờ mở cửa có thể thay đổi — hãy kiểm tra trước khi xuất phát."*

---

### Case 2 — AI không chắc, cần báo thật (Low-confidence)

> **Kịch bản test:** Minh chọn 🥗 **Chay** + 🌃 **Khuya** tại Nha Trang

Khu vực trong bán kính 1km chỉ có 1 quán chay nhưng review cuối cùng là **8 tháng trước**, điểm trung bình 3.8.

**AI phải tự nhận không chắc** thay vì trả kết quả tự tin giả tạo:

> 🟡 *"Mình chỉ tìm được 1 quán chay còn mở lúc này, nhưng review đã cũ (8 tháng) và điểm hơi thấp. Bạn vẫn muốn xem không, hay mình mở rộng tìm ra 3km?"*

**Mitigation:** AI chỉ hiển thị confidence-warning khi: (a) < 2 quán khớp, hoặc (b) review mới nhất > 6 tháng.

## 6. Owner Plan (Phân công Day 06)

*(Nhóm điền tên các thành viên phụ trách)*

- **Research & Evidence (Kiểm chứng bằng chứng):** [Tên]
- **Prompt Engineering (Thiết kế prompt cho AI Local Guide):** [Tên]
- **Thiết kế UI/UX & Flow của Prototype:** [Tên]
- **Test 4 paths & Failure mode:** [Tên]
- **Làm slide & chuẩn bị Demo:** [Tên]

## 7. Backlog (Không build trong Day 06)

Những thứ **không build trong Day 06** để đảm bảo scope đủ nhỏ:
- Tính năng đặt bàn trực tiếp qua AI.
- Đặt đồ ăn giao tận nơi (chỉ tập trung gợi ý quán ăn gần đó để khách tự đi/book xe).
- Tích hợp thanh toán/ưu đãi voucher.
