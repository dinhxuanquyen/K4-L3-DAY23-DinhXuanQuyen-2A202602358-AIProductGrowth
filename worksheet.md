# Worksheet — SmartCheck-in Kiosk

Họ tên: Đinh Xuân Quyền · MSSV: 2A202602358 · Ngày làm: 09/10/2026

## Số liệu đầu vào từ Mô hình tài chính & Monetization One-Pager
- **ARPU:** $162/hotel/tháng (Gói Hybrid: $30 nền + $0.11/hồ sơ với cap 1,200 hồ sơ)
- **Cost/Job:** $0.050212/job (Chi phí API OCR CCCD + Face Verification)
- **Gross Margin mục tiêu:** 68.39% (tại 1,200 hồ sơ/tháng)
- **CAC ước tính:** $320/hotel (Ngân sách trần CAC: $1,329.41)
- **CAC Payback mục tiêu:** 2.89 tháng (ngưỡng an toàn < 12 tháng)
- **Runway:** 6 tháng
- **Điểm gãy mô hình (Break-even):** Giá API/retry > 2.0944 lần cơ sở làm GM < 50%
- **Containment mục tiêu:** 85% (khách tự hoàn tất không cần lễ tân can thiệp)

---

## Trạm 1 — Loại mô hình

### 1. Trả lời 3 câu hỏi phân loại theo thực tế hôm nay:
1. **Ai trả tiền cho bạn?**
   - **Doanh nghiệp (Khách sạn / Hotel):** Chủ khách sạn hoặc Tổng quản lý duyệt chi từ ngân sách vận hành lễ tân để giảm tải nhân sự và ùn tắc sảnh.
2. **Ai dùng sản phẩm?**
   - **Khách hàng của người trả tiền (Khách lưu trú / Guest):** Khách đến khách sạn tự thao tác quét CCCD và chụp khuôn mặt để nhận phòng.
3. **Bạn có chạm được người dùng cuối không?**
   - **CÓ:** Khách lưu trú trực tiếp tương tác với màn hình Web Kiosk của chúng tôi đặt tại sảnh, nhìn thương hiệu SmartCheck-in Kiosk, đưa CCCD vào camera và thực hiện nhận diện khuôn mặt trực tiếp.

### 2. Câu chốt loại mô hình:
> **"Chúng tôi là B2B2C vì tiền đến từ Khách sạn (ngân sách vận hành lễ tân), người dùng thật là khách lưu trú của khách sạn đó, và chúng tôi chạm được họ trực tiếp qua giao diện Web Kiosk đặt tại sảnh khách sạn."**

---

### 3. Bảng đèn §3.3 (B2B2C) từ HANDBOOK và hiện trạng đo lường:

| Đèn trong §3.3 | ✅ / 🔧 / ❌ | Số nằm ở đâu / Cần gì để đo |
|---|:---:|---|
| **Partner activation rate** ⭐ | 🔧 | Cần log hệ thống: % khách sạn go-live có $\ge 10$ lượt check-in của khách thật trong 30 ngày (cần viết query lọc bỏ tài khoản test nội bộ của hotel). |
| **End-user reach trong partner** | 🔧 | Cần kết nối PMS API của khách sạn: lấy tỷ lệ $\frac{\text{Số session Kiosk hoàn tất}}{\text{Tổng số lượt booking đến ngày check-in trên PMS}}$. |
| **Time-to-first-end-user** | 🔧 | Cần tracking event: Timestamp từ lúc bàn giao Kiosk đến event `room_assigned` đầu tiên của khách thật. |
| **Volume volatility** | ❌ | Chưa đo được: Cần tối thiểu 3–6 tháng dữ liệu chạy thực tế qua các mùa cao điểm/thấp điểm du lịch để tính độ lệch chuẩn volume. |
| **GM sau rev-share / COGS** | ✅ | Đo được ngay từ bảng tài chính Day 22: $162 thu vào trừ chi phí API bên thứ ba theo số lượng hồ sơ thực tế. |
| **Chi phí inference ÷ doanh thu — theo TỪNG partner** | 🔧 | Cần gắn `hotel_id` vào log gọi API bên thứ ba (FPT.AI / VNPT eKYC) để tổng hợp chi phí inference riêng của từng khách sạn mỗi tuần. |
| **Tập trung volume** | ✅ | Đo được ngay: Tính % volume từ khách sạn lớn nhất chia cho tổng volume từ database quản trị. |
| **Chất lượng nhìn từ end-user** | ✅ | Đo được ngay: Tính tỷ lệ `manual_desk_escalated` ÷ `session_started` và thời gian hoàn tất p95 trên hệ thống telemetry. |
| **Doanh thu/partner · partner NRR · GM tổng** | ✅ | Đo được ngay từ báo cáo tài chính hàng tháng và hợp đồng gia hạn của các khách sạn. |

