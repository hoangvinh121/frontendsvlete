# Tài liệu API - Hệ thống Quản lý CRM

## Giới thiệu

Tài liệu này mô tả chi tiết các API endpoints của hệ thống Quản lý CRM. Hệ thống cung cấp các API để quản lý người dùng, danh mục, sản phẩm, đối tác, container, đơn hàng, chi phí, lịch trình, tồn kho và các chức năng khác.

## Cấu trúc API

Tất cả các API đều sử dụng định dạng JSON cho request và response. Base URL của API là:

```sh
http://localhost:5000/api

```

## Xác thực

Hầu hết các API đều yêu cầu xác thực bằng JWT token. Token này được gửi trong header của request:

```sh
Authorization: Bearer <token>

```

## Danh sách API

### Xác thực (Authentication)

#### Đăng nhập

- **URL**: `/auth/login`

- **Method**: `POST`

- **Mô tả**: Đăng nhập vào hệ thống

- **Request Body**:

```json
{
  "username": "admin",
  "password": "Admin@123"
}

```

- **Response**:

```json
{
  "success": true,
  "message": "Đăng nhập thành công",
  "data": {
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "user": {
      "id": 1,
      "username": "admin",
      "email": "admin@example.com",
      "full_name": "Admin User",
      "role": "admin"
    }
  }
}

```

#### Đăng xuất

- **URL**: `/auth/logout`

- **Method**: `POST`

- **Mô tả**: Đăng xuất khỏi hệ thống

- **Headers**: `Authorization: Bearer <token>`

- **Response**:

```json
{
  "success": true,
  "message": "Đăng xuất thành công"
}

```

### Người dùng (Users)

#### Lấy danh sách người dùng

- **URL**: `/users`

- **Method**: `GET`

- **Mô tả**: Lấy danh sách tất cả người dùng

- **Headers**: `Authorization: Bearer <token>`

- **Query Parameters**:
   - `page`: Số trang (mặc định: 1)
   - `per_page`: Số lượng mỗi trang (mặc định: 10)
   - `search`: Tìm kiếm theo tên, email, username
   - `role`: Lọc theo vai trò (admin, manager, user)

- **Response**:

```json
{
  "success": true,
  "data": {
    "items": [
      {
        "id": 1,
        "username": "admin",
        "email": "admin@example.com",
        "full_name": "Admin User",
        "phone": "0987654321",
        "position": "Administrator",
        "department": "IT",
        "role": "admin",
        "active": true,
        "created_at": "2023-01-01T00:00:00",
        "updated_at": "2023-01-01T00:00:00"
      }
    ],
    "total": 3,
    "page": 1,
    "per_page": 10,
    "pages": 1
  }
}

```

#### Lấy thông tin người dùng

- **URL**: `/users/<id>`

- **Method**: `GET`

- **Mô tả**: Lấy thông tin chi tiết của một người dùng

- **Headers**: `Authorization: Bearer <token>`

- **Response**:

```json
{
  "success": true,
  "data": {
    "id": 1,
    "username": "admin",
    "email": "admin@example.com",
    "full_name": "Admin User",
    "phone": "0987654321",
    "position": "Administrator",
    "department": "IT",
    "role": "admin",
    "active": true,
    "created_at": "2023-01-01T00:00:00",
    "updated_at": "2023-01-01T00:00:00"
  }
}

```

#### Tạo người dùng mới

- **URL**: `/users`

- **Method**: `POST`

- **Mô tả**: Tạo một người dùng mới

- **Headers**: `Authorization: Bearer <token>`

- **Request Body**:

```json
{
  "username": "newuser",
  "email": "newuser@example.com",
  "password": "Password@123",
  "full_name": "New User",
  "phone": "0987654324",
  "position": "Staff",
  "department": "Sales",
  "role": "user"
}

```

- **Response**:

```json
{
  "success": true,
  "message": "Tạo người dùng thành công",
  "data": {
    "id": 4,
    "username": "newuser",
    "email": "newuser@example.com",
    "full_name": "New User",
    "role": "user"
  }
}

```

#### Cập nhật người dùng

- **URL**: `/users/<id>`

- **Method**: `PUT`

- **Mô tả**: Cập nhật thông tin người dùng

- **Headers**: `Authorization: Bearer <token>`

- **Request Body**:

```json
{
  "full_name": "Updated User Name",
  "phone": "0987654325",
  "position": "Senior Staff",
  "department": "Marketing"
}

```

- **Response**:

```json
{
  "success": true,
  "message": "Cập nhật người dùng thành công",
  "data": {
    "id": 4,
    "username": "newuser",
    "email": "newuser@example.com",
    "full_name": "Updated User Name",
    "phone": "0987654325",
    "position": "Senior Staff",
    "department": "Marketing",
    "role": "user",
    "active": true,
    "created_at": "2023-01-01T00:00:00",
    "updated_at": "2023-01-02T00:00:00"
  }
}

```

#### Xóa người dùng

- **URL**: `/users/<id>`

- **Method**: `DELETE`

- **Mô tả**: Xóa một người dùng

- **Headers**: `Authorization: Bearer <token>`

- **Response**:

```json
{
  "success": true,
  "message": "Xóa người dùng thành công"
}

```

### Danh mục (Categories)

#### Lấy danh sách danh mục

- **URL**: `/categories`

- **Method**: `GET`

- **Mô tả**: Lấy danh sách tất cả danh mục

- **Headers**: `Authorization: Bearer <token>`

- **Query Parameters**:
   - `page`: Số trang (mặc định: 1)
   - `per_page`: Số lượng mỗi trang (mặc định: 10)
   - `search`: Tìm kiếm theo tên, mã
   - `parent_id`: Lọc theo danh mục cha

- **Response**:

```json
{
  "success": true,
  "data": {
    "items": [
      {
        "id": 1,
        "code": "ELEC",
        "name": "Điện tử",
        "description": "Các sản phẩm điện tử",
        "parent_id": null,
        "active": true,
        "created_at": "2023-01-01T00:00:00",
        "updated_at": "2023-01-01T00:00:00"
      }
    ],
    "total": 9,
    "page": 1,
    "per_page": 10,
    "pages": 1
  }
}

```

#### Lấy cấu trúc cây danh mục

- **URL**: `/categories/tree`

- **Method**: `GET`

- **Mô tả**: Lấy cấu trúc cây của tất cả danh mục

- **Headers**: `Authorization: Bearer <token>`

- **Response**:

```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "code": "ELEC",
      "name": "Điện tử",
      "description": "Các sản phẩm điện tử",
      "parent_id": null,
      "active": true,
      "created_at": "2023-01-01T00:00:00",
      "updated_at": "2023-01-01T00:00:00",
      "subcategories": [
        {
          "id": 4,
          "code": "ELEC-SP",
          "name": "Điện thoại thông minh",
          "description": "Các loại điện thoại thông minh",
          "parent_id": 1,
          "active": true,
          "created_at": "2023-01-01T00:00:00",
          "updated_at": "2023-01-01T00:00:00",
          "subcategories": []
        },
        {
          "id": 5,
          "code": "ELEC-LP",
          "name": "Laptop",
          "description": "Các loại máy tính xách tay",
          "parent_id": 1,
          "active": true,
          "created_at": "2023-01-01T00:00:00",
          "updated_at": "2023-01-01T00:00:00",
          "subcategories": []
        }
      ]
    }
  ]
}

```

