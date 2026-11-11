# BỘ KỊCH BẢN KIỂM THỬ CHI TIẾT (TEST CASES)
## DỰ ÁN: CỬA HÀNG MỸ PHẨM WINX (COSMETICS STORE)

* **Kỹ thuật áp dụng chính:** Phân vùng tương đương (Equivalence Partitioning - EP) & Phân tích giá trị biên (Boundary Value Analysis - BVA).
* **Môi trường thực thi:** Local (`http://localhost:8080/CosmeticsStore`), Chrome Browser, JUnit 5, Selenium WebDriver 4.

---

## 1. PHÂN HỆ ĐĂNG NHẬP (AUTHENTICATION - LOGIN)

| Test Case ID | Phân hệ / Màn hình | Tên ca kiểm thử | Kỹ thuật thiết kế | Loại (Pos/Neg) | Các bước thực hiện | Dữ liệu kiểm thử (Test Data) | Kết quả mong đợi | Cấp độ test (Unit/UI) | Trạng thái |
| :---: | :---: | :--- | :---: | :---: | :--- | :--- | :--- | :---: | :---: |
| **TC_AUTH_01** | Login | Đăng nhập thành công với vai trò Khách hàng (Customer) | EP | Positive | 1. Mở trang `/login`<br>2. Nhập Email khách hàng hợp lệ<br>3. Nhập mật khẩu đúng<br>4. Nhấn nút "Đăng nhập" | `email`: customer@winx.com<br>`password`: Customer@123 | Hệ thống lưu session role `CUSTOMER` và chuyển hướng về trang chủ `/home` | Controller & UI | Pending |
| **TC_AUTH_02** | Login | Đăng nhập thành công với vai trò Quản trị viên (Admin) | EP | Positive | 1. Mở trang `/login`<br>2. Nhập Email admin hợp lệ<br>3. Nhập mật khẩu đúng<br>4. Nhấn nút "Đăng nhập" | `email`: admin@winx.com<br>`password`: Admin@123 | Hệ thống lưu session role `ADMIN` và chuyển hướng về `/admin/dashboard` | Controller & UI | Pending |
| **TC_AUTH_03** | Login | Đăng nhập thất bại khi sai mật khẩu | EP | Negative | 1. Mở trang `/login`<br>2. Nhập Email đúng<br>3. Nhập sai mật khẩu<br>4. Nhấn nút "Đăng nhập" | `email`: customer@winx.com<br>`password`: WrongPass#99 | Không chuyển trang, giữ lại `oldEmail`, hiển thị thông báo lỗi `"Sai email hoặc mật khẩu không đúng"` | Controller & UI | Pending |
| **TC_AUTH_04** | Login | Đăng nhập thất bại khi Email không tồn tại | EP | Negative | 1. Mở trang `/login`<br>2. Nhập Email chưa từng đăng ký<br>3. Nhập mật khẩu bất kỳ<br>4. Nhấn nút "Đăng nhập" | `email`: notfound@winx.com<br>`password`: 123456 | Hiển thị thông báo lỗi `"Sai email hoặc mật khẩu không đúng"` | Controller & UI | Pending |
| **TC_AUTH_05** | Login | Bỏ trống trường Email khi submit | EP/BVA | Negative | 1. Mở trang `/login`<br>2. Bỏ trống ô Email<br>3. Nhập mật khẩu<br>4. Nhấn nút "Đăng nhập" | `email`: `""`<br>`password`: 123456 | Trình duyệt chặn submit (HTML5 validation: "Please fill out this field") | UI | Pending |
| **TC_AUTH_06** | Login | Bỏ trống trường Mật khẩu khi submit | EP/BVA | Negative | 1. Mở trang `/login`<br>2. Nhập Email hợp lệ<br>3. Bỏ trống ô Mật khẩu<br>4. Nhấn nút "Đăng nhập" | `email`: customer@winx.com<br>`password`: `""` | Trình duyệt chặn submit (HTML5 validation) hoặc Controller báo lỗi | UI & Controller | Pending |
| **TC_AUTH_07** | Login | Nhập Email sai định dạng cú pháp | EP | Negative | 1. Mở trang `/login`<br>2. Nhập email thiếu `@` hoặc domain<br>3. Nhập mật khẩu<br>4. Nhấn "Đăng nhập" | `email`: invalid-email-format<br>`password`: 123456 | Trình duyệt hiển thị cảnh báo định dạng email không hợp lệ | UI | Pending |

---

## 2. PHÂN HỆ ĐĂNG KÝ TÀI KHOẢN (REGISTRATION)