---

## Trạm 2 — Thẻ đèn

**North Star:** Số lượt check-in tự động hoàn toàn (*Auto-PASS Check-ins*) mỗi tháng — hiện tại 0 (giai đoạn pilot) — mục tiêu 1,000 lượt/tháng (sau 90 ngày).

| # | Tầng (L/O/G) | Đèn | Định nghĩa (đếm gì · **không** đếm gì) | Công thức | Nhịp · ai lấy số | Báo trước cho |
|---|:---:|---|---|---|---|---|
| 1 | **L** | **Partner Activation Rate** ⭐ | % khách sạn đã lắp Kiosk và kết nối PMS có **$\ge 10$ lượt check-in của khách thật** trong 30 ngày kể từ go-live. **Không** đếm lượt test thử nội bộ của nhân viên hay IT khách sạn. | $\frac{\text{Số hotel có } \ge 10 \text{ lượt guest thật}}{\text{Tổng số hotel đã go-live}}$ | Hằng tuần · Đinh Xuân Quyền | End-user Reach & Volume hồ sơ $\to$ Doanh thu Usage |
| 2 | **L** | **Tỉ lệ Straight-Through First-Pass** | % phiên eKYC khách quét CCCD và chụp Live Face thành công ngay lần đầu không cần thử lại. **Không** đếm các phiên khách huỷ giữa chừng trước khi chụp. | $\frac{\text{Số phiên eKYC Pass ngay lần 1}}{\text{Tổng số session có chụp ảnh}}$ | Hằng tuần · Đinh Xuân Quyền | HITL Escalation Rate $\to$ Chi phí AI retry |
| 3 | **L** | **End-user Adoption Rate** | % lượt khách lưu trú đến khách sạn chọn tự check-in tại Kiosk thay vì xếp hàng quầy lễ tân. **Không** đếm khách đoàn lớn hoặc khách walk-in không có booking trước. | $\frac{\text{Số session Kiosk hoàn tất}}{\text{Tổng lượt booking check-in trên PMS}}$ | Hằng tuần · Đinh Xuân Quyền | Volume hồ sơ $\to$ Doanh thu gói Hybrid |
| 4 | **O** | **Tỉ lệ chuyển giao lễ tân (HITL Escalation Rate)** | % phiên check-in tại Kiosk bị lỗi đối chiếu/OCR phải đẩy sang màn hình lễ tân xử lý thủ công. **Không** đếm trường hợp khách bấm thoát để quay lại sau. | $\frac{\text{Số event } manual\_desk\_escalated}{\text{Tổng } session\_started}$ | Hằng tuần · Đinh Xuân Quyền | Trải nghiệm sảnh $\to$ Nguy cơ Hotel Churn |
| 5 | **O** | **Chi phí inference ÷ doanh thu theo TỪNG partner** *(Đèn chi phí AI)* | Tỷ lệ chi phí gọi API AI (OCR + Face Match bao gồm cả lần gọi do retry) so với doanh thu thu từ khách sạn đó trong tháng. **Không** tính chi phí hosting cloud chung hay phí phần cứng. | $\frac{\text{Tổng tiền API AI của hotel X}}{\text{Doanh thu hotel X trả trong tháng}}$ | Hằng tuần · Đinh Xuân Quyền | Gross Margin của từng khách sạn |
| 6 | **G** | **Gross Margin thực tế** | Tỷ lệ lãi gộp trên toàn bộ hệ thống sau khi trừ chi phí API bên thứ ba (COGS trực tiếp) trên tổng doanh thu. **Không** trừ chi phí bán hàng (CAC) hay lương cố định. | $\frac{\text{Tổng doanh thu} - \text{Tổng chi phí API}}{\text{Tổng doanh thu}}$ | Hằng tháng · Đinh Xuân Quyền | Hòa vốn vận hành & Runway |
| 7 | **G** | **CAC Payback thực tế (tháng)** | Số tháng cần thiết để tiền lãi gộp từ một khách sạn bù đắp hết chi phí bán hàng Sales-Led (CAC). **Không** tính doanh thu bán thiết bị phần cứng. | $\frac{\text{CAC khách sạn}}{\text{Doanh thu/tháng} \times \text{Gross Margin}}$ | Hằng quý · Đinh Xuân Quyền | LTV/CAC & Tính bền vững của kênh Sales-Led |