#### Lấy thông tin danh mục

- **URL**: `/categories/<id>`

- **Method**: `GET`

- **Mô tả**: Lấy thông tin chi tiết của một danh mục

- **Headers**: `Authorization: Bearer <token>`

- **Response**:

```json
{
  "success": true,
  "data": {
    "id": 1,
    "code": "ELEC",
    "name": "Điện tử",
    "description": "Các sản phẩm điện tử",
    "parent_id": null,
    "active": true,
    "created_at": "2023-01-01T00:00:00",
    "updated_at": "2023-01-01T00:00:00"
  }
}

```

#### Tạo danh mục mới

- **URL**: `/categories`

- **Method**: `POST`

- **Mô tả**: Tạo một danh mục mới

- **Headers**: `Authorization: Bearer <token>`

- **Request Body**:

```json
{
  "code": "ELEC-TV",
  "name": "Tivi",
  "description": "Các loại tivi",
  "parent_id": 1
}

```

- **Response**:

```json
{
  "success": true,
  "message": "Tạo danh mục thành công",
  "data": {
    "id": 10,
    "code": "ELEC-TV",
    "name": "Tivi",
    "description": "Các loại tivi",
    "parent_id": 1,
    "active": true,
    "created_at": "2023-01-02T00:00:00",
    "updated_at": "2023-01-02T00:00:00"
  }
}

```

#### Cập nhật danh mục

- **URL**: `/categories/<id>`

- **Method**: `PUT`

- **Mô tả**: Cập nhật thông tin danh mục

- **Headers**: `Authorization: Bearer <token>`

- **Request Body**:

```json
{
  "name": "Tivi & Màn hình",
  "description": "Các loại tivi và màn hình"
}

```

- **Response**:

```json
{
  "success": true,
  "message": "Cập nhật danh mục thành công",
  "data": {
    "id": 10,
    "code": "ELEC-TV",
    "name": "Tivi & Màn hình",
    "description": "Các loại tivi và màn hình",
    "parent_id": 1,
    "active": true,
    "created_at": "2023-01-02T00:00:00",
    "updated_at": "2023-01-03T00:00:00"
  }
}

```

#### Xóa danh mục

- **URL**: `/categories/<id>`

- **Method**: `DELETE`

- **Mô tả**: Xóa một danh mục

- **Headers**: `Authorization: Bearer <token>`

- **Response**:

```json
{
  "success": true,
  "message": "Xóa danh mục thành công"
}

```

### Sản phẩm (Products)

#### Lấy danh sách sản phẩm

- **URL**: `/products`

- **Method**: `GET`

- **Mô tả**: Lấy danh sách tất cả sản phẩm

- **Headers**: `Authorization: Bearer <token>`

- **Query Parameters**:
   - `page`: Số trang (mặc định: 1)
   - `per_page`: Số lượng mỗi trang (mặc định: 10)
   - `search`: Tìm kiếm theo tên, mã
   - `category_id`: Lọc theo danh mục
   - `product_type`: Lọc theo loại sản phẩm (IMPORT, DOMESTIC)

- **Response**:

```json
{
  "success": true,
  "data": {
    "items": [
      {
        "id": 1,
        "product_code": "SP001",
        "name": "iPhone 13",
        "product_type": "IMPORT",
        "description": "Điện thoại iPhone 13 128GB",
        "category_id": 4,
        "category_name": "Điện thoại thông minh",
        "unit": "Cái",
        "weight": 0.2,
        "volume": 0.001,
        "price": 20000000,
        "selling_price": 22000000,
        "tax_rate": 10,
        "profit_margin": 15,
        "inventory_level": 50,
        "min_inventory_level": 10,
        "max_inventory_level": 100,
        "inventory_update_method": "FIFO",
        "created_at": "2023-01-01T00:00:00",
        "updated_at": "2023-01-01T00:00:00"
      }
    ],
    "total": 7,
    "page": 1,
    "per_page": 10,
    "pages": 1
  }
}

```

#### Lấy thông tin sản phẩm

- **URL**: `/products/<id>`

- **Method**: `GET`

- **Mô tả**: Lấy thông tin chi tiết của một sản phẩm

- **Headers**: `Authorization: Bearer <token>`

- **Response**:

```json
{
  "success": true,
  "data": {
    "id": 1,
    "product_code": "SP001",
    "name": "iPhone 13",
    "product_type": "IMPORT",
    "description": "Điện thoại iPhone 13 128GB",
    "category_id": 4,
    "category_name": "Điện thoại thông minh",
    "unit": "Cái",
    "weight": 0.2,
    "volume": 0.001,
    "price": 20000000,
    "selling_price": 22000000,
    "tax_rate": 10,
    "profit_margin": 15,
    "inventory_level": 50,
    "min_inventory_level": 10,
    "max_inventory_level": 100,
    "inventory_update_method": "FIFO",
    "created_at": "2023-01-01T00:00:00",
    "updated_at": "2023-01-01T00:00:00",
    "tags": [
      {
        "id": 1,
        "name": "Bán chạy",
        "color": "#FF5733"
      }
    ]
  }
}

```

#### Tạo sản phẩm mới

- **URL**: `/products`

- **Method**: `POST`

- **Mô tả**: Tạo một sản phẩm mới

- **Headers**: `Authorization: Bearer <token>`

- **Request Body**:

```json
{
  "product_code": "SP003",
  "name": "iPhone 14",
  "product_type": "IMPORT",
  "description": "Điện thoại iPhone 14 256GB",
  "category_id": 4,
  "unit": "Cái",
  "weight": 0.21,
  "volume": 0.0011,
  "price": 25000000,
  "selling_price": 27500000,
  "tax_rate": 10,
  "profit_margin": 15,
  "inventory_level": 30,
  "min_inventory_level": 5,
  "max_inventory_level": 50,
  "inventory_update_method": "FIFO"
}

```

- **Response**:

```json
{
  "success": true,
  "message": "Tạo sản phẩm thành công",
  "data": {
    "id": 8,
    "product_code": "SP003",
    "name": "iPhone 14",
    "product_type": "IMPORT",
    "category_id": 4,
    "category_name": "Điện thoại thông minh"
  }
}

```

#### Cập nhật sản phẩm

- **URL**: `/products/<id>`

- **Method**: `PUT`

- **Mô tả**: Cập nhật thông tin sản phẩm

- **Headers**: `Authorization: Bearer <token>`

- **Request Body**:

```json
{
  "name": "iPhone 14 Pro",
  "description": "Điện thoại iPhone 14 Pro 256GB",
  "price": 28000000,
  "selling_price": 30800000
}

```

- **Response**:

