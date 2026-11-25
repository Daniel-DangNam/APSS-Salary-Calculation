# APSS Salary Calculation — Yêu Cầu & Quy Tắc Nghiệp Vụ

**Nguồn:** Các buổi trao đổi nghiệp vụ với bộ phận Tài chính – Kế toán & xác nhận bằng văn bản  
**Trạng thái:** Đã chốt các quy tắc nghiệp vụ cốt lõi; sẵn sàng cho bước thiết kế kỹ thuật và triển khai.

---

## 1. Kỳ Lương & Ngày Công Chuẩn

- **Kỳ chốt công (Cut-off Period):** **Từ ngày 26 tháng trước đến ngày 25 tháng hiện tại**  
  *(Ví dụ: Kỳ lương tháng 11 tính từ 26/10 đến 25/11)*.
- **Số ngày làm việc chuẩn:** Áp dụng chung cho toàn bộ nhân viên — tổng số ngày trong kỳ chốt công **không tính Thứ 7 và Chủ nhật**.

---

## 2. Phân Loại Nhân Sự & Công Thức Tính Lương

### 2.1. Nhân viên Singapore (SG) vs. Nhân viên Contracting / Thuê ngoài
- **Nhân viên Singapore (SG):** Tính lương theo **lương cơ bản hàng tháng** (không tính theo giờ).
  - **CPF:** Do Ban quản lý xử lý riêng (file CPF được cung cấp vào cuối mỗi tháng; nằm ngoài phạm vi tính toán của hệ thống).
- **Nhân viên Contracting (và Intern):** Tính theo **số giờ / ngày công thực tế**:
  - `Đơn giá giờ (Hourly rate) = Lương cơ bản / (Số ngày làm việc chuẩn × 8)`  
    *(Ví dụ: `5,000,000 / (22 × 8) = 28,409.09`)*
  - Quy đổi giờ công theo ngày:
    - **Ngày làm việc bình thường:** `8 giờ` (`100%`)
    - **Nghỉ phép năm đã duyệt (Approved Annual Leave):** Đã nằm trong lương cơ bản hàng tháng — **không bị trừ lương** (tính đủ `8 giờ`).
    - **Nghỉ không lương nửa ngày:** `4 giờ` (`50%`)
    - **Nghỉ không lương cả ngày / Vắng mặt:** `0 giờ` (`0%`)
    - **Ngày lễ (Public Holiday) — không đi làm:** `8 giờ` (`100%`)
    - **Ngày lễ (Public Holiday) — có đi làm:** `16 giờ` (`200%`)
- **Lịch ngày lễ (Public Holiday Calendars):** Cấu hình **theo lịch quốc gia của từng nhân viên** (Việt Nam, Singapore, Indonesia, Thái Lan, Malaysia, Myanmar...).

### 2.2. Quản Lý Nghỉ Phép & Luồng Duyệt
- **Gửi đơn nghỉ phép:** Nhân viên gửi đơn trực tiếp trên hệ thống (thay thế quy trình gửi email).
- **Phê duyệt & Thông báo:**
  - **Team Leader** phụ trách và **Management Approver** nhận thông báo để xem xét và duyệt/từ chối.
  - Riêng đối với **Procurement Team**, cần có thêm **Procurement Approver** trong luồng thông báo/duyệt đơn.
  - **HR** nhận thông báo toàn bộ đơn xin nghỉ và theo dõi được trạng thái duyệt của tất cả nhân viên.
- **Đồng bộ & Kiểm tra:**
  - Đơn đã duyệt tự động cập nhật vào bảng chấm công và tính lương.
  - Hệ thống tạo **bảng tổng hợp chấm công hàng tháng** cho HR kiểm tra (`working days`, `approved leave`, `unpaid leave`, `absences`) trước khi chốt lương.

---

## 3. Các Thành Phần Thu Nhập

