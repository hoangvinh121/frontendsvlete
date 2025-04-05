# Các bước tạo giao diện Frontend Export Management System sử dụng Svelte

Dựa vào tài liệu bạn đã cung cấp, tôi sẽ chia thành các nhóm prompt để triển khai giao diện frontend một cách có hệ thống sử dụng Svelte và shadcn-svelte UI

## 1. Thiết lập dự án & cấu trúc cơ bản

### Prompt 1: Khởi tạo dự án Svelte với vite

```ini
Hãy tạo một dự án Svelte mới sử dụng sveltekit thư mục hiện tại. Dự án cần cài đặt các dependencies sau:
- Svelte và sveltekit
- svelte-routing (cho routing)
- shadcn-svelte
- Tailwind CSS
- svelte-i18n (cho đa ngôn ngữ)
- axios (cho API requests)
- chart.ts
- Api backend: Được giải thích ở file api.md về cách sử dụng API.

Sau khi tạo dự án, hãy thiết lập cấu trúc thư mục dự án như sau:
src/
├── app.html
├── routes/
├── lib/
│   ├── components/
│   ├── stores/
│   ├── api/
│   ├── utils/
│   ├── constants/
│   └── assets/
├── app.css
└── app.d.ts

```

### Prompt 2: Thiết lập Routing và Store

```ini
Thiết lập SvelteKit routing trong cấu trúc thư mục src/routes với cấu hình lazy loading:
- Trang đăng nhập (/login)
- Layout chính cho người dùng đã đăng nhập (/) với các sub-routes:
  - Dashboard (/dashboard)
  - Quản lý người dùng (/users)
  - Quản lý sản phẩm (/products)
  - Quản lý đơn hàng (/orders)
  - Quản lý container (/containers)
  - Quản lý chi phí (/costs)
  - Các module khác như đã mô tả trong tài liệu

Thiết lập các Svelte stores cơ bản trong src/lib/stores:
- auth.ts: Quản lý đăng nhập, đăng xuất, thông tin người dùng
- theme.ts: Quản lý theme
- toast.ts: Quản lý thông báo toast
- filters.ts: Quản lý filter và sorting

Sử dụng cấu trúc +page.svelte, +layout.svelte và +page.server.ts của SvelteKit để quản lý các route.
```

### Prompt 3: Thiết lập Axios Client và Interceptors

```yaml
Tạo Axios client trong file src/lib/api/client.ts với các interceptors sau:
- Request interceptor: Thêm authentication headers, xử lý loading state
- Response interceptor: Xử lý response transformation, global error handling
- Error interceptor: Xử lý refresh token, retry logic, global error handling

Cài đặt các module API cơ bản:
- auth.ts: authenticate, refreshToken, logout
- users.ts: getUsers, getUser, createUser, updateUser, deleteUser
- products.ts: getProducts, getProduct, createProduct, updateProduct, deleteProduct
- orders.ts: tương tự các phương thức CRUD
  
Kết hợp với SvelteKit load functions để tận dụng server-side rendering và data loading.
```

## 2. Layout và Component Design System

### Prompt 4: Thiết lập Layout Component

```ini
Tạo các layout components chính sử dụng cấu trúc SvelteKit:
1. Root Layout (src/routes/+layout.svelte):
   - Cung cấp cấu trúc cơ bản của ứng dụng
   - Theme provider
   - Error boundaries
   - Toast notifications container

2. App Layout (src/routes/(app)/+layout.svelte):
   - Thiết kế menu và dashboard: https://demos.pixinvent.com/vuexy-html-admin-template/html/vertical-menu-template-dark/
   - Sidebar navigation sử dụng shadcn-svelte
   - Top navbar cung cấp chức năng đổi theme, thông báo và người dùng, tham khảo đường dẫn trên
   - User dropdown
   - Breadcrumbs
   - Content area

3. Auth Layout (src/routes/(auth)/+layout.svelte):
   - Layout đơn giản cho trang đăng nhập và xác thực theo giao diện: https://demos.pixinvent.com/vuexy-html-admin-template/html/vertical-menu-template-dark/auth-login-basic.html
   - Giao diện dựa theo shadcn-svelte components

4. Dashboard Layout (src/routes/dashboard/+layout.svelte):
   - Cung cấp cấu trúc cho trang dashboard
   - Sidebar navigation
   - Top navbar
   - Breadcrumbs

Đảm bảo các layout components có responsive design và xử lý các breakpoints khác nhau sử dụng Tailwind CSS.
```

