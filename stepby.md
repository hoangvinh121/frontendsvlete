# Các bước tạo giao diện Frontend Export Management System

Dựa vào tài liệu bạn đã cung cấp, tôi sẽ chia thành các nhóm prompt để triển khai giao diện frontend một cách có hệ thống.

## 1. Thiết lập dự án & cấu trúc cơ bản

### Prompt 1: Khởi tạo dự án Vue 3 với Vite
```
Hãy tạo một dự án Vue 3 mới sử dụng Vite với tên "export-management-system". Dự án cần cài đặt các dependencies sau:
- Vue 3
- Vue Router
- Pinia
- Element Plus
- Axios
- Chart.js
- Vue-i18n

Sau khi tạo dự án, hãy thiết lập cấu trúc thư mục dự án như sau:
src/
├── App.vue
├── main.js
├── router/
├── views/
├── components/
├── stores/
├── api/
├── utils/
├── constants/
└── assets/
```

### Prompt 2: Thiết lập Router và Pinia Store
```
Thiết lập Vue Router trong file src/router/index.js với cấu hình lazy loading cho các route chính:
- Trang đăng nhập (/login)
- Layout chính cho người dùng đã đăng nhập (/) với các sub-routes:
  - Dashboard (/dashboard)
  - Quản lý người dùng (/users)
  - Quản lý sản phẩm (/products)
  - Quản lý đơn hàng (/orders)
  - Quản lý container (/containers)
  - Quản lý chi phí (/costs)
  - Các module khác như đã mô tả trong tài liệu

Thiết lập các Pinia store cơ bản:
- auth.js: Quản lý đăng nhập, đăng xuất, thông tin người dùng
- theme.js: Quản lý theme
- toast.js: Quản lý thông báo toast
- filters.js: Quản lý filter và sorting
```

### Prompt 3: Thiết lập Axios Client và Interceptors
```
Tạo Axios client trong file src/api/client.js với các interceptors sau:
- Request interceptor: Thêm authentication headers, xử lý loading state
- Response interceptor: Xử lý response transformation, global error handling
- Error interceptor: Xử lý refresh token, retry logic, global error handling

Cài đặt các module API cơ bản:
- auth.js: authenticate, refreshToken, logout
- users.js: getUsers, getUser, createUser, updateUser, deleteUser
- products.js: getProducts, getProduct, createProduct, updateProduct, deleteProduct
- orders.js: tương tự các phương thức CRUD
```

## 2. Layout và Component Design System

### Prompt 4: Thiết lập Layout Component
```
Tạo các layout components chính:
1. Root Layout (src/views/layout/RootLayout.vue):
   - Cung cấp cấu trúc cơ bản của ứng dụng
   - Theme provider
   - Error boundaries
   - Toast notifications container

2. App Layout (src/views/layout/AppLayout.vue):
   - Sidebar navigation
   - Top navbar
   - User dropdown
   - Breadcrumbs
   - Content area

3. Auth Layout (src/views/layout/AuthLayout.vue):
   - Layout đơn giản cho trang đăng nhập và xác thực

Đảm bảo các layout components có responsive design và xử lý các breakpoints khác nhau.
```

### Prompt 5: Xây dựng UI Components cơ bản
```
Tạo các UI components cơ bản dựa trên Element Plus:
1. Buttons (src/components/ui/AppButton.vue):
   - Mở rộng từ ElButton với các variants: primary, secondary, danger, success
   - Thêm logic loading state, disabled state

2. Forms (src/components/forms/):
   - AppInput.vue: Mở rộng từ ElInput với validation support
   - AppSelect.vue: Mở rộng từ ElSelect với enhanced search
   - AppCheckbox.vue: Mở rộng từ ElCheckbox
   - AppDatePicker.vue: Mở rộng từ ElDatePicker

3. Layout Components (src/components/layout/):
   - PageHeader.vue: Tiêu đề trang và actions
   - PageContainer.vue: Container với padding & margins
   - SplitView.vue: Layout 2 cột
   - Breadcrumbs.vue: Breadcrumb navigation

Đảm bảo tất cả components đều hỗ trợ i18n và tuân thủ design system.
```

