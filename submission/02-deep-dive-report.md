# 02 — Báo cáo Deep-Dive: Phân loại & Điều hướng phản ánh cư dân Vinhomes

> Lab 02 — AI Product Scoping (Vin Smart Future)
>
> **Phạm vi:** Phân loại + điều hướng + soạn phản hồi cho phản ánh của cư dân Vinhomes.
>
> **Mức độ bằng chứng:** Ý tưởng / phạm vi prototype. Các volume và thời gian vận hành bên dưới được đánh dấu **[ASSUMPTION]** vì bộ tài liệu lab không cung cấp log nội bộ đã xác minh của Vinhomes.

---

# Phase 3 — DEEP-DIVE

## 3.1 Quy trình hiện tại Mapping

### Quy trình hiện tại

```text
┌──────────────────────────┐
│ 1. Cư dân gửi phản ánh   │
│    trên App              │
│    ⏱ [ASSUMPTION] 1 phút │
└────────────┬─────────────┘
             │
             │ 🔄 Handoff: App → hàng đợi CSKH
             ▼
┌──────────────────────────┐
│ 2. CSKH mở & đọc ticket  │
│    ⏱ [ASSUMPTION] 2 phút │
└────────────┬─────────────┘
             │
             ▼
┌──────────────────────────────┐
│ 3. Phân loại vấn đề          │
│    🔴 BOTTLENECK             │
│    ⏱ [ASSUMPTION] 2 phút    │
└────────────┬─────────────────┘
             │ 🔄 Handoff
             ▼
┌──────────────────────────────┐
│ 4. Xác định tòa nhà/nhóm     │
│    🔴 BOTTLENECK             │
│    ⏱ [ASSUMPTION] 2 phút    │
└────────────┬─────────────────┘
             │
             ▼
┌──────────────────────────┐
│ 5. Điều hướng + soạn     │
│    phản hồi ban đầu      │
│    ⏱ [ASSUMPTION] 1 phút │
└──────────────────────────┘

Thời gian xử lý thủ công ước tính:
≈ 8 phút/ticket [ASSUMPTION]
```

### Bottleneck chính

Điểm kém hiệu quả cốt lõi không đơn thuần là **“viết phản hồi”**, mà là **chuyển ngôn ngữ tự nhiên, không có cấu trúc của cư dân thành category + team phụ trách + thông tin ticket đủ nhất quán**.

Các ví dụ dưới đây chỉ nhằm minh họa, không phải log vận hành của Vinhomes:

- “Đèn hành lang tầng 12 bị hư” → facility/điện.
- “Xe tôi bị chặn ở hầm” → bãi xe/an ninh/quyền ra vào.
- “Phí này sao tháng này lại tăng?” → phí/quản lý.
- “Nhà bên cạnh khoan đêm” → hàng xóm/tiếng ồn/quy định cộng đồng.

---

## 3.2 Problem Statement — 6 Fields

| Field | Nội dung |
|---|---|
| **1. Actor / Operator** | CSKH / điều phối viên tiếp nhận phản ánh của cư dân và ban quản lý tòa nhà. |
| **2. Current Workflow** | Cư dân gửi ticket → CSKH đọc nội dung → phân loại → xác định tòa nhà/bộ phận phụ trách → chuyển ticket và soạn phản hồi. Quy trình được scoping như một quy trình thủ công để phục vụ prototype. |
| **3. Bottleneck** | Phân loại intent, trích xuất entity (tòa nhà, khu vực, thời gian, vấn đề) và xác định queue/team phụ trách. |
| **4. Business Impact** | Ticket triage chậm làm tăng thời gian chờ và nguy cơ chuyển sai team. **[ASSUMPTION]** baseline 8 phút/ticket; cần xác nhận từ logs trước pilot. |
| **5. Success Metric** | (a) median triage time <2 phút; (b) routing accuracy ≥95% trên tập test có ground truth; (c) 100% ticket nhạy cảm hoặc có tác động cao được chuyển HITL; (d) JSON schema validity 100% trong prototype test. |
| **6. Operational Boundary** | AI chỉ được classify/extract/recommend/draft. AI **không** được tự phê duyệt khoản phí, kết luận tranh chấp, cam kết bồi thường, thay đổi dữ liệu cư dân, hoặc tự gửi phản hồi ra ngoài. Ticket nhạy cảm hoặc confidence thấp phải chuyển HITL/manual fallback. |

---

## 3.3 Độ phù hợp với AI

### Ma trận độ phù hợp AI

