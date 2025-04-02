# Tài liệu Frontend Export Management System

## Tổng quan hệ thống Frontend

Hệ thống Frontend Export Management System được phát triển bằng Vue 3 và Element Plus để tạo giao diện người dùng hiện đại, tối ưu và dễ sử dụng cho hệ thống quản lý xuất khẩu. Giao diện được thiết kế để hoạt động đồng bộ với Backend API, cung cấp trải nghiệm người dùng mượt mà và hiệu quả.

## Công nghệ và Framework

- **Framework:** Vue 3
- **UI Library:** Element Plus https://element-plus.org/en-US/guide/design.html
- **State Management:** Pinia
- **Router:** Vue Router
- **API Client:** Axios với interceptors
- **Biểu đồ và Visualization:** Chart.js
- **Internationalization:** i18n
- **Testing:** Vitest, Playwright
- **Build Tool:** Vite

## Cấu trúc thư mục

```ini
src/
├── App.vue                # Component gốc
├── main.js                # Entry point
├── router/                # Vue Router configuration
│   └── index.js           # Router setup và routes
├── views/                 # Vue components cho các trang
│   ├── layout/            # Layout chính
│   ├── auth/              # Các trang xác thực
│   ├── dashboard/         # Trang dashboard
│   ├── users/             # Quản lý người dùng
│   ├── products/          # Quản lý sản phẩm
│   ├── orders/            # Quản lý đơn hàng
│   ├── containers/        # Quản lý container
│   ├── costs/             # Quản lý chi phí
│   ├── categories/        # Quản lý danh mục
│   ├── partners/          # Quản lý đối tác
│   ├── tags/              # Quản lý tags
│   ├── schedules/         # Quản lý lịch trình vận chuyển
│   ├── inventory/         # Quản lý tồn kho
│   └── reports/           # Báo cáo và thống kê
├── components/            # Vue components tái sử dụng
│   ├── ui/                # UI components cơ bản
│   ├── layout/            # Layout components
│   ├── tables/            # Data tables components
│   ├── forms/             # Form components
│   ├── charts/            # Chart components
│   └── modals/            # Modal và dialog components
├── stores/                # Pinia stores
│   ├── auth.js            # Authentication store
│   ├── theme.js           # Theme store
│   ├── toast.js           # Toast notifications store
│   └── filters.js         # Filter và sorting store
├── api/                   # API client
│   ├── client.js          # Axios instance và interceptors
│   ├── auth.js            # Auth API
│   ├── users.js           # User API
│   ├── products.js        # Product API
│   └── ...                # Các module API khác
├── utils/                 # Utility functions
│   ├── formatters.js      # Date, currency formatters
│   ├── validators.js      # Form validation
│   ├── permissions.js     # Permission helpers
│   └── localStorage.js    # Local storage helpers
├── constants/             # Constants
└── assets/               # Static assets
    ├── images/            # Images
    ├── styles/            # Global styles
    └── icons/             # SVG icons
```

## Kiến trúc Frontend

### 1. Cấu trúc Layout

Hệ thống sử dụng layout phân cấp để đảm bảo trải nghiệm người dùng nhất quán:

- **Root Layout**: Cung cấp cấu trúc cơ bản của ứng dụng, bao gồm:

   - Theme provider
   - Authentication wrapper
   - Toast notifications
   - Error boundaries

- **App Layout**: Layout chính cho người dùng đã đăng nhập, bao gồm:

   - Sidebar navigation
   - Top navbar
   - User dropdown
   - Breadcrumbs
   - Content area

- **Auth Layout**: Layout đơn giản cho trang đăng nhập và xác thực, không có sidebar

### 2. Quản lý trạng thái (State Management)

- Sử dụng **Pinia** cho global state
- **Provide/Inject** cho component trees
- **Composition API** với `ref`, `reactive`, và `computed`
- **Watchers** với `watch` và `watchEffect`
- **Composables** cho logic tái sử dụng

