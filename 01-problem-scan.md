# 🔍 Phase 1 — SCAN (Cá nhân, 20 min)

Hãy sử dụng **4 Lenses** dưới đây để quét qua hoạt động vận hành của các công ty thành viên Vingroup. Ghi lại **ít nhất 5 bài toán/bottleneck** thực tế.

### 4 Lenses tìm bài toán AI cho Vingroup:
1. **Lặp lại (Repetitive):** Tác vụ lặp đi lặp lại nhiều lần hằng ngày. (Ví dụ: So khớp hóa đơn sạc điện tại VinFast, route lại chuyến taxi tại Xanh SM).
2. **Tốn thời gian (Time-consuming):** Tác vụ ngốn thời gian xử lý thủ công của nhân viên. (Ví dụ: Soạn thảo phản hồi đánh giá 1-star của cư dân Vinhomes).
3. **AI có thể tốt hơn (AI-upgrade):** Dịch vụ khách hàng hiện tại còn chậm hoặc phản hồi rập khuôn. (Ví dụ: Chatbot CSKH Vinpearl hỗ trợ đặt vé vui chơi).
4. **Pain từ người khác (Stakeholder Pain):** Bottleneck khiến khách hàng hoặc nhân viên thực địa phàn nàn. (Ví dụ: Tài xế Xanh SM phàn nàn về việc hệ thống gợi ý điểm đón khách không chính xác).

> [!TIP]
> **🤖 AI Prompts — Partner brainstorm:**
> Hãy sử dụng prompt sau để brainstorm các bài toán thực tế nếu bạn chưa có ý tưởng:
> *"Tôi là AI Engineer tại Vin Smart Future (Vingroup). Tôi đang tìm kiếm các pain point vận hành cụ thể có thể tối ưu bằng AI cho mảng [Chọn một: VinFast / Xanh SM / Vinhomes / Vinmec]. Hãy gợi ý cho tôi 5 quy trình nghiệp vụ thủ công, tốn nhiều thời gian và gây rò rỉ hiệu suất kèm con số thống kê ước tính về tổn thất."*

List bài toán của tôi:
#	Subsidiary 	    Lens	                 Mô tả ngắn bài toán
1	Xanh SM	  | Pain từ người khác  |	Hệ thống gợi ý điểm đón/trả khách không chính xác, khiến tài xế mất 3–5 phút chờ/tìm khách mỗi lượt
2	Xanh SM   |  Tốn thời gian	    |     Nhân viên CSKH tra cứu và tính lại cước thủ công khi khách khiếu nại sai phí (8–15 phút/khiếu nại)
3	Xanh SM	  |  Lặp lại	        |    Điều phối viên theo dõi thủ công mức pin đội xe để quyết định lịch sạc, gây xe dừng khẩn cấp mất 20–30 phút/lượt
4	Xanh SM	  |   Tốn thời gian	    |    Bộ phận vận hành đọc và phân loại thủ công phản hồi đánh giá sao thấp của tài xế (5–7 phút/phản hồi)
5	Xanh SM	  |  AI có thể tốt hơn  |     Điều phối xe theo khu vực/giờ cao điểm dựa vào kinh nghiệm, chưa dự báo theo pattern nhu cầu thực tế


# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn **top 3 bài toán** từ danh sách trên và hoàn thiện **3 Quick Problem Cards** dưới đây (10 phút/card).

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #___                                     │
│                                                             │
│ Bài toán (1 câu): ________________________________________  │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? ______________________________________ │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. ___ ──> 2. ___ ──> 3. ___ ──> 4. ___                   │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? ___ (⏱ ___ phút/lượt)      │
│ AI có thể nhảy vào hỗ trợ ở bước nào? _____________________ │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? ______________________ │
│   VD: "Giảm thời gian soạn phản hồi từ 10 min ──> under 2 min"│
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [ ] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

> [!TIP]
> **🤖 AI Prompts — Stress-Test thẻ bài toán:**
> Hãy dán nội dung thẻ bài toán của bạn vào LLM để nhận phản biện:
> *"Đây là một thẻ bài toán vận hành tôi đề xuất cho Vin Smart Future: [Dán nội dung]. Hãy đóng vai trò là một CFO và Trưởng phòng Vận hành cực kỳ khắt khe, chỉ ra cho tôi 3 điểm yếu về logic, metric, và giải thích vì sao rule-based code thông thường có thể giải quyết bài toán này tốt hơn là dùng AI."*