### Prompt 5: Xây dựng UI Components cơ bản với shadcn-svelte

```ini
Tạo các UI components cơ bản dựa trên shadcn-svelte và Tailwind CSS:
1. Buttons (src/lib/components/ui/button/index.ts):
   - Sử dụng shadcn Button với các variants: primary, secondary, destructive, outline, ghost
   - Thêm logic loading state, disabled state

2. Forms (src/lib/components/ui/form/):
   - Sử dụng shadcn Form components
   - Input: Form input với validation support
   - Select: Dropdown select với enhanced search
   - Checkbox: Checkbox component
   - DatePicker: Date picker component

3. Layout Components (src/lib/components/layout/):
   - PageHeader.svelte: Tiêu đề trang và actions
   - PageContainer.svelte: Container với padding & margins
   - SplitView.svelte: Layout 2 cột
   - Breadcrumbs.svelte: Breadcrumb navigation

Đảm bảo tất cả components đều hỗ trợ i18n và tuân thủ design system của shadcn-svelte.
```

### Prompt 6: Xây dựng Data Components

```ini
Tạo các data components để hiển thị và tương tác với dữ liệu:
1. DataTable (src/lib/components/tables/DataTable.svelte):
   - Sử dụng shadcn-svelte Table component
   - Hỗ trợ sorting, pagination, filtering
   - Selection (single, multiple)
   - Export (CSV, Excel)
   - Column customization
   - Empty state, loading state
   - Responsive design

2. Charts (src/lib/components/charts/):
   - LineChart.svelte: Line chart component sử dụng svelte-chartjs
   - BarChart.svelte: Bar chart component
   - PieChart.svelte: Pie chart component
   - AreaChart.svelte: Area chart component
   - Tất cả đều có interactive tooltips và responsive design

3. Status Components:
   - Badge.svelte (từ shadcn-svelte): Badge hiển thị trạng thái với màu sắc tương ứng
   - Timeline.svelte: Timeline hiển thị lịch sử trạng thái

Sử dụng Svelte slots để tạo các component linh hoạt và có thể tùy chỉnh.
```

## 3. Xây dựng các Module Chính

### Prompt 7: Xây dựng Module Authentication

```yaml
Tạo các routes và components cho module authentication:

1. Login View (src/routes/(auth)/login/+page.svelte):
   - Form đăng nhập với validation sử dụng shadcn Form
   - Xử lý error messages thông qua flash messages
   - Remember me functionality
   - Forgot password link

2. Forgot Password View (src/routes/(auth)/forgot-password/+page.svelte):
   - Form quên mật khẩu với shadcn Form
   - Email validation
   - Success/error states

3. Reset Password View (src/routes/(auth)/reset-password/+page.svelte):
   - Form đặt lại mật khẩu
   - Password validation
   - Success/error states

4. Two-Factor Authentication (src/lib/components/auth/TwoFactorForm.svelte):
   - Input cho mã xác thực
   - Countdown timer với Svelte transitions
   - Resend code functionality

Kết nối với các API endpoints thông qua src/routes/[auth]/**/+page.server.ts để xử lý form submissions và authentication logic:
```

### Prompt 8: Xây dựng Dashboard

