# Bài Tập: Mổ sản phẩm AI thật (App MoMo - Moni)

### CHỌN APP: MOMO - Moni (Trợ thủ tài chính, chatbot trong app)

#### 1. Dùng thử (Mô phỏng trải nghiệm)
*   **Promise (Lời hứa sản phẩm)**: Trợ lý thông minh giúp theo dõi, phân tích chi tiêu cá nhân nhanh chóng, thấu hiểu qua ngôn ngữ tự nhiên.
*   **Query thực tế**: *"Tháng này tôi tốn bao nhiêu tiền cho Shopee và Grab?"*
*   **Điểm gãy (Kỳ vọng vs Thực tế)**:
    *   *Kỳ vọng*: Bot bóc tách được 2 thực thể "Shopee" (thuộc nhóm Mua sắm) và "Grab" (thuộc nhóm Di chuyển/Ăn uống), sau đó tra cứu lịch sử giao dịch và cộng tổng số tiền tương ứng.
    *   *Thực tế (Low-confidence/Failure)*: Bot bị lúng túng khi câu hỏi chứa nhiều hơn 1 thực thể (multi-intent) hoặc từ khóa thương hiệu không khớp chính xác với tên danh mục chi tiêu tĩnh của MoMo. Bot rơi vào fallback và trả lời: *"Xin lỗi, Moni chưa hiểu yêu cầu của bạn. Bạn có thể thử hỏi 'Tổng chi tiêu tháng này' nhé."*
    *   *User kẹt ở đâu*: User không biết phải gõ lại như thế nào để bot hiểu, dẫn đến bực bội và thoát luồng (drop-off) để tự mở lịch sử giao dịch dò bằng tay.

---

#### 2 & 3. Vẽ Flow (As-is) và Đề xuất sửa path yếu nhất (To-be)

Vấn đề lớn nhất ở đây là **Failure Path (đường dẫn thất bại)** khi Bot không tự tin với truy vấn. Thay vì chặn cụt đường của user, ta cần thêm cơ chế **Correction (Sửa sai)** bằng cách hỏi lại (Clarification) và cung cấp nút bấm (Button).

**Bản Sketch Flow (As-is & To-be):**

```mermaid
graph TD
    subgraph AS-IS: LUỒNG HIỆN TẠI (YẾU)
        A1[User: "Tháng này tiêu bao nhiêu cho Shopee và Grab?"] --> B1(Bot phân tích NLP)
        B1 --> C1{Tìm thấy danh mục khớp?}
        C1 -->|Không/Độ tự tin thấp| D1[Bot: "Xin lỗi, Moni chưa hiểu..."]
        D1 --> E1((User bế tắc, thoát bot))
    end

    subgraph TO-BE: ĐỀ XUẤT SỬA LỖI (MƯỢT HƠN)
        A2[User: "Tháng này tiêu bao nhiêu cho Shopee và Grab?"] --> B2(Bot phân tích NLP)
        B2 --> C2{Phân loại Keyword}
        C2 -->|Keyword không chuẩn nhưng map được với danh mục| D2[Xử lý: Semantic Search & Gợi ý]
        D2 --> E2["Bot: Moni thấy bạn hay dùng Shopee cho 'Mua sắm' và Grab cho 'Đi lại'. Ý bạn là xem chi tiêu 2 nhóm này?"]
        E2 --> F2[Nút: 'Đúng vậy, xem luôn']
        E2 --> G2[Nút: 'Không, tìm chính xác chữ Shopee/Grab']
        
        F2 --> H2[Hiển thị biểu đồ & Tổng tiền 2 danh mục]
        G2 --> I2[Dùng Search Text lọc đúng mô tả giao dịch & Cộng tiền]
    end
```

---

#### 4. Output cuối cùng (Product Decision)

> **Product Decision:** 
> "Khi người dùng hỏi truy vấn chứa các từ khóa thương hiệu riêng (ví dụ: Shopee, Grab) không nằm trong danh mục tĩnh của app, thay vì ném ra câu trả lời lỗi *"Không hiểu"*, bot sẽ chủ động nội suy từ khóa đó về danh mục cha gần nhất, sau đó hiển thị câu hỏi xác nhận kèm theo các **Nút bấm (Quick Replies)** để định hướng người dùng. Điều này giúp cứu vãn các lượt hội thoại Low-confidence, biến lỗi (failure) thành một luồng giao tiếp gợi mở."