```json
{
  "success": true,
  "message": "Cập nhật sản phẩm thành công",
  "data": {
    "id": 8,
    "product_code": "SP003",
    "name": "iPhone 14 Pro",
    "product_type": "IMPORT",
    "description": "Điện thoại iPhone 14 Pro 256GB",
    "category_id": 4,
    "category_name": "Điện thoại thông minh",
    "price": 28000000,
    "selling_price": 30800000
  }
}

```

#### Xóa sản phẩm

- **URL**: `/products/<id>`

- **Method**: `DELETE`

- **Mô tả**: Xóa một sản phẩm

- **Headers**: `Authorization: Bearer <token>`

- **Response**:

```json
{
  "success": true,
  "message": "Xóa sản phẩm thành công"
}

```

### Đối tác (Partners)

#### Lấy danh sách đối tác

- **URL**: `/partners`

- **Method**: `GET`

- **Mô tả**: Lấy danh sách tất cả đối tác

- **Headers**: `Authorization: Bearer <token>`

- **Query Parameters**:
   - `page`: Số trang (mặc định: 1)
   - `per_page`: Số lượng mỗi trang (mặc định: 10)
   - `search`: Tìm kiếm theo tên, mã, email
   - `partner_type`: Lọc theo loại đối tác (DOMESTIC_CUSTOMER, FOREIGN_CUSTOMER, SUPPLIER)

- **Response**:

```json
{
  "success": true,
  "data": {
    "items": [
      {
        "id": 1,
        "code": "DC000001",
        "name": "Công ty TNHH ABC",
        "partner_type": "DOMESTIC_CUSTOMER",
        "address": "123 Nguyễn Văn Linh, Quận 7, TP.HCM",
        "phone": "0987654321",
        "email": "contact@abc.com.vn",
        "tax_code": "0123456789",
        "contact_person": "Nguyễn Văn A",
        "contact_phone": "0987654321",
        "bank_account": "123456789",
        "bank_name": "Vietcombank",
        "active": true,
        "created_at": "2023-01-01T00:00:00",
        "updated_at": "2023-01-01T00:00:00"
      }
    ],
    "total": 6,
    "page": 1,
    "per_page": 10,
    "pages": 1
  }
}

```

#### Lấy thông tin đối tác

- **URL**: `/partners/<id>`

- **Method**: `GET`

- **Mô tả**: Lấy thông tin chi tiết của một đối tác

- **Headers**: `Authorization: Bearer <token>`

- **Response**:

```json
{
  "success": true,
  "data": {
    "id": 1,
    "code": "DC000001",
    "name": "Công ty TNHH ABC",
    "partner_type": "DOMESTIC_CUSTOMER",
    "address": "123 Nguyễn Văn Linh, Quận 7, TP.HCM",
    "phone": "0987654321",
    "email": "contact@abc.com.vn",
    "tax_code": "0123456789",
    "contact_person": "Nguyễn Văn A",
    "contact_phone": "0987654321",
    "bank_account": "123456789",
    "bank_name": "Vietcombank",
    "active": true,
    "created_at": "2023-01-01T00:00:00",
    "updated_at": "2023-01-01T00:00:00",
    "tags": [
      {
        "id": 6,
        "name": "VIP",
        "color": "#CC33FF"
      }
    ]
  }
}

```

#### Tạo đối tác mới

- **URL**: `/partners`

- **Method**: `POST`

- **Mô tả**: Tạo một đối tác mới

- **Headers**: `Authorization: Bearer <token>`

- **Request Body**:

```json
{
  "name": "Công ty TNHH XYZ",
  "partner_type": "DOMESTIC_CUSTOMER",
  "address": "456 Điện Biên Phủ, Quận 3, TP.HCM",
  "phone": "0987654326",
  "email": "contact@xyz.com.vn",
  "tax_code": "0123456792",
  "contact_person": "Trần Văn B",
  "contact_phone": "0987654326",
  "bank_account": "123456792",
  "bank_name": "Vietinbank"
}

```

- **Response**:

```json
{
  "success": true,
  "message": "Tạo đối tác thành công",
  "data": {
    "id": 7,
    "code": "DC000003",
    "name": "Công ty TNHH XYZ",
    "partner_type": "DOMESTIC_CUSTOMER"
  }
}

```

#### Cập nhật đối tác

- **URL**: `/partners/<id>`

- **Method**: `PUT`

- **Mô tả**: Cập nhật thông tin đối tác

- **Headers**: `Authorization: Bearer <token>`

- **Request Body**:

```json
{
  "name": "Công ty Cổ phần XYZ",
  "address": "789 Điện Biên Phủ, Quận 3, TP.HCM",
  "contact_person": "Trần Văn C"
}

```

- **Response**:

```json
{
  "success": true,
  "message": "Cập nhật đối tác thành công",
  "data": {
    "id": 7,
    "code": "DC000003",
    "name": "Công ty Cổ phần XYZ",
    "partner_type": "DOMESTIC_CUSTOMER",
    "address": "789 Điện Biên Phủ, Quận 3, TP.HCM",
    "contact_person": "Trần Văn C",
    "active": true,
    "created_at": "2023-01-02T00:00:00",
    "updated_at": "2023-01-03T00:00:00"
  }
}

```

#### Xóa đối tác

- **URL**: `/partners/<id>`

- **Method**: `DELETE`

- **Mô tả**: Xóa một đối tác

- **Headers**: `Authorization: Bearer <token>`

- **Response**:

```json
{
  "success": true,
  "message": "Xóa đối tác thành công"
}

```

### Container

#### Lấy danh sách container

- **URL**: `/containers`

- **Method**: `GET`

- **Mô tả**: Lấy danh sách tất cả container

- **Headers**: `Authorization: Bearer <token>`

- **Query Parameters**:
   - `page`: Số trang (mặc định: 1)
   - `per_page`: Số lượng mỗi trang (mặc định: 10)
   - `search`: Tìm kiếm theo mã container
   - `status`: Lọc theo trạng thái (PENDING, IN_TRANSIT, ARRIVED, CLEARED, DELIVERED)
   - `from_date`: Lọc từ ngày (định dạng: YYYY-MM-DD)
   - `to_date`: Lọc đến ngày (định dạng: YYYY-MM-DD)

- **Response**:

```json
{
  "success": true,
  "data": {
    "items": [
      {
        "id": 1,
        "container_code": "CONT001",
        "container_number": "ABCD1234567",
        "container_type": "40HC",
        "seal_number": "SL12345",
        "status": "IN_TRANSIT",
        "departure_date": "2023-01-15T00:00:00",
        "arrival_date": "2023-02-15T00:00:00",
        "shipping_line": "Maersk",
        "vessel_name": "MSC ANNA",
        "voyage": "VN001",
        "port_of_loading": "Shanghai",
        "port_of_discharge": "Ho Chi Minh",
        "created_at": "2023-01-01T00:00:00",
        "updated_at": "2023-01-01T00:00:00"
      }
    ],
    "total": 5,
    "page": 1,
    "per_page": 10,
    "pages": 1
  }
}

```

#### Lấy thông tin container

- **URL**: `/containers/<id>`

- **Method**: `GET`

- **Mô tả**: Lấy thông tin chi tiết của một container