### Prompt 6: Xây dựng Data Components
```
Tạo các data components để hiển thị và tương tác với dữ liệu:
1. DataTable (src/components/tables/DataTable.vue):
   - Mở rộng từ ElTable
   - Hỗ trợ sorting, pagination, filtering
   - Selection (single, multiple)
   - Export (CSV, Excel)
   - Column customization
   - Empty state, loading state
   - Responsive design

2. Charts (src/components/charts/):
   - LineChart.vue: Line chart component sử dụng Chart.js
   - BarChart.vue: Bar chart component
   - PieChart.vue: Pie chart component
   - AreaChart.vue: Area chart component
   - Tất cả đều có interactive tooltips và responsive design

3. Status Components:
   - StatusBadge.vue: Badge hiển thị trạng thái với màu sắc tương ứng
   - StatusTimeline.vue: Timeline hiển thị lịch sử trạng thái
```

## 3. Xây dựng các Module Chính

### Prompt 7: Xây dựng Module Authentication
```
Tạo các views và components cho module authentication:

1. Login View (src/views/auth/LoginView.vue):
   - Form đăng nhập với validation
   - Xử lý error messages
   - Remember me functionality
   - Forgot password link

2. Forgot Password View (src/views/auth/ForgotPasswordView.vue):
   - Form quên mật khẩu
   - Email validation
   - Success/error states

3. Reset Password View (src/views/auth/ResetPasswordView.vue):
   - Form đặt lại mật khẩu
   - Password validation
   - Success/error states

4. Two-Factor Authentication (src/components/auth/TwoFactorForm.vue):
   - Input cho mã xác thực
   - Countdown timer
   - Resend code functionality

Kết nối với các API endpoints:
- POST /api/auth/login
- POST /api/auth/forgot-password
- POST /api/auth/reset-password
- POST /api/auth/verify-2fa
```

### Prompt 8: Xây dựng Dashboard
```
Tạo dashboard view (src/views/dashboard/DashboardView.vue) với các components sau:

1. KPI Cards (src/components/dashboard/KpiCard.vue):
   - Hiển thị các chỉ số KPI: Doanh thu, Đơn hàng, Tồn kho, Lợi nhuận
   - So sánh với kỳ trước (% tăng/giảm)
   - Animated number khi load

2. Revenue Trends Chart (src/components/dashboard/RevenueTrendsChart.vue):
   - Line chart hiển thị doanh thu theo thời gian
   - Filter theo ngày/tuần/tháng/năm
   - Tooltips chi tiết

3. Order Status Distribution (src/components/dashboard/OrderStatusChart.vue):
   - Pie/donut chart hiển thị phân phối trạng thái đơn hàng
   - Interactive legend

4. Top Selling Products (src/components/dashboard/TopSellingProducts.vue):
   - Bảng/card hiển thị các sản phẩm bán chạy
   - Sorting và pagination

5. Recent Orders & Upcoming Shipments (src/components/dashboard/RecentActivityList.vue):
   - Hiển thị đơn hàng gần đây và lịch trình sắp tới
   - Quick actions

Kết nối với các API endpoints:
- GET /api/dashboard/overview
- GET /api/dashboard/kpi
- GET /api/dashboard/charts/trends
- GET /api/dashboard/alerts
```

