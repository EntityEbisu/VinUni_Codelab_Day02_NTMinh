# 🏗️ Phase 3 — DEEP-DIVE (Nhóm, 85 min)

## 3.1. Current-State Workflow Mapping (25 min)
**Vẽ quy trình hiện tại lên bảng/giấy A3.** Sử dụng các ký hiệu:
* 🔴 **Bottleneck:** Bước gây tắc nghẽn, tốn thời gian, hoặc sai sót nhiều nhất.
* 🔄 **Handoff:** Điểm chuyển giao thông tin giữa người và hệ thống, hoặc giữa các bộ phận.
* Ghi rõ thời gian vận hành trung bình: **Tổng cộng = ____ phút/lượt**.
Quy trình: 

[Khách đặt xe] 
      │
      ▼
🔴 [Hệ thống gợi ý điểm đón theo bản đồ tĩnh]  ← Bottleneck chính
      │  (POI/địa chỉ không cập nhật, không tính traffic real-time)
      ▼
🔄 [Handoff: Hệ thống → Tài xế] 
      │  App điều hướng tài xế đến điểm đón gợi ý
      ▼
[Tài xế di chuyển đến điểm đón]
      │
      ▼
🔴 [Điểm đón không khớp thực tế]  ← Bottleneck phụ
      │  (cổng cấm dừng, sai tòa nhà, khách đứng chỗ khác)
      ▼
🔄 [Handoff: Tài xế → Khách]
      │  Gọi điện/nhắn tin xác nhận lại vị trí
      ▼
[Khách hàng và tài xế gặp nhau]

-> Tổng cộng = 6–8 phút/lượt (trung bình toàn hệ thống, so với baseline lý tưởng ~3 phút)


## 3.2. Problem Statement (6-field) & Metrics (15 min)
Điền đầy đủ 6 trường thông tin của bài toán:

Field |  Nội dung chi tiết
1. Actor / Operator	|   Tài xế Xanh SM (người thực hiện di chuyển đến điểm đón) và khách hàng đặt xe (người chờ/xác nhận vị trí)
2. Current Workflow |	Hệ thống gợi ý điểm đón dựa trên bản đồ tĩnh và tọa độ GPS đơn thuần từ app đặt xe, không đối chiếu với dữ liệu thực địa (điểm cấm dừng, lối vào tòa nhà) hay traffic real-time. Khi sai, tài xế và khách phải gọi điện thoại xác nhận thủ công
3. Bottleneck  |	Bước gợi ý điểm đón (do dữ liệu bản đồ/POI chưa chuẩn hóa) và bước xác nhận lại vị trí qua điện thoại — chiếm phần lớn thời gian phát sinh ngoài kế hoạch
4. Business Impact  |	Trung bình 3–5 phút chờ/tìm khách phát sinh mỗi lượt sai điểm đón; với quy mô lớn, ước tính hàng nghìn giờ xe "chết" mỗi tháng, kéo theo giảm số chuyến hoàn thành/ca và tăng tỷ lệ khách hủy chuyến
5. Success Metric   |	Giảm thời gian chờ đón khách trung bình từ 5 phút xuống dưới 2 phút; giảm tỷ lệ hủy chuyến do sai điểm đón tối thiểu 30%; ≥90% điểm đón gợi ý nằm trong bán kính 50m so với vị trí thực tế khách đứng
6. Operational Boundary  |	AI được phép: đề xuất điểm đón thay thế dựa trên lịch sử GPS + phản hồi tài xế. AI TUYỆT ĐỐI không được: tự động điều hướng tài xế vào khu vực cấm dừng/vi phạm luật giao thông mà không có xác nhận. Cần duyệt: khi model đề xuất điểm đón mới chưa từng xuất hiện trong dữ liệu lịch sử (điểm lạ) — phải qua kiểm duyệt vận hành trước khi đưa vào hệ thống chính thức


## 3.3. Future-State Flow & AI Fit (25 min)
* **Xác định mức AI Fit (AI-Fit Matrix):** Giải pháp thuộc nhóm nào? [ ] Rule / State-Machine [ ] LLM Feature [ ] Agentic Loop.
* **Vẽ Future-State Flow:** Đánh dấu rõ:
  * 🔵 **AI Step:** Tác vụ LLM xử lý.
  * 🟢 **Human Step (HITL):** Bước con người phê duyệt/review (Human-in-the-loop).
  * ↩️ **Fallback:** Kế hoạch dự phòng khi LLM trả về kết quả lỗi hoặc không tự tin.