- **Headers**: `Authorization: Bearer <token>`

- **Response**:

```json
{
  "success": true,
  "data": {
    "id": 1,
    "container_code": "CONT001",
    "container_number": "ABCD1234567",
    "container_type": "40HC",
    "seal_number": "SL12345",
    "status": "IN_TRANSIT",
    "departure_date": "2023-01-15T00:00:00",
    "arrival_date": "2023-02-15T00:00:00",
    "shipping_line": "Maersk",
    "vessel_name": "MSC ANNA",
    "voyage": "VN001",
    "port_of_loading": "Shanghai",
    "port_of_discharge": "Ho Chi Minh",
    "bill_of_lading": "BL123456",
    "booking_number": "BK123456",
    "forwarder_id": 3,
    "forwarder_name": "Công ty Vận tải XYZ",
    "notes": "Container chứa hàng điện tử",
    "created_at": "2023-01-01T00:00:00",
    "updated_at": "2023-01-01T00:00:00",
    "products": [
      {
        "id": 1,
        "product_code": "SP001",
        "name": "iPhone 13",
        "quantity": 500
      }
    ]
  }
}

```

#### Tạo container mới

- **URL**: `/containers`

- **Method**: `POST`

- **Mô tả**: Tạo một container mới

- **Headers**: `Authorization: Bearer <token>`

- **Request Body**:

```json
{
  "container_number": "EFGH7654321",
  "container_type": "20GP",
  "seal_number": "SL54321",
  "status": "PENDING",
  "departure_date": "2023-03-15",
  "arrival_date": "2023-04-15",
  "shipping_line": "CMA CGM",
  "vessel_name": "CMA CGM MARCO POLO",
  "voyage": "VN002",
  "port_of_loading": "Ningbo",
  "port_of_discharge": "Ho Chi Minh",
  "bill_of_lading": "BL654321",
  "booking_number": "BK654321",
  "forwarder_id": 3,
  "notes": "Container chứa hàng gia dụng",
  "products": [
    {
      "product_id": 2,
      "quantity": 300
    }
  ]
}

```

- **Response**:

```json
{
  "success": true,
  "message": "Tạo container thành công",
  "data": {
    "id": 6,
    "container_code": "CONT006",
    "container_number": "EFGH7654321",
    "status": "PENDING"
  }
}

```

#### Cập nhật container

- **URL**: `/containers/<id>`

- **Method**: `PUT`

- **Mô tả**: Cập nhật thông tin container

- **Headers**: `Authorization: Bearer <token>`

- **Request Body**:

```json
{
  "status": "IN_TRANSIT",
  "vessel_name": "CMA CGM ALEXANDER",
  "voyage": "VN003",
  "notes": "Container đã rời cảng"
}

```

- **Response**:

```json
{
  "success": true,
  "message": "Cập nhật container thành công",
  "data": {
    "id": 6,
    "container_code": "CONT006",
    "container_number": "EFGH7654321",
    "status": "IN_TRANSIT",
    "vessel_name": "CMA CGM ALEXANDER",
    "voyage": "VN003",
    "notes": "Container đã rời cảng"
  }
}

```

#### Xóa container

- **URL**: `/containers/<id>`

- **Method**: `DELETE`

- **Mô tả**: Xóa một container

- **Headers**: `Authorization: Bearer <token>`

- **Response**:

```json
{
  "success": true,
  "message": "Xóa container thành công"
}

```

### Đơn hàng (Orders)

#### Lấy danh sách đơn hàng

- **URL**: `/orders`

- **Method**: `GET`

- **Mô tả**: Lấy danh sách tất cả đơn hàng

- **Headers**: `Authorization: Bearer <token>`

- **Query Parameters**:
   - `page`: Số trang (mặc định: 1)
   - `per_page`: Số lượng mỗi trang (mặc định: 10)
   - `search`: Tìm kiếm theo mã đơn hàng, tên khách hàng
   - `status`: Lọc theo trạng thái (DRAFT, CONFIRMED, PROCESSING, SHIPPING, DELIVERED, COMPLETED, CANCELLED)
   - `from_date`: Lọc từ ngày (định dạng: YYYY-MM-DD)
   - `to_date`: Lọc đến ngày (định dạng: YYYY-MM-DD)
   - `partner_id`: Lọc theo đối tác

- **Response**:

```json
{
  "success": true,
  "data": {
    "items": [
      {
        "id": 1,
        "order_code": "ORD001",
        "order_date": "2023-01-10T00:00:00",
        "status": "COMPLETED",
        "partner_id": 1,
        "partner_name": "Công ty TNHH ABC",
        "total_amount": 110000000,
        "discount_amount": 5000000,
        "tax_amount": 10000000,
        "final_amount": 115000000,
        "payment_status": "PAID",
        "payment_method": "BANK_TRANSFER",
        "delivery_address": "123 Nguyễn Văn Linh, Quận 7, TP.HCM",
        "delivery_date": "2023-01-20T00:00:00",
        "notes": "Giao hàng trong giờ hành chính",
        "created_by": 1,
        "created_by_name": "Admin User",
        "created_at": "2023-01-01T00:00:00",
        "updated_at": "2023-01-01T00:00:00"
      }
    ],
    "total": 8,
    "page": 1,
    "per_page": 10,
    "pages": 1
  }
}

```

#### Lấy thông tin đơn hàng

- **URL**: `/orders/<id>`

- **Method**: `GET`

- **Mô tả**: Lấy thông tin chi tiết của một đơn hàng

- **Headers**: `Authorization: Bearer <token>`

- **Response**:

```json
{
  "success": true,
  "data": {
    "id": 1,
    "order_code": "ORD001",
    "order_date": "2023-01-10T00:00:00",
    "status": "COMPLETED",
    "partner_id": 1,
    "partner_name": "Công ty TNHH ABC",
    "total_amount": 110000000,
    "discount_amount": 5000000,
    "tax_amount": 10000000,
    "final_amount": 115000000,
    "payment_status": "PAID",
    "payment_method": "BANK_TRANSFER",
    "delivery_address": "123 Nguyễn Văn Linh, Quận 7, TP.HCM",
    "delivery_date": "2023-01-20T00:00:00",
    "notes": "Giao hàng trong giờ hành chính",
    "created_by": 1,
    "created_by_name": "Admin User",
    "created_at": "2023-01-01T00:00:00",
    "updated_at": "2023-01-01T00:00:00",
    "items": [
      {
        "id": 1,
        "product_id": 1,
        "product_code": "SP001",
        "product_name": "iPhone 13",
        "quantity": 5,
        "unit_price": 22000000,
        "discount": 1000000,
        "tax_rate": 10,
        "tax_amount": 2000000,
        "total_amount": 23000000
      }
    ]
  }
}

```

#### Tạo đơn hàng mới

- **URL**: `/orders`

- **Method**: `POST`

- **Mô tả**: Tạo một đơn hàng mới

- **Headers**: `Authorization: Bearer <token>`

- **Request Body**:

