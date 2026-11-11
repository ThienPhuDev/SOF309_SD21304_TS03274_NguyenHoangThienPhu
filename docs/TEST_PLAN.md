# KẾ HOẠCH KIỂM THỬ (TEST PLAN)
## DỰ ÁN: CỬA HÀNG MỸ PHẨM WINX (COSMETICS STORE)

* **Học phần:** SOF309 - Kiểm thử nâng cao cùng AI
* **Sinh viên thực hiện:** Nguyễn Hoàng Thiên Phú
* **Mã số sinh viên:** TS03274
* **Lớp:** SD21304
* **Giảng viên hướng dẫn:** ThS. Trần Quang Bình (binhtq7 - Thầy Mười Giáo)
* **Thời gian lập:** 11/2026

---

## 1. TỔNG QUAN & MỤC TIÊU KIỂM THỬ (TEST OBJECTIVES)
* Đảm bảo tính đúng đắn về mặt logic nghiệp vụ (Business Rules) của các Servlet điều khiển Backend (`LoginController`, `RegisterController`, `ProductServlet`).
* Kiểm thử tính toàn vẹn dữ liệu, bắt lỗi nhập liệu và các giá trị biên thông qua Unit Test với **JUnit 5** (`@ParameterizedTest`, `assertThrows`, `@Timeout`).
* Kiểm thử giao diện tự động (End-to-End UI Automation Test) bằng **Selenium WebDriver 4** áp dụng mô hình chuẩn **Page Object Model (POM)** và điều phối bởi **TestNG**.
* Tích hợp quy trình kiểm thử tự động vào luồng CI/CD với **GitHub Actions** chạy ở chế độ Headless Chrome, tự động xuất báo cáo và chụp ảnh màn hình khi có lỗi.

---

## 2. PHẠM VI KIỂM THỬ (TEST SCOPE)

### 2.1. Trong phạm vi kiểm thử (In-Scope)
* **Chức năng Đăng nhập (`/login`):**
  * Đăng nhập thành công với tài khoản Khách hàng (`CUSTOMER`) $\rightarrow$ chuyển hướng về `/home`.
  * Đăng nhập thành công với tài khoản Quản trị (`ADMIN`) $\rightarrow$ chuyển hướng về `/admin/dashboard`.
  * Xử lý lỗi khi sai email, sai mật khẩu, hoặc để trống các trường bắt buộc.
* **Chức năng Đăng ký tài khoản (`/register`):**
  * Đăng ký tài khoản khách hàng mới với đầy đủ thông tin hợp lệ (`name`, `email`, `phone`, `password`).
  * Xử lý trùng lặp Email trong cơ sở dữ liệu (`Customer`).
  * Kiểm tra các ràng buộc bắt buộc nhập và tính hợp lệ của dữ liệu.
* **Chức năng Quản lý Sản phẩm - CRUD Admin (`/admin/product`):**
  * Thêm mới sản phẩm: Kiểm tra các giá trị biên của `price` ($> 0$), `stockQuantity` ($\ge 0$), `productName` (không để trống), và bắt buộc chọn `categoryID`.
  * Cập nhật thông tin sản phẩm và xử lý ngoại lệ số học (`NumberFormatException`).
  * Xóa sản phẩm theo ID.

### 2.2. Ngoài phạm vi kiểm thử (Out-of-Scope)
* Kiểm thử hiệu năng tải cao (Stress/Load Testing với JMeter).
* Kiểm thử lỗ hổng bảo mật chuyên sâu (Penetration Testing, SQL Injection mức hạ tầng).
* Kiểm thử tương thích trên các trình duyệt cũ (IE, Edge Legacy) hoặc thiết bị di động vật lý.

---

## 3. CHIẾN LƯỢC & PHƯƠNG PHÁP KIỂM THỬ (TEST STRATEGY & TECHNIQUES)

### 3.1. Kỹ thuật thiết kế ca kiểm thử (Test Design Techniques)
* **Phân vùng tương đương (Equivalence Partitioning - EP):**
  * Chia tập dữ liệu đầu vào thành các vùng hợp lệ (Valid Partition) và không hợp lệ (Invalid Partition) cho các trường: Email, Mật khẩu, Giá sản phẩm, Số lượng tồn kho.
* **Phân tích giá trị biên (Boundary Value Analysis - BVA):**
  * Áp dụng triệt để cho các trường số liệu:
    * Giá sản phẩm (`price`): Giá trị biên dưới: -1 (không hợp lệ), 0 (không hợp lệ), 0.01 / 1 (hợp lệ).
    * Số lượng tồn kho (`stockQuantity`): Biên dưới: -1 (không hợp lệ), 0 (hợp lệ - hết hàng), 1 (hợp lệ).
    * Giá giảm (`discountPrice`): 0 (hợp lệ - không giảm), > 0 (hợp lệ).

### 3.2. Cấp độ kiểm thử (Testing Levels)
1. **Unit / Controller Testing:** Sử dụng JUnit 5 + Mockito để kiểm thử logic xử lý trong các phương thức `doGet()`, `doPost()`, các hàm validation và ném ngoại lệ (`assertThrows`).
2. **UI Automation Testing:** Sử dụng Selenium WebDriver + TestNG theo cấu trúc Page Object Model:
   * Tách biệt các class Page (`LoginPage`, `RegisterPage`, `ProductPage`) với class Test (`LoginTest`, `CrudProductTest`).
   * Sử dụng `WebDriverWait` (Explicit Wait) để đồng bộ DOM.
   * Áp dụng `@DataProvider` cho Data-Driven Testing và `ITestListener` để chụp ảnh màn hình lưu vào `screenshots/` khi thất bại.
3. **CI/CD Integration Testing:** Chạy toàn bộ test suite trên runner Ubuntu (`ubuntu-latest`) của GitHub Actions với cờ Chrome `--headless=new`.

---

## 4. TIÊU CHÍ HOÀN THÀNH & NGHIỆM THU (CRITERIA FOR PASS/FAIL)
* **Tiêu chí hoàn thành kiểm thử:**
  * 100% các ca kiểm thử trong bộ Test Case được thực thi.
  * Tỷ lệ Pass của Unit Test Controller đạt **100%** (`mvn test` không có Failure hoặc Error).
  * Automation Test Selenium trên máy local và môi trường CI chạy mượt mà, không treo luồng.
  * Pipeline **GitHub Actions** báo tích Xanh (Success).
  * Đầy đủ hồ sơ minh chứng tương tác AI trong thư mục `docs/ai-logs/` theo yêu cầu điểm A.
