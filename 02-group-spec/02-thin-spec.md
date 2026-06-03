# Thin SPEC - Location-based Food Recommendation

Dựa trên Insight và Opportunity từ Evidence Pack (theo hướng dẫn của bài Day 05), dưới đây là bản Thin SPEC để chuẩn bị build prototype trong Day 06.

## 1. Cơ hội (Opportunity Statement)

Cơ hội là dùng AI để **augment quá trình ra quyết định chọn quán ăn**, thay thế vai trò của người địa phương biết rành khu vực — trust signal cao nhất mà khách du lịch không có —
giúp họ **vượt qua "dining decision anxiety"**: chọn được quán phù hợp khẩu vị/ngân sách, tránh tourist trap, không tốn công đọc hàng chục review trái chiều bằng cách AI chủ động hỏi để hiểu intent thay vì chỉ lọc theo category,
trong khi vẫn kiểm soát **rủi ro thông tin lỗi thời (quán đóng cửa, đổi địa chỉ) khiến khách mất công di chuyển và có trải nghiệm tệ làm hỏng cả chuyến đi**.

## 2. Quyết định AI (Auto/Aug decision)

Prototype nên đi theo hướng **AI Augment**: AI đóng vai trò hỗ trợ người dùng tìm kiếm, lọc và so sánh các lựa chọn quán ăn, nhưng **không tự động quyết định thay người dùng**. Trong phạm vi Day 06, hệ thống không nên để AI tự đặt món, tự đặt bàn hoặc tự chọn quán cuối cùng, vì quyết định ăn uống phụ thuộc nhiều vào khẩu vị cá nhân, ngân sách, tình trạng sức khỏe và bối cảnh thực tế tại địa điểm du lịch.

- **Phương pháp:** AI Augmentation — AI hỗ trợ gợi ý, người dùng giữ quyền quyết định cuối cùng.
- **Mức độ tự động hóa:** AI chỉ dừng ở mức tư vấn và đề xuất. Các hành động có rủi ro như đặt món, đặt bàn, gọi quán, mở chỉ đường hoặc thanh toán đều cần người dùng tự xác nhận và thực hiện.
- **Lý do không chọn full automation:** Dữ liệu quán ăn có thể thay đổi theo thời gian thực, ví dụ quán đóng cửa, hết món, đổi giá, thay đổi giờ mở cửa hoặc chất lượng dịch vụ không ổn định. Ngoài ra, review/rating có thể bị nhiễu và không phản ánh đúng khẩu vị của từng khách du lịch. Nếu AI tự động quyết định, người dùng có thể mất thời gian di chuyển đến quán không phù hợp hoặc gặp rủi ro liên quan đến dị ứng, ăn kiêng và ngân sách.

| Rủi ro | Vì sao cần human-in-the-loop | Cách prototype xử lý |
|---|---|---|
| Quán đóng cửa hoặc đổi giờ mở cửa | Dữ liệu bản đồ/review có thể chưa cập nhật kịp thời | AI hiển thị cảnh báo và yêu cầu user kiểm tra lại trên bản đồ trước khi đi |
| Giá thay đổi | Giá món ăn có thể khác so với thông tin cũ | AI chỉ đưa mức giá tham khảo, không cam kết giá chính xác |
| Review không đáng tin | Rating có thể bị nhiễu, seeding hoặc không phù hợp với nhu cầu cá nhân | AI tóm tắt cả điểm mạnh và điểm cần cân nhắc từ review |
| Khẩu vị cá nhân khác nhau | Cùng một quán có thể hợp với người này nhưng không hợp với người khác | AI hỏi trước về món muốn ăn, khẩu vị, ngân sách và khoảng cách chấp nhận được |
| Dị ứng hoặc ăn kiêng | Sai sót có thể ảnh hưởng trực tiếp đến sức khỏe người dùng | AI luôn hỏi về dị ứng/ăn kiêng và nhắc user xác nhận lại với quán |

**Human giữ quyền ở đâu:** Người dùng giữ quyền kiểm soát ở các bước quan trọng trong flow: chọn tiêu chí ban đầu, xem danh sách gợi ý, so sánh 3 quán, chọn quán cuối cùng, bấm chỉ đường, gọi quán hoặc đặt món qua nền tảng bên ngoài. AI không được tự động thực hiện các hành động này nếu chưa có xác nhận rõ ràng từ người dùng.

