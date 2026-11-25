# Theo Dõi Tiến Độ Dự Án Theo JIRA

**Atlassian Project:** `APSST-16` — Salary Calculation  
**Jira Epic:** `KAN-101` — Salary Calculation and Controlled G/L Posting  
**Nguồn trao đổi:**
- Trao đổi nghiệp vụ Tài chính (Commission & Kế toán) — 20/11/2026
- Buổi hướng dẫn quy trình giữa Kế toán & Kỹ thuật — 25/11/2026
- Phản hồi xác nhận bằng văn bản từ Kế toán — 25/11/2026

---

## 1. Mục Tiêu Epic & Bối Cảnh Kiến Trúc (`KAN-101`)

**Mục tiêu:** Xây dựng quy trình tính lương hàng tháng có kiểm soát và bảo mật trong **Business Central (BC)**, bao gồm kiểm tra dữ liệu đầu vào, tính toán, rà soát, tổng hợp thanh toán và hạch toán G/L có kiểm soát — giúp giảm kiểm tra thủ công và tạo bút toán có thể kiểm toán (auditable), không cho phép hạch toán âm thầm.

### Hai Luồng Nghiệp Vụ Tách Biệt
1. **Luồng nhân viên Singapore (SG):**
   - Hiện đang sử dụng chức năng payroll có sẵn trong BC (chọn kỳ, tính và post G/L; kỳ lương đôi khi không tự động chuyển và cần chỉnh tay).
   - Tính theo lương cơ bản hàng tháng; CPF do Ban quản lý xử lý riêng (file CPF cung cấp cuối tháng).
2. **Luồng nhân viên Overseas / Contract-Service:**
   - Hiện đang hạch toán thủ công, gom nhóm theo phòng ban và quy đổi sang đơn vị tiền tệ kế toán yêu cầu.
   - Tính theo giờ / ngày công thực tế (`Lương cơ bản / (Ngày công chuẩn × 8)`).
- **Yêu cầu bắt buộc:** Hai luồng này phải được **phân biệt rõ ràng** trong hệ thống.

### Các Kiểm Soát Bắt Buộc & Giới Hạn Release 1
- **Phân quyền truy cập:** Chi tiết lương chỉ giới hạn cho Payroll, Kế toán và Quản lý được cấp quyền (`Payroll Viewer` cho các quản lý được chỉ định ở Management / Procurement). Nhân viên khi nộp thông tin **không được thấy lương của nhân viên khác**.
- **Chốt chặn kiểm tra (Validation Gate):** Thiếu chấm công, tỷ giá, phòng ban hoặc mã G/L **phải chặn không cho chốt batch**.
- **Phê duyệt trước khi hạch toán:** Yêu cầu Kế toán có thẩm quyền kiểm tra và phê duyệt trước khi tạo hoặc post bút toán (journal).
- **Lưu vết kiểm toán (Audit Trail):** Lưu đầy đủ dữ liệu đầu vào, lịch sử duyệt, bút toán sinh ra và lịch sử thay đổi.
- **Hệ thống kế toán gốc:** Business Central (BC) vẫn là hệ thống ghi nhận kế toán chính thức.
- **Ngoài phạm vi Release 1:**
  - Tự động đẩy lệnh thanh toán lên ngân hàng (bảng tổng hợp Excel là đủ cho R1).
  - Tự động post G/L khi chưa có Kế toán rà soát.
  - Lưu trữ dữ liệu lương ở nơi có quyền truy cập rộng rãi.

---

## 2. Danh Sách Task JIRA (`KAN-102` – `KAN-109`)