Đèn chi phí AI là đèn số: **5** (và được hỗ trợ bởi đèn **2**).

---

## Trạm 3 — Ngưỡng

| # | Đèn | 🟢 (Ổn) | 🟡 (Cảnh báo) | 🔴 (Nguy cơ) | Nguồn | Lý do (1 câu) · Ngày kiểm tra nếu [BM] |
|:---:|---|:---:|:---:|:---:|:---:|---|
| 1 | **Partner Activation Rate** ⭐ | $\ge 60\%$ | $30\% - 60\%$ | $< 30\%$ | **[TB]** | Chưa có chuẩn ngành B2B2C Kiosk; áp dụng khung HANDBOOK §3.3; đo 2 chu kỳ pilot (kết thúc 06/01/2027) để chốt baseline. |
| 2 | **Tỉ lệ Straight-Through First-Pass** | $\ge 85\%$ | $70\% - 85\%$ | $< 70\%$ | **[MH] 1** | Suy từ mục tiêu Containment 85%; nếu first-pass $< 70\%$ thì khách retry nhiều làm chi phí AI tăng vọt ép Gross Margin tụt dưới 50%. |
| 3 | **End-user Adoption Rate** | $\ge 40\%$ | $20\% - 40\%$ | $< 20\%$ | **[TB]** | Kiosk cần phục vụ ít nhất 20% lượng khách để lễ tân sảnh thực sự giảm tải; đo baseline trên 2 hotel pilot đầu tiên (dự kiến có số: 22/10/2026). |
| 4 | **Tỉ lệ chuyển giao lễ tân (HITL Escalation Rate)** | $< 15\%$ | $15\% - 30\%$ | $> 30\%$ | **[MH] 2** | Nghịch đảo của Containment $\ge 85\%$; nếu Escalation $> 30\%$ (Containment $< 70\%$) thì giá trị tiết kiệm công lễ tân biến mất và vi phạm cam kết với hotel. |
| 5 | **Chi phí inference ÷ Doanh thu theo TỪNG partner** *(Đèn AI)* | $< 25\%$ | $25\% - 38\%$ | $> 38\%$ | **[MH] 3** | Suy từ điểm gãy mô hình Day 22: khi chi phí API/retry $> 2.0944$ lần mức cơ sở thì chi phí inference vượt $38\%$ doanh thu, làm Gross Margin tụt dưới $50\%$. |
| 6 | **Gross Margin thực tế** | $\ge 60\%$ | $50\% - 60\%$ | $< 50\%$ | **[BM]** | Benchmark AI-native software đạt ~53% (ICONIQ / Bessemer State of AI, kiểm tra ngày 08/10/2026); mức $< 50\%$ là ngưỡng mô hình gãy của Day 22. |
| 7 | **CAC Payback thực tế (tháng)** | $< 6$ tháng | $6 - 12$ tháng | $> 12$ tháng | **[BM]** | Benchmark SaaS SMB/Mid-market chu kỳ bán Sales-Led phải có Payback $< 12$ tháng (Bessemer Cloud Index, kiểm tra ngày 08/10/2026) để bảo vệ runway 6 tháng. |

---

### Phụ lục [MH] — Phép tính suy từ mô hình tài chính (≥2 ngưỡng)

