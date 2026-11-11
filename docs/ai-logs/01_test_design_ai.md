# Nhật ký AI: 01. Thiết kế Test Plan & Bộ Test Case bằng AI (Y2 - Điểm A)

## 1. Ngữ cảnh & Mục tiêu
- **Mục tiêu:** Sử dụng đặc tả nghiệp vụ từ file `docs/SPECIFICATIONS.md` làm Context (RAG) để yêu cầu AI trong vai trò QA Lead thiết kế Kế hoạch kiểm thử (`TEST_PLAN.md`) và Bộ kịch bản kiểm thử (`TEST_CASES.md`).
- **Phạm vi áp dụng:** Áp dụng cho 3 phân hệ chính của dự án Cửa hàng Mỹ phẩm WINX (`CosmeticsStore`):
  1. Chức năng Đăng nhập (`LoginController` & `/views/auth/login.jsp`).
  2. Chức năng Đăng ký tài khoản (`RegisterController` & `/views/auth/register.jsp`).
  3. Chức năng Quản lý Sản phẩm - CRUD Admin (`ProductServlet` & `/views/admin/products/form.jsp`).
- **Kỹ thuật yêu cầu:** Phân vùng tương đương (Equivalence Partitioning - EP) và Phân tích giá trị biên (Boundary Value Analysis - BVA).

---

## 2. Câu lệnh đã gửi cho AI (Prompt)
> "Bạn là QA Lead. Dưới đây là đặc tả nghiệp vụ dự án Web của tôi: [Toàn bộ nội dung file docs/SPECIFICATIONS.md]. Hãy áp dụng kỹ thuật Phân vùng tương đương và Phân tích giá trị biên để thiết kế Test Plan và Bộ Test Case chi tiết cho Controller và UI."

---

## 3. Kết quả do AI sinh ra (Raw Output)
* **Về Test Plan:**
  - AI đã phân chia cấu trúc Test Plan chuẩn gồm: Mục tiêu kiểm thử, Phạm vi trong/ngoài (In-scope / Out-of-scope), Chiến lược kỹ thuật kiểm thử (EP, BVA, POM cho UI, Mockito cho Controller), Tiêu chí hoàn thành (100% Pass, pipeline CI/CD GitHub Actions báo xanh).
* **Về Test Cases:**
  - AI sinh 24 test cases bao phủ đầy đủ các màn hình:
    - Login: Đăng nhập Customer, Admin, sai mật khẩu, sai email, bỏ trống trường, sai định dạng email.
    - Register: Đăng ký thành công, trùng lặp email, bỏ trống các trường bắt buộc (name, email, phone, password).
    - Product CRUD: Thêm thành công, tên rỗng/khoảng trắng, biên giá ($Price \le 0$, $Price = 1$), biên tồn kho ($Stock < 0$, $Stock = 0$), thiếu danh mục, sai định dạng số, sửa và xóa sản phẩm.

---

## 4. Chỉnh sửa của Sinh viên & Lý do sửa
- **Những điểm AI làm ban đầu chưa sát với code thực tế:**
  1. Ban đầu AI giả định form đăng ký có trường `confirmPassword`, tuy nhiên qua rà soát file `register.jsp` và `RegisterController.java` của dự án thì form thực tế chỉ có 4 trường: `name`, `email`, `phone`, `password`.
  2. Một số test case về giá chỉ kiểm tra số âm, sinh viên đã yêu cầu bổ sung các ca kiểm thử giá trị biên cụ thể theo BVA: $Price = 0$ (biên không hợp lệ), $Price = 1$ (biên dương nhỏ nhất hợp lệ), $Stock = 0$ (biên hợp lệ khi hết hàng), $Stock = -1$ (biên âm không hợp lệ).
  3. Chuẩn hóa lại mã định danh Test Case theo cấu trúc quy ước của môn học: `TC_AUTH_xx`, `TC_REG_xx`, `TC_PROD_xx`.
- **Kết quả nghiệm thu:**
  - Đã cập nhật trọn vẹn vào 2 file: `docs/TEST_PLAN.md` và `docs/TEST_CASES.md`.
