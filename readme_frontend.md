# Xây dựng Ứng Dụng Quản Lý Xuất Nhập Khẩu

## Mục tiêu:

Xây dựng một giao diện web hoàn chỉnh để quản lý quy trình xuất nhập khẩu frontend kết nối với backend theo api trong file readme_backend.md.

## Yêu cầu chi tiết:

### 1. Frontend (Svelte v5, Skeleton):

- **Framework:** Svelte 5
- **Thư viện UI:** sử dụng Skeleton cho css https://www.skeleton.dev/docs/get-started/core-api
- **Các thư viện và công cụ khác:** Axios (cho HTTP requests).
- **Page**: Mỗi model có ít nhất 3 menu bao gồm: List, detail, create new

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
- Menu chính Quản lý người dùng và 3 menu con

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
- Sử dụng Skeleton 3 để đảm bảo tính nhất quán và khả năng tùy chỉnh.
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
1. Cấu trúc thư mục:
Thêm một phần mô tả cấu trúc thư mục dự án Svelte. Điều này giúp người phát triển hiểu rõ hơn về tổ chức của các components, routes, và các tài nguyên khác. Ví dụ:
src/
├── lib/        # Các components và tiện ích tái sử dụng
│   ├── components/
│   │   ├── Button.svelte
│   │   ├── Table.svelte
│   │   └── ...
│   ├── utils/
│   │   ├── api.js      # Hàm gọi API
│   │   ├── auth.js     # Xử lý xác thực
│   │   └── ...
├── routes/       # Các trang của ứng dụng
│   ├── login/
│   │   └── +page.svelte
│   ├── dashboard/
│   │   └── +page.svelte
│   ├── users/
│   │   ├── +page.svelte        # Danh sách người dùng
│   │   ├── [id]/             # Trang chi tiết người dùng
│   │   │   └── +page.svelte
│   │   └── create/
│   │       └── +page.svelte
│   └── ...
├── app.html    # File HTML gốc
└── app.d.ts    # Khai báo kiểu (nếu dùng TypeScript)



2. Mô tả chi tiết về Components:
Component Table: Mô tả các props của component Table, ví dụ: headers, data, actions (các nút như xem, sửa, xóa).
Component Form: Nếu có một component form chung, mô tả cách sử dụng nó để tạo các form thêm/sửa cho các model khác nhau.
Component Chart: Nếu có component chart chung, mô tả các loại chart có thể hiển thị (line, bar, pie), các props để truyền dữ liệu và tùy chọn hiển thị.
3. API Endpoints và Định dạng dữ liệu:
Bổ sung chi tiết hơn về các API endpoint mà frontend sẽ gọi, bao gồm:
URL
HTTP method
Các tham số (query parameters, request body)
Định dạng dữ liệu trả về (JSON)
Ví dụ về request và response (có thể dùng Markdown code blocks)
Ví dụ:
### Lấy danh sách người dùng
-   **URL:** `/api/users`
-   **Method:** `GET`
-   **Parameters:**
    -   `page`: Số trang (optional, default: 1)
    -   `limit`: Số lượng kết quả trên trang (optional, default: 20)
    -   `search`: chuỗi tìm kiếm
-   **Response:**
    ```json
    {
        "data": [
            {
                "id": "1",
                "username": "john.doe",
                "email": "john.doe@example.com",
                "full_name": "John Doe",
                "role": "Admin",
                "active": true
            },
            ...
        ],
        "total": 100, // Tổng số lượng người dùng
        "page": 1,
        "limit": 20
    }
    ```

### Tạo người dùng mới
-   **URL**: `/api/users`
-   **Method**: `POST`
-   **Request Body**:
    ```json
    {
        "username": "jane.doe",
        "email": "jane.doe@example.com",
        "password": "password123",
        "full_name": "Jane Doe",
        "role": "Editor",
        "active": true
    }
    ```
-   **Response**:
    ```json
    {
        "id": "101",
        "username": "jane.doe",
        "email": "jane.doe@example.com",
        "full_name": "Jane Doe",
        "role": "Editor",
        "active": true
    }
    ```

### 4.  Xử lý lỗi chi tiết:

Mô tả các loại lỗi có thể xảy ra và cách frontend nên xử lý chúng (ví dụ: hiển thị thông báo lỗi thân thiện, chuyển hướng người dùng). Nêu rõ các mã lỗi HTTP dự kiến từ backend.

### 5.  Các ràng buộc và xác thực dữ liệu:

Mô tả các ràng buộc dữ liệu mà frontend cần áp dụng trước khi gửi dữ liệu đến backend (ví dụ: độ dài tối đa của trường, định dạng email, các trường bắt buộc).

### 6.  Chi tiết về Biểu đồ:

-   Thư viện được sử dụng: Nếu bạn quyết định sử dụng một thư viện biểu đồ cụ thể, hãy đề cập đến nó (ví dụ: Recharts, ApexCharts).
-   Các loại biểu đồ: Chỉ rõ các loại biểu đồ sẽ được sử dụng (ví dụ: cột, đường, tròn) và mục đích của chúng.
-   Dữ liệu: Mô tả cấu trúc dữ liệu mà frontend cần để hiển thị biểu đồ.
-   Tùy chỉnh: Nếu có bất kỳ tùy chỉnh đặc biệt nào cho biểu đồ (ví dụ: màu sắc, nhãn, chú thích), hãy đề cập đến chúng.

### 7.  Xác thực và Phân quyền:

Mô tả chi tiết hơn về cách xác thực người dùng và phân quyền sẽ được thực hiện:

-   Cơ chế xác thực (ví dụ: JWT, Cookies).
-   Các loại vai trò người dùng (ví dụ: Admin, Editor, Viewer) và quyền của họ.
-   Cách frontend kiểm tra quyền của người dùng trước khi hiển thị các chức năng hoặc trang nhất định.
-   Cách xử lý các trường hợp người dùng không có quyền truy cập.

### 8.  Các tương tác và hiệu ứng:

Mô tả các tương tác và hiệu ứng dự kiến trong ứng dụng:

-   Các hiệu ứng chuyển trang (transitions).
-   Các hiệu ứng khi người dùng tương tác với các phần tử trên trang (ví dụ: hover, click).
-   Cách sử dụng Skeleton để tạo các hiệu ứng này.

### 9.  Các yêu cầu về hiệu năng:

-   Lazy loading: Mô tả những phần nào của ứng dụng sẽ được tải một cách tuần tự.
-   Tối ưu hóa hình ảnh: Các định dạng hình ảnh và kích thước sẽ được sử dụng.
-   Bộ nhớ đệm: Nếu có bất kỳ cơ chế bộ nhớ đệm nào được sử dụng ở phía trước, hãy mô tả nó.

### 10. Các yêu cầu về khả năng mở rộng:

-   Mô tả cách ứng dụng có thể được mở rộng để xử lý lượng lớn người dùng và dữ liệu.
-   Đề cập đến bất kỳ cân nhắc nào về kiến trúc hoặc thiết kế có liên quan đến khả năng mở rộng.