### 3. Tích hợp API

Sử dụng Axios với custom interceptors để xử lý:

- Authentication headers
- Token refresh
- Error handling
- Loading states
- Response transformation
- Retry logic cho các request thất bại
- Caching cho các request phổ biến
- Offline support với local storage
- Batch requests để giảm số lượng HTTP calls
- Debounce và throttle cho các request thường xuyên

### 4. Component Design System

Hệ thống được xây dựng trên Element Plus, mở rộng với custom components:

#### 4.1 UI Components cơ bản

- Button (primary, secondary, danger, success, etc.)
- Input, Select, Checkbox, Radio
- Badge, Card, Alert
- Modal, Drawer, Tooltip
- Tabs, Accordion
- Date picker, Time picker
- File uploader
- Progress indicators

#### 4.2 Data Components

- **DataTable**: Table component với các tính năng:

   - Sorting
   - Pagination
   - Filtering
   - Selection (single, multiple)
   - Export (CSV, Excel)
   - Column customization
   - Responsive design

- **Charts**: Biểu đồ hiển thị dữ liệu

   - Line chart
   - Bar chart
   - Pie chart
   - Area chart
   - Combined charts
   - Interactive tooltips

#### 4.3 Layout Components

- **PageHeader**: Tiêu đề trang và actions
- **PageContainer**: Container với padding & margins
- **SplitView**: Layout 2 cột
- **Tabs**: Tabs navigation
- **Sidebar**: Sidebar navigation
- **Breadcrumbs**: Breadcrumb navigation

### 5. Mô tả các trang chính

#### 5.1 Dashboard (Trang chủ)

**Mô tả**: Hiển thị tổng quan về hoạt động kinh doanh, bao gồm các chỉ số KPI và biểu đồ phân tích.

**Components**:

- KPI Cards (Doanh thu, Đơn hàng, Tồn kho, Lợi nhuận)
- Revenue Trends Chart (biểu đồ xu hướng doanh thu)
- Order Status Distribution (phân phối trạng thái đơn hàng)
- Top Selling Products (sản phẩm bán chạy)
- Low Stock Alerts (cảnh báo tồn kho thấp)
- Recent Orders (đơn hàng gần đây)
- Upcoming Shipments (lịch trình sắp tới)

**API Endpoints**:

- GET /api/dashboard/overview
- GET /api/dashboard/kpi
- GET /api/dashboard/charts/trends
- GET /api/dashboard/alerts

#### 5.2 Quản lý người dùng

##### 5.2.1 Danh sách người dùng (User List)

**Mô tả**: Hiển thị danh sách người dùng với phân trang và bộ lọc.

**Components**:

- Filter Bar (lọc theo vai trò, phòng ban, trạng thái)
- UserTable (hiển thị danh sách người dùng)
- ActionButtons (thêm, sửa, xóa)
- ExportOptions (xuất dữ liệu)

**API Endpoints**:

- GET /api/users

##### 5.2.2 Chi tiết người dùng (User Detail)

**Mô tả**: Hiển thị và chỉnh sửa thông tin chi tiết của người dùng.

**Components**:

- UserProfile (thông tin cá nhân)
- RolePermissions (vai trò và quyền hạn)
- ActivityLog (lịch sử hoạt động)
- TwoFactorSettings (cài đặt xác thực 2 lớp)

**API Endpoints**:

- GET /api/users/{id}
- PUT /api/users/{id}
- PUT /api/users/{id}/role

##### 5.2.3 Tạo người dùng mới (User Create)

**Mô tả**: Form tạo người dùng mới với xác thực dữ liệu.

**Components**:

- UserForm (form nhập thông tin)
- RoleSelector (chọn vai trò)
- DepartmentSelector (chọn phòng ban)
- PasswordGenerator (tạo mật khẩu)

**API Endpoints**:

- POST /api/users

#### 5.3 Quản lý sản phẩm

##### 5.3.1 Danh sách sản phẩm (Product List)

**Mô tả**: Hiển thị danh sách sản phẩm với phân trang, tìm kiếm và lọc.

**Components**:

- SearchBar (tìm kiếm sản phẩm)
- FilterPanel (lọc theo danh mục, tồn kho, giá)
- ProductTable (bảng sản phẩm)
- ProductGrid (hiển thị dạng lưới với hình ảnh)
- InventoryStatus (trạng thái tồn kho)
- BulkActions (thao tác hàng loạt)

**API Endpoints**:

- GET /api/products
- GET /api/products/search
- GET /api/products/category/{category_id}

##### 5.3.2 Chi tiết sản phẩm (Product Detail)

**Mô tả**: Hiển thị và chỉnh sửa thông tin chi tiết sản phẩm.

**Components**:

- ProductInfo (thông tin cơ bản)
- PricingInfo (thông tin giá)
- InventoryHistory (lịch sử tồn kho)
- ProductImages (quản lý hình ảnh)
- RelatedProducts (sản phẩm liên quan)
- TagsManager (quản lý nhãn sản phẩm)

**API Endpoints**:

- GET /api/products/{id}
- PUT /api/products/{id}
- GET /api/products/{id}/inventory

##### 5.3.3 Tạo sản phẩm mới (Product Create)

**Mô tả**: Form tạo sản phẩm mới với đầy đủ thông tin và xác thực.

**Components**:

- ProductForm (form thông tin sản phẩm)
- CategorySelector (chọn danh mục)
- PricingCalculator (tính toán giá bán)
- ImageUploader (tải lên hình ảnh)
- TagSelector (chọn nhãn)

**API Endpoints**:

- POST /api/products

#### 5.4 Quản lý đơn hàng

##### 5.4.1 Danh sách đơn hàng (Order List)

**Mô tả**: Hiển thị danh sách đơn hàng với các bộ lọc và trạng thái.

**Components**:

- OrderFilters (lọc theo trạng thái, khách hàng, ngày)
- OrderTable (bảng đơn hàng)
- StatusBadge (hiển thị trạng thái đơn hàng)
- QuickActions (thao tác nhanh)
- OrderSummary (tóm tắt đơn hàng)

**API Endpoints**:

- GET /api/orders
- GET /api/orders/search
- GET /api/orders/by-partner/{partner_id}

##### 5.4.2 Chi tiết đơn hàng (Order Detail)

**Mô tả**: Hiển thị chi tiết đơn hàng, sản phẩm, chi phí và theo dõi trạng thái.

**Components**:

- OrderHeader (thông tin chung đơn hàng)
- OrderItems (danh sách sản phẩm trong đơn)
- OrderTimeline (timeline trạng thái đơn hàng)
- OrderCosts (chi phí liên quan)
- ShippingInfo (thông tin vận chuyển)
- InvoicePreview (xem trước hóa đơn)
- StatusUpdateForm (cập nhật trạng thái)

**API Endpoints**:

- GET /api/orders/{id}
- GET /api/orders/{id}/items
- PUT /api/orders/{id}/status
- PUT /api/orders/{id}/payment
- GET /api/costs/by-order/{order_id}

##### 5.4.3 Tạo đơn hàng mới (Order Create)

**Mô tả**: Form tạo đơn hàng mới với chọn sản phẩm, khách hàng và thông tin vận chuyển.

**Components**:

- OrderTypeSelector (chọn loại đơn hàng)
- PartnerSelector (chọn khách hàng/nhà cung cấp)
- ProductSelector (chọn sản phẩm)
- QuantityEditor (nhập số lượng)
- PricingCalculator (tính toán giá và chi phí)
- ShippingForm (thông tin vận chuyển)
- ContainerSelector (chọn container)
- CostEstimator (ước tính chi phí)

