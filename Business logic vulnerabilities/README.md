# 1. Định nghĩa

- Lỗi trong thiết kế và triển khai ứng dụng cho phép kẻ tấn công thực hiện hành vi ngoài ý muốn. Những sai sót này thường là kết quả của việc không lường trước được các trạng thái ứng dụng bất thường có thể xảy ra và do đó không xử lý chúng một cách an toàn.

- Lỗi logic rất đa dạng và thường là duy nhất cho từng ứng dụng và chức năng cụ thể. Do đó rất khó để test tự động.

# 2. Lý do phát sinh

- Kiểm tra đầu vào của người dùng không đầy đủ.
    
    **Ví dụ,** nếu nhà phát triển giả định rằng người dùng sẽ truyền dữ liệu chỉ qua trình duyệt web, ứng dụng có thể chỉ dựa vào các điều khiển bên phía khách hàng yếu để xác thực đầu vào. Điều này có thể bị kẻ tấn công vượt qua bằng cách sử dụng một proxy chặn.

# 3. Ảnh hưởng

- Về cơ bản, tác động của bất kỳ lỗ hổng logic nào phụ thuộc vào chức năng liên quan đến lỗ hổng đó. Nếu lỗ hổng nằm trong cơ chế xác thực, ví dụ, kẻ tấn công có thể khai thác điều này để leo thang đặc quyền hoặc để hoàn toàn vượt qua xác thực, truy cập vào dữ liệu và chức năng nhạy cảm. Điều này cũng tạo ra một bề mặt tấn công gia tăng cho các cuộc tấn công khác.

- Logic bị sai trong các giao dịch tài chính có thể dẫn đến các khoản thiệt hại lớn cho doanh nghiệp thông qua việc mất tiền, gian lận và như vậy.

- Mặc dù lỗ hổng logic có thể không cho phép kẻ tấn công được lợi ích trực tiếp, chúng vẫn có thể cho phép một bên xấu hại doanh nghiệp theo một cách nào đó.

# [4. Ví dụ](./lab/part1.md)

## ***4.1. Tin tưởng quá mức vào các biện pháp kiểm soát phía client (lab 1)***

- Tin tưởng rằng người dùng chỉ tương tác qua giao diện web được cung cấp. Điều này có thể bị kẻ tấn công vượt qua bằng cách sử dụng `Proxy` để giả mạo dữ liệu.

## ***4.2. Không xử lý đầu vào bất thường (lab 2, 3, 4)***

- Đầu vào là số âm.

- ...

## ***4.3. Đưa ra các giả định sai lầm về hành vi của người dùng***

- Người dùng đáng tin cậy không phải lúc nào cũng đáng tin cậy (lab 5)

    ...

- Người dùng không phải lúc nào cũng cung cấp thông tin đầu vào bắt buộc (lab 6)

    ...

- Người dùng không phải lúc nào cũng làm theo trình tự đã định (lab 7, 8)

    ...

## ***4.4. Lỗi dành riêng cho tên miền (lab 9, 10)***

    ...

## ***4.5. Cung cấp một oracle mã hóa (lab 11)***

    ...

# 5. Ngăn chặn/ Phòng chống

-  Đảm bảo nhà phát triển và người kiểm thử hiểu miền mà ứng dụng phục vụ

- Tránh đưa ra các giả định ngầm về hành vi của người dùng hoặc hành vi của các phần khác của ứng dụng 

- Nên xác định những giả định bạn đã đưa ra về trạng thái phía máy chủ và thực hiện logic cần thiết để xác minh rằng những giả định này được đáp ứng. Điều này bao gồm đảm bảo rằng giá trị của bất kỳ đầu vào nào là hợp lý trước khi tiến hành.

- Ngoài ra, quan trọng là phải đảm bảo rằng cả các nhà phát triển và người kiểm thử đều có thể hiểu rõ những giả định này và cách ứng dụng phản ứng trong các tình huống khác nhau. Điều này có thể giúp nhóm phát hiện ra các lỗi logic càng sớm càng tốt.