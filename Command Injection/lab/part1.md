# [Lab 1: OS command injection, simple case](https://portswigger.net/web-security/os-command-injection/lab-simple)

![Yêu cầu lab 1](../image/lab1/0.png)

> - **Mô tả lab:** lỗi `Command Injection` trong chức năng `Check stock`, thực thi commmand chứa sản phẩm do người dùng cung cấp và ID của hàng, đồng thời có trả về kết quả trong response.
>
> - **Mục tiêu:** thực thi câu lệnh `whoami` để xác định tên người dùng hiện tại.

Trang web chính, ta chưa thấy chức năng `Check stock`

![main](../image/lab1/01.png)

`View details` thì thấy function lỗi rồi

![Check Stock](../image/lab1/02.png)

Tại Request thực hiện `checkStock` ta thấy có sản phẩm và storeID như mô tả, ta chèn thêm command mới thì thực hiện thành công

![Alt text](../image/lab1/03.png)

ngoài ra có thể F12 rồi sửa giá trị và `Check stock`

![Alt text](../image/lab1/04.png)

solve the lab

![Alt text](../image/lab1/05.png)

> **Test bằng Scan**

- chèn command

![Alt text](../image/lab1/06.png)

![Alt text](../image/lab1/07.png)

tuy nhiên ta thấy 3 `echo` giống nhau khó có thể biết lỗi ở `echo` nào, ta có thể test xem nó đúng ở `echo` nào hoặc không thì `whoami` cả 3 echo

![Alt text](../image/lab1/08.png)

![Alt text](../image/lab1/09.png)

# [Lab 2: Blind OS command injection with time delays](https://portswigger.net/web-security/os-command-injection/lab-blind-time-delays)

![Yêu cầu lab 2](../image/lab2/0.png)

> - **Mô tả lab:** lỗi `Command Injection` trong chức năng `Feedback`, thực thi commmand chứa thông tin do người dùng cung cấp, đồng thời không trả về kết quả trong response.
>
> - **Mục tiêu:** thực thi câu lệnh làm trễ 10s.

Trang web chính của lab này, và thấy function lỗi `Feedback` rồi

![main](../image/lab2/01.png)

Chức năng chứa lỗi `Command Injection`

![Submit feedback](../image/lab2/02.png)

Chèn command thử xem thì vẫn submit thành công, tuy nhiên kết quả trả về ta không hề thấy kết quả của command `whoami`

![Alt text](../image/lab2/03.png)

Kích hoạt time delays xem có phản ứng gì không

![Alt text](../image/lab2/04.png)

0k trễ 10s và solve lab

![Alt text](../image/lab2/05.png)

> **Test bằng Active Scan**

![Alt text](../image/lab2/06.png)


![Alt text](../image/lab2/07.png)

# [Lab 3: Blind OS command injection with output redirection](https://portswigger.net/web-security/os-command-injection/lab-blind-output-redirection)

![Yêu cầu lab 3](../image/lab3/0.png)

> - **Mô tả lab:** lỗi `Command Injection` trong chức năng `Feedback`, thực thi commmand chứa thông tin do người dùng cung cấp, đồng thời kết quả không được trả về trong response. Tuy nhiên, có thể chuyển hướng kết quả bằng command vào thư mục `/var/www/images/`, có thể dùng URL tải hình ảnh để truy xuất nôi dung của tệp.
>
> - **Mục tiêu:** thực thi câu lệnh `whoami` và trích xuất kết quả.

main web

![main](../image/lab3/01.png)

Chức năng chứa lỗi `Command Injection`

![submit feedback](../image/lab3/02.png)

Test thử xem lỗi như thế nào, chèn command đơn giản thì thấy rằng response trả về không hiển thị kết quả của command

![Alt text](../image/lab3/03.png)

Kích hoạt thời gian trễ thì ta thấy web có phản hổi trễ, vậy đây là Blind SQLi rồi

![Alt text](../image/lab3/04.png)

test thử chuyển hướng kết quả của command vào 1 tệp thì thấy thành công nha, ta sẽ dùng nó để bypass bài này

![Alt text](../image/lab3/05.png)

chú ý trang web có những file ảnh sản phẩm và nó được lưu tại `var/www/images/`, ta sẽ chuyển hướng ouput của command `whoami` vào path này và sẽ thay đổi URL đọc file ảnh để trích xuất kết quả

![Alt text](../image/lab3/06.png)

Thay đổi URL để xem file output

![Alt text](../image/lab3/07.png)

solve the lab

![Alt text](../image/lab3/08.png)

> **Test bằng Active Scan**

tìm được lỗi

![Alt text](../image/lab3/09.png)

![Alt text](../image/lab3/10.png)

# [Lab 4: Blind OS command injection with out-of-band interaction](https://portswigger.net/web-security/os-command-injection/lab-blind-out-of-band)

![Yêu cầu lab 4](../image/lab4/0.png)

> - **Mô tả lab:** lỗi `Command Injection` trong chức năng `Feedback`, thực thi commmand chứa thông tin do người dùng cung cấp, đồng thời kết quả không được trả về trong response. Thậm chí chuyển hướng đầu ra cũng không được nhưng có thể kích hoạt OOB
>
> - **Mục tiêu:** khai thác để tra cứu DNS cho Burp Collaborator.

Trang web chính và chức năng lỗi `Feedback`

![main web](../image/lab4/01.png)

Chức năng chứa lỗi `Command Injection`

![submit feedback](../image/lab4/02.png)

test thử thêm command bình thường và kích hoạt thời gian trễ thì ta đều không thấy có ảnh hưởng gì.

try OOB, sử dụng `nslookup` để tìm kiếm DNS cho Burp Collaborator

![Alt text](../image/lab4/03.png)

có phản hồi từ Collaborator

![Alt text](../image/lab4/04.png)

solve the lab

![Alt text](../image/lab4/05.png)

> **Test bằng Active Scan**

![Alt text](../image/lab4/06.png)

![Alt text](../image/lab4/07.png)

# [Lab 5: Blind OS command injection with out-of-band data exfiltration](https://portswigger.net/web-security/os-command-injection/lab-blind-out-of-band-data-exfiltration)

![Yêu cầu lab 5](../image/lab5/0.png)

> - **Mô tả lab:** lỗi `Command Injection` trong chức năng `Feedback`, thực thi commmand chứa thông tin do người dùng cung cấp, đồng thời kết quả không được trả về trong response. Thậm chí chuyển hướng đầu ra cũng không được nhưng có thể kích hoạt OOB
>
> - **Mục tiêu:** thực thi `whoami` và trích xuất kết quả thông qua truy vấn DNS với Burp Collaborator và submit kết quả.

tương tự như lab 5, chỉ khác yêu cầu thôi. Thử thực thi `whoami` luôn.

![Alt text](../image/lab5/01.png)

phản hổi từ Collaborator

![Alt text](../image/lab5/02.png)

submit and solve the lab

![Alt text](../image/lab5/03.png)

> **Test bằng Active Scan**

![Alt text](../image/lab5/04.png)

![Alt text](../image/lab5/05.png)