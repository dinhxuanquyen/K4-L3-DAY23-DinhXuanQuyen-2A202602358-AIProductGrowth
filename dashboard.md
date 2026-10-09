# OPERATING DASHBOARD — SmartCheck-in Kiosk

**Loại mô hình:** B2B2C · **Cập nhật:** 09/10/2026 · **Đinh Xuân Quyền – 2A202602358**  
**NORTH STAR:** Số lượt check-in tự động hoàn toàn (*Auto-PASS Check-ins*) mỗi tháng — Hiện tại: **0** — Mục tiêu 90 ngày: **1,000 lượt/tháng**

---

### Đèn báo sớm (Leading — nhìn hằng ngày/tuần)

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn | Báo trước cho |
|---|:---:|:---:|:---:|---|
| **Partner Activation Rate** ⭐ | 0% | 🟢 $\ge 60\%$ · 🟡 $30–60\%$ · 🔴 $< 30\%$ | [TB] | End-user reach & Volume hồ sơ $\to$ Doanh thu Hybrid |
| **Straight-Through First-Pass** | Chưa đo | 🟢 $\ge 85\%$ · 🟡 $70–85\%$ · 🔴 $< 70\%$ | [MH] 1 | Tỉ lệ chuyển lễ tân $\to$ Chi phí AI retry |
| **End-user Adoption Rate** | Chưa đo | 🟢 $\ge 40\%$ · 🟡 $20–40\%$ · 🔴 $< 20\%$ | [TB] | Dung lượng hồ sơ $\to$ Doanh thu gói Hybrid |

---

### Đèn vận hành (Operating — nhìn hằng tuần/tháng)

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn | Báo trước cho |
|---|:---:|:---:|:---:|---|
| **HITL Escalation Rate** | Chưa đo | 🟢 $< 15\%$ · 🟡 $15–30\%$ · 🔴 $> 30\%$ | [MH] 2 | Trải nghiệm sảnh $\to$ Nguy cơ Hotel Churn |
| **Chi phí inference ÷ Doanh thu/hotel** *(AI)* | 31.6% | 🟢 $< 25\%$ · 🟡 $25–38\%$ · 🔴 $> 38\%$ | [MH] 3 | Gross Margin riêng của từng khách sạn |

---

### Đèn kết quả (Lagging — nhìn hằng quý)

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn |
|---|:---:|:---:|:---:|
| **Gross Margin thực tế** | 68.39% (mô hình) | 🟢 $\ge 60\%$ · 🟡 $50–60\%$ · 🔴 $< 50\%$ | [BM] (ICONIQ 08/10/2026) |
| **CAC Payback thực tế** | 2.89 tháng (mô hình) | 🟢 $< 6$ tháng · 🟡 $6–12$ tháng · 🔴 $> 12$ tháng | [BM] (Bessemer 08/10/2026) |

---

### 5 luật quyết định (⏹ = luật dừng)

