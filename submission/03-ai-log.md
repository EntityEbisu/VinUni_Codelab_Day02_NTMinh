# 03 — Nhật ký tương tác AI & Reflection

> Lab 02 — AI Product Scoping (Vin Smart Future)

## 1. AI được dùng như thế nào?

Trong quá trình làm bài, tôi sử dụng AI như một **thought-partner** để:

- brainstorm các pain point có thể xuất hiện trong vận hành Vingroup;
- stress-test Problem Card;
- so sánh Rule-based, LLM Feature và Agentic Loop;
- kiểm tra xem metric, boundary và fallback có hợp lý hay chưa;
- gợi ý các adversarial input để kiểm tra ranh giới của prototype.

Tôi không dùng nội dung AI sinh ra như bằng chứng về tình hình vận hành thực tế của Vinhomes.

## 2. AI đã giúp gì?

AI đặc biệt hữu ích khi phản biện assumption và ép problem statement phải cụ thể hơn.

Ví dụ, ban đầu có thể dễ rơi vào mô tả chung chung như:

> “Dùng AI để tự động hóa việc xử lý khiếu nại cư dân.”

Qua quá trình stress-test, bài toán được thu hẹp thành:

> **Phân loại intent + trích xuất thông tin + đề xuất queue + soạn draft, với HITL bắt buộc cho case nhạy cảm hoặc không chắc chắn.**

AI cũng giúp chỉ ra rằng Agentic Loop không cần thiết khi workflow đã tương đối cố định.

## 3. AI có thể sai hoặc gây hiểu nhầm ở đâu?

### Vấn đề 1 — Con số vận hành

AI có thể dễ dàng đề xuất các số liệu trông rất hợp lý như:

- 6–8 phút/ticket;
- 95% routing accuracy;
- 20–30% ticket cần re-route.

Những con số này **không có trong bộ tài liệu được cung cấp**. Vì vậy tôi không coi chúng là facts.

Tôi chuyển chúng thành **[ASSUMPTION]** hoặc **target metric**, đồng thời ghi rõ rằng phải thay bằng dữ liệu thực tế trước pilot.

### Vấn đề 2 — Đồng nhất “AI có thể làm” với “AI được phép làm”

AI có thể đề xuất tự động gửi phản hồi hoặc xử lý trực tiếp ticket, nhưng điều đó không phù hợp với operational boundary của bài toán.

Vì vậy phạm vi được giới hạn thành:

- classify;
- extract;
- recommend;
- draft.

Các quyết định có tác động đáng kể vẫn thuộc về con người.

### Vấn đề 3 — Bị cuốn theo worked example

Worked Example của lab rất hữu ích để hiểu tiêu chuẩn output, nhưng nếu sao chép gần nguyên ví dụ Xanh SM thì bài làm không còn thể hiện quá trình scoping độc lập.

Do đó tôi dùng một use case khác có trong Inspiration Kit: **Vinhomes — Phân loại & Điều hướng phản ánh cư dân**.

## 4. Tôi đã sửa gì sau khi review output của AI?

### Thay đổi A — Metric

Thay vì trình bày KPI như một sự thật đã được xác minh, tôi tách thành:

- **target metric:** thành công nên được đo bằng gì;
- **baseline assumption:** con số tạm dùng trong scope/prototype;
- **evidence requirement:** dữ liệu cần đo trước production/pilot.

### Thay đổi B — Kiến trúc

Tôi loại bỏ ý tưởng Agent tự trị vì workflow đã có cấu trúc tương đối rõ.

Kiến trúc cuối cùng là:

> **Rule / policy checks + LLM Feature + HITL + manual fallback**

### Thay đổi C — Operational Boundary

Tôi xác định rõ AI không được:

- tự phê duyệt khoản phí;
- đưa ra kết luận pháp lý;
- thay đổi dữ liệu cư dân;
- tự gửi thông điệp ra bên ngoài.

Case nhạy cảm hoặc confidence thấp phải quay về hàng đợi xử lý thủ công/HITL.

## 5. Reflection

Bài học lớn nhất là AI hữu ích khi đóng vai trò **mở rộng và stress-test tư duy**, nhưng không nên được xem là nguồn bằng chứng về cách một doanh nghiệp thực tế đang vận hành.

Giá trị lớn nhất của quá trình AI-assisted nằm ở việc phát hiện khoảng trống:

> **“Điều gì thực sự được hỗ trợ bởi tài liệu nguồn, và điều gì chỉ là giả định của chúng ta?”**

Đối với bài lab này, AI có giá trị nhất khi nó giúp tìm ra các điểm chưa đủ evidence và các boundary còn lỏng, thay vì chỉ tạo ra phần văn bản đẹp hơn.

## 6. Trạng thái prototype

File Python đi kèm được chuẩn bị để kiểm tra:

- system prompt nghiêm ngặt;
- structured JSON output;
- operational boundary;
- các adversarial input.

Việc chạy thử Gemini thực tế yêu cầu có `GEMINI_API_KEY`. Khi chưa có key, tôi **không ghi nhận một kết quả giả là kết quả chạy thật**.