#### **[MH] 1 — Tỉ lệ Straight-Through First-Pass**
```
Đầu vào (từ Monetization One-Pager Day 22):
- Containment mục tiêu = 85% (1_Cost_Job!B10).
- Chi phí cơ sở Cost/Job = $0.050212/job.
- Điểm gãy mô hình: Nếu số lần gọi API do retry/chụp lại > 2.0944 lần cơ sở thì Gross Margin tụt dưới 50% (2_Pricing!H53).

Phép tính:
- Mỗi ca không pass lần đầu sẽ sinh ra ít nhất 1 lần chụp lại (tốn thêm $0.050212 API).
- Nếu First-Pass < 70%, trung bình một hồ sơ cần 1 + (1 - 0.70) × 2 ≈ 1.6 - 2.0 lượt gọi API, tiệm cận ngưỡng gãy mô hình 2.0944 lần.

Kết quả:
🟢 ≥ 85% (đạt mục tiêu thiết kế ban đầu)
🟡 70% – 85% (chi phí tăng nhưng GM vẫn > 55%)
🔴 < 70% (kích hoạt báo động gãy biên lợi nhuận)
```

#### **[MH] 2 — Tỉ lệ chuyển giao lễ tân (HITL Escalation Rate)**
```
Đầu vào (từ Monetization One-Pager Day 22):
- Neo giá trị tạo ra: Tiết kiệm 10 phút/job của lễ tân ($4/giờ) = $0.667/job.
- Với 1,020 job tự động thành công/tháng, giá trị tạo ra cho khách sạn là $680/tháng.
- Giá bán đề xuất: $162/tháng (bằng 23.8% giá trị tạo ra).
- Điều kiện để giá bán duy trì ≤ 25% giá trị tạo ra: Khách sạn cần đạt Containment ≥ 81% (tương đương Escalation Rate ≤ 19%).

Phép tính:
- Nếu Escalation Rate > 30% (Containment < 70%), số ca thành công giảm xuống dưới 840 ca/tháng.
- Giá trị tạo ra cho khách sạn lúc này chỉ còn: 840 ca × $0.667 = $560.
- Tỷ lệ phí thu $162 ÷ $560 = 28.9% > 25% (vượt trần chấp nhận neo giá của chủ khách sạn).

Kết quả:
🟢 < 15% (rất tốt, khách sạn tiết kiệm tối đa nhân công)
🟡 15% – 30% (khách sạn bắt đầu phàn nàn lễ tân vẫn phải hỗ trợ nhiều)
🔴 > 30% (mô hình mất giá trị neo, nguy cơ hủy hợp đồng cao)
```

#### **[MH] 3 — Chi phí inference ÷ Doanh thu theo TỪNG partner**
```
Đầu vào (từ Monetization One-Pager Day 22):
- Doanh thu quy đổi trung bình mỗi hồ sơ trong gói Hybrid: $0.158824/hồ sơ (2_Pricing!B19).
- Chi phí API AI cơ sở: $0.050212/hồ sơ.
- Tỷ lệ chi phí API cơ sở / Doanh thu = $0.050212 ÷ $0.158824 = 31.6%.
- Gross Margin mục tiêu cơ sở: 68.39%.

Phép tính:
- Mức gãy Gross Margin (< 50%): Xảy ra khi chi phí COGS vượt quá 50% doanh thu.
- Vì COGS ngoài API còn có chi phí server/hạ tầng (~12%), nên trần tối đa cho chi phí API inference là: 50% - 12% = 38% doanh thu.
- Tương đương chi phí API tối đa trên mỗi hồ sơ là: $0.158824 × 38% ≈ $0.0603/hồ sơ (tức mức retry/lỗi tối đa không quá 1.2 lần cơ sở).

Kết quả:
🟢 < 25% (Gross Margin đạt > 65%, rất an toàn)
🟡 25% – 38% (Gross Margin nằm trong khoảng 50% – 65%)
🔴 > 38% (Gross Margin rơi xuống dưới 50%, khách sạn này bị lỗ vận hành)
```

---

## Trạm 4 — 5 luật quyết định

*(⏹ = Luật dừng làm gì đó — bài có 3 luật dừng, đạt yêu cầu $\ge 2$)*

