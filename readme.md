# Tài liệu Backend Export Management System

## Tổng quan hệ thống

Hệ thống Backend Export Management System được phát triển để quản lý quy trình xuất khẩu, từ quản lý sản phẩm, container đến đơn hàng và chi phí. Hệ thống cung cấp API RESTful cho phép frontend tương tác và thực hiện các chức năng quản lý.

## Cài đặt và thiết lập môi trường

```bash
# Tạo môi trường ảo
python -m venv venv

# Kích hoạt môi trường ảo
# Windows
venv\Scripts\activate
# Linux/Mac
source venv/bin/activate

# Cài đặt thư viện
pip install -r requirements.txt

# Thiết lập biến môi trường
# Windows
set FLASK_APP=app.py
set FLASK_ENV=development
set DATABASE_URL=postgresql://postgres:1221@localhost:5432/import_export_db
# Linux/Mac
export FLASK_APP=app.py
export FLASK_ENV=development
export DATABASE_URL=postgresql://postgres:1221@localhost:5432/import_export_db

# Khởi tạo cơ sở dữ liệu
flask db init
flask db migrate
flask db upgrade

# Chạy ứng dụng
flask run
```

## 1. Models

- **Ngôn ngữ và Framework:** Python 3.x, Flask
- **Cơ sở dữ liệu:** PostgreSQL
- __Quản lý môi trường:__ Sử dụng virtual environment (venv), TEST_DATABASE_URL=postgresql://postgres:1221@localhost:5432/import_export_test_db
- Tạo scripts/seed_data.py để tạo dữ liệu mẫu cho TEST_DATABASE_URL sau khi tạo models và routes, utils
- Phân quyền truy cập dựa trên cấp độ người dùng (ví dụ: chỉ Admin mới có thể thêm người dùng).
- Bảo mật mật khẩu (sử dụng hashing).
- Tính toán chi phí liên quan đến đơn hàng (dựa trên sản phẩm, số lượng, chi phí vận chuyển, thuế, tỷ giá hối đoái). Mỗi container sẽ chứa nhiều sản phẩm khác nhau, mỗi sản phẩm sẽ có chiếm tỉ trọng khác nhau trên container. Tính giá tiền xuất khẩu mỗi sản phẩm dựa trên chi phí container, giá nhập, thuế và phần trăm lợi nhuận.
- Công thức tính chi phí cho mỗi sản phẩm trong container: tính theo khối lượng hoặc tính theo thể tích

   + (Chi phí vận chuyển / Tổng khối lượng container) * Khối lượng sản phẩm + Giá nhập + Thuế + (Giá nhập * Phần trăm lợi nhuận).
   + (Chi phí vận chuyển / Thể tích container) * Thể tích sản phẩm + Giá nhập + Thuế + (Giá nhập * Phần trăm lợi nhuận).

- Tính khối lượng hàng tồn kho của sản phẩm ở mỗi đơn hàng mua và bán
- Xác thực dữ liệu đầu vào (số lượng, thông tin sản phẩm).
- Cập nhật trạng thái đơn hàng tự động hoặc thủ công.
- Tạo và quản lý vận đơn.
- Xác thực dữ liệu đầu vào ở tất cả các chức năng.
- Số lượng sản phẩm phải là số nguyên dương.
- Email phải đúng định dạng (ví dụ: example@domain.com).
- Mã container, đơn hàng phải là duy nhất.
- Ngày tạo đơn hàng không được là ngày trong tương lai.
- Trạng thái đơn hàng phải thuộc một trong các giá trị cho phép (đang tạo, đã xác nhận, đang vận chuyển, đã hoàn thành, đã hủy).

### Cấu trúc quan hệ giữa các Models

```ini
User ─┬─ Order ──── OrderItem ─── Product ─── Category
      │     │              │           │
      └─ Cost              └───────────┴─── Tag
          │
          │             Partner ─── Order
          │                │
     Container ─┬─ Order   └─── Tag
                │
                └─ Cost ─── Schedule
```

- User có thể tạo nhiều Order và Cost
- Partner (khách hàng/nhà cung cấp) liên kết với nhiều Order
- Product thuộc một Category và xuất hiện trong nhiều OrderItem
- OrderItem kết nối Order và Product với quan hệ nhiều-nhiều
- Container liên kết với Order và có các Cost riêng
- Schedule quản lý lịch trình vận chuyển
- Tag được gắn với Product, Order và Partner để phân loại và lọc thông tin

### 1.1. User Model