| Test Case ID | Phân hệ / Màn hình | Tên ca kiểm thử | Kỹ thuật thiết kế | Loại (Pos/Neg) | Các bước thực hiện | Dữ liệu kiểm thử (Test Data) | Kết quả mong đợi | Cấp độ test (Unit/UI) | Trạng thái |
| :---: | :---: | :--- | :---: | :---: | :--- | :--- | :--- | :---: | :---: |
| **TC_REG_01** | Register | Đăng ký tài khoản thành công với thông tin hợp lệ | EP | Positive | 1. Mở trang `/register`<br>2. Nhập đầy đủ Tên, Email, SĐT, Mật khẩu<br>3. Bấm "Đăng ký tài khoản" | `name`: Nguyễn Văn A<br>`email`: newuser_01@winx.com<br>`phone`: 0987654321<br>`password`: Pass@1234 | Tạo bản ghi mới trong bảng `Customer`, chuyển hướng sang `/login` | Controller & UI | Pending |
| **TC_REG_02** | Register | Đăng ký thất bại khi Email đã tồn tại trong hệ thống | EP | Negative | 1. Mở trang `/register`<br>2. Nhập Email đã có trong CSDL<br>3. Nhập các trường còn lại hợp lệ<br>4. Bấm "Đăng ký tài khoản" | `name`: Người Dùng Cũ<br>`email`: customer@winx.com<br>`phone`: 0912345678<br>`password`: 123456 | Không thêm vào DB, hiển thị thông báo lỗi: `"Email này đã được sử dụng"` | Controller & UI | Pending |
| **TC_REG_03** | Register | Bỏ trống trường Họ và tên | EP/BVA | Negative | 1. Mở `/register`<br>2. Để trống Họ và tên<br>3. Điền email, phone, password<br>4. Submit | `name`: `""`<br>`email`: test@winx.com<br>`phone`: 0900000001<br>`password`: 123456 | Trình duyệt kích hoạt thuộc tính `required` chặn gửi form | UI | Pending |
| **TC_REG_04** | Register | Bỏ trống Số điện thoại | EP/BVA | Negative | 1. Mở `/register`<br>2. Điền họ tên, email, password<br>3. Để trống trường phone<br>4. Submit | `name`: Trần B<br>`email`: tranb@winx.com<br>`phone`: `""`<br>`password`: 123456 | Bị chặn bởi validation form, yêu cầu nhập số điện thoại | UI | Pending |
| **TC_REG_05** | Register | Bỏ trống trường Mật khẩu | EP/BVA | Negative | 1. Mở `/register`<br>2. Điền tên, email, phone<br>3. Để trống password<br>4. Submit | `name`: Lê C<br>`email`: lec@winx.com<br>`phone`: 0933333333<br>`password`: `""` | Bị chặn bởi validation form, yêu cầu nhập mật khẩu | UI | Pending |

---

## 3. PHÂN HỆ QUẢN LÝ SẢN PHẨM - CRUD ADMIN (PRODUCT MANAGEMENT)