```ini
Tạo dashboard view (src/routes/dashboard/+page.svelte) với các components sau:

1. KPI Cards (src/lib/components/dashboard/KpiCard.svelte):
   - Hiển thị các chỉ số KPI: Doanh thu, Đơn hàng, Tồn kho, Lợi nhuận
   - So sánh với kỳ trước (% tăng/giảm)
   - Sử dụng Counter trong Svelte để tạo hiệu ứng animated number

2. Revenue Trends Chart (src/lib/components/dashboard/RevenueTrendsChart.svelte):
   - Line chart hiển thị doanh thu theo thời gian sử dụng svelte-chartjs
   - Filter theo ngày/tuần/tháng/năm với shadcn-svelte Tabs
   - Tooltips chi tiết

3. Order Status Distribution (src/lib/components/dashboard/OrderStatusChart.svelte):
   - Pie/donut chart hiển thị phân phối trạng thái đơn hàng
   - Interactive legend

4. Top Selling Products (src/lib/components/dashboard/TopSellingProducts.svelte):
   - Bảng hiển thị các sản phẩm bán chạy sử dụng shadcn-svelte Table
   - Sorting và pagination

5. Recent Orders & Upcoming Shipments (src/lib/components/dashboard/RecentActivityList.svelte):
   - Hiển thị đơn hàng gần đây và lịch trình sắp tới
   - Quick actions với shadcn-svelte DropdownMenu

Kết nối với các API endpoints và sử dụng SvelteKit load function trong +page.server.ts để tải dữ liệu.
```

### Prompt 9: Xây dựng Module Quản lý người dùng

```ini
Tạo các routes và components cho module quản lý người dùng:

1. User List View (src/routes/users/+page.svelte):
   - Filter Bar: lọc theo vai trò, phòng ban, trạng thái sử dụng shadcn-svelte Filter
   - UserTable: bảng hiển thị người dùng với phân trang
   - ActionButtons: thêm, sửa, xóa người dùng
   - ExportOptions: xuất dữ liệu

2. User Detail View (src/routes/users/[id]/+page.svelte):
   - UserProfile: thông tin cá nhân
   - RolePermissions: vai trò và quyền hạn
   - ActivityLog: lịch sử hoạt động
   - TwoFactorSettings: cài đặt xác thực 2 lớp

3. User Create/Edit View (src/routes/users/[id]/edit/+page.svelte và src/routes/users/new/+page.svelte):
   - Form nhập thông tin người dùng với validation
   - RoleSelector: chọn vai trò
   - DepartmentSelector: chọn phòng ban
   - PasswordGenerator: tạo mật khẩu

Kết nối với các API endpoints thông qua +page.server.ts load functions.
```

### Prompt 10: Xây dựng Module Quản lý sản phẩm

```yaml
Tạo các routes và components cho module quản lý sản phẩm:

1. Product List View (src/routes/products/+page.svelte):
   - SearchBar: tìm kiếm sản phẩm với shadcn-svelte Input
   - FilterPanel: lọc theo danh mục, tồn kho, giá
   - ViewToggle: chuyển đổi giữa dạng bảng và lưới sử dụng ToggleGroup
   - ProductTable/ProductGrid: hiển thị sản phẩm
   - BulkActions: thao tác hàng loạt

2. Product Detail View (src/routes/products/[id]/+page.svelte):
   - ProductInfo: thông tin cơ bản
   - PricingInfo: thông tin giá
   - InventoryHistory: lịch sử tồn kho
   - ProductImages: quản lý hình ảnh
   - RelatedProducts: sản phẩm liên quan
   - TagsManager: quản lý nhãn sản phẩm

3. Product Create/Edit View (src/routes/products/[id]/edit/+page.svelte và src/routes/products/new/+page.svelte):
   - Form nhập thông tin sản phẩm với validation
   - CategorySelector: chọn danh mục
   - PricingCalculator: tính toán giá bán
   - ImageUploader: tải lên hình ảnh
   - TagSelector: chọn nhãn

Kết nối với các API endpoints thông qua +page.server.ts load functions.
```