1. ⏹ **Luật 1 (Partner Activation Rate — Luật dừng ký mới):**  
   **NẾU** Partner Activation Rate $< 30\%$  
   **TRONG** 30 ngày kể từ ngày bàn giao Kiosk  
   **VÀ** đã có $\ge 3$ khách sạn ký hợp đồng go-live  
   **THÌ** dừng toàn bộ hoạt động đi chào bán khách sạn mới, Đinh Xuân Quyền trực tiếp đến sảnh 3 khách sạn hiện tại trong 2 tuần để đào tạo lại lễ tân và lắp đặt biển chỉ dẫn hướng dẫn khách ra Kiosk.  
   **KHÔNG THÌ** không được tiếp tục dùng "số khách sạn đã ký hợp đồng" để báo cáo tiến độ kinh doanh hay chạy thêm quảng cáo tìm khách sạn mới.

2. ⏹ **Luật 2 (HITL Escalation Rate — Luật dừng tính năng/phạm vi):**  
   **NẾU** Tỉ lệ chuyển giao lễ tân (HITL Escalation Rate) $> 30\%$  
   **TRÊN** 100 phiên check-in gần nhất tại một khách sạn  
   **VÀ** mỗi phiên đều có ảnh CCCD và Live Face gửi lên server  
   **THÌ** đóng băng luồng check-in cho khách quốc tế/hộ chiếu tại Kiosk đó, thu hẹp phạm vi chỉ phục vụ CCCD gắn chip Việt Nam và bổ sung cơ chế kiểm tra độ nét ảnh trước khi gửi gọi API eKYC.  
   **KHÔNG THÌ** không được yêu cầu nhân viên lễ tân tiếp tục hỗ trợ thủ công từng ca lỗi mà không giới hạn lại phạm vi nghiệp vụ của Kiosk.

3. **Luật 3 (Chi phí inference ÷ Doanh thu — Luật bảo vệ Gross Margin):**  
   **NẾU** Chi phí inference ÷ Doanh thu $> 38\%$ tại một khách sạn  
   **TRONG** 2 tuần liên tiếp  
   **VÀ** khách sạn đó phát sinh $\ge 100$ lượt check-in/tuần  
   **THÌ** khóa cứng số lần chụp lại tối đa 2 lần/khách (retry cap), sau 2 lần không đạt thì tự động chuyển ca sang lễ tân và kích hoạt điều khoản đàm phán phụ thu $0.05/lần retry vượt hạn mức vào kỳ đối soát tháng sau.  
   **KHÔNG THÌ** không được tự gánh chi phí API phát sinh do khách thao tác sai nhiều lần làm âm biên lợi nhuận của khách sạn đó.

4. **Luật 4 (Tỉ lệ Straight-Through First-Pass — Luật xử lý môi trường vật lý):**  
   **NẾU** Tỉ lệ Straight-Through First-Pass $< 70\%$  
   **TRONG** 1 tuần  
   **VÀ** có $\ge 50$ phiên chụp ảnh tại sảnh  
   **THÌ** lắp thêm đèn LED trợ sáng chuyên dụng tại đỉnh Kiosk và cập nhật khung định vị khuôn mặt (oval guide) trên giao diện Web Kiosk trong vòng 3 ngày làm việc.  
   **KHÔNG THÌ** không được vội vàng đổi sang model AI eKYC đắt tiền hơn khi chưa xử lý lỗi thiếu sáng và ngược sáng tại sảnh khách sạn.

5. ⏹ **Luật 5 (End-user Adoption Rate — Luật dừng hỗ trợ & thu hồi):**  
   **NẾU** End-user Adoption Rate $< 20\%$  
   **SAU** 30 ngày kể từ go-live tại một khách sạn  
   **VÀ** khách sạn có $\ge 200$ lượt booking check-in thực tế trong tháng trên PMS  
   **THÌ** dừng chính sách hỗ trợ kỹ thuật miễn phí tại chỗ, gửi văn bản cảnh báo cho Chủ khách sạn và kích hoạt điều khoản thu hồi thiết bị Kiosk sau 14 ngày nếu khách sạn không cải thiện luồng điều phối khách.  
   **KHÔNG THÌ** không được giảm giá phí thuê bao nền $30/tháng hay nhượng bộ thêm tính năng tùy biến riêng để níu giữ đối tác không chịu thúc đẩy khách dùng.