```json
{
  "order_date": "2023-02-10",
  "partner_id": 1,
  "discount_amount": 2000000,
  "payment_method": "BANK_TRANSFER",
  "delivery_address": "123 Nguyễn Văn Linh, Quận 7, TP.HCM",
  "delivery_date": "2023-02-20",
  "notes": "Giao hàng trong giờ hành chính",
  "items": [
    {
      "product_id": 1,
      "quantity": 2,
      "unit_price": 22000000,
      "discount": 500000
    },
    {
      "product_id": 2,
      "quantity": 1,
      "unit_price": 15000000,
      "discount": 0
    }
  ]
}

```

- **Response**:

```json
{
  "success": true,
  "message": "Tạo đơn hàng thành công",
  "data": {
    "id": 9,
    "order_code": "ORD009",
    "order_date": "2023-02-10T00:00:00",
    "status": "DRAFT",
    "partner_name": "Công ty TNHH ABC",
    "final_amount": 60500000
  }
}

```

#### Cập nhật đơn hàng

- **URL**: `/orders/<id>`

- **Method**: `PUT`

- **Mô tả**: Cập nhật thông tin đơn hàng

- **Headers**: `Authorization: Bearer <token>`

- **Request Body**:

```json
{
  "status": "CONFIRMED",
  "discount_amount": 3000000,
  "notes": "Giao hàng vào buổi sáng"
}

```

- **Response**:

```json
{
  "success": true,
  "message": "Cập nhật đơn hàng thành công",
  "data": {
    "id": 9,
    "order_code": "ORD009",
    "status": "CONFIRMED",
    "discount_amount": 3000000,
    "final_amount": 59500000,
    "notes": "Giao hàng vào buổi sáng"
  }
}

```

#### Xóa đơn hàng

- **URL**: `/orders/<id>`

- **Method**: `DELETE`

- **Mô tả**: Xóa một đơn hàng

- **Headers**: `Authorization: Bearer <token>`

- **Response**:

```json
{
  "success": true,
  "message": "Xóa đơn hàng thành công"
}

```

### Chi phí (Costs)

#### Lấy danh sách chi phí

- **URL**: `/costs`

- **Method**: `GET`

- **Mô tả**: Lấy danh sách tất cả chi phí

- **Headers**: `Authorization: Bearer <token>`

- **Query Parameters**:
   - `page`: Số trang (mặc định: 1)
   - `per_page`: Số lượng mỗi trang (mặc định: 10)
   - `search`: Tìm kiếm theo mô tả, mã chi phí
   - `cost_type`: Lọc theo loại chi phí (SHIPPING, CUSTOMS, TRANSPORTATION, WAREHOUSE, OPERATION, OTHER)
   - `from_date`: Lọc từ ngày (định dạng: YYYY-MM-DD)
   - `to_date`: Lọc đến ngày (định dạng: YYYY-MM-DD)
   - `container_id`: Lọc theo container
   - `order_id`: Lọc theo đơn hàng

- **Response**:

```json
{
  "success": true,
  "data": {
    "items": [
      {
        "id": 1,
        "cost_code": "COST001",
        "cost_date": "2023-01-15T00:00:00",
        "cost_type": "SHIPPING",
        "description": "Cước vận chuyển container CONT001",
        "amount": 50000000,
        "currency": "VND",
        "exchange_rate": 1,
        "container_id": 1,
        "container_code": "CONT001",
        "order_id": null,
        "order_code": null,
        "payment_status": "PAID",
        "payment_date": "2023-01-20T00:00:00",
        "notes": "Đã thanh toán qua ngân hàng",
        "created_by": 1,
        "created_by_name": "Admin User",
        "created_at": "2023-01-01T00:00:00",
        "updated_at": "2023-01-01T00:00:00"
      }
    ],
    "total": 12,
    "page": 1,
    "per_page": 10,
    "pages": 2
  }
}

```

#### Lấy thông tin chi phí

- **URL**: `/costs/<id>`

- **Method**: `GET`

- **Mô tả**: Lấy thông tin chi tiết của một chi phí

- **Headers**: `Authorization: Bearer <token>`

- **Response**:

```json
{
  "success": true,
  "data": {
    "id": 1,
    "cost_code": "COST001",
    "cost_date": "2023-01-15T00:00:00",
    "cost_type": "SHIPPING",
    "description": "Cước vận chuyển container CONT001",
    "amount": 50000000,
    "currency": "VND",
    "exchange_rate": 1,
    "container_id": 1,
    "container_code": "CONT001",
    "order_id": null,
    "order_code": null,
    "payment_status": "PAID",
    "payment_date": "2023-01-20T00:00:00",
    "payment_method": "BANK_TRANSFER",
    "reference_number": "REF123456",
    "notes": "Đã thanh toán qua ngân hàng",
    "created_by": 1,
    "created_by_name": "Admin User",
    "created_at": "2023-01-01T00:00:00",
    "updated_at": "2023-01-01T00:00:00",
    "attachments": [
      {
        "id": 1,
        "file_name": "invoice.pdf",
        "file_path": "/uploads/costs/1/invoice.pdf",
        "file_size": 1024,
        "file_type": "application/pdf",
        "uploaded_at": "2023-01-01T00:00:00"
      }
    ]
  }
}

```

#### Tạo chi phí mới

- **URL**: `/costs`

- **Method**: `POST`

- **Mô tả**: Tạo một chi phí mới

- **Headers**: `Authorization: Bearer <token>`

- **Request Body**:

```json
{
  "cost_date": "2023-02-15",
  "cost_type": "CUSTOMS",
  "description": "Thuế nhập khẩu container CONT002",
  "amount": 30000000,
  "currency": "VND",
  "exchange_rate": 1,
  "container_id": 2,
  "payment_status": "PENDING",
  "payment_method": "BANK_TRANSFER",
  "notes": "Cần thanh toán trước ngày 20/02/2023"
}

```

- **Response**:

```json
{
  "success": true,
  "message": "Tạo chi phí thành công",
  "data": {
    "id": 13,
    "cost_code": "COST013",
    "cost_date": "2023-02-15T00:00:00",
    "cost_type": "CUSTOMS",
    "description": "Thuế nhập khẩu container CONT002",
    "amount": 30000000
  }
}

```

#### Cập nhật chi phí

- **URL**: `/costs/<id>`

- **Method**: `PUT`

- **Mô tả**: Cập nhật thông tin chi phí

- **Headers**: `Authorization: Bearer <token>`

- **Request Body**:

```json
{
  "payment_status": "PAID",
  "payment_date": "2023-02-18",
  "reference_number": "REF789012",
  "notes": "Đã thanh toán qua ngân hàng ngày 18/02/2023"
}

```

- **Response**:

```json
{
  "success": true,
  "message": "Cập nhật chi phí thành công",
  "data": {
    "id": 13,
    "cost_code": "COST013",
    "payment_status": "PAID",
    "payment_date": "2023-02-18T00:00:00",
    "reference_number": "REF789012",
    "notes": "Đã thanh toán qua ngân hàng ngày 18/02/2023"
  }
}

```

#### Xóa chi phí

- **URL**: `/costs/<id>`