- **Mô tả**: Quản lý thông tin người dùng
- **Các trường chính**:
   - id: ID người dùng (primary key)
   - username: Tên đăng nhập
   - email: Email
   - password_hash: Mật khẩu đã được mã hóa
   - full_name: Họ tên đầy đủ
   - phone: Số điện thoại
   - position: Vị trí công việc
   - department: Phòng ban
   - role: Vai trò người dùng
   - active: Trạng thái hoạt động
   - two_factor_enabled: Bật/tắt xác thực 2 lớp

### 1.2. Container Model

- **Mô tả**: Quản lý thông tin container
- **Các trường chính**:
   - id: ID container
   - container_number: Số hiệu container
   - container_type: Loại container (STANDARD_20, STANDARD_40, HIGH_CUBE_40, REEFER_20, REEFER_40)
   - status: Trạng thái (EMPTY, LOADING, LOADED, IN_TRANSIT, ARRIVED)
   - location: Vị trí hiện tại
   - capacity: Dung tích (m3)
   - max_weight: Trọng lượng tối đa (kg)
   - current_weight: Trọng lượng hiện tại (kg)
   - length/width/height: Kích thước (m)
   - tare_weight: Trọng lượng bì (kg)
   - daily_cost: Chi phí thuê ngày
   - shipping_cost: Chi phí vận chuyển

### 1.3. Product Model

- **Mô tả**: Quản lý thông tin sản phẩm
- **Các trường chính**:
   - id: ID sản phẩm
   - product_code: Mã sản phẩm
   - name: Tên sản phẩm
   - produc_type: Sản xuất trong nước / nhập khẩu
   - description: Mô tả
   - category_id: ID danh mục
   - unit: Đơn vị tính
   - weight: Khối lượng (kg)
   - volume: Thể tích (m3)
   - price: Giá nhập
   - selling_price: Giá bán
   - tax_rate: Tỷ lệ thuế (%)
   - profit_margin: Tỷ lệ lợi nhuận (%)
   - inventory_level: Số lượng tồn kho
   - min_inventory_level: Ngưỡng tồn kho tối thiểu (để cảnh báo)
   - max_inventory_level: Ngưỡng tồn kho tối đa (để tối ưu chi phí lưu kho)
   - inventory_update_method: Phương thức cập nhật (FIFO, LIFO, Weighted Average)

### 1.4. Order Model

- **Mô tả**: Quản lý đơn hàng
- **Các trường chính**:
   - id: ID đơn hàng
   - order_number: Số đơn hàng, tự động tạo, phân biệt đơn hàng trong nước, xuất khẩu
   - order_type: Loại đơn hàng
   - status: Trạng thái đơn hàng
   - payment_status: Trạng thái thanh toán
   - payment_method: Phương thức thanh toán
   - container_id: ID container (cho đơn xuất)
   - total_amount: Tổng tiền hàng
   - tax_amount: Tiền thuế
   - shipping_cost: Chi phí vận chuyển
   - total_weight: Tổng khối lượng
   - total_volume: Tổng thể tích

### 1.5. Cost Model

- **Mô tả**: Quản lý chi phí
- **Các trường chính**:
   - id: ID chi phí
   - order_id: ID đơn hàng
   - container_id: ID container
   - cost_type: Loại chi phí
   - amount: Số tiền
   - currency: Loại tiền tệ
   - exchange_rate: Tỷ giá
   - description: Mô tả
   - reference_number: Số chứng từ
   - cost_date: Ngày phát sinh

### 1.6. Category Model

- **Mô tả**: Quản lý danh mục sản phẩm
- **Các trường chính**:
   - id: ID danh mục
   - code: Mã danh mục
   - name: Tên danh mục
   - description: Mô tả
   - parent_id: ID danh mục cha
   - active: Trạng thái hoạt động

### 1.7. Partner Model

- **Mô tả**: Quản lý khách hàng và nhà cung cấp
- **Các trường chính**:
   - id: ID
   - code: Mã khách hàng/nhà cung cấp, chia làm 3 loại mã tự động tạo gồm: khách hàng trong nước, khách hàng ngoài nước, nhà cung cấp
   - name: Tên
   - address: Địa chỉ
   - phone: Số điện thoại
   - email: Email
   - tax_code: Mã số thuế
   - contact_person: Người liên hệ
   - contact_phone: SĐT liên hệ
   - bank_account: Số tài khoản
   - bank_name: Tên ngân hàng

### 1.8. Tag Model