| Lựa chọn | Mức phù hợp | Lý do |
|---|---|---|
| Rule / State-Machine | **Lớp bắt buộc** | Các mapping cố định, policy, blacklist/allowlist và escalation cần lớp quyết định xác định. |
| **LLM Feature** | **Chính** | Mạnh ở hiểu ngôn ngữ tự nhiên, chuẩn hóa, trích xuất và soạn thảo. |
| Agentic Loop | **Không cần** | Workflow đã có các bước cố định; thực thi tự trị nhiều bước làm tăng rủi ro mà chưa tạo thêm giá trị rõ ràng. |

### Kiến trúc đề xuất

**Hybrid: Rule + LLM Feature + HITL**

1. **Pre-check / rules:** kiểm tra schema, required fields, từ khóa nhạy cảm và các tuyến xử lý xác định trước.
2. **LLM:** phân loại issue, trích xuất entity, tạo draft ngắn gọn.
3. **Policy layer:** loại output vi phạm boundary.
4. **HITL:** CSKH xác nhận route + draft.
5. **Hệ thống:** chỉ sau khi con người phê duyệt mới thực hiện thao tác nghiệp vụ tiếp theo.

### Future-State Flow

```text
┌──────────────────────┐
│ 1. Nhận ticket       │
│    từ App             │
└──────────┬───────────┘
           │
           ▼
┌────────────────────────────┐
│ 2. Pre-check / Rules       │
│    - kiểm tra schema       │
│    - kiểm tra nhạy cảm     │
└────────────┬───────────────┘
             │
             ▼
┌────────────────────────────┐
│ 3. 🔵 LLM Feature         │
│    - phân loại             │
│    - trích xuất            │
│    - đề xuất queue         │
│    - soạn draft             │
└────────────┬───────────────┘
             │
             ▼
┌────────────────────────────┐
│ 4. 🔎 Policy / Boundary    │
│    Kiểm tra output         │
└────────────┬───────────────┘
             │
        ┌────┴────┐
        │         │
        ▼         ▼
   Confidence    Nhạy cảm /
      cao        không chắc chắn
        │         │
        │         ▼
        │   🟢 HITL / CSKH review
        │         │
        └────┬────┘
             ▼
┌────────────────────────────┐
│ 5. 🟢 Con người xác nhận   │
│    route + draft            │
└────────────┬───────────────┘
             │
             ▼
┌────────────────────────────┐
│ 6. Hoàn tất thao tác       │
│    nghiệp vụ trong hệ thống │
└────────────────────────────┘

↩️ Fallback:
Nếu LLM lỗi / output không hợp lệ /
confidence thấp → quay về route thủ công.
```

---

# Phase 5 — EVALUATE

## AI Readiness Checklist

| # | Tiêu chí | Đánh giá | Ghi chú |
|---|---|---|---|
| 1 | Có dữ liệu mẫu/logs sạch để test? | ⚠️ **Chưa xác nhận** | Bộ tài liệu lab không cung cấp log Vinhomes. Cần dataset đã ẩn danh + ground truth. |
| 2 | Rủi ro khi AI sai có nằm trong tầm kiểm soát? | ✅ **Có thể kiểm soát** | Có HITL, policy layer, schema validation và manual fallback. |
| 3 | Stakeholders sẵn sàng thay đổi workflow? | ⚠️ **Chưa xác nhận** | Cần xác nhận với CSKH/ban quản lý trước pilot. |

## Quyết định cuối cùng

### ✅ **NOT YET — Cần tích lũy thêm dữ liệu/xác lập baseline**

### Justification

Bài toán có **AI fit khá rõ**, scope nhỏ và có thể thiết kế ranh giới vận hành chặt chẽ. Tuy nhiên, theo đúng tiêu chí lab, hiện chưa có dữ liệu/log nội bộ Vinhomes để chứng minh baseline, routing accuracy, volume, hoặc mức cải thiện thực tế.

Vì vậy quyết định phù hợp hơn là **NOT YET**, thay vì GO dựa trên các con số giả định. Sau khi có dataset đã ẩn danh, ground truth và baseline triage time, có thể chạy một prototype offline để kiểm tra:

- routing accuracy;
- intent classification quality;
- mức độ đầy đủ của entity extraction;
- tỷ lệ case phải chuyển HITL;
- tỷ lệ output JSON hợp lệ;
- thời gian xử lý so với baseline thủ công.

> **Kết luận:** Problem rõ + AI fit hợp lý + boundary kiểm soát được, nhưng **thiếu evidence thực tế để biện minh cho GO**. Do đó **NOT YET** là quyết định trung thực hơn trong giai đoạn scoping.