---
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                        │
│                                                                │
│ Bài toán (1 câu): Hệ thống gợi ý điểm đón/trả khách không     │
│   chính xác, khiến tài xế chờ lâu và khách hủy chuyến.        │
│ Công ty thành viên: [x] Xanh SM                               │
│                                                                │
│ Ai đang đau (Actor)? Tài xế Xanh SM và khách hàng đặt xe      │
│                                                                │
│ Workflow thủ công hiện tại:                                   │
│   1. Khách đặt xe ──> 2. Hệ thống gợi ý điểm đón cố định      │
│   theo bản đồ tĩnh ──> 3. Tài xế di chuyển đến điểm không     │
│   khớp thực tế ──> 4. Tài xế gọi khách xác nhận lại vị trí    │
│                                                                │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-3 (⏱ ~3-5 phút/lượt)  │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 — mô hình gợi ý  │
│   điểm đón học từ lịch sử GPS thực tế + traffic real-time     │
│                                                                │
│ Đo thành công bằng gì (Metric có số)?                         │
│   Giảm thời gian chờ đón khách trung bình từ 5 phút xuống     │
│   dưới 2 phút; giảm tỷ lệ hủy chuyến do sai điểm đón 30%      │
│                                                                │
│ Quick Architecture: [x] LLM/ML  [ ] Rule  [ ] Agent  [ ] No AI│
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                         │
│                                                                │
│ Bài toán (1 câu): Nhân viên CSKH tra cứu và tính lại cước     │
│   thủ công khi khách khiếu nại sai phí chuyến đi.             │
│ Công ty thành viên: [x] Xanh SM                               │
│                                                                │
│ Ai đang đau (Actor)? Nhân viên CSKH và khách hàng khiếu nại   │
│                                                                │
│ Workflow thủ công hiện tại:                                   │
│   1. Khách gửi khiếu nại cước ──> 2. Nhân viên tra lịch sử    │
│   chuyến đi + log GPS ──> 3. Tính lại cước thủ công theo biểu │
│   phí ──> 4. Phản hồi và hoàn/thu chênh lệch cho khách        │
│                                                                │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-3 (⏱ ~8-15 phút/lượt) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-3 — tự động đối  │
│   soát log GPS với biểu phí, tính lại cước và đề xuất phản hồi│
│                                                                │
│ Đo thành công bằng gì (Metric có số)?                         │
│   Giảm thời gian xử lý khiếu nại từ 12 phút ──> dưới 3 phút;  │
│   tăng tỷ lệ giải quyết trong ngày từ 50% lên 90%              │
│                                                                │
│ Quick Architecture: [x] Rule  [ ] LLM  [ ] Agent  [ ] No AI   │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                         │
│                                                                │
│ Bài toán (1 câu): Điều phối viên theo dõi thủ công mức pin đội│
│   xe, gây xe hết pin giữa chuyến hoặc chờ sạc không cần thiết.│
│ Công ty thành viên: [x] Xanh SM                               │
│                                                                │
│ Ai đang đau (Actor)? Điều phối viên và tài xế Xanh SM         │
│                                                                │
│ Workflow thủ công hiện tại:                                   │
│   1. Điều phối viên kiểm tra % pin qua dashboard theo giờ ──> │
│   2. Ước lượng thủ công xe nào cần sạc sớm ──> 3. Gọi/nhắn tài│
│   xế điều hướng vào trạm sạc ──> 4. Tài xế gián đoạn chuyến   │
│   đang chạy để vào sạc                                        │
│                                                                │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 (⏱ ~20-30 phút/lượt   │
│   gián đoạn khi xử lý trễ)                                    │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 — mô hình dự báo │
│   pin cạn theo lịch trình + gợi ý lịch sạc chủ động trước     │
│                                                                │
│ Đo thành công bằng gì (Metric có số)?                         │
│   Giảm số lượt xe hết pin giữa chuyến 50%; giảm thời gian     │
│   gián đoạn/xe từ 25 phút xuống dưới 10 phút                  │
│                                                                │
│ Quick Architecture: [x] LLM/ML  [ ] Rule  [ ] Agent  [ ] No AI│
└─────────────────────────────────────────────────────────────┘