### Prompt 11: Xây dựng Module Quản lý đơn hàng

```yaml
Tạo các routes và components cho module quản lý đơn hàng:

1. Order List View (src/routes/orders/+page.svelte):
   - OrderFilters: lọc theo trạng thái, khách hàng, ngày
   - OrderTable: bảng đơn hàng với phân trang sử dụng shadcn-svelte Table
   - StatusBadge: hiển thị trạng thái đơn hàng với shadcn Badge
   - QuickActions: thao tác nhanh với DropdownMenu
   - OrderSummary: tóm tắt đơn hàng

2. Order Detail View (src/routes/orders/[id]/+page.svelte):
   - OrderHeader: thông tin chung đơn hàng
   - OrderItems: danh sách sản phẩm trong đơn
   - OrderTimeline: timeline trạng thái đơn hàng
   - OrderCosts: chi phí liên quan
   - ShippingInfo: thông tin vận chuyển
   - InvoicePreview: xem trước hóa đơn
   - StatusUpdateForm: cập nhật trạng thái

3. Order Create/Edit View (src/routes/orders/[id]/edit/+page.svelte và src/routes/orders/new/+page.svelte):
   - OrderTypeSelector: chọn loại đơn hàng sử dụng shadcn-svelte RadioGroup
   - PartnerSelector: chọn khách hàng/nhà cung cấp
   - ProductSelector: chọn sản phẩm
   - QuantityEditor: nhập số lượng
   - PricingCalculator: tính toán giá và chi phí
   - ShippingForm: thông tin vận chuyển
   - ContainerSelector: chọn container
   - CostEstimator: ước tính chi phí

Kết nối với các API endpoints thông qua +page.server.ts load functions.
```

### Prompt 12: Xây dựng Module Quản lý container

```yaml
Tạo các routes và components cho module quản lý container:

1. Container List View (src/routes/containers/+page.svelte):
   - ContainerFilters: lọc theo loại, trạng thái, vị trí
   - ContainerTable: bảng container với phân trang
   - StatusIndicator: hiển thị trạng thái container với shadcn-svelte Badge
   - CapacityIndicator: hiển thị khả năng chứa với Progress component
   - LocationMap: bản đồ vị trí container

2. Container Detail View (src/routes/containers/[id]/+page.svelte):
   - ContainerInfo: thông tin cơ bản
   - ContainerContent: nội dung chứa
   - ContainerDimensions: kích thước container
   - ShippingSchedule: lịch trình vận chuyển
   - CostBreakdown: chi tiết chi phí
   - OrdersList: danh sách đơn hàng trong container
   - StatusHistory: lịch sử trạng thái

3. Container Create/Edit View (src/routes/containers/[id]/edit/+page.svelte và src/routes/containers/new/+page.svelte):
   - ContainerForm: form thông tin container
   - ContainerTypeSelector: chọn loại container
   - DimensionsCalculator: tính toán kích thước
   - CostEstimator: ước tính chi phí
   - LocationSelector: chọn vị trí

Kết nối với các API endpoints thông qua +page.server.ts load functions.
```

## 4. Xây dựng các Module Phụ trợ

### Prompt 13: Xây dựng Module Quản lý chi phí

```yaml
Tạo các routes và components cho module quản lý chi phí:

1. Cost List View (src/routes/costs/+page.svelte):
   - CostFilters: lọc theo loại, đơn hàng, container
   - CostTable: bảng chi phí với phân trang sử dụng shadcn-svelte Table
   - CostSummary: tổng hợp chi phí
   - CostChart: biểu đồ phân tích chi phí
   - ExportOptions: xuất dữ liệu

2. Cost Detail View (src/routes/costs/[id]/+page.svelte):
   - CostInfo: thông tin chi tiết
   - RelatedOrders: đơn hàng liên quan
   - RelatedContainers: container liên quan
   - DocumentViewer: xem chứng từ
   - CurrencyConverter: chuyển đổi tiền tệ

3. Cost Create/Edit View (src/routes/costs/[id]/edit/+page.svelte và src/routes/costs/new/+page.svelte):
   - CostForm: form thông tin chi phí
   - CostTypeSelector: chọn loại chi phí
   - OrderSelector: chọn đơn hàng liên quan
   - ContainerSelector: chọn container liên quan
   - CurrencySelector: chọn loại tiền tệ
   - DocumentUploader: tải lên chứng từ

Kết nối với các API endpoints thông qua +page.server.ts load functions.
```

