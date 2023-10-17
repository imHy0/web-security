# 1. Định nghĩa

- ***File upload vulnerabilities (Lỗ hổng tải lên tệp)*** là khi máy chủ web cho phép người dùng tải tệp lên hệ thống tệp của nó mà không xác thực đầy đủ tên, loại, nội dung hoặc kích thước của chúng.

    → gây nguy hiểm, người dùng có thể tải lên tập tùy ý thậm chí có thể là các tệp tập lệnh phía máy chủ cho phép thực thi mã từ xa.

# 2. Ảnh hưởng: phụ thuộc vào 2 yếu tố chính

* trang web không xác thực đúng cách: tên, loại,...

* những hạn chế được áp dụng khi tải tệp thành công

# 3. Lý do phát sinh

- Không có bất cứ hạn chế nào đối với các tệp mà người dùng được phép tải lên.

# 3. Một số loại phổ biến

## [***3.1. Khai thác tải lên tệp không hạn chế để triển khai web shell***](./lab/part1.md)

> (lab 1)

* Web shell là một tập lệnh độc hại cho phép kẻ tấn công thực hiện các lệnh tùy ý trên máy chủ web từ xa chỉ bằng cách gửi các yêu cầu HTTP đến đúng điểm cuối.

    Nếu bạn có thể tải lên thành công một web shell, bạn có toàn quyền kiểm soát máy chủ một cách hiệu quả. Điều này có nghĩa là bạn có thể đọc và ghi các tệp tùy ý, trích xuất dữ liệu nhạy cảm, thậm chí sử dụng máy chủ để xoay các cuộc tấn công chống lại cả cơ sở hạ tầng nội bộ và các máy chủ khác bên ngoài mạng.

## [***3.2. Khai thác xác thực thiếu sót của tệp tải lên***](./lab/part2.md)

### 3.2.1. Xác thực loại tệp bị thiếu sót (lab 2)

### 3.2.2. Ngăn chặn việc thực thi tệp trong các thư mục mà người dùng có thể truy cập (lab 3)

### 3.2.3. Danh sách đen không đầy đủ các loại tệp nguy hiểm

* Ghi đè cấu hình máy chủ (lab 4)

* Làm xáo trộn phần mở rộng tập tin (lab 5)

### 3.2.4. Xác thực sai sót nội dung của tập tin (lab 6)

### 3.2.5. Khai thác các điều kiện tải lên tệp (Race conditions) **[EXPERT]**

# 4. Phòng chống

* Kiểm tra file extension dựa trên whitelist thay vì blacklist. Việc đoán những tiện ích mở rộng nào bạn có thể muốn cho phép sẽ dễ dàng hơn nhiều so với việc đoán những tiện ích mở rộng nào mà kẻ tấn công có thể cố tải lên.

* Đảm bảo tên tệp không chứa bất kỳ chuỗi con nào có thể được hiểu là một thư mục hoặc một chuỗi truyền tải `../`.

* Đổi tên các file đã tải lên để tránh xung đột có thể khiến các file hiện có bị ghi đè.

* Không tải tệp lên hệ thống tệp cố định của máy chủ cho đến khi chúng được xác thực đầy đủ.

* Càng nhiều càng tốt, hãy sử dụng một khung đã được thiết lập để xử lý trước các tệp tải lên thay vì cố gắng viết các cơ chế xác thực của riêng bạn.