---
                [Khách đặt xe]
                    │
                    ▼
                🔵 AI Step (Rule Engine): Chuẩn hóa địa chỉ + snap về điểm dừng hợp lệ
                gần nhất (loại trừ khu vực cấm dừng) từ danh sách POI đã xác thực
                    │
                    ▼
                🔵 AI Step (ML, giai đoạn 2): Với các khu vực rule-based vẫn sai lệch
                cao, mô hình học từ lịch sử GPS thực tế + traffic real-time để
                tinh chỉnh điểm đón
                    │
                    ▼
                🟢 Human Step (HITL): Tài xế xác nhận/điều chỉnh điểm đón ngay trên app
                trước khi bắt đầu di chuyển (1 chạm, không cần gọi điện)
                    │
                    ▼
                ↩️ Fallback: Nếu model không tự tin (confidence thấp) hoặc điểm đón
                nằm ngoài phạm vi đã xác thực → tự động quay về gợi ý bản đồ tĩnh
                gốc + yêu cầu khách xác nhận thủ công qua app (không gọi AI)
                    │
                    ▼
                [Khách hàng và tài xế gặp nhau]

                
# 🏁 Phase 5 — EVALUATE (Nhóm, 20 min)

### AI Readiness Checklist:
1. [ ] Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test? → Chưa đạt. Có log GPS lịch sử chuyến đi, nhưng dữ liệu POI (điểm dừng hợp lệ, khu vực cấm dừng) chưa được chuẩn hóa — đây là input bắt buộc cho cả rule-based lẫn ML.
2. [ ] Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)? → Đạt. Đã thiết kế Fallback (quay về bản đồ tĩnh gốc khi model không tự tin) và HITL (tài xế xác nhận điểm đón trên app trước khi di chuyển).
3. [ ] Stakeholders sẵn sàng thay đổi quy trình làm việc cũ? → Chưa đạt. Tài xế hiện quen xác nhận vị trí qua gọi điện; cần đào tạo và thử nghiệm để chuyển sang thao tác xác nhận trên app.

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
[ ] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển với scope hẹp.
[X] **NOT YET (Cần tích lũy thêm dữ liệu/xác lập baseline):** Trì hoãn để chuẩn bị thêm.
[ ] **NO-GO (Không khả thi / Rule-based tốt hơn):** Hủy bỏ dự án AI này.

**Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí):**
> 2/3 điều kiện trong Checklist chưa đạt: dữ liệu POI chưa sạch (mục 1) và thói quen tài xế chưa sẵn sàng đổi quy trình (mục 3). Trong đó, dữ liệu sạch là điều kiện tiên quyết bắt buộc — build model hay rule-based trên dữ liệu POI chưa chuẩn hóa đều cho kết quả không đáng tin cậy, dẫn tới rủi ro làm sai ngay từ bước đầu và lãng phí ngân sách.Đề xuất lộ trình 2 giai đoạn trước khi quay lại đánh giá GO:
- Giai đoạn chuẩn bị (4–6 tuần): làm sạch và chuẩn hóa dữ liệu POI (điểm dừng hợp lệ, khu vực cấm dừng), đồng thời chạy thử nghiệm rule-based snap-to-nearest-valid-stop trên phạm vi hẹp (1 quận/khu vực) để xác lập baseline định lượng (thời gian chờ, tỷ lệ hủy chuyến trước/sau).
- Giai đoạn thử nghiệm hành vi: pilot tính năng xác nhận điểm đón trên app với một nhóm tài xế nhỏ, đo tỷ lệ chấp nhận thay đổi thói quen trước khi triển khai rộng.
→ Sau khi cả 3 mục Checklist đạt và có baseline rule-based rõ ràng, mới nên đưa dự án quay lại đánh giá GO — lúc đó cũng sẽ có cơ sở dữ liệu để quyết định có cần nâng cấp lên ML (giai đoạn 2) hay rule-based đã đủ giải quyết vấn đề.

---