### Prompt 14: Xây dựng Module Quản lý danh mục

```yaml
Tạo các routes và components cho module quản lý danh mục:

1. Category List View (src/routes/categories/+page.svelte):
   - CategoryTree: hiển thị cây danh mục với shadcn-svelte Tree component
   - CategoryActions: thêm, sửa, xóa
   - CategoryStats: thống kê sản phẩm theo danh mục
   - SearchCategory: tìm kiếm danh mục

2. Category Detail View (src/routes/categories/[id]/+page.svelte):
   - CategoryInfo: thông tin danh mục
   - SubcategoriesList: danh sách danh mục con
   - ProductsList: danh sách sản phẩm thuộc danh mục
   - CategoryMetrics: số liệu thống kê

3. Category Create/Edit View (src/routes/categories/[id]/edit/+page.svelte và src/routes/categories/new/+page.svelte):
   - CategoryForm: form thông tin danh mục
   - ParentCategorySelector: chọn danh mục cha
   - CategoryCodeGenerator: tạo mã danh mục

Kết nối với các API endpoints thông qua +page.server.ts load functions.
```

### Prompt 15: Xây dựng Module Quản lý đối tác

```ini
Tạo các routes và components cho module quản lý đối tác:

1. Partner List View (src/routes/partners/+page.svelte):
   - PartnerFilters: lọc theo loại, quốc gia, trạng thái
   - PartnerTable: bảng đối tác với phân trang
   - PartnerMap: bản đồ phân bố đối tác
   - PartnerStats: thống kê đối tác
   - ExportOptions: xuất dữ liệu

2. Partner Detail View (src/routes/partners/[id]/+page.svelte):
   - PartnerInfo: thông tin cơ bản
   - ContactInfo: thông tin liên hệ
   - OrderHistory: lịch sử đơn hàng
   - BalanceInfo: thông tin công nợ
   - DocumentsList: danh sách tài liệu
   - TagsManager: quản lý nhãn đối tác

3. Partner Create/Edit View (src/routes/partners/[id]/edit/+page.svelte và src/routes/partners/new/+page.svelte):
   - PartnerForm: form thông tin đối tác
   - PartnerTypeSelector: chọn loại đối tác
   - AddressForm: form địa chỉ
   - ContactForm: form thông tin liên hệ
   - BankInfoForm: form thông tin ngân hàng
   - TagSelector: chọn nhãn

Kết nối với các API endpoints thông qua +page.server.ts load functions.
```

### Prompt 16: Xây dựng Module Quản lý tồn kho

```yaml
Tạo các routes và components cho module quản lý tồn kho:

1. Inventory Transaction List View (src/routes/inventory/+page.svelte):
   - InventoryTransactionFilters: lọc theo loại giao dịch, sản phẩm, thời gian
   - InventoryTransactionTable: bảng giao dịch tồn kho
   - TransactionTypeBadge: hiển thị loại giao dịch với shadcn-svelte Badge
   - QuantityIndicator: hiển thị số lượng nhập/xuất
   - ProductInfo: thông tin sản phẩm
   - OrderInfo: thông tin đơn hàng liên quan
   - ExportOptions: xuất dữ liệu

2. Inventory Transaction Detail View (src/routes/inventory/[id]/+page.svelte):
   - TransactionInfo: thông tin giao dịch
   - ProductInfo: thông tin sản phẩm
   - OrderInfo: thông tin đơn hàng liên quan
   - WarehouseInfo: thông tin kho
   - AuditTrail: lịch sử thay đổi
   - DocumentViewer: xem chứng từ

3. Inventory Transaction Create/Edit View (src/routes/inventory/[id]/edit/+page.svelte và src/routes/inventory/new/+page.svelte):
   - TransactionForm: form thông tin giao dịch
   - TransactionTypeSelector: chọn loại giao dịch
   - ProductSelector: chọn sản phẩm
   - QuantityInput: nhập số lượng
   - WarehouseSelector: chọn kho
   - OrderSelector: chọn đơn hàng liên quan
   - DocumentUploader: tải lên chứng từ

Kết nối với các API endpoints thông qua +page.server.ts load functions.
```

