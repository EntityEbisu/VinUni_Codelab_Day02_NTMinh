# Phase 1–2 — Problem Scan & Quick Cards

**Học viên:** Văn Nhân (NTMinh)  
**Vai trò trong lab:** AI Product Engineer, Vin Smart Future  
**Mảng quan sát chính:** Xanh SM (GSM), bổ sung VinFast / Vinhomes / Vinmec / Vinpearl

Tôi đóng vai AI Engineer tại Vin Smart Future, đi quét các điểm nghẽn vận hành ở các công ty thành viên Vingroup. Mục tiêu không phải “tìm chỗ để nhét AI”, mà là tìm bài toán có người đang đau thật, có quy trình lặp lại, và có thể đo bằng số.

---

# Phase 1 — SCAN (4 Lenses)

Dùng 4 ống kính: **Lặp lại**, **Tốn thời gian**, **AI-upgrade**, **Pain từ người khác**.

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **Xanh SM** | Lặp lại | Mỗi khi khách đổi điểm đến giữa cuốc, điều phối viên phải so khớp lại cuốc xe, cập nhật lộ trình và thông báo tài xế bằng tin nhắn thủ công. |
| 2 | **Xanh SM** | Tốn thời gian | Tài xế báo hết pin / kẹt sạc giữa đường; điều phối viên tra cứu GPS + trụ sạc VinFast trống rồi soạn hướng dẫn — khoảng 12–15 phút/lượt vào giờ cao điểm. |
| 3 | **VinFast** | Lặp lại | Đối chiếu hóa đơn sạc điện từ trạm đối tác với log sạc nội bộ mỗi tuần; lệch số liệu phải lần từng dòng trên Excel. |
| 4 | **Vinhomes** | AI-upgrade | Phản ánh cư dân trên App Vinhomes Resident (mất nước, đèn hành lang, ồn, thẻ xe) được CSKH đọc tay và forward; phản hồi chậm, hay rập khuôn. |
| 5 | **Vinmec** | Pain từ người khác | Bác sĩ mất 20–30 phút/ca để viết tóm tắt xuất viện từ EMR, xét nghiệm và ghi chú lâm sàng; ca cuối ngày dồn ứ. |
| 6 | **Vinpearl** | Pain từ người khác | Review khẩn trên Booking/Agoda/Google (“phòng bẩn”, “thái độ nhân viên”) không được lọc sớm; manager chỉ thấy khi điểm rating đã tụt. |

---

# Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

Chọn top 3 từ SCAN: **#2 (Xanh SM sự cố pin)**, **#4 (Vinhomes CSKH)**, **#5 (Vinmec xuất viện)**.

## Card #1 — Xanh SM: Xử lý sự cố hết pin thực địa

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                         │
│                                                             │
│ Bài toán (1 câu): Tài xế Xanh SM hết pin / kẹt sạc giữa     │
│ đường cần được chỉ đúng trạm sạc trống hoặc cứu hộ pin.     │
│ Công ty thành viên: [x] Xanh SM                             │
│                                                             │
│ Ai đang đau (Actor)? Tài xế (không chạy được cuốc),         │
│ điều phối viên (quá tải giờ cao điểm), khách đang chờ xe.    │
│                                                             │
│ Workflow thủ công hiện tại:                                 │
│   1. Tài xế gọi/chat tổng đài báo pin thấp                  │
│   → 2. Điều phối mở bản đồ nội bộ, tìm biển số / GPS        │
│   → 3. Mở dashboard trạm sạc VinFast, lọc trụ trống         │
│   → 4. Soạn tin chỉ đường + loại cổng sạc gửi App tài xế   │
│   → 5. Gọi đội cứu hộ pin nếu xe không tới được trạm        │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3–4 (⏱ 10–12 phút)   │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3–4:            │
│ kéo vị trí + trạng thái trụ → draft tin hướng dẫn.          │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                      │
│ Giảm thời gian xử lý sự cố từ ~15 phút ──> dưới 3 phút;     │
│ tỉ lệ chỉ đúng trạm / đúng loại cổng ≥ 98%.                 │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