### Prompt 9: Xây dựng Module Quản lý người dùng
```
Tạo các views và components cho module quản lý người dùng:

1. User List View (src/views/users/UserListView.vue):
   - Filter Bar: lọc theo vai trò, phòng ban, trạng thái
   - UserTable: bảng hiển thị người dùng với phân trang
   - ActionButtons: thêm, sửa, xóa người dùng
   - ExportOptions: xuất dữ liệu

2. User Detail View (src/views/users/UserDetailView.vue):
   - UserProfile: thông tin cá nhân
   - RolePermissions: vai trò và quyền hạn
   - ActivityLog: lịch sử hoạt động
   - TwoFactorSettings: cài đặt xác thực 2 lớp

3. User Create/Edit View (src/views/users/UserFormView.vue):
   - Form nhập thông tin người dùng với validation
   - RoleSelector: chọn vai trò
   - DepartmentSelector: chọn phòng ban
   - PasswordGenerator: tạo mật khẩu

Kết nối với các API endpoints:
- GET /api/users
- GET /api/users/{id}
- POST /api/users
- PUT /api/users/{id}
- DELETE /api/users/{id}
```

### Prompt 10: Xây dựng Module Quản lý sản phẩm
```
Tạo các views và components cho module quản lý sản phẩm:

1. Product List View (src/views/products/ProductListView.vue):
   - SearchBar: tìm kiếm sản phẩm
   - FilterPanel: lọc theo danh mục, tồn kho, giá
   - ViewToggle: chuyển đổi giữa dạng bảng và lưới
   - ProductTable/ProductGrid: hiển thị sản phẩm
   - BulkActions: thao tác hàng loạt

2. Product Detail View (src/views/products/ProductDetailView.vue):
   - ProductInfo: thông tin cơ bản
   - PricingInfo: thông tin giá
   - InventoryHistory: lịch sử tồn kho
   - ProductImages: quản lý hình ảnh
   - RelatedProducts: sản phẩm liên quan
   - TagsManager: quản lý nhãn sản phẩm

3. Product Create/Edit View (src/views/products/ProductFormView.vue):
   - Form nhập thông tin sản phẩm với validation
   - CategorySelector: chọn danh mục
   - PricingCalculator: tính toán giá bán
   - ImageUploader: tải lên hình ảnh
   - TagSelector: chọn nhãn

Kết nối với các API endpoints:
- GET /api/products
- GET /api/products/{id}
- POST /api/products
- PUT /api/products/{id}
- DELETE /api/products/{id}
- GET /api/products/{id}/inventory
```

### Prompt 11: Xây dựng Module Quản lý đơn hàng
```
Tạo các views và components cho module quản lý đơn hàng:

1. Order List View (src/views/orders/OrderListView.vue):
   - OrderFilters: lọc theo trạng thái, khách hàng, ngày
   - OrderTable: bảng đơn hàng với phân trang
   - StatusBadge: hiển thị trạng thái đơn hàng
   - QuickActions: thao tác nhanh
   - OrderSummary: tóm tắt đơn hàng

2. Order Detail View (src/views/orders/OrderDetailView.vue):
   - OrderHeader: thông tin chung đơn hàng
   - OrderItems: danh sách sản phẩm trong đơn
   - OrderTimeline: timeline trạng thái đơn hàng
   - OrderCosts: chi phí liên quan
   - ShippingInfo: thông tin vận chuyển
   - InvoicePreview: xem trước hóa đơn
   - StatusUpdateForm: cập nhật trạng thái

3. Order Create/Edit View (src/views/orders/OrderFormView.vue):
   - OrderTypeSelector: chọn loại đơn hàng
   - PartnerSelector: chọn khách hàng/nhà cung cấp
   - ProductSelector: chọn sản phẩm
   - QuantityEditor: nhập số lượng
   - PricingCalculator: tính toán giá và chi phí
   - ShippingForm: thông tin vận chuyển
   - ContainerSelector: chọn container
   - CostEstimator: ước tính chi phí

Kết nối với các API endpoints:
- GET /api/orders
- GET /api/orders/{id}
- POST /api/orders
- PUT /api/orders/{id}
- PUT /api/orders/{id}/status
- GET /api/orders/{id}/items
```