**API Endpoints**:

- POST /api/orders
- GET /api/products
- GET /api/partners
- GET /api/containers/available

#### 5.5 Quản lý Container

##### 5.5.1 Danh sách Container (Container List)

**Mô tả**: Hiển thị danh sách container với trạng thái và thông tin cơ bản.

**Components**:

- ContainerFilters (lọc theo loại, trạng thái, vị trí)
- ContainerTable (bảng container)
- StatusIndicator (hiển thị trạng thái container)
- CapacityIndicator (hiển thị khả năng chứa)
- LocationMap (bản đồ vị trí container)

**API Endpoints**:

- GET /api/containers
- GET /api/containers/search
- GET /api/containers/available

##### 5.5.2 Chi tiết Container (Container Detail)

**Mô tả**: Hiển thị thông tin chi tiết container, bao gồm đơn hàng và lịch trình.

**Components**:

- ContainerInfo (thông tin cơ bản)
- ContainerContent (nội dung chứa)
- ContainerDimensions (kích thước container)
- ShippingSchedule (lịch trình vận chuyển)
- CostBreakdown (chi tiết chi phí)
- OrdersList (danh sách đơn hàng trong container)
- StatusHistory (lịch sử trạng thái)

**API Endpoints**:

- GET /api/containers/{id}
- GET /api/containers/{id}/orders
- GET /api/containers/{id}/costs
- GET /api/schedules?container_id={id}

##### 5.5.3 Tạo Container mới (Container Create)

**Mô tả**: Form tạo container mới với thông tin cơ bản.

**Components**:

- ContainerForm (form thông tin container)
- ContainerTypeSelector (chọn loại container)
- DimensionsCalculator (tính toán kích thước)
- CostEstimator (ước tính chi phí)
- LocationSelector (chọn vị trí)

**API Endpoints**:

- POST /api/containers

#### 5.6 Quản lý Chi phí

##### 5.6.1 Danh sách Chi phí (Cost List)

**Mô tả**: Hiển thị danh sách chi phí với các bộ lọc và phân loại.

**Components**:

- CostFilters (lọc theo loại, đơn hàng, container)
- CostTable (bảng chi phí)
- CostSummary (tổng hợp chi phí)
- CostChart (biểu đồ phân tích chi phí)
- ExportOptions (xuất dữ liệu)

**API Endpoints**:

- GET /api/costs
- GET /api/costs/by-order/{order_id}
- GET /api/costs/by-container/{container_id}

##### 5.6.2 Chi tiết Chi phí (Cost Detail)

**Mô tả**: Hiển thị thông tin chi tiết chi phí và các liên kết.

**Components**:

- CostInfo (thông tin chi tiết)
- RelatedOrders (đơn hàng liên quan)
- RelatedContainers (container liên quan)
- DocumentViewer (xem chứng từ)
- CurrencyConverter (chuyển đổi tiền tệ)

**API Endpoints**:

- GET /api/costs/{id}

##### 5.6.3 Tạo Chi phí mới (Cost Create)

**Mô tả**: Form tạo chi phí mới với các thông tin liên quan.

**Components**:

- CostForm (form thông tin chi phí)
- CostTypeSelector (chọn loại chi phí)
- OrderSelector (chọn đơn hàng liên quan)
- ContainerSelector (chọn container liên quan)
- CurrencySelector (chọn loại tiền tệ)
- DocumentUploader (tải lên chứng từ)

**API Endpoints**:

- POST /api/costs

#### 5.7 Quản lý Danh mục

##### 5.7.1 Danh sách Danh mục (Category List)

**Mô tả**: Hiển thị cấu trúc cây danh mục sản phẩm.

**Components**:

- CategoryTree (hiển thị cây danh mục)
- CategoryActions (thêm, sửa, xóa)
- CategoryStats (thống kê sản phẩm theo danh mục)
- SearchCategory (tìm kiếm danh mục)