## Card #2 — Vinhomes: Phân loại & điều hướng phản ánh cư dân

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                         │
│                                                             │
│ Bài toán (1 câu): Phản ánh cư dân trên App bị đọc tay,       │
│ route sai ban, CSKH soạn trả lời chậm và rập khuôn.         │
│ Công ty thành viên: [x] Vinhomes                            │
│                                                             │
│ Ai đang đau (Actor)? Cư dân (chờ SLA), CSKH tòa nhà,        │
│ kỹ thuật viên kỹ thuật tòa.                                 │
│                                                             │
│ Workflow thủ công hiện tại:                                 │
│   1. Cư dân gửi ticket trên App Vinhomes Resident             │
│   → 2. CSKH đọc nội dung, tự đoán loại sự cố                 │
│   → 3. Forward Zalo/email sang ban kỹ thuật hoặc an ninh     │
│   → 4. Soạn tin phản hồi “đã ghi nhận” gửi lại cư dân        │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2–3 (⏱ 8–15 phút,    │
│ route sai làm ticket quay vòng thêm nửa ngày).              │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 (phân loại    │
│ intent + tòa/căn) và Bước 4 (draft phản hồi, không tự gửi).  │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                      │
│ First-response từ ~12 giờ ──> dưới 30 phút;                 │
│ tỉ lệ route đúng ban ≥ 90%.                                │
│                                                             │
│ Quick Architecture: [ ] No AI  [x] Rule  [x] LLM  [ ] Agent │
│ (Rule router cho danh mục cố định + LLM cho câu tự do)     │
└─────────────────────────────────────────────────────────────┘
```

## Card #3 — Vinmec: Draft tóm tắt xuất viện

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                         │
│                                                             │
│ Bài toán (1 câu): Bác sĩ mất 20–30 phút/ca để viết           │
│ discharge summary từ EMR, kết quả xét nghiệm và ghi chú.    │
│ Công ty thành viên: [x] Vinmec                              │
│                                                             │
│ Ai đang đau (Actor)? Bác sĩ điều trị, điều dưỡng xuất viện, │
│ bệnh nhân chờ giấy tờ.                                     │
│                                                             │
│ Workflow thủ công hiện tại:                                 │
│   1. Mở EMR, lướt diễn biến nội trú                         │
│   → 2. Copy tay kết quả CLS / thuốc đang dùng               │
│   → 3. Viết tóm tắt + hướng dẫn theo dõi tại nhà             │
│   → 4. In / ký / giải thích cho bệnh nhân                    │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2–3 (⏱ 20–30 phút)    │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2–3: trích      │
│ xuất sự kiện lâm sàng và draft bản tóm tắt dễ đọc.          │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                      │
│ Thời gian soạn thảo từ 25 phút ──> dưới 8 phút (bác sĩ       │
│ chỉ review/sửa); 100% bản draft phải được bác sĩ ký.         │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

---

# Quyết định lựa chọn để Deep-Dive

Nhóm/cá nhân chọn **Card #1 — Xanh SM xử lý sự cố hết pin thực địa**.

**Vì sao chọn Card #1**
- Bottleneck nằm ở tra cứu + soạn ngôn ngữ, đo được bằng phút/lượt.
- Rủi ro sai (chỉ nhầm trạm, bỏ qua pin cực thấp) kiểm soát được bằng HITL và rule cứng (pin < 5%).
- Starter prototype của lab bám đúng ranh giới `[DRAFT_ONLY]` và `dispatch_mobile_charger`.

**Vì sao chưa chọn Card #2 (Vinhomes)**  
Ticket liên quan phí quản lý / tranh chấp căn hộ dễ thành khiếu nại pháp lý. Cần taxonomy sự cố sạch và rule-router trước khi cho LLM soạn văn bản.

**Vì sao chưa chọn Card #3 (Vinmec)**  
Rủi ro lâm sàng cao: AI không được phép chẩn đoán hay ký giấy xuất viện. Bài toán khả thi nhưng cần HITL bác sĩ + dữ liệu EMR mẫu — phù hợp “NOT YET” hơn là prototype 30 phút trong lab.