## 5. Testing, i18n và Deployment

### Prompt 17: Thiết lập đa ngôn ngữ với svelte-i18n

```ini
Thiết lập hỗ trợ đa ngôn ngữ cho ứng dụng:

1. Tạo cấu trúc i18n trong src/lib/i18n/index.ts:
   - Thiết lập svelte-i18n
   - Tạo các file ngôn ngữ (vi.tson, en.tson)
   - Thêm cơ chế lazy loading cho file ngôn ngữ

2. Tạo các file ngôn ngữ cho các module chính:
   - src/lib/i18n/locales/vi/common.tson: Các text chung
   - src/lib/i18n/locales/vi/auth.tson: Text cho module authentication
   - src/lib/i18n/locales/vi/products.tson: Text cho module sản phẩm
   - src/lib/i18n/locales/vi/orders.tson: Text cho module đơn hàng
   - Tương tự cho tiếng Anh và các ngôn ngữ khác

3. Tạo Language Selector component (src/lib/components/ui/LanguageSelector.svelte):
   - Dropdown chọn ngôn ngữ
   - Lưu ngôn ngữ đã chọn vào local storage

4. Cập nhật tất cả các components để sử dụng i18n:
   - Thay thế các hardcoded text bằng $_("key")
   - Đảm bảo tất cả text đều có trong file ngôn ngữ

5. Tạo hook trong +layout.svelte để khởi tạo i18n với ngôn ngữ đã lưu
```

### Prompt 18: Thiết lập Unit Testing với Vitest

```ini
Thiết lập unit testing cho các components và utils:

1. Cấu hình Vitest trong svelte.config.ts/vite.config.ts:
   - Thêm cấu hình test
   - Thiết lập test coverage
   - Cấu hình svelte và jsdom

2. Tạo các test cho UI components cơ bản:
   - src/lib/components/ui/button/Button.test.ts
   - src/lib/components/ui/form/Input.test.ts
   - src/lib/components/ui/form/Select.test.ts

3. Tạo các test cho utility functions:
   - src/lib/utils/formatters.test.ts
   - src/lib/utils/validators.test.ts
   - src/lib/utils/permissions.test.ts

4. Tạo các test cho stores:
   - src/lib/stores/auth.test.ts
   - src/lib/stores/filters.test.ts

5. Sử dụng @testing-library/svelte để test components như thật

6. Thiết lập mock cho các API calls trong tests
```

### Prompt 19: Thiết lập End-to-End Testing với Playwright

```ini
Thiết lập E2E testing cho các luồng chính của ứng dụng:

1. Cấu hình Playwright trong e2e/playwright.config.ts:
   - Thiết lập các browsers cần test
   - Cấu hình screenshots và videos
   - Thiết lập test reports

2. Tạo các test cases cho các luồng chính:
   - e2e/tests/auth.spec.ts: Test đăng nhập, đăng xuất
   - e2e/tests/products.spec.ts: Test thêm, sửa, xóa sản phẩm
   - e2e/tests/orders.spec.ts: Test tạo và quản lý đơn hàng
   - e2e/tests/dashboard.spec.ts: Test hiển thị dashboard

3. Tạo các Page Objects để tái sử dụng logic testing:
   - e2e/pages/LoginPage.ts
   - e2e/pages/DashboardPage.ts
   - e2e/pages/ProductsPage.ts
   - e2e/pages/OrdersPage.ts

4. Sử dụng Playwright với SvelteKit để dễ dàng tạo test cho routing và load functions
```