**API Endpoints**:

- GET /api/categories
- GET /api/categories/tree
- GET /api/categories/statistics

##### 5.7.2 Chi tiết Danh mục (Category Detail)

**Mô tả**: Hiển thị thông tin chi tiết danh mục và sản phẩm thuộc danh mục.

**Components**:

- CategoryInfo (thông tin danh mục)
- SubcategoriesList (danh sách danh mục con)
- ProductsList (danh sách sản phẩm thuộc danh mục)
- CategoryMetrics (số liệu thống kê)

**API Endpoints**:

- GET /api/categories/{id}
- GET /api/categories/{id}/products

##### 5.7.3 Tạo Danh mục mới (Category Create)

**Mô tả**: Form tạo danh mục mới với thông tin cơ bản.

**Components**:

- CategoryForm (form thông tin danh mục)
- ParentCategorySelector (chọn danh mục cha)
- CategoryCodeGenerator (tạo mã danh mục)

**API Endpoints**:

- POST /api/categories

#### 5.8 Quản lý Đối tác

##### 5.8.1 Danh sách Đối tác (Partner List)

**Mô tả**: Hiển thị danh sách đối tác (khách hàng, nhà cung cấp) với các bộ lọc.

**Components**:

- PartnerFilters (lọc theo loại, quốc gia, trạng thái)
- PartnerTable (bảng đối tác)
- PartnerMap (bản đồ phân bố đối tác)
- PartnerStats (thống kê đối tác)
- ExportOptions (xuất dữ liệu)

**API Endpoints**:

- GET /api/partners
- GET /api/partners/customers
- GET /api/partners/suppliers

##### 5.8.2 Chi tiết Đối tác (Partner Detail)

**Mô tả**: Hiển thị thông tin chi tiết đối tác và lịch sử giao dịch.

**Components**:

- PartnerInfo (thông tin cơ bản)
- ContactInfo (thông tin liên hệ)
- OrderHistory (lịch sử đơn hàng)
- BalanceInfo (thông tin công nợ)
- DocumentsList (danh sách tài liệu)
- TagsManager (quản lý nhãn đối tác)

**API Endpoints**:

- GET /api/partners/{id}
- GET /api/partners/{id}/orders
- GET /api/partners/{id}/balance

##### 5.8.3 Tạo Đối tác mới (Partner Create)

**Mô tả**: Form tạo đối tác mới với thông tin đầy đủ.

**Components**:

- PartnerForm (form thông tin đối tác)
- PartnerTypeSelector (chọn loại đối tác)
- AddressForm (form địa chỉ)
- ContactForm (form thông tin liên hệ)
- BankInfoForm (form thông tin ngân hàng)
- TagSelector (chọn nhãn)

**API Endpoints**:

- POST /api/partners

#### 5.9 Quản lý Tags

##### 5.9.1 Danh sách Tags (Tag List)

**Mô tả**: Hiển thị danh sách tags với phân loại và thống kê sử dụng.

**Components**:

- TagFilters (lọc theo loại đối tượng, màu sắc)
- TagTable (bảng tags)
- TagCloud (đám mây tags)
- TagUsageStats (thống kê sử dụng)
- ColorPalette (bảng màu tags)

**API Endpoints**:

- GET /api/tags
- GET /api/tags/by-entity-type/{type}
- GET /api/tags/usage-stats

##### 5.9.2 Chi tiết Tag (Tag Detail)

**Mô tả**: Hiển thị thông tin chi tiết tag và các đối tượng được gắn tag.

**Components**:

- TagInfo (thông tin tag)
- EntityList (danh sách đối tượng được gắn tag)
- UsageMetrics (số liệu sử dụng)
- ColorPicker (chọn màu tag)

**API Endpoints**:

- GET /api/tags/{id}
- GET /api/tags/filter/{entity_type}?tag_id={id}