### Prompt 12: Xây dựng Module Quản lý container
```
Tạo các views và components cho module quản lý container:

1. Container List View (src/views/containers/ContainerListView.vue):
   - ContainerFilters: lọc theo loại, trạng thái, vị trí
   - ContainerTable: bảng container với phân trang
   - StatusIndicator: hiển thị trạng thái container
   - CapacityIndicator: hiển thị khả năng chứa
   - LocationMap: bản đồ vị trí container

2. Container Detail View (src/views/containers/ContainerDetailView.vue):
   - ContainerInfo: thông tin cơ bản
   - ContainerContent: nội dung chứa
   - ContainerDimensions: kích thước container
   - ShippingSchedule: lịch trình vận chuyển
   - CostBreakdown: chi tiết chi phí
   - OrdersList: danh sách đơn hàng trong container
   - StatusHistory: lịch sử trạng thái

3. Container Create/Edit View (src/views/containers/ContainerFormView.vue):
   - ContainerForm: form thông tin container
   - ContainerTypeSelector: chọn loại container
   - DimensionsCalculator: tính toán kích thước
   - CostEstimator: ước tính chi phí
   - LocationSelector: chọn vị trí

Kết nối với các API endpoints:
- GET /api/containers
- GET /api/containers/{id}
- POST /api/containers
- PUT /api/containers/{id}
- GET /api/containers/{id}/orders
- GET /api/containers/{id}/costs
```

## 4. Xây dựng các Module Phụ trợ

### Prompt 13: Xây dựng Module Quản lý chi phí
```
Tạo các views và components cho module quản lý chi phí:

1. Cost List View (src/views/costs/CostListView.vue):
   - CostFilters: lọc theo loại, đơn hàng, container
   - CostTable: bảng chi phí với phân trang
   - CostSummary: tổng hợp chi phí
   - CostChart: biểu đồ phân tích chi phí
   - ExportOptions: xuất dữ liệu

2. Cost Detail View (src/views/costs/CostDetailView.vue):
   - CostInfo: thông tin chi tiết
   - RelatedOrders: đơn hàng liên quan
   - RelatedContainers: container liên quan
   - DocumentViewer: xem chứng từ
   - CurrencyConverter: chuyển đổi tiền tệ

3. Cost Create/Edit View (src/views/costs/CostFormView.vue):
   - CostForm: form thông tin chi phí
   - CostTypeSelector: chọn loại chi phí
   - OrderSelector: chọn đơn hàng liên quan
   - ContainerSelector: chọn container liên quan
   - CurrencySelector: chọn loại tiền tệ
   - DocumentUploader: tải lên chứng từ

Kết nối với các API endpoints:
- GET /api/costs
- GET /api/costs/{id}
- POST /api/costs
- PUT /api/costs/{id}
- GET /api/costs/by-order/{order_id}
- GET /api/costs/by-container/{container_id}
```

### Prompt 14: Xây dựng Module Quản lý danh mục
```
Tạo các views và components cho module quản lý danh mục:

1. Category List View (src/views/categories/CategoryListView.vue):
   - CategoryTree: hiển thị cây danh mục
   - CategoryActions: thêm, sửa, xóa
   - CategoryStats: thống kê sản phẩm theo danh mục
   - SearchCategory: tìm kiếm danh mục

2. Category Detail View (src/views/categories/CategoryDetailView.vue):
   - CategoryInfo: thông tin danh mục
   - SubcategoriesList: danh sách danh mục con
   - ProductsList: danh sách sản phẩm thuộc danh mục
   - CategoryMetrics: số liệu thống kê

3. Category Create/Edit View (src/views/categories/CategoryFormView.vue):
   - CategoryForm: form thông tin danh mục
   - ParentCategorySelector: chọn danh mục cha
   - CategoryCodeGenerator: tạo mã danh mục

Kết nối với các API endpoints:
- GET /api/categories
- GET /api/categories/tree
- GET /api/categories/{id}
- POST /api/categories
- PUT /api/categories/{id}
- GET /api/categories/{id}/products
```