### Prompt 20: Triển khai Production Build và Optimization

```yaml
Cấu hình production build và tối ưu hiệu năng:

1. Cấu hình SvelteKit cho production build trong svelte.config.ts:
   - Thiết lập adapter-auto hoặc adapter-node cho deployment
   - Cấu hình server-side rendering và hydration
   - Thiết lập code splitting
   - Tối ưu bundle size

2. Tạo Docker setup cho deployment:
   - Dockerfile cho build và deployment
   - Docker Compose cho development environment
   - Nginx configuration cho production server

3. Cài đặt Progressive Web App với service-worker của SvelteKit:
   - Cấu hình offline support
   - Web app manifest
   - App shell architecture

4. Tối ưu performance:
   - Sử dụng Svelte.preload và SvelteKit prefetch
   - Tận dụng SSR và hydration strategy
   - Code splitting tự động dựa trên routes
   - Lazy-loading cho images và components
   - Caching strategies

5. Cấu hình CI/CD với GitHub Actions hoặc GitLab CI
```

## 6. Tối ưu hiệu suất và Code chất lượng

### Prompt 21: Tối ưu hiệu suất và Rendering

```yaml
Triển khai các kỹ thuật tối ưu hiệu suất cho ứng dụng Svelte:

1. Tối ưu reactive rendering:
   - Tận dụng reactive declarations ($: statements) để tối ưu re-renders
   - Sử dụng {{#key}} blocks để kiểm soát re-rendering
   - Chia nhỏ components để giảm render scope
   - Sử dụng immutable data patterns để tận dụng Svelte change detection

2. Nâng cao performance với virtualization:
   - Áp dụng svelte-virtual-list cho các danh sách dài
   - Tích hợp skeleton loading với virtual list
   - Incremental loading cho large datasets

3. Tối ưu Svelte stores:
   - Phân chia stores theo functionality
   - Sử dụng derived stores thay vì computed values
   - Tracking store subscriptions và cleanup
   - Sử dụng store contracts pattern

4. Thiết lập component lazy loading:
   - Sử dụng dynamic imports với SvelteKit
   - Áp dụng loading skeletons cho các components chính

5. Tối ưu transitions và animations:
   - Sử dụng hardware-accelerated properties
   - Giảm thiểu layout thrashing
   - Áp dụng FLIP animation technique
   - Tận dụng Svelte transitions và animations
```

### Prompt 22: Code Quality và Best Practices

```yaml
Triển khai các kỹ thuật và công cụ để đảm bảo chất lượng code:

1. Thiết lập TypeScript cho Svelte:
   - Cấu hình tsconfig.tson với strict mode
   - Sử dụng <script lang="ts">
   - Tạo các interface và type cho các đối tượng chính
   - Áp dụng generic types cho các components và stores

2. Tối ưu actions và helpers:
   - Tạo src/lib/actions/ cho Svelte actions tái sử dụng
   - clickOutside: Xử lý click bên ngoài component
   - inView: Theo dõi element visibility
   - pannable: Thêm touch và drag support
   - src/lib/helpers/ cho các utility functions

3. Áp dụng ESLint và Prettier:
   - Cài đặt eslint-plugin-svelte với recommended ruleset
   - Kết hợp với TypeScript ESLint
   - Thiết lập pre-commit hooks với husky
   - Cấu hình Prettier cho .svelte files

4. Tối ưu CSS và styling:
   - Sử dụng CSS variables cho theme
   - Áp dụng Tailwind JIT mode
   - Tận dụng :global và :local modifiers
   - Sử dụng component-specific styles

5. Tối ưu state management:
   - Áp dụng context API khi cần
   - Tổ chức stores theo domain
   - Sử dụng action/reducer pattern cho complex state
   - Thiết lập local store persistence khi cần thiết
```