##### 5.9.3 Tạo Tag mới (Tag Create)

**Mô tả**: Form tạo tag mới với thông tin cơ bản.

**Components**:

- TagForm (form thông tin tag)
- EntityTypeSelector (chọn loại đối tượng)
- ColorPicker (chọn màu tag)
- DefaultToggle (đặt làm tag mặc định)

**API Endpoints**:

- POST /api/tags

#### 5.10 Quản lý Lịch trình vận chuyển

##### 5.10.1 Danh sách Lịch trình (Schedule List)

**Mô tả**: Hiển thị danh sách lịch trình vận chuyển với các bộ lọc và timeline.

**Components**:

- ScheduleFilters (lọc theo trạng thái, container, cảng)
- ScheduleTable (bảng lịch trình)
- ScheduleTimeline (timeline lịch trình)
- ScheduleMap (bản đồ lộ trình)
- ScheduleCalendar (lịch vận chuyển)

**API Endpoints**:

- GET /api/schedules
- GET /api/schedules/upcoming

##### 5.10.2 Chi tiết Lịch trình (Schedule Detail)

**Mô tả**: Hiển thị thông tin chi tiết lịch trình và theo dõi hành trình.

**Components**:

- ScheduleInfo (thông tin cơ bản)
- RouteMap (bản đồ lộ trình)
- StatusTimeline (timeline trạng thái)
- ContainerInfo (thông tin container)
- OrdersList (danh sách đơn hàng)
- WeatherInfo (thông tin thời tiết trên lộ trình)
- NotificationSettings (cài đặt thông báo)

**API Endpoints**:

- GET /api/schedules/{id}
- GET /api/schedules/{id}/tracking

##### 5.10.3 Tạo Lịch trình mới (Schedule Create)

**Mô tả**: Form tạo lịch trình vận chuyển mới.

**Components**:

- ScheduleForm (form thông tin lịch trình)
- ContainerSelector (chọn container)
- PortSelector (chọn cảng đi/đến)
- DateRangePicker (chọn thời gian)
- CarrierSelector (chọn hãng vận chuyển)
- NotificationSettings (cài đặt thông báo)

**API Endpoints**:

- POST /api/schedules

### 5.11. Quy trình quản lý tồn kho (Inventory Management)

##### 5.11.1 Danh sách giao dịch tồn kho (Inventory Transaction List)

**Mô tả**: Hiển thị danh sách các giao dịch tồn kho, bao gồm nhập, xuất, điều chỉnh.

**Components**:

- InventoryTransactionFilters (lọc theo loại giao dịch, sản phẩm, thời gian)
- InventoryTransactionTable (bảng giao dịch tồn kho)
- TransactionTypeBadge (hiển thị loại giao dịch)
- QuantityIndicator (hiển thị số lượng nhập/xuất)
- ProductInfo (thông tin sản phẩm)
- OrderInfo (thông tin đơn hàng liên quan)
- ExportOptions (xuất dữ liệu)

**API Endpoints**:

- GET /api/inventory/transactions
- GET /api/inventory/transactions/by-order/{order_id}

##### 5.11.2 Chi tiết giao dịch tồn kho (Inventory Transaction Detail)

**Mô tả**: Hiển thị thông tin chi tiết của một giao dịch tồn kho.

**Components**:

- TransactionInfo (thông tin giao dịch)
- ProductInfo (thông tin sản phẩm)
- OrderInfo (thông tin đơn hàng liên quan)
- WarehouseInfo (thông tin kho)
- AuditTrail (lịch sử thay đổi)
- DocumentViewer (xem chứng từ)

**API Endpoints**:

- GET /api/inventory/transactions/{id}
- GET /api/inventory/transactions/{id}/audit-trail

##### 5.11.3 Tạo giao dịch tồn kho mới (Inventory Transaction Create)

**Mô tả**: Form tạo giao dịch tồn kho mới.