**AI làm gì:** AI đóng vai trò như một **Local Guide Assistant**. AI hỏi 2-3 câu để hiểu nhu cầu, ví dụ người dùng muốn ăn món gì, ngân sách bao nhiêu, đi một mình hay đi nhóm, có dị ứng hay ăn kiêng không, muốn đi bộ hay chấp nhận di chuyển xa hơn. Sau đó AI lọc các lựa chọn phù hợp, xếp hạng mức độ phù hợp, đưa ra 3 quán đề xuất kèm lý do ngắn gọn, cảnh báo nếu dữ liệu chưa chắc chắn và gợi ý phương án thay thế khi không tìm thấy quán phù hợp.

**Ranh giới trách nhiệm của AI trong prototype:** AI chỉ nên đưa ra khuyến nghị có giải thích, không cam kết thông tin là tuyệt đối chính xác. Mỗi gợi ý cần có lý do rõ ràng như phù hợp ngân sách, gần vị trí hiện tại, món ăn đúng nhu cầu, review tốt hoặc phù hợp với khách du lịch. Nếu dữ liệu yếu hoặc không chắc chắn, AI phải nói rõ mức độ không chắc chắn thay vì đưa ra câu trả lời quá tự tin.

**Kết luận quyết định:** Với bài toán gợi ý quán ăn theo vị trí cho khách du lịch, lựa chọn hợp lý nhất là **AI Augment, không phải AI Automate**. Cách tiếp cận này giúp prototype vẫn tạo giá trị rõ ràng là giảm thời gian tìm kiếm và ra quyết định, đồng thời giữ người dùng trong vòng kiểm soát để hạn chế rủi ro từ dữ liệu sai, thông tin lỗi thời và khác biệt khẩu vị cá nhân.

## 3. Build Slice

Cho **khách du lịch đến một địa phương lạ** đang **không biết ăn gì và ở đâu**,
prototype dùng AI để **hỏi nhu cầu cá nhân và gợi ý 3 quán ăn phù hợp gần đó**,
tạo ra **danh sách 3 quán kèm theo lý do gợi ý ngắn gọn (tổng hợp từ review)**,
và xử lý **lỗi AI không tìm thấy quán phù hợp** bằng cách **mở rộng bán kính tìm kiếm hoặc gợi ý món ăn phổ thông an toàn**.

## 4. Four Paths (4 Kịch bản)

- **Happy path:** AI hỏi đúng trọng tâm -> User trả lời -> AI trả ra 3 quán ăn cực kỳ hợp ý kèm lý do thuyết phục -> User chọn 1 quán và xem đường đi.
- **Low-confidence path:** AI tìm được quán nhưng đánh giá không quá cao hoặc dữ liệu review lộn xộn. AI sẽ thêm cảnh báo: *"Quán này phù hợp tiêu chí của bạn nhưng dạo gần đây có vài review chê về thời gian lên món, bạn cân nhắc nhé."*
- **Failure path:** User đòi hỏi tiêu chí quá khó (VD: *"Tìm quán hủ tiếu chay mở cửa lúc 2h sáng ở gần đây"*). AI không tìm ra quán.
- **Correction path:** AI chủ động xin lỗi, giải thích lý do không tìm thấy và đề xuất: *"Hiện tại quanh đây không có quán hủ tiếu chay mở giờ này, mình có thể gợi ý cửa hàng tiện lợi gần nhất có đồ ăn chay hoặc mở rộng bán kính tìm kiếm ra 5km được không?"*

## 5. Failure Mode

- **Lỗi nguy hiểm nhất:** AI gợi ý một quán ăn đã đóng cửa vĩnh viễn hoặc chuyển địa điểm, khiến khách du lịch mất công đi tới, gây đói và có trải nghiệm rất tệ.
- **Cách prototype xử lý (Mitigation):** Giao diện gợi ý của AI luôn kèm theo disclaimer/cảnh báo: *"Giờ mở cửa có thể thay đổi, hãy nhấn vào link bản đồ để kiểm tra lại trước khi xuất phát nhé."*

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
