# Xây dựng Ứng Dụng Quản Lý Xuất Nhập Khẩu

## Mục tiêu:

Xây dựng một giao diện web hoàn chỉnh để quản lý quy trình xuất nhập khẩu frontend kết nối với backend theo api trong file readme_backend.md.

## Yêu cầu chi tiết:

### 1. Frontend (Svelte v5, Skeleton):

- **Framework:** Svelte 5
- **Thư viện UI:** sử dụng Skeleton cho css https://www.skeleton.dev/docs/get-started/core-api
- **Các thư viện và công cụ khác:** Axios (cho HTTP requests).

#### Các chức năng chính:

##### Trang đăng nhập:

- Giao diện đăng nhập với form đăng nhập.
- Xác thực người dùng thông qua API backend.
- Chuyển hướng đến dashboard sau khi đăng nhập thành công.

##### Dashboard:

- Giao diện tổng quan, hiển thị:
   - Sử dụng thư viện biểu đồ Chart.js để hiển thị dữ liệu trực quan.
   - Biểu đồ số lượng đơn hàng theo trạng thái (đang tạo, đã xác nhận, đang vận chuyển, đã hoàn thành, đã hủy) trong khoảng thời gian gần đây (ví dụ: 7 ngày, 30 ngày). Dữ liệu được cập nhật định kỳ mỗi 5 phút.
   - Danh sách các đơn hàng mới nhất (ví dụ: 5-10 đơn hàng).
   - Tổng số lượng container đang được quản lý (chỉ tính đơn hàng xuất khẩu).
   - Tổng doanh thu ước tính (có thể hiển thị theo tháng, quý).
   - Thông tin người dùng đã đăng nhập.

##### Quản lý người dùng:

- Giao diện quản lý danh sách người dùng.
- Chức năng thêm, sửa, xóa người dùng (chỉ dành cho Admin).
- Phân quyền người dùng (chỉ dành cho Admin).
- Chức năng đổi mật khẩu cho người dùng.
- Menu chính Quản lý người dùng và 3 menu con: Chi ti

##### Quản lý đơn hàng:

- Giao diện quản lý danh sách đơn hàng.
- Chức năng thêm, sửa, xóa đơn hàng.
- Xem chi tiết đơn hàng.
- Tìm kiếm và lọc đơn hàng theo trạng thái, khách hàng, thời gian, loại đơn hàng (trong nước, xuất khẩu).
- Cập nhật trạng thái đơn hàng.
- Hiển thị thông tin liên quan (sản phẩm, container, chi phí).
- Container chỉ hiển thị với đơn hàng xuất khẩu.

##### Quản lý sản phẩm:

- Giao diện quản lý danh sách sản phẩm.
- Chức năng thêm, sửa, xóa sản phẩm.
- Tìm kiếm và lọc sản phẩm.

##### Quản lý khách hàng:

- Giao diện quản lý danh sách khách hàng.
- Chức năng thêm, sửa, xóa khách hàng.
- Tìm kiếm và lọc khách hàng.

##### Quản lý nhà cung cấp:

- Giao diện quản lý danh sách nhà cung cấp.
- Chức năng thêm, sửa, xóa nhà cung cấp.
- Tìm kiếm và lọc nhà cung cấp.

##### Quản lý container:

- Giao diện quản lý danh sách container.
- Chức năng thêm, sửa, xóa container.
- Cập nhật trạng thái container.
- Liên kết container với đơn hàng (chỉ dành cho đơn hàng xuất khẩu).

##### Quản lý chi phí:

- Giao diện quản lý chi phí.
- Chức năng thêm, sửa, xóa chi phí.
- Liên kết chi phí với đơn hàng hoặc container.

##### Thống kê và báo cáo:

- Thống kê số lượng đơn hàng, container theo thời gian.
- Thống kê doanh thu.
- Biểu đồ trực quan hóa dữ liệu (ví dụ: biểu đồ cột, biểu đồ đường).
- Thống kê theo khách hàng.
- Thống kê nhà cung cấp.
- Thống kê đơn hàng theo loại (trong nước, xuất khẩu).

##### Xuất báo cáo:

- Chức năng xuất dữ liệu ra file Excel hoặc PDF (gọi API backend).
- Tùy chọn xuất theo khoảng thời gian và điều kiện lọc.

### Yêu cầu bổ sung:

##### Xử lý lỗi:

- Frontend: Hiển thị thông báo lỗi thân thiện cho người dùng.

##### Giao diện người dùng:

- Thiết kế giao diện hiện đại, thân thiện, dễ sử dụng.
- Sử dụng Tailwind CSS 4 để đảm bảo tính nhất quán và khả năng tùy chỉnh.
- Đảm bảo ứng dụng hoạt động tốt trên các thiết bị khác nhau (responsive design).

##### Bảo mật:

- Bảo vệ ứng dụng khỏi các lỗ hổng bảo mật phổ biến (CSRF, XSS, SQL injection).
- Sử dụng HTTPS để mã hóa dữ liệu truyền tải.

##### Hiệu năng:

- Tối ưu hóa hiệu năng của cả backend và frontend để đảm bảo thời gian phản hồi nhanh.

##### Tài liệu:

- Cung cấp tài liệu hướng dẫn cài đặt và sử dụng ứng dụng.
- Viết comment rõ ràng trong code.
- Đối với class thì dùng comment bắt đầu bằng #SECTION tên class và kết thúc class là #!SECTION tên class.
- Đối với function thì dùng #TODO tên function để ghi chú.
- Sử dụng tiếng Việt trong ghi chú.