### Prompt 15: Xây dựng Module Quản lý đối tác
```
Tạo các views và components cho module quản lý đối tác:

1. Partner List View (src/views/partners/PartnerListView.vue):
   - PartnerFilters: lọc theo loại, quốc gia, trạng thái
   - PartnerTable: bảng đối tác với phân trang
   - PartnerMap: bản đồ phân bố đối tác
   - PartnerStats: thống kê đối tác
   - ExportOptions: xuất dữ liệu

2. Partner Detail View (src/views/partners/PartnerDetailView.vue):
   - PartnerInfo: thông tin cơ bản
   - ContactInfo: thông tin liên hệ
   - OrderHistory: lịch sử đơn hàng
   - BalanceInfo: thông tin công nợ
   - DocumentsList: danh sách tài liệu
   - TagsManager: quản lý nhãn đối tác

3. Partner Create/Edit View (src/views/partners/PartnerFormView.vue):
   - PartnerForm: form thông tin đối tác
   - PartnerTypeSelector: chọn loại đối tác
   - AddressForm: form địa chỉ
   - ContactForm: form thông tin liên hệ
   - BankInfoForm: form thông tin ngân hàng
   - TagSelector: chọn nhãn

Kết nối với các API endpoints:
- GET /api/partners
- GET /api/partners/{id}
- POST /api/partners
- PUT /api/partners/{id}
- GET /api/partners/{id}/orders
- GET /api/partners/{id}/balance
```

### Prompt 16: Xây dựng Module Quản lý tồn kho
```
Tạo các views và components cho module quản lý tồn kho:

1. Inventory Transaction List View (src/views/inventory/TransactionListView.vue):
   - InventoryTransactionFilters: lọc theo loại giao dịch, sản phẩm, thời gian
   - InventoryTransactionTable: bảng giao dịch tồn kho
   - TransactionTypeBadge: hiển thị loại giao dịch
   - QuantityIndicator: hiển thị số lượng nhập/xuất
   - ProductInfo: thông tin sản phẩm
   - OrderInfo: thông tin đơn hàng liên quan
   - ExportOptions: xuất dữ liệu

2. Inventory Transaction Detail View (src/views/inventory/TransactionDetailView.vue):
   - TransactionInfo: thông tin giao dịch
   - ProductInfo: thông tin sản phẩm
   - OrderInfo: thông tin đơn hàng liên quan
   - WarehouseInfo: thông tin kho
   - AuditTrail: lịch sử thay đổi
   - DocumentViewer: xem chứng từ

3. Inventory Transaction Create/Edit View (src/views/inventory/TransactionFormView.vue):
   - TransactionForm: form thông tin giao dịch
   - TransactionTypeSelector: chọn loại giao dịch
   - ProductSelector: chọn sản phẩm
   - QuantityInput: nhập số lượng
   - WarehouseSelector: chọn kho
   - OrderSelector: chọn đơn hàng liên quan
   - DocumentUploader: tải lên chứng từ

Kết nối với các API endpoints:
- GET /api/inventory/transactions
- GET /api/inventory/transactions/{id}
- POST /api/inventory/transactions
- GET /api/inventory/transactions/by-order/{order_id}
```

## 5. Testing, i18n và Deployment

### Prompt 17: Thiết lập đa ngôn ngữ với i18n
```
Thiết lập hỗ trợ đa ngôn ngữ cho ứng dụng:

1. Tạo cấu trúc i18n trong src/i18n/index.js:
   - Thiết lập Vue-i18n
   - Tạo các file ngôn ngữ (vi.js, en.js)
   - Thêm cơ chế lazy loading cho file ngôn ngữ

2. Tạo các file ngôn ngữ cho các module chính:
   - src/i18n/locales/vi/common.js: Các text chung
   - src/i18n/locales/vi/auth.js: Text cho module authentication
   - src/i18n/locales/vi/products.js: Text cho module sản phẩm
   - src/i18n/locales/vi/orders.js: Text cho module đơn hàng
   - Tương tự cho tiếng Anh và các ngôn ngữ khác

3. Tạo Language Selector component (src/components/ui/LanguageSelector.vue):
   - Dropdown chọn ngôn ngữ
   - Lưu ngôn ngữ đã chọn vào local storage

4. Cập nhật tất cả các components để sử dụng i18n:
   - Thay thế các hardcoded text bằng $t('key')
   - Đảm bảo tất cả text đều có trong file ngôn ngữ
```