### Prompt 23: Code Splitting và Bundle Optimization

```yaml
Triển khai chiến lược code splitting và tối ưu bundle size:

1. Tận dụng SvelteKit code splitting:
   - Automatic code-splitting dựa trên routes
   - Sử dụng dynamic imports cho các module lớn
   - Áp dụng Intersection Observer để preload components

2. Tối ưu import dependencies:
   - Sử dụng tree-shaking triệt để
   - Import cụ thể từ lodash, date-fns và các thư viện lớn
   - Sử dụng rollup-plugin-analyzer để kiểm tra bundle
   - Xử lý các dependencies lớn với module federation

3. Tối ưu assets:
   - Sử dụng vite-imagetools cho image optimization
   - Áp dụng responsive images với srcset
   - Lazy loading với loading="lazy"
   - Preload critical assets

4. Thiết lập caching strategy:
   - Áp dụng ETag và cache headers
   - Sử dụng content hashing cho file names
   - Áp dụng service worker caching

5. Tận dụng SvelteKit rendering options:
   - Sử dụng SSR khi cần SEO
   - Áp dụng hybrid rendering với page.server.ts
   - Cấu hình tradeoff giữa SSR và CSR
   - Sử dụng streaming rendering khi có thể
```

### Prompt 24: Memory Management và Memoization

```yaml
Triển khai các kỹ thuật quản lý bộ nhớ và memoization:

1. Tối ưu memory footprint:
   - Theo dõi memory leaks với Chrome DevTools
   - Đảm bảo cleanup trong onDestroy lifecycle
   - Unsubscribe từ tất cả stores
   - Xóa event listeners khi component unmount

2. Áp dụng memoization cho các tính toán phức tạp:
   - Tạo memoized helpers cho reactive calculations
   - Sử dụng svelte-memo cho intensive computations
   - Sử dụng reactive declarations ($:) một cách hiệu quả
   - Sử dụng web workers cho heavy computations

3. Tối ưu chart rendering:
   - Áp dụng incremental chart updates
   - Sử dụng web workers cho data processing
   - Giảm thiểu re-renders cho charts
   - Canvas thay vì SVG cho large datasets

4. Thiết lập pagination và filtering tối ưu:
   - Áp dụng server-side pagination với load functions
   - Sử dụng debounce cho search inputs
   - Thiết lập caching cho result sets
   - Sử dụng URL params để lưu trạng thái filter và pagination

5. Tối ưu rendering cho large lists:
   - Áp dụng svelte-virtual-list
   - Incremental rendering với setTimeout và requestAnimationFrame
   - Tách list rendering thành chunks
   - Sử dụng {#key} blocks cho targeted updates
```

## 7. Tổng kết

Với các prompt trên, bạn đã có thể triển khai một hệ thống frontend hoàn chỉnh sử dụng Svelte, SvelteKit và shadcn-svelte. Các bước triển khai được chia thành các prompts chi tiết, mỗi prompt tập trung vào một khía cạnh cụ thể của hệ thống. Cấu trúc này cho phép bạn:

1. Bắt đầu từ cấu hình cơ bản của dự án SvelteKit
2. Xây dựng các layout và component design system với shadcn-svelte
3. Triển khai các module chính của hệ thống theo kiến trúc SvelteKit
4. Thêm các module phụ trợ
5. Hoàn thiện với testing, i18n và deployment
6. Áp dụng các kỹ thuật tối ưu hiệu suất riêng cho Svelte

Các prompt đã bao gồm đầy đủ các functionality được yêu cầu và được tổ chức theo quy trình phát triển phần mềm hiện đại, đặc biệt là tận dụng các ưu điểm của Svelte và SvelteKit như: file-based routing, server-side rendering, $: reactive declarations, actions, và lifecycle hooks.