| Test Case ID | Phân hệ / Màn hình | Tên ca kiểm thử | Kỹ thuật thiết kế | Loại (Pos/Neg) | Các bước thực hiện | Dữ liệu kiểm thử (Test Data) | Kết quả mong đợi | Cấp độ test (Unit/UI) | Trạng thái |
| :---: | :---: | :--- | :---: | :---: | :--- | :--- | :--- | :---: | :---: |
| **TC_PROD_01** | Product CRUD | Thêm mới sản phẩm hợp lệ đầy đủ thông tin | EP | Positive | 1. Mở `/admin/product?action=new`<br>2. Điền tên, giá, số lượng, chọn danh mục<br>3. Bấm "Lưu" | `productName`: "Son Kem Lì Winx"<br>`price`: "150000"<br>`stockQuantity`: "50"<br>`categoryID`: "1" | Thêm thành công vào DB, chuyển hướng về `/admin/product`, sản phẩm hiển thị trên danh sách | Controller & UI | Pending |
| **TC_PROD_02** | Product CRUD | Thêm sản phẩm thất bại khi để trống Tên sản phẩm | EP/BVA | Negative | 1. Mở form thêm mới<br>2. Bỏ trống Tên sản phẩm<br>3. Điền giá và số lượng hợp lệ<br>4. Bấm "Lưu" | `productName`: `""`<br>`price`: "100000"<br>`stockQuantity`: "10"<br>`categoryID`: "1" | Báo lỗi nghiệp vụ: `"Tên sản phẩm không được để trống."`, không lưu vào DB | Controller & UI | Pending |
| **TC_PROD_03** | Product CRUD | Thêm sản phẩm với Tên chỉ chứa khoảng trắng | EP/BVA | Negative | 1. Nhập tên toàn dấu cách `"   "`<br>2. Điền các trường khác hợp lệ<br>3. Bấm "Lưu" | `productName`: `"   "`<br>`price`: "100000"<br>`stockQuantity`: "10"<br>`categoryID`: "1" | Controller ném lỗi: `"Tên sản phẩm không được để trống."` | Controller | Pending |
| **TC_PROD_04** | Product CRUD | Kiểm tra biên Giá bán bằng 0 (Biên dưới không hợp lệ) | BVA | Negative | 1. Mở form thêm mới<br>2. Nhập Giá = 0<br>3. Điền các thông tin khác hợp lệ<br>4. Bấm "Lưu" | `productName`: "Kem Dưỡng Da"<br>`price`: "0"<br>`stockQuantity`: "20"<br>`categoryID`: "1" | Hệ thống ném lỗi: `"Giá phải lớn hơn 0."` (hoặc chặn bởi `min="1"`) | Controller & UI | Pending |
| **TC_PROD_05** | Product CRUD | Kiểm tra biên Giá bán âm (Số âm không hợp lệ) | BVA | Negative | 1. Nhập Giá = -50000<br>2. Điền các thông tin khác hợp lệ<br>3. Bấm "Lưu" | `productName`: "Serum Trắng Da"<br>`price`: "-50000"<br>`stockQuantity`: "10"<br>`categoryID`: "1" | Hệ thống ném lỗi: `"Giá phải lớn hơn 0."` | Controller & UI | Pending |
| **TC_PROD_06** | Product CRUD | Kiểm tra biên Giá bán nhỏ nhất hợp lệ (Giá = 1 VNĐ) | BVA | Positive | 1. Nhập Giá = 1 VNĐ (biên dương nhỏ nhất)<br>2. Các thông tin khác hợp lệ<br>3. Bấm "Lưu" | `productName`: "Mặt Nạ Dưỡng Mẫu"<br>`price`: "1"<br>`stockQuantity`: "5"<br>`categoryID`: "1" | Lưu thành công vào DB, chuyển hướng về `/admin/product` | Controller & UI | Pending |
| **TC_PROD_07** | Product CRUD | Kiểm tra biên Số lượng tồn kho bằng 0 (Biên hợp lệ - Hết hàng) | BVA | Positive | 1. Nhập Số lượng = 0<br>2. Điền các trường khác hợp lệ<br>3. Bấm "Lưu" | `productName`: "Son Hết Hàng"<br>`price`: "200000"<br>`stockQuantity`: "0"<br>`categoryID`: "1" | Lưu thành công vào DB ($Stock = 0$), hiển thị trên bảng | Controller & UI | Pending |
| **TC_PROD_08** | Product CRUD | Kiểm tra biên Số lượng tồn kho âm (Số âm không hợp lệ) | BVA | Negative | 1. Nhập Số lượng = -1<br>2. Điền các trường khác hợp lệ<br>3. Bấm "Lưu" | `productName`: "Nước Tẩy Trang"<br>`price`: "120000"<br>`stockQuantity`: "-1"<br>`categoryID`: "1" | Hệ thống ném lỗi: `"Số lượng không được âm."` | Controller & UI | Pending |
| **TC_PROD_09** | Product CRUD | Thêm sản phẩm thất bại khi không chọn Loại sản phẩm (Category) | EP | Negative | 1. Mở form thêm mới<br>2. Không chọn dropdown loại sản phẩm<br>3. Bấm "Lưu" | `productName`: "Phấn Phủ"<br>`price`: "80000"<br>`stockQuantity`: "15"<br>`categoryID`: `""` | Hệ thống ném lỗi: `"Vui lòng chọn loại sản phẩm."` | Controller & UI | Pending |
| **TC_PROD_10** | Product CRUD | Nhập dữ liệu chữ cái vào ô Giá tiền (Lỗi định dạng số) | EP | Negative | 1. Gửi chuỗi không phải số vào ô Giá (`price = "abc"`)<br>2. Gửi request POST | `productName`: "Sữa Rửa Mặt"<br>`price`: "abc"<br>`stockQuantity`: "10"<br>`categoryID`: "1" | Bắt ngoại lệ `NumberFormatException`, hiển thị: `"Dữ liệu số không hợp lệ."` | Controller | Pending |
| **TC_PROD_11** | Product CRUD | Cập nhật thông tin sản phẩm thành công | EP | Positive | 1. Mở `/admin/product?action=edit&id=1`<br>2. Đổi tên sản phẩm và giá<br>3. Bấm "Lưu" | `productID`: "1"<br>`productName`: "Son Winx Mới"<br>`price`: "180000"<br>`stockQuantity`: "40"<br>`categoryID`: "1" | Bản ghi được cập nhật trong DB, chuyển hướng về `/admin/product` | Controller & UI | Pending |
| **TC_PROD_12** | Product CRUD | Xóa sản phẩm thành công theo ID | EP | Positive | 1. Truy cập URL `/admin/product?action=delete&id=1`<br>2. Xác nhận điều hướng | `id`: "1" | Bản ghi bị xóa khỏi CSDL, chuyển hướng về `/admin/product` | Controller & UI | Pending |