- **Method**: `DELETE`

- **Mô tả**: Xóa một chi phí

- **Headers**: `Authorization: Bearer <token>`

- **Response**:

```json
{
  "success": true,
  "message": "Xóa chi phí thành công"
}

```

### Lịch trình (Schedules)

#### Lấy danh sách lịch trình

- **URL**: `/schedules`

- **Method**: `GET`

- **Mô tả**: Lấy danh sách tất cả lịch trình

- **Headers**: `Authorization: Bearer <token>`

- **Query Parameters**:
   - `page`: Số trang (mặc định: 1)
   - `per_page`: Số lượng mỗi trang (mặc định: 10)
   - `search`: Tìm kiếm theo tiêu đề, mô tả
   - `schedule_type`: Lọc theo loại lịch trình (MEETING, TASK, REMINDER, OTHER)
   - `from_date`: Lọc từ ngày (định dạng: YYYY-MM-DD)
   - `to_date`: Lọc đến ngày (định dạng: YYYY-MM-DD)
   - `status`: Lọc theo trạng thái (PENDING, COMPLETED, CANCELLED)
   - `user_id`: Lọc theo người dùng được gán

- **Response**:

```json
{
  "success": true,
  "data": {
    "items": [
      {
        "id": 1,
        "title": "Họp với khách hàng ABC",
        "schedule_type": "MEETING",
        "description": "Thảo luận về đơn hàng mới",
        "start_time": "2023-01-15T09:00:00",
        "end_time": "2023-01-15T10:30:00",
        "location": "Văn phòng công ty",
        "status": "COMPLETED",
        "priority": "HIGH",
        "related_partner_id": 1,
        "related_partner_name": "Công ty TNHH ABC",
        "related_order_id": null,
        "related_order_code": null,
        "assigned_to": [
          {
            "user_id": 1,
            "username": "admin",
            "full_name": "Admin User"
          }
        ],
        "created_by": 1,
        "created_by_name": "Admin User",
        "created_at": "2023-01-01T00:00:00",
        "updated_at": "2023-01-01T00:00:00"
      }
    ],
    "total": 15,
    "page": 1,
    "per_page": 10,
    "pages": 2
  }
}

```

#### Lấy thông tin lịch trình

- **URL**: `/schedules/<id>`

- **Method**: `GET`

- **Mô tả**: Lấy thông tin chi tiết của một lịch trình

- **Headers**: `Authorization: Bearer <token>`

- **Response**:

```json
{
  "success": true,
  "data": {
    "id": 1,
    "title": "Họp với khách hàng ABC",
    "schedule_type": "MEETING",
    "description": "Thảo luận về đơn hàng mới",
    "start_time": "2023-01-15T09:00:00",
    "end_time": "2023-01-15T10:30:00",
    "location": "Văn phòng công ty",
    "status": "COMPLETED",
    "priority": "HIGH",
    "related_partner_id": 1,
    "related_partner_name": "Công ty TNHH ABC",
    "related_order_id": null,
    "related_order_code": null,
    "notes": "Chuẩn bị tài liệu báo giá",
    "assigned_to": [
      {
        "user_id": 1,
        "username": "admin",
        "full_name": "Admin User"
      }
    ],
    "created_by": 1,
    "created_by_name": "Admin User",
    "created_at": "2023-01-01T00:00:00",
    "updated_at": "2023-01-01T00:00:00",
    "attachments": [
      {
        "id": 5,
        "file_name": "meeting_notes.pdf",
        "file_path": "/uploads/schedules/1/meeting_notes.pdf",
        "file_size": 512,
        "file_type": "application/pdf",
        "uploaded_at": "2023-01-15T11:00:00"
      }
    ]
  }
}

```

#### Tạo lịch trình mới

- **URL**: `/schedules`

- **Method**: `POST`

- **Mô tả**: Tạo một lịch trình mới

- **Headers**: `Authorization: Bearer <token>`

- **Request Body**:

```json
{
  "title": "Gặp đối tác XYZ",
  "schedule_type": "MEETING",
  "description": "Thảo luận về hợp tác mới",
  "start_time": "2023-03-10T14:00:00",
  "end_time": "2023-03-10T15:30:00",
  "location": "Văn phòng đối tác",
  "status": "PENDING",
  "priority": "MEDIUM",
  "related_partner_id": 7,
  "notes": "Chuẩn bị tài liệu giới thiệu công ty",
  "assigned_to": [1, 2]
}

```

- **Response**:

```json
{
  "success": true,
  "message": "Tạo lịch trình thành công",
  "data": {
    "id": 16,
    "title": "Gặp đối tác XYZ",
    "schedule_type": "MEETING",
    "start_time": "2023-03-10T14:00:00",
    "end_time": "2023-03-10T15:30:00",
    "status": "PENDING"
  }
}

```

#### Cập nhật lịch trình

- **URL**: `/schedules/<id>`

- **Method**: `PUT`

- **Mô tả**: Cập nhật thông tin lịch trình

- **Headers**: `Authorization: Bearer <token>`

- **Request Body**:

```json
{
  "status": "COMPLETED",
  "notes": "Cuộc họp đã diễn ra tốt đẹp, đối tác đồng ý hợp tác",
  "assigned_to": [1, 2, 3]
}

```

- **Response**:

```json
{
  "success": true,
  "message": "Cập nhật lịch trình thành công",
  "data": {
    "id": 16,
    "title": "Gặp đối tác XYZ",
    "status": "COMPLETED",
    "notes": "Cuộc họp đã diễn ra tốt đẹp, đối tác đồng ý hợp tác"
  }
}

```

#### Xóa lịch trình

- **URL**: `/schedules/<id>`

- **Method**: `DELETE`

- **Mô tả**: Xóa một lịch trình

- **Headers**: `Authorization: Bearer <token>`

- **Response**:

```json
{
  "success": true,
  "message": "Xóa lịch trình thành công"
}

```

### Tồn kho (Inventory)

#### Lấy danh sách tồn kho

- **URL**: `/inventory`

- **Method**: `GET`

- **Mô tả**: Lấy danh sách tồn kho của tất cả sản phẩm

- **Headers**: `Authorization: Bearer <token>`

- **Query Parameters**:
   - `page`: Số trang (mặc định: 1)
   - `per_page`: Số lượng mỗi trang (mặc định: 10)
   - `search`: Tìm kiếm theo tên sản phẩm, mã sản phẩm
   - `category_id`: Lọc theo danh mục
   - `status`: Lọc theo trạng thái (IN_STOCK, LOW_STOCK, OUT_OF_STOCK)

- **Response**:

```json
{
  "success": true,
  "data": {
    "items": [
      {
        "id": 1,
        "product_id": 1,
        "product_code": "SP001",
        "product_name": "iPhone 13",
        "category_id": 4,
        "category_name": "Điện thoại thông minh",
        "quantity": 50,
        "min_level": 10,
        "max_level": 100,
        "status": "IN_STOCK",
        "unit": "Cái",
        "location": "Kho A - Kệ 1",
        "last_updated": "2023-01-01T00:00:00"
      }
    ],
    "total": 7,
    "page": 1,
    "per_page": 10,
    "pages": 1
  }
}

```