- **Mô tả**: Quản lý nhãn phân loại cho sản phẩm, đối tác và đơn hàng
- **Các trường chính**:
   - id: ID nhãn
   - name: Tên nhãn
   - color: Mã màu (hex code)
   - entity_type: Loại đối tượng được gắn (PRODUCT, ORDER, PARTNER)
   - description: Mô tả nhãn
   - is_default: Có phải nhãn mặc định
   - active: Trạng thái hoạt động
   - created_by: ID người tạo
   - created_at: Thời điểm tạo
   - updated_at: Thời điểm cập nhật

### 1.9. TagMapping Model

- **Mô tả**: Quản lý mối quan hệ nhiều-nhiều giữa Tag và các đối tượng
- **Các trường chính**:
   - id: ID mapping
   - tag_id: ID nhãn
   - entity_id: ID đối tượng được gắn nhãn
   - entity_type: Loại đối tượng (PRODUCT, ORDER, PARTNER)
   - created_by: ID người tạo
   - created_at: Thời điểm tạo

### 1.10. OrderItem Model

- **Mô tả**: Quản lý chi tiết sản phẩm trong đơn hàng
- **Các trường chính**:
   - id: ID chi tiết đơn hàng
   - order_id: ID đơn hàng
   - product_id: ID sản phẩm
   - quantity: Số lượng
   - unit_price: Đơn giá (VND hoặc USD tùy theo thị trường)
   - tax_rate: Thuế suất áp dụng
   - discount: Giảm giá
   - total_price: Tổng tiền
   - weight: Khối lượng sản phẩm
   - volume: Thể tích sản phẩm
   - shipping_cost_share: Chi phí vận chuyển được phân bổ
   - currency: Đơn vị tiền tệ (VND, USD, EUR, v.v.)
   - note: Ghi chú

### 1.11. Schedule Model

- **Mô tả**: Quản lý lịch trình vận chuyển
- **Các trường chính**:
   - id: ID lịch trình
   - container_id: ID container
   - departure_port: Cảng khởi hành
   - arrival_port: Cảng đến
   - estimated_departure_date: Ngày dự kiến khởi hành
   - actual_departure_date: Ngày thực tế khởi hành
   - estimated_arrival_date: Ngày dự kiến đến
   - actual_arrival_date: Ngày thực tế đến
   - status: Trạng thái (SCHEDULED, IN_TRANSIT, COMPLETED, DELAYED)
   - vessel_name: Tên tàu
   - voyage_number: Số chuyến
   - carrier: Hãng vận chuyển
   - tracking_number: Số theo dõi
   - notes: Ghi chú

### 1.12. InventoryTransaction Model

- **Mô tả**: Quản lý giao dịch tồn kho (nhập, xuất, điều chỉnh)
- **Các trường chính**:
   - id: ID giao dịch
   - product_id: ID sản phẩm
   - order_id: ID đơn hàng (nullable)
   - transaction_type: Loại giao dịch (PURCHASE, SALE, ADJUSTMENT, RETURN, TRANSFER)
   - quantity: Số lượng (số dương cho nhập, số âm cho xuất)
   - unit_price: Đơn giá tại thời điểm giao dịch
   - previous_inventory: Số lượng tồn trước giao dịch
   - current_inventory: Số lượng tồn sau giao dịch
   - transaction_date: Thời điểm giao dịch
   - notes: Ghi chú
   - warehouse_id: ID kho hàng (nếu quản lý nhiều kho)
   - created_by: Người thực hiện
   - location: Vị trí trong kho (nếu có)
   - batch_number: Số lô hàng
   - expiry_date: Ngày hết hạn (cho sản phẩm có hạn sử dụng)

## 2. Routes

### 2.1. Auth Routes (/api/auth)

- **POST /login**: Đăng nhập với username/email và password
- **POST /logout**: Đăng xuất và vô hiệu hóa token
- **POST /change-password**: Đổi mật khẩu người dùng
- **POST /2fa/enable**: Bật xác thực 2 lớp
- **POST /2fa/verify**: Xác thực mã 2FA
- **POST /forgot-password**: Yêu cầu khôi phục mật khẩu
- **POST /reset-password**: Đặt lại mật khẩu với token

### 2.2. User Routes (/api/users)