- **Lương cơ bản (Basic Salary):** Lưu theo đơn vị tiền tệ gốc của nhân viên (`VND`, `SGD`, `IDR`, `THB`, `MYR`...), cập nhật khi có điều chỉnh tăng lương.
- **Hoa hồng (Commission):** Dữ liệu trích xuất từ **Business Central (BC)**, tính toán thủ công hàng tháng rồi nhập/import vào hệ thống.
- **Thưởng dự án cho Intern (Intern Project Reward):** Khoản thanh toán bổ sung cho thực tập sinh, theo dõi tách biệt khỏi lương cơ bản.

---

## 4. Quy Đổi Tỷ Giá & Kênh Thanh Toán

- **Tiền tệ gốc:** Giao dịch được ghi nhận bằng tiền tệ gốc trong BC.
- **Quy đổi chi phí sang USD:**
  - Chi phí được quy đổi sang **USD** để tổng hợp theo từng phòng ban.
  - **Tỷ giá USD từ BC** được **nhập tay** vào hệ thống tại ngày **25** hàng tháng (ngày cut-off).
- **Ghi nhận thanh toán bằng SGD:**
  - Các khoản thanh toán được ghi nhận bằng **SGD** qua 2 kênh:
    - **Instarem:** Dùng cho phần lớn nhân sự (quy đổi sang SGD, miễn phí chuyển khoản). **Tỷ giá SGD của Instarem** được **nhập tay sau khi đã tạo lệnh thanh toán trên Instarem**.
    - **OCBC Singapore:** Dùng để chuyển trực tiếp cho **nhân viên Singapore** và **nhân viên Myanmar** (do Instarem chưa hỗ trợ chuyển sang Myanmar).
- **Định dạng file ngân hàng:**
  - Xuất **bảng tổng hợp Excel** để kiểm tra và đối chiếu thanh toán là đủ cho Release 1 (có thể bổ sung template upload lên ngân hàng ở giai đoạn sau nếu cần).

---

## 5. Mã Tài Khoản Kế Toán (G/L Codes) & Đầu Ra Cho BC

Hệ thống phải xuất **bảng tổng hợp lương phân bổ theo mã G/L, phòng ban và số tiền** phục vụ hạch toán vào **Business Central (BC)** và đối chiếu *(cần Finance xác nhận chính thức trước khi chạy production)*:

| Mã G/L | Phòng ban / Hạng mục |
|---|---|
| `7200` | SCC (Commission) |
| `7200` | New customer |
| `7200` | e-Store |
| `8040` | Procurement |
| `8040` | Logistic |
| `8040` | BD and Marketing |
| `8040` | Account |
| `8040` | HR |
| `8040` | IT |
| `9440` | PASTOR CHE KO |

---

## 6. Vai Trò & Phân Quyền Hệ Thống

| Vai trò | Quyền hạn / Trách nhiệm |
|---|---|
| `Staff / Intern` | Gửi đơn xin nghỉ phép, xem trạng thái đơn, ngày công và thông tin lương của cá nhân (không xem được lương người khác) |
| `Approver` | Xem xét, duyệt hoặc từ chối đơn xin nghỉ phép của nhóm/phòng ban được phân công |
| `Payroll Viewer` | Quyền chỉ xem (read-only) bảng tổng hợp lương *(cấp kèm quyền `Approver` cho các quản lý được chỉ định ở Management / Procurement)* |
| `HR` | Theo dõi toàn bộ đơn nghỉ phép/trạng thái, kiểm tra bảng tổng hợp chấm công hàng tháng trước khi chốt lương |
| `Accountant` | Quản lý tỷ giá (BC ngày 25 USD, Instarem SGD), nhập commission/thưởng, chạy tính lương, xuất bảng thanh toán & bảng hạch toán G/L |
| `Admin` | Quản lý người dùng, phân quyền, phòng ban, mã G/L, lịch ngày lễ theo quốc gia và cấu hình hệ thống |