### Prompt 18: Thiết lập Unit Testing với Vitest
```
Thiết lập unit testing cho các components và utils:

1. Cấu hình Vitest trong vite.config.js:
   - Thêm cấu hình test
   - Thiết lập test coverage

2. Tạo các test cho UI components cơ bản:
   - src/components/ui/__tests__/AppButton.spec.js
   - src/components/ui/__tests__/AppInput.spec.js
   - src/components/ui/__tests__/AppSelect.spec.js

3. Tạo các test cho utility functions:
   - src/utils/__tests__/formatters.spec.js
   - src/utils/__tests__/validators.spec.js
   - src/utils/__tests__/permissions.spec.js

4. Tạo các test cho stores:
   - src/stores/__tests__/auth.spec.js
   - src/stores/__tests__/filters.spec.js

5. Thiết lập mock cho các API calls trong tests
```

### Prompt 19: Thiết lập End-to-End Testing với Playwright
```
Thiết lập E2E testing cho các luồng chính của ứng dụng:

1. Cấu hình Playwright trong e2e/playwright.config.js:
   - Thiết lập các browsers cần test
   - Cấu hình screenshots và videos
   - Thiết lập test reports

2. Tạo các test cases cho các luồng chính:
   - e2e/tests/auth.spec.js: Test đăng nhập, đăng xuất
   - e2e/tests/products.spec.js: Test thêm, sửa, xóa sản phẩm
   - e2e/tests/orders.spec.js: Test tạo và quản lý đơn hàng
   - e2e/tests/dashboard.spec.js: Test hiển thị dashboard

3. Tạo các Page Objects để tái sử dụng logic testing:
   - e2e/pages/LoginPage.js
   - e2e/pages/DashboardPage.js
   - e2e/pages/ProductsPage.js
   - e2e/pages/OrdersPage.js
```

### Prompt 20: Triển khai Production Build và Optimization
```
Cấu hình production build và tối ưu hiệu năng:

1. Cấu hình Vite cho production build trong vite.config.js:
   - Thiết lập code splitting
   - Thiết lập dynamic imports
   - Tối ưu bundle size

2. Tạo Docker setup cho deployment:
   - Dockerfile cho build và deployment
   - Docker Compose cho development environment
   - Nginx configuration cho production server

3. Cài đặt Progressive Web App:
   - Service worker
   - Web app manifest
   - Offline support

4. Tối ưu performance:
   - Lazy loading cho images và components
   - Code splitting dựa trên routes
   - Preloading cho critical paths
   - Caching strategies
   - Server-side rendering option
```

## 6. Tổng kết

Với các prompt trên, bạn đã có thể triển khai một hệ thống frontend hoàn chỉnh theo tài liệu đã cung cấp. Các bước triển khai được chia thành 20 prompts chính, mỗi prompt tập trung vào một khía cạnh cụ thể của hệ thống. Cấu trúc này cho phép bạn:

1. Bắt đầu từ cấu hình cơ bản của dự án
2. Xây dựng các layout và component design system
3. Triển khai các module chính của hệ thống
4. Thêm các module phụ trợ
5. Hoàn thiện với testing, i18n và deployment

Các prompt đã bao gồm đầy đủ các functionality được mô tả trong tài liệu frontend_vue.md và được tổ chức theo quy trình phát triển phần mềm hiện đại.