- **GET /**: Lấy danh sách người dùng (Admin)
- **GET /{id}**: Lấy thông tin người dùng cụ thể
- **POST /**: Tạo người dùng mới (Admin)
- **PUT /{id}**: Cập nhật thông tin người dùng
- **DELETE /{id}**: Vô hiệu hóa người dùng (Admin)
- **GET /profile**: Lấy thông tin người dùng hiện tại
- **PUT /profile**: Cập nhật thông tin cá nhân
- **PUT /{id}/role**: Thay đổi vai trò người dùng (Admin)

### 2.3. Product Routes (/api/products)

- **GET /**: Lấy danh sách sản phẩm (hỗ trợ phân trang, lọc)
- **GET /{id}**: Lấy thông tin chi tiết sản phẩm
- **POST /**: Tạo sản phẩm mới
- **PUT /{id}**: Cập nhật thông tin sản phẩm
- **DELETE /{id}**: Xóa sản phẩm
- **GET /search**: Tìm kiếm sản phẩm theo từ khóa
- **GET /category/{category_id}**: Lấy sản phẩm theo danh mục
- **GET /{id}/inventory**: Xem lịch sử tồn kho của sản phẩm
- **PUT /{id}/inventory**: Điều chỉnh tồn kho sản phẩm
- **GET /low-stock**: Lấy danh sách sản phẩm tồn kho thấp
- **GET /inventory-alerts**: Lấy các cảnh báo tồn kho (thấp, cao, sắp hết hạn)
- **POST /inventory/adjust**: Điều chỉnh tồn kho thủ công (khi kiểm kê)
- **GET /inventory-transactions**: Lấy lịch sử giao dịch tồn kho
- **POST /inventory/transfer**: Chuyển sản phẩm giữa các kho (nếu quản lý nhiều kho)

### 2.4. Order Routes (/api/orders)

- **GET /**: Lấy danh sách đơn hàng (hỗ trợ phân trang, lọc)
- **GET /{id}**: Lấy thông tin chi tiết đơn hàng
- **POST /**: Tạo đơn hàng mới
- **PUT /{id}**: Cập nhật thông tin đơn hàng
- **DELETE /{id}**: Hủy đơn hàng
- **GET /{id}/items**: Lấy danh sách sản phẩm trong đơn hàng
- **PUT /{id}/status**: Cập nhật trạng thái đơn hàng
- **PUT /{id}/payment**: Cập nhật trạng thái thanh toán
- **GET /search**: Tìm kiếm đơn hàng theo từ khóa
- **GET /by-partner/{partner_id}**: Lấy đơn hàng theo đối tác
- **POST /{id}/duplicate**: Tạo bản sao của đơn hàng

### 2.5. Container Routes (/api/containers)

- **GET /**: Lấy danh sách container (hỗ trợ phân trang, lọc)
- **GET /{id}**: Lấy thông tin chi tiết container
- **POST /**: Tạo container mới
- **PUT /{id}**: Cập nhật thông tin container
- **DELETE /{id}**: Xóa container
- **PUT /{id}/status**: Cập nhật trạng thái container
- **GET /{id}/orders**: Lấy danh sách đơn hàng trong container
- **GET /{id}/costs**: Lấy chi phí liên quan đến container
- **GET /available**: Lấy danh sách container có sẵn
- **POST /{id}/calculate-costs**: Tính toán chi phí cho container
- **GET /search**: Tìm kiếm container theo từ khóa

### 2.6. Cost Routes (/api/costs)

- **GET /**: Lấy danh sách chi phí (hỗ trợ phân trang, lọc)
- **GET /{id}**: Lấy thông tin chi tiết chi phí
- **POST /**: Tạo chi phí mới
- **PUT /{id}**: Cập nhật thông tin chi phí
- **DELETE /{id}**: Xóa chi phí
- **GET /by-order/{order_id}**: Lấy chi phí theo đơn hàng
- **GET /by-container/{container_id}**: Lấy chi phí theo container
- **GET /report/summary**: Báo cáo tổng hợp chi phí
- **GET /report/by-type**: Báo cáo chi phí theo loại
- **GET /report/by-period**: Báo cáo chi phí theo thời gian

### 2.7. Category Routes (/api/categories)

- **GET /**: Lấy danh sách danh mục
- **GET /{id}**: Lấy thông tin chi tiết danh mục
- **POST /**: Tạo danh mục mới
- **PUT /{id}**: Cập nhật thông tin danh mục
- **DELETE /{id}**: Xóa danh mục
- **GET /tree**: Lấy cấu trúc cây danh mục
- **GET /{id}/products**: Lấy sản phẩm thuộc danh mục
- **GET /search**: Tìm kiếm danh mục theo từ khóa
- **GET /statistics**: Thống kê sản phẩm theo danh mục

### 2.8. Partner Routes (/api/partners)

- **GET /**: Lấy danh sách đối tác (khách hàng, nhà cung cấp)
- **GET /{id}**: Lấy thông tin chi tiết đối tác
- **POST /**: Tạo đối tác mới
- **PUT /{id}**: Cập nhật thông tin đối tác
- **DELETE /{id}**: Xóa đối tác
- **GET /customers**: Lấy danh sách khách hàng
- **GET /suppliers**: Lấy danh sách nhà cung cấp
- **GET /{id}/orders**: Lấy đơn hàng của đối tác
- **GET /search**: Tìm kiếm đối tác theo từ khóa
- **GET /{id}/balance**: Xem công nợ của đối tác

### 2.9. Tag Routes (/api/tags)

- **GET /**: Lấy danh sách tất cả các tag
- **GET /{id}**: Lấy thông tin chi tiết tag
- **POST /**: Tạo tag mới
- **PUT /{id}**: Cập nhật thông tin tag
- **DELETE /{id}**: Xóa tag
- **GET /by-entity-type/{type}**: Lấy tag theo loại đối tượng (PRODUCT, ORDER, PARTNER)
- **POST /assign**: Gắn tag cho đối tượng
- **POST /remove**: Gỡ tag khỏi đối tượng
- **POST /bulk-update**: Cập nhật hàng loạt tag
- **GET /entity/{entity_type}/{entity_id}**: Lấy tag của đối tượng cụ thể
- **GET /usage-stats**: Thống kê sử dụng tag
- **GET /filter/{entity_type}**: Lọc đối tượng theo tag
- **GET /unused**: Lấy danh sách tag không được sử dụng

### 2.10. Report Routes (/api/reports)

- **GET /revenue/by-period**: Thống kê doanh thu theo thời gian
- **GET /shipping/by-container**: Báo cáo chi phí vận chuyển theo container
- **GET /profit/by-product**: Báo cáo lợi nhuận theo sản phẩm
- **GET /profit/by-category**: Báo cáo lợi nhuận theo nhóm sản phẩm
- **GET /orders/by-status**: Thống kê đơn hàng theo trạng thái
- **GET /inventory/current**: Báo cáo tồn kho hiện tại
- **GET /inventory/movement**: Báo cáo biến động tồn kho
- **GET /partners/balance**: Đối chiếu công nợ khách hàng/nhà cung cấp
- **POST /export/{report_type}**: Xuất báo cáo (PDF, Excel, CSV)
- **GET /shipping/performance**: Báo cáo hiệu suất vận chuyển
- **GET /trend-analysis**: Biểu đồ phân tích xu hướng doanh thu/chi phí

### 2.11. Schedule Routes (/api/schedules)

- **GET /**: Lấy danh sách lịch trình vận chuyển
- **GET /{id}**: Lấy thông tin chi tiết lịch trình
- **POST /**: Tạo lịch trình vận chuyển mới
- **PUT /{id}**: Cập nhật thông tin lịch trình
- **DELETE /{id}**: Xóa lịch trình
- **PUT /{id}/status**: Cập nhật trạng thái lịch trình
- **POST /{id}/notify**: Gửi thông báo về thay đổi lịch trình
- **GET /{id}/tracking**: Theo dõi hành trình vận chuyển
- **GET /search**: Tìm kiếm và lọc lịch trình
- **GET /upcoming**: Lấy danh sách lịch trình sắp tới

### 2.12. Dashboard Routes (/api/dashboard)

- **GET /overview**: Thống kê tổng quan (doanh thu, chi phí, lợi nhuận)
- **GET /kpi**: Chỉ số hoạt động chính (KPI)
- **GET /charts/trends**: Biểu đồ theo dõi xu hướng
- **GET /alerts**: Cảnh báo hệ thống (tồn kho thấp, đơn hàng chậm)
- **GET /user-activity**: Thống kê hoạt động người dùng
- **GET /realtime-data**: Dữ liệu real-time qua WebSocket

### 2.13. Inventory Routes (/api/inventory)

- **GET /transactions**: Lấy lịch sử giao dịch tồn kho
- **GET /transactions/{id}**: Xem chi tiết giao dịch tồn kho
- **POST /transactions**: Tạo giao dịch tồn kho thủ công
- **GET /product/{product_id}**: Xem tồn kho theo sản phẩm
- **GET /transactions/by-order/{order_id}**: Xem giao dịch tồn kho theo đơn hàng
- **GET /report/value**: Báo cáo giá trị hàng tồn kho
- **GET /report/turnover**: Báo cáo tỷ lệ quay vòng hàng tồn kho
- **POST /stocktake**: Thực hiện kiểm kê kho
- **GET /adjustments/history**: Xem lịch sử điều chỉnh tồn kho
- **GET /transactions/export**: Xuất dữ liệu giao dịch tồn kho

## 3. Utils

### 3.1. auth_utils

- Xác thực JWT
- Phân quyền
- Xác thực 2 lớp

### 3.2. export_utils

- Xuất báo cáo
- Xử lý file Excel

### 3.3. validation_utils

- Kiểm tra dữ liệu
- Validate form

### 3.4. error_utils

- Xử lý lỗi
- Log lỗi

### 3.5. logging_utils

- Ghi log hệ thống
- Theo dõi hoạt động

### 3.6. pagination_utils

- Phân trang kết quả
- Sắp xếp dữ liệu

### 3.7. report_utils

- Tạo các báo cáo định kỳ
- Tính toán chỉ số thống kê (KPI)
- Tạo biểu đồ dữ liệu
- Phân tích xu hướng
- Xuất dữ liệu ra nhiều định dạng (PDF, Excel, CSV, JSON)

### 3.8. websocket_utils

- Quản lý kết nối WebSocket
- Gửi thông báo real-time
- Cập nhật trạng thái đơn hàng và lịch trình trực tiếp
- Xử lý sự kiện từ clients
- Quản lý phòng (room) cho các chủ đề khác nhau
- Caching data với Redis
- Quản lý thời gian sống (TTL) cho dữ liệu cache
- Cache invalidation khi dữ liệu thay đổi
- Tối ưu hóa truy vấn phổ biến
- Lưu trữ session và dữ liệu tạm thời

### 3.9. inventory_utils

- Tự động cập nhật tồn kho khi tạo, cập nhật, hoặc hủy đơn hàng
- Tính toán giá vốn theo các phương pháp khác nhau (FIFO, LIFO, Weighted Average)
- Tạo báo cáo tồn kho và phân tích
- Cảnh báo khi tồn kho dưới ngưỡng tối thiểu
- Dự báo nhu cầu và đề xuất mức tồn kho tối ưu
- Tính toán điểm đặt hàng lại (reorder point) dựa trên mức tiêu thụ trung bình và thời gian giao hàng
- Xử lý trả hàng và tác động đến tồn kho
- Tính toán giá trị hàng tồn kho theo nhiều phương pháp định giá


### 4.1. Production Environment

- Sử dụng Gunicorn hoặc uWSGI làm WSGI server
- Nginx làm reverse proxy
- PostgreSQL production database với kết nối SSL
- Biến môi trường được cấu hình qua .env file (không commit vào repo)

### 4.2. Monitoring & Alerting

- Prometheus cho metrics collection
- Grafana cho visualization dashboard
- Cảnh báo qua email/Slack khi hệ thống gặp vấn đề
- Theo dõi hiệu năng API và thời gian phản hồi
- Log aggregation với ELK stack (Elasticsearch, Logstash, Kibana)
- Uptime monitoring và health checks

### 4.3. Logging Middleware

- Ghi log mỗi request và response
- Theo dõi thời gian xử lý request
- Lưu thông tin người dùng thực hiện thao tác

### 4.4. Rate Limiting Middleware

- Giới hạn số lượng request trong một khoảng thời gian
- Ngăn chặn tấn công DDOS và brute force
- Ưu tiên API key doanh nghiệp
- Cảnh báo khi phát hiện truy cập bất thường
- Mức giới hạn khác nhau cho từng loại endpoint

### 4.5. Cache Middleware

- Kiểm tra và phục vụ response từ cache
- Lưu trữ response vào cache cho các request giống nhau
- Quản lý ETag và conditional requests
- Tối ưu hiệu suất cho các endpoint đọc nhiều
- Cache header management

### 4.6. Backup & Recovery

- Backup tự động hàng ngày cho database
- Backup incremental mỗi giờ
- Lưu trữ backup ở nhiều vị trí khác nhau
- Quy trình khôi phục dữ liệu khi có sự cố
- Point-in-time recovery cho database
- Kiểm tra tính toàn vẹn của backup định kỳ

### 4.7. API Documentation

- Swagger/OpenAPI để tạo tài liệu API tự động
- Mô tả chi tiết từng endpoint, tham số và response
- Môi trường test API cho developers
- Phiên bản API (v1, v2) để hỗ trợ backward compatibility
- Tài liệu hướng dẫn tích hợp API cho frontend

### 4.8. Security

- HTTPS với TLS 1.3 cho tất cả kết nối
- JWT (JSON Web Tokens) với thời hạn 7 ngày
- CORS (Cross-Origin Resource Sharing) được cấu hình chặt chẽ, bổ sung thêm test localhost
- Bảo vệ chống SQL Injection và XSS
- Giới hạn kích thước request để tránh DoS
- Kiểm tra bảo mật định kỳ và quét lỗ hổng
- Mã hóa dữ liệu nhạy cảm trong database

### 4.9. API Authentication & Authorization

#### 4.9.1. Xác thực API

- Tất cả API endpoints đều yêu cầu xác thực (private) trừ /api/auth/*
- Sử dụng JWT token trong header Authorization: Bearer <token>
- Token được cấp sau khi đăng nhập thành công
- Token chứa thông tin user_id và role
- Token có thời hạn 7 ngày
- Refresh token có thời hạn 30 ngày
- Blacklist cho các token đã logout

#### 4.9.2. Phân quyền truy cập

- Role-based Access Control (RBAC) với các cấp độ:
  + SUPER_ADMIN: Toàn quyền hệ thống
  + ADMIN: Quản trị viên
  + MANAGER: Quản lý
  + STAFF: Nhân viên
  + VIEWER: Chỉ xem

- Quyền chi tiết cho từng endpoint:
  + /api/auth/*: Tất cả user chưa đăng nhập
  + /api/users/*: Chỉ ADMIN và SUPER_ADMIN
  + /api/products/*: ADMIN, MANAGER, STAFF (VIEWER chỉ xem)
  + /api/orders/*: ADMIN, MANAGER, STAFF (VIEWER chỉ xem)
  + /api/containers/*: ADMIN, MANAGER, STAFF (VIEWER chỉ xem)
  + /api/costs/*: ADMIN, MANAGER (STAFF, VIEWER chỉ xem)
  + /api/reports/*: ADMIN, MANAGER (VIEWER chỉ xem)
  + /api/dashboard/*: Tất cả user đã đăng nhập

#### 4.9.3. Middleware Bảo mật

- Authentication Middleware:
  + Kiểm tra token trong mọi request
  + Validate token signature và expiration
  + Kiểm tra token trong blacklist
  + Tự động refresh token khi gần hết hạn

- Authorization Middleware:
  + Kiểm tra role của user
  + Validate quyền truy cập endpoint
  + Log các truy cập không được phép
  + Gửi cảnh báo cho admin khi có truy cập bất thường

#### 4.9.4. Bảo mật Session

- Session timeout sau 120 phút không hoạt động
- Giới hạn số lượng session đồng thời
- Lưu log đăng nhập/đăng xuất
- Thông báo khi có đăng nhập từ thiết bị mới
- Cho phép xem và quản lý session đang hoạt động

#### 4.9.5. Audit Logging

- Ghi log chi tiết mọi hoạt động:
  + Thời gian thực hiện
  + User thực hiện
  + IP address
  + Endpoint được gọi
  + Tham số request
  + Kết quả thực hiện
  + Thay đổi dữ liệu

- Lưu trữ log tối thiểu 1 năm
- Có thể xuất log để phân tích
- Cảnh báo khi phát hiện hoạt động bất thường

## 5. Quy trình nghiệp vụ

### 5.1. Quy trình quản lý tồn kho

#### 5.1.1. Cập nhật tồn kho tự động

- Khi có đơn hàng mua (nhập) được tạo và xác nhận:
  - Hệ thống tự động tạo InventoryTransaction với transaction_type = PURCHASE
  - Cập nhật inventory_level của Product, tăng theo số lượng mua
  - Lưu giá nhập hiện tại để tính giá vốn cho hàng bán ra sau này

- Khi có đơn hàng bán (xuất) được tạo và xác nhận:
  - Hệ thống kiểm tra tồn kho hiện tại, nếu đủ để đáp ứng:
    - Tạo InventoryTransaction với transaction_type = SALE
    - Cập nhật inventory_level của Product, giảm theo số lượng bán
    - Tính toán giá vốn theo phương pháp đã chọn (FIFO, LIFO, Weighted Average)
  - Nếu không đủ tồn kho:
    - Thông báo lỗi hoặc đặt đơn hàng ở trạng thái chờ
    - Tạo cảnh báo tồn kho thấp

- Khi đơn hàng bị hủy:
  - Nếu là đơn mua: giảm tồn kho tương ứng
  - Nếu là đơn bán: tăng tồn kho trở lại
  - Tạo InventoryTransaction với transaction_type tương ứng (ADJUSTMENT)

#### 5.1.2. Điều chỉnh tồn kho thủ công

- Cho phép người dùng điều chỉnh tồn kho khi:
  - Phát hiện sai lệch sau kiểm kê
  - Sản phẩm bị hỏng, mất mát, hết hạn
  - Chuyển kho giữa các địa điểm (nếu có quản lý nhiều kho)

- Mỗi điều chỉnh cần có:
  - Lý do điều chỉnh
  - Số lượng điều chỉnh (tăng/giảm)
  - Người thực hiện
  - Thời gian thực hiện
  - Chứng từ liên quan (nếu có)

#### 5.1.3. Phân tích và báo cáo tồn kho

- Báo cáo tồn kho theo thời gian thực
- Phân tích ABC để phân loại sản phẩm theo mức độ quan trọng
- Dự báo nhu cầu và đề xuất kế hoạch mua hàng
- Báo cáo tuổi tồn kho để phát hiện hàng tồn lâu
- Báo cáo giá trị tồn kho và tỷ lệ quay vòng

### 5.2. Quy trình xử lý đơn hàng

#### 5.2.1. Tạo đơn hàng mới

- Kiểm tra thông tin khách hàng và sản phẩm
- Tính toán giá bán dựa trên:
  + Giá nhập
  + Chi phí vận chuyển
  + Thuế
  + Tỷ lệ lợi nhuận
- Kiểm tra tồn kho (cho đơn bán)
- Tạo mã đơn hàng tự động theo quy tắc
- Lưu thông tin chi tiết sản phẩm và số lượng
- Tính toán tổng tiền và các khoản phí

#### 5.2.2. Xử lý đơn hàng xuất khẩu

- Kiểm tra và phân bổ container phù hợp
- Tính toán chi phí vận chuyển và bảo hiểm
- Xử lý thủ tục hải quan
- Lập lịch trình vận chuyển
- Theo dõi trạng thái container
- Cập nhật thông tin vận chuyển real-time

#### 5.2.3. Xử lý đơn hàng nhập khẩu

- Kiểm tra và xác nhận đơn hàng từ nhà cung cấp
- Theo dõi lịch trình vận chuyển
- Xử lý thủ tục hải quan nhập khẩu
- Kiểm tra chất lượng hàng hóa
- Cập nhật tồn kho và giá nhập
- Xử lý thanh toán và hóa đơn

### 5.3. Quy trình quản lý container

#### 5.3.1. Quản lý container trống

- Theo dõi số lượng container trống
- Lên kế hoạch sử dụng container
- Tính toán chi phí thuê container
- Quản lý vị trí container
- Báo cáo hiệu suất sử dụng container

#### 5.3.2. Quản lý container đang vận chuyển

- Theo dõi trạng thái container
- Cập nhật thông tin vị trí
- Xử lý các sự cố trong quá trình vận chuyển
- Thông báo cho khách hàng về tình trạng
- Lưu trữ thông tin tracking

### 5.4. Quy trình quản lý chi phí

#### 5.4.1. Quản lý chi phí đơn hàng

- Phân loại chi phí (vận chuyển, bảo hiểm, thuế, v.v.)
- Tính toán và phân bổ chi phí cho từng sản phẩm
- Theo dõi chi phí thực tế so với dự kiến
- Báo cáo lợi nhuận theo đơn hàng
- Phân tích chi phí để tối ưu

#### 5.4.2. Quản lý chi phí container

- Theo dõi chi phí thuê container
- Tính toán chi phí bảo trì và sửa chữa
- Quản lý chi phí vận chuyển
- Báo cáo hiệu quả sử dụng container
- Tối ưu chi phí container

### 5.5. Quy trình báo cáo và phân tích

#### 5.5.1. Báo cáo tài chính

- Báo cáo doanh thu theo thời gian
- Phân tích lợi nhuận theo sản phẩm
- Theo dõi công nợ khách hàng/nhà cung cấp
- Báo cáo chi phí và lợi nhuận
- Phân tích tỷ lệ hoàn vốn

#### 5.5.2. Báo cáo hoạt động

- Thống kê đơn hàng theo trạng thái
- Phân tích hiệu suất vận chuyển
- Báo cáo tồn kho và quay vòng
- Theo dõi hiệu suất nhân viên
- Phân tích xu hướng và dự báo