| Mã | Công việc | Độ ưu tiên | Trạng thái | Ghi chú / Đối chiếu nghiệp vụ |
|---|---|---|---|---|
| **KAN-102** | Confirm payroll rules, employee scope and security access | Medium | `To Do` *(Đang thực hiện)* | **Mô tả:** Chốt nhóm nhân viên, kỳ cut-off, quy tắc ngày công, xử lý nghỉ phép/nửa ngày, hoa hồng, nguồn tỷ giá, làm tròn, kênh thanh toán, tài khoản G/L, người duyệt và phân quyền truy cập dữ liệu lương.<br>**Ghi chú:** Kế toán đã xác nhận phần lớn quy tắc (xem `docs/requirements.vi.md`). Chỉ còn quy tắc làm tròn, Finance duyệt chính thức mã G/L và cơ chế post BC ở R1. |
| **KAN-103** | Design monthly salary input and payroll data model | Medium | `To Do` | **Mô tả:** Thiết kế cấu hình lương, bảng header theo tháng & dòng lương nhân viên, dữ liệu chấm công, phân bổ phòng ban, hoa hồng, tiền tệ, tỷ giá, kênh thanh toán, ánh xạ G/L, trạng thái và trường audit. Đảm bảo giới hạn quyền truy cập dữ liệu lương. |
| **KAN-104** | Build attendance, leave and working-day calculation | Medium | `To Do` | Bao gồm gửi đơn nghỉ phép, thông báo & duyệt bởi Team Leader + Management Approver (+ Procurement Approver cho nhóm Procurement), theo dõi của HR, lịch ngày lễ theo từng quốc gia (8h/16h) và bảng tổng hợp chấm công tháng. |
| **KAN-105** | Build posting preview, validation and approval flow | Medium | `To Do` | Kiểm tra hợp lệ batch (chặn khi thiếu dữ liệu), xem trước (preview) bút toán dự kiến và luồng Kế toán rà soát/duyệt. |
| **KAN-106** | Test reconciliation, permissions, audit trail and month-end process | Medium | `To Do` | Kiểm thử đối chiếu thanh toán SGD vs chi phí USD, phân quyền (`Payroll Viewer`, `Approver`, `HR`, `Accountant`, `Admin`), audit log và quy trình khóa sổ cuối tháng. |
| **KAN-107** | Configure department and G/L account mappings | Medium | `To Do` | Cấu hình ánh xạ phòng ban với mã G/L đã được Finance xác nhận (`7200`, `8040`, `9440`...). |
| **KAN-108** | Build posting preview, validation and approval flow | Medium | `To Do` | *(Lưu ý: Tiêu đề bị trùng với KAN-105 trên Jira — cần kiểm tra để gộp hoặc tách rõ luồng SG và Overseas).* |
| **KAN-109** | Create controlled G/L journal and posting integration in BC | Medium | `To Do` | Tích hợp batch lương đã duyệt vào G/L Journal của Business Central (tạo/post có kiểm soát). |

---

## 3. Trạng Thái Các Điểm Cần Chốt (JIRA vs. Phản Hồi Kế Toán)

| Điểm cần chốt (từ JIRA) | Trạng thái | Kết quả / Việc cần làm tiếp |
|---|---|---|
| Công thức lương & mẫu số chia | **Cơ bản đã chốt** | SG = lương cơ bản tháng; Contracting = `Lương cơ bản / (Ngày công chuẩn × 8)`. **Cần chốt thêm:** quy tắc làm tròn theo từng loại tiền tệ. |
| Kỳ chốt công, cuối tuần & ngày lễ | **Đã chốt** | Cut-off: 26 tháng trước – 25 tháng này; trừ Thứ 7 & CN; Ngày lễ theo lịch quốc gia của từng nhân viên (8h nghỉ / 16h đi làm). |
| Nghỉ có lương, không lương, tăng ca & nửa ngày | **Đã chốt** | Phép năm đã duyệt = có lương (không trừ); nghỉ không lương = 0h cả ngày / 4h nửa ngày. *(Xác nhận thêm nếu có OT ngày thường ngoài ngày lễ).* |
| Cách tính & hạch toán Commission | **Đã chốt** | Trích xuất từ BC & tính thủ công hàng tháng (nhập tay/import); Intern có thêm Project Reward riêng; G/L `7200` (SCC). |
| Nguồn tỷ giá, ngày lấy & làm tròn | **Cơ bản đã chốt** | USD lấy từ BC tại ngày 25 (nhập tay); SGD lấy từ Instarem sau khi tạo lệnh thanh toán (nhập tay). **Cần chốt thêm:** độ chính xác làm tròn tỷ giá. |
| Ánh xạ phòng ban & mã G/L cuối cùng | **Chờ Finance ký duyệt** | Đề xuất từ Kế toán: `7200` (SCC, New customer, e-Store), `8040` (Procurement, Logistic, BD & Marketing, Account, HR, IT), `9440` (PASTOR CHE KO). *Finance cần xác nhận chính thức trước khi chạy production.* |
| Vai trò duyệt & R1 chỉ tạo journal hay cho phép post có kiểm soát | **Chốt một phần** | Đã chốt vai trò duyệt phép & `Payroll Viewer`. **Cần chốt thêm:** R1 chỉ tạo Unposted G/L Journal trong BC hay hỗ trợ nút Controlled Posting? |
| Định dạng file ngân hàng / tích hợp thanh toán | **Đã chốt** | Không tự động đẩy lệnh ngân hàng ở R1; bảng tổng hợp Excel để kiểm tra/rà soát thanh toán là đủ. |
| **Câu hỏi kiến trúc (Mới)** | **Cần thống nhất nội bộ** | Vì nhân viên/intern cần tự gửi đơn nghỉ phép mà không thấy lương người khác và không cần cấp license BC cho tất cả, cần chốt R1 dùng **Web Portal riêng + tích hợp BC** hay **chỉ BC + form ngoài**. |
