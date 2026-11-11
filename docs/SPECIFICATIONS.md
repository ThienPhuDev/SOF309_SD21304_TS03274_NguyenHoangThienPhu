# ĐẶC TẢ NGHIỆP VỤ WEB CÁ NHÂN (SPECIFICATIONS)
> *Dùng làm ngữ cảnh (Context) gửi cho AI để sinh Test Plan & Test Case chính xác theo dự án riêng của bạn.*

---

## 1. THÔNG TIN DỰ ÁN
* **Tên đề tài:** Cửa hàng Mỹ phẩm WINX (CosmeticsStore)
* **Sinh viên thực hiện:** Nguyễn Hoàng Thiên Phú - TS03274 - SD21304
* **Công nghệ nền tảng:** Java 17 / Jakarta EE (Servlet 6.0, JSP, JSTL), Hibernate ORM 6.5, Microsoft SQL Server (mssql-jdbc), Maven
* **URL chạy local:** `http://localhost:8080/CosmeticsStore` (hoặc `http://localhost:8080`)

---

## 2. DANH SÁCH MÀN HÌNH VÀ URL ĐIỀU HƯỚNG
| Màn hình / Chức năng | URL (Route) | Method | Giao diện JSP tương ứng | Ghi chú |
| :--- | :--- | :---: | :--- | :--- |
| **Đăng nhập** | `/login` | GET, POST | `/views/auth/login.jsp` | Phân quyền Khách hàng (`CUSTOMER`) & Quản trị viên (`ADMIN`) |
| **Đăng ký tài khoản** | `/register` | GET, POST | `/views/auth/register.jsp` | Đăng ký tài khoản khách hàng mới |
| **Trang chủ khách hàng** | `/home` | GET | `/index.jsp` | Hiển thị sản phẩm, danh mục, tin tức |
| **Admin Dashboard** | `/admin/dashboard` | GET | `/views/admin/dashboard.jsp` | Bảng điều khiển quản trị |
| **Danh sách sản phẩm (Admin)**| `/admin/product`, `/admin/products` | GET | `/views/admin/products/list-sp.jsp` | Xem danh sách toàn bộ sản phẩm |
| **Thêm mới sản phẩm** | `/admin/product?action=new` | GET, POST | `/views/admin/products/form.jsp` | Form nhập liệu thêm sản phẩm (POST gửi về `/product`) |
| **Chỉnh sửa sản phẩm** | `/admin/product?action=edit&id={id}` | GET, POST | `/views/admin/products/form.jsp` | Cập nhật thông tin sản phẩm |
| **Xóa sản phẩm** | `/admin/product?action=delete&id={id}` | GET | Chuyển hướng về `/admin/product` | Xóa sản phẩm theo ID |

---

## 3. QUY TẮC NGHIỆP VỤ & RÀNG BUỘC DỮ LIỆU (BUSINESS RULES & VALIDATION)

### 3.1. Chức năng Đăng nhập (`/login`)
* **Các trường dữ liệu:**
  * `email` (Email): Bắt buộc nhập (`required`), đúng định dạng email.
  * `password` (Mật khẩu): Bắt buộc nhập (`required`), kiểu mật khẩu ẩn.
* **Quy tắc xử lý:**
  * Nếu đăng nhập bằng tài khoản Customer hợp lệ: Lưu session `currentUser`, role `CUSTOMER`, chuyển hướng sang trang chủ `/home`.
  * Nếu đăng nhập bằng tài khoản Admin hợp lệ: Lưu session `currentUser`, role `ADMIN`, chuyển hướng sang trang quản trị `/admin/dashboard`.
  * Nếu sai email hoặc sai mật khẩu: Giữ lại giá trị `oldEmail` trên form, hiển thị thông báo lỗi: `"Sai email hoặc mật khẩu không đúng"` tại thẻ `div.error-message`.
  * Bỏ trống trường bắt buộc: Báo lỗi validation từ trình duyệt hoặc hệ thống.

### 3.2. Chức năng Đăng ký tài khoản (`/register`)
* **Các trường dữ liệu:**
  * `name` (Họ và tên): Bắt buộc nhập (`required`), chuỗi ký tự text.
  * `email` (Email): Bắt buộc nhập (`required`), định dạng email hợp lệ.
  * `phone` (Số điện thoại): Bắt buộc nhập (`required`), kiểu tel.
  * `password` (Mật khẩu): Bắt buộc nhập (`required`), kiểu password.
* **Quy tắc xử lý:**
  * Kiểm tra trùng lặp Email: Nếu email đã tồn tại trong CSDL $\rightarrow$ Báo lỗi: `"Email này đã được sử dụng"`.
  * Nếu dữ liệu hợp lệ: Thêm bản ghi mới vào bảng `Customer` (địa chỉ mặc định `N'Chưa cập nhật'`), chuyển hướng về trang đăng nhập `/login`.
  * Bắt lỗi ngoại lệ SQL/Hệ thống: Hiển thị `"Lỗi hệ thống: <chi tiết>"`.

### 3.3. Chức năng Quản lý Sản phẩm - CRUD Admin (`/admin/product`)
* **Các trường dữ liệu trên form:**
  * `productID`: Ẩn (`type="hidden"`). Rỗng/0 khi thêm mới, có giá trị số khi chỉnh sửa.
  * `productName` (Tên sản phẩm): Text, bắt buộc nhập (`required`). Không được để trống hoặc chỉ chứa khoảng trắng. Vi phạm $\rightarrow$ Báo lỗi: `"Tên sản phẩm không được để trống."`.
  * `image` (URL hình ảnh): Text link URL ảnh trực tiếp (xem trước qua JavaScript `previewImg`).
  * `price` (Giá bán): Số (`type="number"`, `step="any"`, `min="1"`), bắt buộc nhập (`required`). Giá phải lớn hơn 0 ($Price > 0$). Vi phạm $\rightarrow$ Báo lỗi: `"Vui lòng nhập giá."` hoặc `"Giá phải lớn hơn 0."`.
  * `discountPrice` (Giá đã giảm): Số (`type="number"`, `step="any"`, `min="0"`), tùy chọn. Nếu có nhập thì phải lớn hơn 0 ($DiscountPrice > 0$).
  * `stockQuantity` (Số lượng trong kho): Số nguyên (`type="number"`, `min="0"`), bắt buộc nhập (`required`). Không được âm ($Stock \ge 0$). Vi phạm $\rightarrow$ Báo lỗi: `"Vui lòng nhập số lượng."` hoặc `"Số lượng không được âm."`.
  * `description` (Mô tả chi tiết): Đoạn văn bản dài (`textarea`), tùy chọn.
  * `categoryID` (Loại sản phẩm): Chọn từ dropdown (`select`), bắt buộc chọn (`required`). Vi phạm $\rightarrow$ Báo lỗi: `"Vui lòng chọn loại sản phẩm."`.
* **Quy tắc xử lý:**
  * Nhập sai định dạng số (NumberFormatException): Báo lỗi `"Dữ liệu số không hợp lệ."`.
  * Thêm mới thành công: Lưu vào DB qua `ProductDao.create(p)` $\rightarrow$ Chuyển hướng về `/admin/product`.
  * Cập nhật thành công: Cập nhật DB qua `ProductDao.update(p)` $\rightarrow$ Chuyển hướng về `/admin/product`.
  * Xóa sản phẩm: Xóa bản ghi trong DB qua `ProductDao.delete(product)` $\rightarrow$ Chuyển hướng về `/admin/product`.