#### Lấy thông tin tồn kho của sản phẩm

- __URL__: `/inventory/products/<product_id>`

- **Method**: `GET`

- **Mô tả**: Lấy thông tin tồn kho chi tiết của một sản phẩm

- **Headers**: `Authorization: Bearer <token>`

- **Response**:

```json
{
  "success": true,
  "data": {
    "id": 1,
    "product_id": 1,
    "product_code": "SP001",
    "product_name": "iPhone 13",
    "category_id": 4,
    "category_name": "Điện thoại thông minh",
    "quantity": 50,
    "min_level": 10,
    "max_level": 100,
    "status": "IN_STOCK",
    "unit": "Cái",
    "location": "Kho A - Kệ 1",
    "last_updated": "2023-01-01T00:00:00",
    "history": [
      {
        "id": 1,
        "transaction_type": "IMPORT",
        "quantity": 100,
        "reference_type": "CONTAINER",
        "reference_id": 1,
        "reference_code": "CONT001",
        "transaction_date": "2023-01-01T00:00:00",
        "notes": "Nhập hàng từ container",
        "created_by": 1,
        "created_by_name": "Admin User"
      },
      {
        "id": 2,
        "transaction_type": "EXPORT",
        "quantity": 50,
        "reference_type": "ORDER",
        "reference_id": 1,
        "reference_code": "ORD001",
        "transaction_date": "2023-01-10T00:00:00",
        "notes": "Xuất hàng cho đơn hàng",
        "created_by": 1,
        "created_by_name": "Admin User"
      }
    ]
  }
}

```

#### Cập nhật tồn kho

- **URL**: `/inventory/update`

- **Method**: `POST`

- **Mô tả**: Cập nhật tồn kho của sản phẩm

- **Headers**: `Authorization: Bearer <token>`

- **Request Body**:

```json
{
  "product_id": 1,
  "transaction_type": "IMPORT",
  "quantity": 20,
  "reference_type": "MANUAL",
  "reference_id": null,
  "transaction_date": "2023-02-15",
  "notes": "Điều chỉnh tồn kho thủ công",
  "location": "Kho A - Kệ 1"
}

```

- **Response**:

```json
{
  "success": true,
  "message": "Cập nhật tồn kho thành công",
  "data": {
    "product_id": 1,
    "product_name": "iPhone 13",
    "previous_quantity": 50,
    "current_quantity": 70,
    "transaction_type": "IMPORT",
    "quantity": 20
  }
}

```

### Báo cáo (Reports)

#### Báo cáo doanh thu

- **URL**: `/reports/revenue`

- **Method**: `GET`

- **Mô tả**: Lấy báo cáo doanh thu theo thời gian

- **Headers**: `Authorization: Bearer <token>`

- **Query Parameters**:
   - `from_date`: Từ ngày (định dạng: YYYY-MM-DD)
   - `to_date`: Đến ngày (định dạng: YYYY-MM-DD)
   - `group_by`: Nhóm theo (day, week, month, quarter, year)
   - `partner_id`: Lọc theo đối tác
   - `product_id`: Lọc theo sản phẩm
   - `category_id`: Lọc theo danh mục

- **Response**:

```json
{
  "success": true,
  "data": {
    "total_revenue": 500000000,
    "total_cost": 350000000,
    "total_profit": 150000000,
    "profit_margin": 30,
    "details": [
      {
        "period": "2023-01",
        "period_name": "Tháng 1/2023",
        "revenue": 200000000,
        "cost": 140000000,
        "profit": 60000000,
        "profit_margin": 30
      },
      {
        "period": "2023-02",
        "period_name": "Tháng 2/2023",
        "revenue": 300000000,
        "cost": 210000000,
        "profit": 90000000,
        "profit_margin": 30
      }
    ]
  }
}

```

#### Báo cáo sản phẩm

- **URL**: `/reports/products`

- **Method**: `GET`

- **Mô tả**: Lấy báo cáo về sản phẩm bán chạy

- **Headers**: `Authorization: Bearer <token>`

- **Query Parameters**:
   - `from_date`: Từ ngày (định dạng: YYYY-MM-DD)
   - `to_date`: Đến ngày (định dạng: YYYY-MM-DD)
   - `limit`: Số lượng sản phẩm hiển thị (mặc định: 10)
   - `category_id`: Lọc theo danh mục

- **Response**:

```json
{
  "success": true,
  "data": {
    "total_products_sold": 500,
    "total_revenue": 500000000,
    "details": [
      {
        "product_id": 1,
        "product_code": "SP001",
        "product_name": "iPhone 13",
        "category_name": "Điện thoại thông minh",
        "quantity_sold": 200,
        "revenue": 220000000,
        "percentage": 44
      },
      {
        "product_id": 2,
        "product_code": "SP002",
        "product_name": "Samsung Galaxy S21",
        "category_name": "Điện thoại thông minh",
        "quantity_sold": 150,
        "revenue": 150000000,
        "percentage": 30
      }
    ]
  }
}

```

#### Báo cáo đối tác

- **URL**: `/reports/partners`

- **Method**: `GET`

- **Mô tả**: Lấy báo cáo về đối tác

- **Headers**: `Authorization: Bearer <token>`

- **Query Parameters**:
   - `from_date`: Từ ngày (định dạng: YYYY-MM-DD)
   - `to_date`: Đến ngày (định dạng: YYYY-MM-DD)
   - `limit`: Số lượng đối tác hiển thị (mặc định: 10)
   - `partner_type`: Lọc theo loại đối tác

- **Response**:

```json
{
  "success": true,
  "data": {
    "total_partners": 6,
    "total_revenue": 500000000,
    "details": [
      {
        "partner_id": 1,
        "partner_code": "DC000001",
        "partner_name": "Công ty TNHH ABC",
        "partner_type": "DOMESTIC_CUSTOMER",
        "order_count": 5,
        "revenue": 250000000,
        "percentage": 50
      },
      {
        "partner_id": 2,
        "partner_code": "DC000002",
        "partner_name": "Công ty TNHH DEF",
        "partner_type": "DOMESTIC_CUSTOMER",
        "order_count": 3,
        "revenue": 150000000,
        "percentage": 30
      }
    ]
  }
}

```

#### Báo cáo tồn kho

- **URL**: `/reports/inventory`

- **Method**: `GET`

- **Mô tả**: Lấy báo cáo tồn kho

- **Headers**: `Authorization: Bearer <token>`

- **Query Parameters**:
   - `category_id`: Lọc theo danh mục
   - `status`: Lọc theo trạng thái (IN_STOCK, LOW_STOCK, OUT_OF_STOCK)

- **Response**:

```json
{
  "success": true,
  "data": {
    "total_products": 7,
    "total_value": 1500000000,
    "status_summary": {
      "IN_STOCK": 5,
      "LOW_STOCK": 1,
      "OUT_OF_STOCK": 1
    },
    "details": [
      {
        "product_id": 1,
        "product_code": "SP001",
        "product_name": "iPhone 13",
        "category_name": "Điện thoại thông minh",
        "quantity": 70,
        "min_level": 10,
        "max_level": 100,
        "status": "IN_STOCK",
        "value": 700000000
      },
      {
        "product_id": 2,
        "product_code": "SP002",
        "product_name": "Samsung Galaxy S21",
        "category_name": "Điện thoại thông minh",
        "quantity": 5,
        "min_level": 10,
        "max_level": 50,
        "status": "LOW_STOCK",
        "value": 50000000
      }
    ]
  }
}

```

### Thông báo (Notifications)

#### Lấy danh sách thông báo

- **URL**: `/notifications`

- **Method**: `GET`

- **Mô tả**: Lấy danh sách thông báo của người dùng hiện tại

- **Headers**: `Authorization: Bearer <token>`

- **Query Parameters**:
   - `page`: Số trang (mặc định: 1)
   - `per_page`: Số lượng mỗi trang (mặc định: 10)
   - `is_read`: Lọc theo trạng thái đã đọc (true, false)

- **Response**:

```json
{
  "success": true,
  "data": {
    "items": [
      {
        "id": 1,
        "title": "Đơn hàng mới",
        "content": "Đơn hàng ORD009 vừa được tạo",
        "notification_type": "ORDER",
        "reference_id": 9,
        "reference_code": "ORD009",
        "is_read": false,
        "created_at": "2023-02-10T10:30:00"
      }
    ],
    "total": 25,
    "page": 1,
    "per_page": 10,
    "pages": 3
  }
}

```

#### Đánh dấu thông báo đã đọc

- **URL**: `/notifications/<id>/read`

- **Method**: `PUT`

- **Mô tả**: Đánh dấu một thông báo là đã đọc

- **Headers**: `Authorization: Bearer <token>`

- **Response**:

```json
{
  "success": true,
  "message": "Đánh dấu thông báo đã đọc thành công"
}

```

#### Đánh dấu tất cả thông báo đã đọc

- **URL**: `/notifications/read-all`

- **Method**: `PUT`

- **Mô tả**: Đánh dấu tất cả thông báo là đã đọc

- **Headers**: `Authorization: Bearer <token>`

- **Response**:

```json
{
  "success": true,
  "message": "Đánh dấu tất cả thông báo đã đọc thành công"
}

```

### Tệp tin (Files)

#### Tải lên tệp tin

- **URL**: `/files/upload`

- **Method**: `POST`

- **Mô tả**: Tải lên một tệp tin mới

- **Headers**: `Authorization: Bearer <token>`

- **Request Body**: `multipart/form-data`
   - `file`: Tệp tin cần tải lên
   - `reference_type`: Loại tham chiếu (ORDER, PARTNER, CONTAINER, COST, SCHEDULE)
   - `reference_id`: ID của đối tượng tham chiếu
   - `description`: Mô tả tệp tin (tùy chọn)

- **Response**:

```json
{
  "success": true,
  "message": "Tải lên tệp tin thành công",
  "data": {
    "id": 10,
    "file_name": "invoice.pdf",
    "file_path": "/uploads/orders/9/invoice.pdf",
    "file_size": 1024,
    "file_type": "application/pdf",
    "reference_type": "ORDER",
    "reference_id": 9,
    "uploaded_at": "2023-02-15T10:30:00"
  }
}

```

#### Lấy danh sách tệp tin

- **URL**: `/files`

- **Method**: `GET`

- **Mô tả**: Lấy danh sách tệp tin

- **Headers**: `Authorization: Bearer <token>`

- **Query Parameters**:
   - `page`: Số trang (mặc định: 1)
   - `per_page`: Số lượng mỗi trang (mặc định: 10)
   - `reference_type`: Lọc theo loại tham chiếu
   - `reference_id`: Lọc theo ID tham chiếu

- **Response**:

```json
{
  "success": true,
  "data": {
    "items": [
      {
        "id": 10,
        "file_name": "invoice.pdf",
        "file_path": "/uploads/orders/9/invoice.pdf",
        "file_size": 1024,
        "file_type": "application/pdf",
        "reference_type": "ORDER",
        "reference_id": 9,
        "reference_code": "ORD009",
        "description": "Hóa đơn đơn hàng",
        "uploaded_by": 1,
        "uploaded_by_name": "Admin User",
        "uploaded_at": "2023-02-15T10:30:00"
      }
    ],
    "total": 15,
    "page": 1,
    "per_page": 10,
    "pages": 2
  }
}

```

#### Tải xuống tệp tin

- **URL**: `/files/<id>/download`
- **Method**: `GET`
- **Mô tả**: Tải xuống một tệp tin
- **Headers**: `Authorization: Bearer <token>`
- **Response**: Tệp tin được tải xuống

#### Xóa tệp tin

- **URL**: `/files/<id>`

- **Method**: `DELETE`

- **Mô tả**: Xóa một tệp tin

- **Headers**: `Authorization: Bearer <token>`

- **Response**:

```json
{
  "success": true,
  "message": "Xóa tệp tin thành công"
}

```

### Tag

#### Lấy danh sách tag

- **URL**: `/tags`

- **Method**: `GET`

- **Mô tả**: Lấy danh sách tất cả tag

- **Headers**: `Authorization: Bearer <token>`

- **Response**:

```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "name": "Bán chạy",
      "color": "#FF5733",
      "created_at": "2023-01-01T00:00:00",
      "updated_at": "2023-01-01T00:00:00"
    },
    {
      "id": 6,
      "name": "VIP",
      "color": "#CC33FF",
      "created_at": "2023-01-01T00:00:00",
      "updated_at": "2023-01-01T00:00:00"
    }
  ]
}

```

#### Tạo tag mới

- **URL**: `/tags`

- **Method**: `POST`

- **Mô tả**: Tạo một tag mới

- **Headers**: `Authorization: Bearer <token>`

- **Request Body**:

```json
{
  "name": "Khuyến mãi",
  "color": "#33FF57"
}

```

- **Response**:

```json
{
  "success": true,
  "message": "Tạo tag thành công",
  "data": {
    "id": 10,
    "name": "Khuyến mãi",
    "color": "#33FF57"
  }
}

```

#### Cập nhật tag

- **URL**: `/tags/<id>`

- **Method**: `PUT`

- **Mô tả**: Cập nhật thông tin tag

- **Headers**: `Authorization: Bearer <token>`

- **Request Body**:

```json
{
  "name": "Khuyến mãi đặc biệt",
  "color": "#33FF99"
}

```

- **Response**:

```json
{
  "success": true,
  "message": "Cập nhật tag thành công",
  "data": {
    "id": 10,
    "name": "Khuyến mãi đặc biệt",
    "color": "#33FF99"
  }
}

```

#### Xóa tag

- **URL**: `/tags/<id>`

- **Method**: `DELETE`

- **Mô tả**: Xóa một tag

- **Headers**: `Authorization: Bearer <token>`

- **Response**:

```json
{
  "success": true,
  "message": "Xóa tag thành công"
}

```

### Nhật ký kiểm toán (Audit Logs)