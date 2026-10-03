# BÁO CÁO NGÀY 4_10
## Thông tin cá nhân

* Họ và tên: Nguyễn Thị Kiều Trang
* Lớp: 74DCTT28
* Branch Git: `trang`
* Dự án: [QuanLyNhaTro](https://github.com/Trang-thn/QuanLyNhaTro)

#  HỆ THỐNG WEB QUẢN LÝ PHÒNG TRỌ (BOARDING HOUSE MANAGEMENT SYSTEM)

# I. TỔNG THỂ DỰ ÁN

##  1. TỔNG QUAN DỰ ÁN

Hệ thống Web Quản lý Phòng trọ nhằm tự động hóa quy trình quản lý dãy trọ và chung cư mini:
- **Đối với Chủ trọ (Admin):** Tối ưu hóa việc quản lý danh sách phòng, loại phòng, lập hợp đồng thuê, tính tiền điện nước, sinh hóa đơn tự động và theo dõi công nợ, doanh thu qua bảng thống kê.
- **Đối với Người thuê (Tenant):** Tra cứu chi phí hàng tháng, xem lịch sử thanh toán, gửi yêu cầu sửa chữa sự cố và nhận thông báo nhanh chóng từ chủ trọ.

## 2. CÔNG NGHỆ SỬ DỤNG (TECH STACK)

- **Frontend:** ReactJS + TypeScript + Tailwind CSS
- **Backend:** Node.js (Express Framework)
- **Cơ sở dữ liệu:** MySQL 8.0+
- **Công cụ quản lý & Phát triển:** Git, GitHub, Postman, Figma, Trello/Jira

## 3. PHÂN CÔNG NHIỆM VỤ DỰ ÁN

| Thành viên | Vai trò / Branch | Trách nhiệm chính |
| :--- | :--- | :--- |
| **Trang** | **Leader**<br>`trang/auth-dashboard` | Khởi tạo khung dự án, Module Tài khoản & Phân quyền (`users`), Dashboard & Báo cáo thống kê, quản lý Merge code. |
| **Bình** | **Dev**<br>`binh/room-management` | Module Quản lý Phòng (`rooms`, `room_types`), Tiện nghi (`amenities`, `room_amenities`), Sự cố (`maintenance_requests`) & Thông báo (`notifications`). |
| **Ngọc** | **Dev**<br>`ngoc/tenant-contract` | Module Hồ sơ người thuê (`tenants`), Hợp đồng thuê trọ (`contracts`), Danh sách người ở cùng (`contract_members`), Lịch sử thuê. |
| **Linh** | **Dev**<br>`linh/billing-payment` | Module Điện nước (`utility_readings`), Hóa đơn hàng tháng (`invoices`), Lịch sử thanh toán nhiều đợt (`payments`) & Quản lý công nợ. |

## 4. CHI TIẾT CÔNG VIỆC CỦA TRANG

### 4.1. Quản lý Dự án & Kỹ thuật
- **Khởi tạo:** Thiết lập Repository Git, tạo cấu trúc thư mục Frontend & Backend, phân quyền cho thành viên.
- **Cơ sở dữ liệu:** Tạo và tối ưu hoá Database MySQL.
- **Git Flow:** Review Pull Request, quản lý merge code từ các branch thành viên vào `develop` và `main`.

### 4.2. Các Module Chức năng & Use Case Đảm nhận

#### 1. Module Tài khoản & Phân quyền (`users`)
* **UC01 - Đăng ký tài khoản Khách thuê (Tự động liên kết CCCD):**
  - Cho phép khách thuê tự đăng ký tài khoản.
  - **Logic tự động:** Đối soát số CCCD (`identity_card_number`) nhập vào với bảng `tenants`. Nếu tìm thấy, tự động gán `tenants.user_id = users.id` để liên kết người dùng với phòng trọ và hợp đồng.
* **UC02 - Đăng nhập hệ thống:**
  - Xác thực thông tin qua `username` và `password` (mã hóa mã hash với Bcrypt).
  - Cấp **JWT Token** chứa thông tin người dùng (`id`, `username`, `role`).
  - Phân quyền điều hướng: `ADMIN` vào Dashboard Quản trị, `TENANT` vào Trang Khách thuê.
* **UC02-EXT - Quên / Đặt lại mật khẩu qua Email OTP:**
  - Gửi mã OTP 6 số qua Email (sử dụng Nodemailer/SMTP, hết hạn trong 5 phút).
  - Cho phép người dùng đặt lại mật khẩu mới sau khi xác minh OTP thành công.

####  2. Module Xem Thông tin Cá nhân & Phòng (Tenant)
* **UC03 - Xem thông tin cá nhân & Phòng:**
  - Cung cấp giao diện tra cứu hồ sơ cá nhân (Họ tên, SĐT, Email, CCCD) và thông tin chi tiết về phòng trọ đang thuê (Số phòng, Tầng, Loại phòng, Giá thuê).

####  3. Module Báo cáo & Dashboard (Admin)
* **UC04 - Xem Báo cáo & Dashboard:**
  - Tổng hợp dữ liệu hiển thị thẻ chỉ số: Tổng số phòng, phòng đang thuê, phòng trống, tổng công nợ chưa thu.
  - Vẽ biểu đồ doanh thu thực thu theo tháng dựa trên dữ liệu bảng `payments`.
  - Vẽ biểu đồ tỷ lệ lấp đầy phòng dựa trên trạng thái bảng `rooms` (`TRONG`, `DANG_THUE`, `BAO_TRI`).

### 4.3. Danh sách RESTful API Trang thực hiện

| HTTP Method | API Endpoint | Mô tả |
| :--- | :--- | :--- |
| `POST` | `/api/v1/auth/register` | Đăng ký tài khoản người dùng mới (Tự động liên kết CCCD) |
| `POST` | `/api/v1/auth/login` | Đăng nhập hệ thống, trả về JWT Token |
| `POST` | `/api/v1/auth/forgot-password` | Yêu cầu gửi mã xác thực OTP qua Email |
| `POST` | `/api/v1/auth/reset-password` | Xác thực mã OTP và đặt lại mật khẩu mới |
| `GET` | `/api/v1/users/me` | Lấy thông tin tài khoản cá nhân & chi tiết phòng đang ở |
| `GET` | `/api/v1/users` | Xem danh sách toàn bộ tài khoản (Admin) |
| `GET` | `/api/v1/dashboard/overview` | Lấy dữ liệu thống kê tổng quan, biểu đồ doanh thu và tỷ lệ lấp đầy |

### 4.4. Lộ trình Thực hiện của Trang

- Tạo repository, phân quyền Git, khởi tạo CSDL MySQL với 11 bảng chuẩn DDL. Thiết kế Wireframe UI trên Figma cùng nhóm.
- Phát triển API & UI cho Đăng ký, Đăng nhập (JWT), Quên mật khẩu OTP, Phân quyền Admin/Tenant.
- Phát triển API & UI Báo cáo Dashboard (Biểu đồ doanh thu, tỷ lệ lấp đầy, dư nợ).
-  Merge code vào branch `develop` và `main`, kiểm thử tích hợp (Integration Test), fix bug.

------

# II. ĐÃ LÀM VÀ CHUẨN BỊ LÀM

## 1. KẾT QUẢ ĐÃ HOÀN THÀNH

### Khởi tạo Cấu trúc & Chuẩn bị Context
- Đã hoàn thiện cấu trúc thư mục Monorepo chuẩn mực với `backend/`, `frontend/` và `doc/`.
- Đã tập trung toàn bộ tài liệu thiết kế hệ thống vào thư mục `doc/`:
  - `doc/schema.sql`: Khởi tạo CSDL MySQL với 11 bảng chuẩn ràng buộc khóa ngoại & UUID.
  - `doc/plantUML_txt`: Sơ đồ Use Case và biểu đồ luồng nghiệp vụ.
  - `doc/UI_trang.md`: Thiết kế giao diện và luồng người dùng cho Leader (Trang).
  - `doc/README.md`: Mô tả yêu cầu bài toán & danh sách Use Case.

### Thiết lập Quy chuẩn cho AI Agent (`.codyrules`)
- Đã khởi tạo file luật `.codyrules` tại thư mục gốc để Cody AI tuân thủ:
  - Quy chuẩn đặt tên (Database: `snake_case`, Backend: `camelCase`, Frontend Component: `PascalCase`).
  - Định dạng chuẩn cho API Response (`success`, `message`, `data`).
  - Quy định mã lỗi HTTP (200, 201, 400, 401, 403, 500) và ràng buộc bảo mật (bắt buộc Bcrypt mã hóa password, Token JWT).

### Thiết lập Quy trình Git & Phân công Nhánh
- Đã khởi tạo repository và phân chia nhánh làm việc cho từng thành viên:
  - Nhánh `trang`: Module Auth (UC01, UC02, UC02-EXT), Cá nhân (UC03) & Dashboard (UC04).
  - Nhánh `develop` & `main`: Nhánh gộp chung do Leader quản lý.
- Đã thiết lập quy trình merge code từ nhánh cá nhân sang `develop` và push an toàn lên Server (`origin`).

### Tạo UI bằng Figma
- Tạo giao diện đăng nhập, phân quyền, dashboard.....

## 2. CHUẨN BỊ LÀM
- **Giai đoạn tiếp theo
  - Tiến hành ra lệnh (Prompt) cho Cody AI sinh mã nguồn backend cho Module Auth của Trang:
    1. Cấu hình kết nối MySQL (`backend/src/config/db.ts`).
    2. Định nghĩa DTOs & TypeScript Types (`backend/src/types/auth.ts`).
    3. Viết API UC01: Đăng ký tự động liên kết CCCD (`identity_card_number`).
    4. Viết API UC02 & UC02-EXT: Đăng nhập JWT & Quên mật khẩu OTP.