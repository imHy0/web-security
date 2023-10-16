
# 1. Định nghĩa

- ***Information Disclosure*** (tiết lộ thông tin hay rò rỉ thông tin) là việc trang web vô tình bị tiết lộ thông tin nhạy cảm cho người dùng như:

  - Dữ liệu về người dùng khác như tên người dùng, ...

  - Dữ liệu thương mại hoặc kinh doanh nhạy cảm

  - Chi tiết kỹ thuật về trang web và cơ sở hạ tầng của nó

# 2. Ví dụ về tiết lộ thông tin

  - Tiết lộ tên của các thư mục ẩn, cấu trúc và nội dung của chúng thông qua tệp `robots.txt` hoặc danh sách thư mục

  - Cung cấp quyền truy cập vào các tệp mã nguồn thông qua các bản sao lưu tạm thời

  - Đề cập rõ ràng đến tên bảng hoặc cột cơ sở dữ liệu trong thông báo lỗi

  - Tiết lộ thông tin có độ nhạy cảm cao một cách không cần thiết, chẳng hạn như chi tiết thẻ tín dụng

  - Khóa API mã hóa cứng, địa chỉ IP, thông tin xác thực cơ sở dữ liệu, v.v. trong mã nguồn

  - Gợi ý sự tồn tại hay vắng mặt của tài nguyên, tên người dùng, v.v. thông qua những khác biệt nhỏ trong hành vi ứng dụng

# 3. Lý do phát sinh

- Không xóa được nội dung nội bộ khỏi nội dung công khai. Ví dụ: nhận xét của nhà phát triển trong đánh dấu đôi khi được hiển thị cho người dùng trong môi trường sản xuất.

- Cấu hình không an toàn của trang web và các công nghệ liên quan. Ví dụ: việc không tắt các tính năng chẩn đoán và gỡ lỗi đôi khi có thể cung cấp cho kẻ tấn công những công cụ hữu ích để giúp chúng lấy được thông tin nhạy cảm. Cấu hình mặc định cũng có thể khiến các trang web dễ bị tấn công, chẳng hạn như bằng cách hiển thị các thông báo lỗi quá chi tiết.

- Thiết kế và hành vi thiếu sót của ứng dụng. Ví dụ: nếu một trang web trả về các phản hồi riêng biệt khi xảy ra các trạng thái lỗi khác nhau, điều này cũng có thể cho phép kẻ tấn công liệt kê dữ liệu nhạy cảm, chẳng hạn như thông tin xác thực người dùng hợp lệ.

# 4. Ảnh hưởng

- Các lỗ hổng tiết lộ thông tin có thể có cả tác động trực tiếp và gián tiếp tùy thuộc vào mục đích của trang web và do đó, kẻ tấn công có thể lấy được thông tin gì.

- Mặt khác, việc rò rỉ thông tin kỹ thuật, chẳng hạn như cấu trúc thư mục hoặc khuôn khổ của bên thứ ba nào đang được sử dụng, có thể có ít hoặc không có tác động trực tiếp. Tuy nhiên, nếu rơi vào tay kẻ xấu, đây có thể là thông tin quan trọng cần thiết để thực hiện bất kỳ hoạt động khai thác nào khác. Mức độ nghiêm trọng trong trường hợp này phụ thuộc vào những gì kẻ tấn công có thể làm với thông tin này.

# 5. Testing

- Fuzzing

- Burp Scanner

- Burp's engagement tools: 

  - Search: tìm kiểm regex hoặc tìm kiếm phủ định.

  - Find comments: trích xuất comment trong code

  - Discover content: tìm kiếm thư mục và tệp bổ sung

- Phản hồi thông tin kỹ thuật

# [6. Common sources of information disclosure](./lab/part1.md)

- Tệp dành cho trình thu thập dữ liệu web

  - Nhiều trang web cung cấp các tệp tại `robots.txt` và `sitemap.xml` để giúp trình thu thập thông tin điều hướng trang web của họ. Trong số những thứ khác, những tệp này thường liệt kê các thư mục cụ thể mà trình thu thập thông tin nên bỏ qua, chẳng hạn vì chúng có thể chứa thông tin nhạy cảm.

- Danh sách thư mục: dirsearch, discover content

- Developer comments

- Error messages (lab 1)

- Debug data (lab 2)

- Backup files (lab 3)

- Cấu hình không an toàn (lab 4)

- Lịch sử kiểm soát phiên bản: .git (lab 5)

# 7. Ngăn chặn

- Đảm bảo rằng mọi người tham gia vào việc tạo ra trang web đều hoàn toàn nhận thức được thông tin nào được coi là nhạy cảm. 

- Kiểm tra bất kỳ mã nguồn nào dễ dàng bị tiết lộ thông tin 

- Sử dụng các thông báo lỗi chung chung càng nhiều càng tốt. Đừng cung cấp cho kẻ tấn công những manh mối về hành vi ứng dụng không cần thiết.

- Kiểm tra lại để đảm bảo rằng bất kỳ tính năng debug hoặc chẩn đoán nào đều bị vô hiệu hóa.

- Đảm bảo rằng bạn hiểu hoàn toàn các cài đặt cấu hình và hậu quả về bảo mật của bất kỳ công nghệ của bên thứ ba nào mà bạn triển khai. Dành thời gian để điều tra và vô hiệu hóa bất kỳ tính năng và cài đặt nào mà bạn không thực sự cần.