1. ⏹ **NẾU** Partner Activation Rate $< 30\%$ **TRONG** 30 ngày từ khi bàn giao Kiosk **VÀ** có $\ge 3$ khách sạn go-live **THÌ** dừng toàn bộ việc bán mới, phụ trách trực tiếp đến sảnh đào tạo lễ tân và cắm biển chỉ dẫn trong 2 tuần **KHÔNG THÌ** không được dùng số khách sạn đã ký để báo cáo tăng trưởng.
2. ⏹ **NẾU** HITL Escalation Rate $> 30\%$ **TRÊN** 100 phiên check-in gần nhất tại một hotel **VÀ** mỗi phiên đủ ảnh CCCD/Live Face **THÌ** đóng băng luồng khách quốc tế/hộ chiếu, chỉ phục vụ CCCD gắn chip và thêm bước tiền kiểm nét ảnh **KHÔNG THÌ** không bắt lễ tân hỗ trợ thủ công từng ca mà không thu hẹp phạm vi.
3. **NẾU** Chi phí inference ÷ Doanh thu $> 38\%$ tại một hotel **TRONG** 2 tuần **VÀ** phát sinh $\ge 100$ lượt/tuần **THÌ** khóa cứng số lần chụp lại tối đa 2 lần/khách và kích hoạt phụ thu $0.05/lần retry vượt định mức ở kỳ sau **KHÔNG THÌ** không được tự chịu bù chi phí API làm âm biên lợi nhuận.
4. **NẾU** Straight-Through First-Pass $< 70\%$ **TRONG** 1 tuần **VÀ** có $\ge 50$ phiên chụp tại sảnh **THÌ** lắp thêm đèn LED trợ sáng tại đỉnh Kiosk và cập nhật khung căn mặt oval trên giao diện Web trong 3 ngày **KHÔNG THÌ** không vội đổi model vendor AI đắt hơn khi chưa xử lý môi trường sáng tại sảnh.
5. ⏹ **NẾU** End-user Adoption Rate $< 20\%$ **SAU** 30 ngày từ go-live **VÀ** hotel có $\ge 200$ booking/tháng trên PMS **THÌ** dừng hỗ trợ kỹ thuật miễn phí tại chỗ, gửi cảnh báo cho Chủ khách sạn và kích hoạt thu hồi Kiosk sau 14 ngày **KHÔNG THÌ** không giảm giá phí nền $30 hay nhượng bộ thêm tính năng tùy biến riêng.

---

### Cổng gác 90 ngày

| Ngày | Metric (đúng 1) | Ngưỡng qua cổng | Bằng chứng vật lý | Nếu trượt |
|:---:|---|---|---|:---:|
| **30** | **Eval eKYC & Ký thỏa thuận Pilot v1** *(Cổng học)* | Đạt $\ge 300$ ca eval (containment $\ge 85\%$) VÀ có $\ge 1$ hotel ký MoU pilot tích hợp PMS | Báo cáo `Eval_Report_300_cases.pdf` + Hợp đồng MoU pilot có chữ ký | **FIX** (ở lại 30 ngày sửa module camera/liveness) |
| **60** | **Lượt check-in tự động tại Hotel Pilot** | Đạt $\ge 200$ lượt Auto-PASS thật VÀ Escalation $\le 15\%$, chi phí API $\le \$0.06$/hồ sơ | File xuất dữ liệu `Telemetry_Pilot_200sessions.csv` đối soát PMS | **PIVOT** (chuyển sang Mobile Web Check-in qua QR/Zalo) |
| **90** | **Khách sạn trả phí lũy kế & Gross Margin** | Có $\ge 5$ khách sạn trả phí gói Hybrid ($162/tháng) VÀ Gross Margin thực tế $\ge 60\%$ | 5 Hợp đồng dịch vụ SaaS ký số + Báo cáo P&L dòng tiền tháng 3 | **KILL** (thanh lý Kiosk, dừng dự án) |

**KILL CRITERIA:**  
Dừng toàn bộ dự án Kiosk và thanh lý thiết bị trước ngày **08/01/2027** nếu sau khi đã FIX một lần mà không có tối thiểu **3 khách sạn trả phí** hoặc **Gross Margin thực tế duy trì dưới 50%** trong 2 tháng liên tiếp.

**CHƯA ĐO ĐƯỢC:**  
1. **Volume volatility:** Cần tối thiểu 6 tháng dữ liệu vận hành qua 2 mùa du lịch để đo độ lệch chuẩn; dự kiến có số ngày **08/04/2027**.  
2. **Chi phí inference theo từng hotel:** Cần bổ sung trường `hotel_id` vào request log gọi vendor eKYC; dự kiến hoàn thành tracking ngày **22/10/2026**.  
3. **End-user adoption rate:** Cần hoàn thành API tích hợp webhook PMS để lấy tổng số booking check-in trong ngày; dự kiến có số ngày **29/10/2026**.
