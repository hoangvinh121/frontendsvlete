# Tài liệu Backend Export Management System

## 1. Models

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

### 1.4. Order Model

- **Mô tả**: Quản lý đơn hàng
- **Các trường chính**:
   - id: ID đơn hàng
   - order_number: Số đơn hàng
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

### 1.7. Customer/Supplier Model

- **Mô tả**: Quản lý khách hàng và nhà cung cấp
- **Các trường chính**:
   - id: ID
   - code: Mã khách hàng/nhà cung cấp
   - name: Tên
   - address: Địa chỉ
   - phone: Số điện thoại
   - email: Email
   - tax_code: Mã số thuế
   - contact_person: Người liên hệ
   - contact_phone: SĐT liên hệ
   - bank_account: Số tài khoản
   - bank_name: Tên ngân hàng

## 2. Routes

### 2.1. Auth Routes (/api/auth)

- Đăng nhập
- Đăng xuất
- Đổi mật khẩu
- Xác thực 2 lớp

### 2.2. User Routes (/api/users)

- CRUD người dùng
- Phân quyền
- Cập nhật thông tin

### 2.3. Product Routes (/api/products)

- CRUD sản phẩm
- Tìm kiếm sản phẩm
- Quản lý tồn kho

### 2.4. Order Routes (/api/orders)

- CRUD đơn hàng
- Xử lý trạng thái
- Thanh toán

### 2.5. Container Routes (/api/containers)

- CRUD container
- Cập nhật trạng thái
- Tính toán chi phí

### 2.6. Cost Routes (/api/costs)

- CRUD chi phí
- Báo cáo chi phí
- Thống kê

### 2.7. Category Routes (/api/categories)

- CRUD Category
- Tìm kiếm category
- Thống kê

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

### 3.4. error_handlers

- Xử lý lỗi
- Log lỗi

### 3.5. logger

- Ghi log hệ thống
- Theo dõi hoạt động

### 3.6. pagination

- Phân trang kết quả
- Sắp xếp dữ liệu

## 4. TypeScript Interfaces

Dưới đây là các interface TypeScript tương ứng với các model trong backend, có thể sử dụng trong Svelte frontend:

```typescript
// User Interface
interface User {
  id: string;
  username: string;
  email: string;
  full_name: string;
  phone?: string;
  position?: string;
  department?: string;
  role: string;
  active: boolean;
  two_factor_enabled: boolean;
}

// Container Types Enum
enum ContainerType {
  STANDARD_20 = 'STANDARD_20',
  STANDARD_40 = 'STANDARD_40',
  HIGH_CUBE_40 = 'HIGH_CUBE_40',
  REEFER_20 = 'REEFER_20',
  REEFER_40 = 'REEFER_40'
}

// Container Status Enum
enum ContainerStatus {
  EMPTY = 'EMPTY',
  LOADING = 'LOADING',
  LOADED = 'LOADED',
  IN_TRANSIT = 'IN_TRANSIT',
  ARRIVED = 'ARRIVED'
}

// Container Interface
interface Container {
  id: string;
  container_number: string;
  container_type: ContainerType;
  status: ContainerStatus;
  location?: string;
  capacity: number; // m3
  max_weight: number; // kg
  current_weight: number; // kg
  length: number; // m
  width: number; // m
  height: number; // m
  tare_weight: number; // kg
  daily_cost: number;
  shipping_cost: number;
}

// Product Interface
interface Product {
  id: string;
  product_code: string;
  name: string;
  description?: string;
  category_id: string;
  unit: string;
  weight: number; // kg
  volume: number; // m3
  price: number;
  selling_price: number;
  tax_rate: number; // percentage
  profit_margin: number; // percentage
  inventory_level: number;
}

// Order Status Enum
enum OrderStatus {
  DRAFT = 'DRAFT',
  PENDING = 'PENDING',
  PROCESSING = 'PROCESSING',
  SHIPPED = 'SHIPPED',
  DELIVERED = 'DELIVERED',
  CANCELLED = 'CANCELLED'
}

// Payment Status Enum
enum PaymentStatus {
  UNPAID = 'UNPAID',
  PARTIAL = 'PARTIAL',
  PAID = 'PAID',
  REFUNDED = 'REFUNDED'
}

// Order Type Enum
enum OrderType {
  IMPORT = 'IMPORT',
  EXPORT = 'EXPORT'
}

// Order Interface
interface Order {
  id: string;
  order_number: string;
  order_type: OrderType;
  status: OrderStatus;
  payment_status: PaymentStatus;
  payment_method?: string;
  container_id?: string;
  total_amount: number;
  tax_amount: number;
  shipping_cost: number;
  total_weight: number; // kg
  total_volume: number; // m3
  customer_id?: string;
  created_at: Date;
  items: OrderItem[];
}

// Order Item Interface
interface OrderItem {
  id: string;
  order_id: string;
  product_id: string;
  product_name: string;
  quantity: number;
  unit_price: number;
  total_price: number;
  tax_rate: number;
  weight: number; // kg
  volume: number; // m3
}

// Cost Type Enum
enum CostType {
  SHIPPING = 'SHIPPING',
  CUSTOMS = 'CUSTOMS',
  HANDLING = 'HANDLING',
  INSURANCE = 'INSURANCE',
  STORAGE = 'STORAGE',
  OTHER = 'OTHER'
}

// Cost Interface
interface Cost {
  id: string;
  order_id?: string;
  container_id?: string;
  cost_type: CostType;
  amount: number;
  currency: string;
  exchange_rate: number;
  description?: string;
  reference_number?: string;
  cost_date: Date;
  created_at: Date;
  created_by: string;
}

// Category Interface
interface Category {
  id: string;
  code: string;
  name: string;
  description?: string;
  parent_id?: string;
  active: boolean;
  created_at: Date;
  updated_at?: Date;
}

// Partner Type Enum
enum PartnerType {
  CUSTOMER = 'CUSTOMER',
  SUPPLIER = 'SUPPLIER',
  BOTH = 'BOTH'
}

// Customer/Supplier Interface
interface Partner {
  id: string;
  code: string;
  name: string;
  partner_type: PartnerType;
  address?: string;
  phone?: string;
  email?: string;
  tax_code?: string;
  contact_person?: string;
  contact_phone?: string;
  bank_account?: string;
  bank_name?: string;
  active: boolean;
  created_at: Date;
  updated_at?: Date;
}
```