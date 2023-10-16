# [Lab 1: Information disclosure in error messages](https://portswigger.net/web-security/information-disclosure/exploiting/lab-infoleak-in-error-messages)

![Yêu cầu lab 1](../image/lab1/0.png)

> - **Mô tả lab:** Thông báo lỗi của lab tiết lộ rằng nó sử dụng framework dễ bị tấn công
>
> - **Mục tiêu:** tìm version của framework này.

Trang web chính của ta, có vẻ là trang web bán hàng.

![Alt text](../image/lab1/01.png)

`View details` 1 sản phẩm bất kỳ, ta nhận thấy id là 1 số

![Alt text](../image/lab1/02.png)

sửa thành 1 chữ coi có lỗi gì không

![Alt text](../image/lab1/03.png)

lỗi chắc luôn và ta thấy có xuất hiện framework ở cuối thông báo lỗi

![Alt text](../image/lab1/04.png)

submit version number and solve the lab

![Alt text](../image/lab1/05.png)

# [Lab 2: Information disclosure on debug page](https://portswigger.net/web-security/information-disclosure/exploiting/lab-infoleak-on-debug-page)

![Yêu cầu lab 2](../image/lab2/0.png)

> - **Mô tả lab:** Lab này chứa trang debug tiết lộ thông tin nhạy cảm về ứng dụng.
>
> - **Mục tiêu:** submit `SECRET_KEY`

vẫn trang web tương tự như lab trên

![Alt text](../image/lab2/01.png)

quan sát code của trang web ta thấy dòng code đã bị comment dẫn tới `Debug` page `<!-- <a href=/cgi-bin/phpinfo.php>Debug</a> -->`

![Alt text](../image/lab2/02.png)

ngoài ra ta có thể sử dụng `dirsearch` xem có thư mục ẩn nào mà ta có thể truy cập không

![Alt text](../image/lab2/03.png)

![Alt text](../image/lab2/04.png)

go to that endpoint

![Alt text](../image/lab2/05.png)

find `SECRET_KEY`

![Alt text](../image/lab2/06.png)

submit and solve the lab

![Alt text](../image/lab2/07.png)

# [Lab 3: Source code disclosure via backup files](https://portswigger.net/web-security/information-disclosure/exploiting/lab-infoleak-via-backup-files)

![Yêu cầu lab 3](../image/lab3/0.png)

> - **Mô tả lab:** Tiết lộ thông tin qua cá tập tin `backup` trong thư mục ẩn.
>
> - **Mục tiêu:** xác định và submit mật khẩu được mã hóa.

cũng không khác gì cũng lab trên lắm

![Alt text](../image/lab3/01.png)

sử dụng `dirsearch` để tìm xem có thư mục ẩn nào mà ta có thể truy cập không

![Alt text](../image/lab3/02.png)

go to that site

![Alt text](../image/lab3/03.png)

![Alt text](../image/lab3/04.png)

submit and solve the lab

![Alt text](../image/lab3/05.png)

# [Lab 4: Authentication bypass via information disclosure](https://portswigger.net/web-security/information-disclosure/exploiting/lab-infoleak-authentication-bypass)

![Yêu cầu lab 4](../image/lab4/0.png)

> - **Mô tả lab:** Giao diện `admin` có lỗ hổng bỏ qua xác thực, khai thác về HTTP header.
>
> - **Mục tiêu:** sử dụng HTTP header để bỏ qua xác thực. Truy cập vào giao diện `admin` và xóa tài khoản `carlos`.

web không còn gì lạ nữa

![Alt text](../image/lab4/01.png)

dirsearch ta thấy `/admin` đã bị cấm truy cập

![Alt text](../image/lab4/02.png)

truy cập đến xem giao diện `admin`

![Alt text](../image/lab4/03.png)

thông báo chỉ ra rằng chỉ có thể đăng nhập với các local users

![Alt text](../image/lab4/04.png)

thay đổi method sang `TRACE`

![Alt text](../image/lab4/05.png)

ta thấy response có header author IP, đây là IP của máy mình không phải là IP của local user

![Alt text](../image/lab4/06.png)

giờ ta sẽ đổi thành `127.0.0.1` (local) để gửi request

***Cách 1:*** **Proxy → Options** : cách này giúp ta load lại web với header như hình và sẽ nhanh hơn cách 2 bên dưới.

![Alt text](../image/lab4/07.png)

Trở lại trình duyệt, ta đã thấy xuất hiện admin panel

![Alt text](../image/lab4/08.png)

Truy cập và solve

![Alt text](../image/lab4/09.png)

***Cách 2:*** thêm HTTP Header bên Request

![Alt text](../image/lab4/10.png)

show response in browser thì ta thấy vẫn đường link xóa tài khoản, tuy nhiên bấm xóa thì không được vì IP không phải local

![Alt text](../image/lab4/11.png)

tìm endppoint xóa

![Alt text](../image/lab4/12.png)

thay vào request

![Alt text](../image/lab4/13.png)

thì vẫn solve thôi

![Alt text](../image/lab4/14.png)

# [Lab 5: Information disclosure in version control history](https://portswigger.net/web-security/information-disclosure/exploiting/lab-infoleak-in-version-control-history)

![Yêu cầu lab 5](../image/lab5/0.png)

vẫn sử dụng `dirsearch` để tìm các thư mục ẩn, ở đây ta tìm thấy các thư mục `./git`

![Alt text](../image/lab5/01.png)

sử dụng `githack`, tuy nhiên công cụ này chỉ down được các file code thôi

![Alt text](../image/lab5/02.png)

ta thấy có hành động gì đó đối với admin password

![Alt text](../image/lab5/03.png)

tải hết file của trang web về

![Alt text](../image/lab5/04.png)

ta thấy commit có `Remove password`

![Alt text](../image/lab5/05.png)

xem log để coi lịch sử chỉnh sửa: `Remove admin password from config` chi tiết hơn

![Alt text](../image/lab5/06.png)

coi cụ thể log này xem nó remove gì

![Alt text](../image/lab5/07.png)

→ get pass → login → delete user carlos → solve the lab

![Alt text](../image/lab5/08.png)
