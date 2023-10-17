# [Lab 1: Unprotected admin functionality](https://portswigger.net/web-security/access-control/lab-unprotected-admin-functionality)

![Yêu cầu lab 1](../image/lab1/0.png)

> - **Mô tả lab:** `admin panel` không được bảo vệ.
>
> - **Mục tiêu:** xóa `carlos`.

Tìm kiếm trong source HTML cũng không phát hiện được gì, `dirsearch` xem có đường dẫn nhạy cảm không

![Alt text](../image/lab1/01.png)

Phát hiện thấy có file `robots.txt` có thể đọc được, trong này chứa đường dẫn không được phép

![Alt text](../image/lab1/02.png)

Tuy nhiên truy cập thì không thấy bị cấm gì hết dù mình không phải `admin`

![Alt text](../image/lab1/03.png)

xóa `carlos` và solve lab

![Alt text](../image/lab1/04.png)

# [Lab 2: Unprotected admin functionality with unpredictable URL](https://portswigger.net/web-security/access-control/lab-unprotected-admin-functionality-with-unpredictable-url)

![Yêu cầu lab 2](../image/lab2/0.png)

tìm kiếm trong HTML thì phát hiện luôn một đoạn script check `admin` và có đường dẫn đến `admin panel`

![Alt text](../image/lab2/01.png)

truy cập `/admin-eiev4j` thì cũng vẫn được dù không phải là `admin`

![Alt text](../image/lab2/02.png)

delete `carlos` and solve the lab

![Alt text](../image/lab2/03.png)

# [Lab 3: User role controlled by request parameter](https://portswigger.net/web-security/access-control/lab-user-role-controlled-by-request-parameter)

![Yêu cầu lab 2](../image/lab3/0.png)

> - **Mô tả lab:** bảng quán trị tại `/admin`, bảng này xác định các quản trị viên sử dụng cookie có thể giả mạo.
>
> - **Mục tiêu:** truy cập `/admin` và xóa `carlos`.

Đăng nhập `wiener:peter`và truy cập `/admin`, thì ta không thể truy cập được như 2 lab trước mà phải đăng nhập là một `administrator`

![Alt text](../image/lab3/01.png)

Quan sát request ta thấy tại cookie có tham số check `Admin=false`

![Alt text](../image/lab3/02.png)

Ta đổi thành `true` thì truy cập thành công rồi

![Alt text](../image/lab3/03.png)

xóa `carlos` và solve lab

![Alt text](../image/lab3/04.png)

# [Lab 4: User role can be modified in user profile](https://portswigger.net/web-security/access-control/lab-user-role-can-be-modified-in-user-profile)

![Yêu cầu lab 4](../image/lab4/0.png)

> - **Mô tả lab:** bảng quán trị tại `/admin`, bảng này chỉ truy cập được với người dùng có `roleid=2`.
>
> - **Mục tiêu:** truy cập `/admin` và xóa `carlos`.

Sau khi đăng nhập `wiener:peter`, hiển nhiên khi truy cập `/admin` thì bị block. Giờ ta sẽ tìm cách truy cập.

![Alt text](../image/lab4/01.png)

Khi sử dụng `Update email` để cập nhật email thì phát hiện trong response lại trả về thông tin người dùng, trong đó có `roleid`

![Alt text](../image/lab4/02.png)

Thay đổi roleid thành 2 để có thể truy cập `admin panel`

![Alt text](../image/lab4/03.png)

Đến `/admin` thì đã vượt quyền thành công rồi

![Alt text](../image/lab4/04.png)

xóa `carlos` và solve lab

![Alt text](../image/lab4/05.png)

# [Lab 5: URL-based access control can be circumvented](https://portswigger.net/web-security/access-control/lab-url-based-access-control-can-be-circumvented)

![Yêu cầu lab 5](../image/lab5/0.png)

> - **Mô tả lab:** bảng quán trị tại `/admin`, nhưng đã chặn quyền truy cập từ bên ngoài vào đường dẫn này. Tuy nhiên có hỗ trợ tiêu đề `X-Original-URL`.
>
> - **Mục tiêu:** truy cập `/admin` và xóa `carlos`.

Truy cập trang web, ta thấy có `Admin panel`, tuy nhiên truy cập thì bị cấm `403`

![Alt text](../image/lab5/01.png)

Vậy ta sẽ truy cập gián tiếp `/admin` qua header `X-Original-URL`

![Alt text](../image/lab5/02.png)

Truy cập đường dẫn xóa để xóa `carlos`, thì bị `missing parameter`

![Alt text](../image/lab5/03.png)

Ta sẽ đưa tham số lên ơath

![Alt text](../image/lab5/04.png)

solve lab

![Alt text](../image/lab5/05.png)

# [Lab 6: Method-based access control can be circumvented](https://portswigger.net/web-security/access-control/lab-method-based-access-control-can-be-circumvented)

![Yêu cầu lab 6](../image/lab6/0.png)

> - **Mô tả lab:** Triển khai kiểm soát truy cập một phần dựa trên HTTP method. Làm quen với bảng quản trị bằng tài khoản `administrator:admin`
>
> - **Mục tiêu:** đăng nhập `wiener:peter`, khai thác lỗi để tự nâng cấp thành quản trị viên.

Đăng nhập với `administrator:admin` rồi vào `Admin panel` thấy có chức năng update role cho các tài khoản

![Alt text](../image/lab6/01.png)

Thử update, ta nhận được 1 Request với method là `POST`

![Alt text](../image/lab6/02.png)

Logout và đăng nhập với `wiener:peter` và tự thăng cấp

Lấy Request thay đổi quyền ở trên và lưu ý cần phải thay đổi session thành session của `wiener`, tuy nhiên thử POST thì không update được

![Alt text](../image/lab6/03.png)

vậy thử đổi sang method GET thì lại ok

![Alt text](../image/lab6/04.png)

solve lab

![Alt text](../image/lab6/05.png)

# [Lab 7: User ID controlled by request parameter](https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter)

![Yêu cầu lab 7](../image/lab7/0.png)

> - **Mô tả lab:** Lỗ hổng leo thang đặc quyền theo chiều ngang trên trang tài khoản của người dùng.
> 
> - **Mục tiêu:** lấy khóa API của `carlos` và nộp.

Trang tài khoản người dùng có khóa API

![Alt text](../image/lab7/01.png)

Quan sát URL ta thấy rằng có tham số `id=wiener`

![Alt text](../image/lab7/02.png)

ta sẽ đổi thành `carlos` và ta có thể đọc thông tin của `carlos` thành công

![Alt text](../image/lab7/02.png)

lấy được API Key, submit and solve the lab

![Alt text](../image/lab7/04.png)

# [Lab 8: User ID controlled by request parameter, with unpredictable user IDs ](https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter-with-unpredictable-user-ids)

![Yêu cầu lab 8](../image/lab8/0.png)

> - **Mô tả lab:** Lỗ hổng leo thang đặc quyền theo chiều ngang trên trang tài khoản của người dùng nhưng xác định người dùng bằng `GUID`.
> 
> - **Mục tiêu:** lấy khóa API của `carlos` và nộp.

Đăng nhập `wiener:peter`, Trang tài khoản người dùng có thông tin khóa API, ta sẽ từ `wiener` có thể đọc thông tin tài khoản của `carlos`

![Alt text](../image/lab8/01.png)

Tuy nhiên ta thấy `id` đã thay đổi không còn là `username` mà là một chuỗi khó đoán hơn

![Alt text](../image/lab8/02.png)

Vậy ta cần tìm `id` của `carlos`, chú ý rằng trang web của ta có rất nhiều các bài viết

![Alt text](../image/lab8/03.png)

`View post` của tác giả `wiener` ta thấy có tham số `userID` và ta phát hiện giá trị này giống với `id` ở trang tài khoản

![Alt text](../image/lab8/04.png)

Vậy ta cần tìm bài viết của `carlos` để lấy `userId`

![Alt text](../image/lab8/05.png)

Đến trang tài khoản, thay id thàng userId ta vừa lấy được và thành công lấy khóa API

![Alt text](../image/lab8/06.png)

submit và solve

![Alt text](../image/lab8/07.png)

# [Lab 9: User ID controlled by request parameter with data leakage in redirect](https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter-with-data-leakage-in-redirect)

![Yêu cầu lab 9](../image/lab9/0.png)

> - **Mô tả lab:** Lỗ hổng kiểm soát truy cập khiến thông tin bị rò rỉ trong nội dung redirect.
> 
> - **Mục tiêu:** lấy khóa API của `carlos` và nộp.
main web

Ở lab này, thì trang tài khoản đã được truy cập với `id=username`, ta sẽ đổi thành `carlos` xem có thể thấy được thông tin không

![Alt text](../image/lab9/01.png)

Tuy nhiên thì trang web sẽ ngay lập tức redirect sang trang `login`, nhưng xem Request truyền đi thì thấy trước khi redirect thì trang thông tin tài khoản của `carlos` đã được hiển thị rồi

![Alt text](../image/lab9/02.png)

submit và solve lab

![Alt text](../image/lab9/03.png)

# [Lab 10: User ID controlled by request parameter with password disclosure](https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter-with-password-disclosure)

![Yêu cầu lab 10](../image/lab10/0.png)

> - **Mô tả lab:** Trang tài khoản người dùng chứa mật khẩu của người dùng hiện tại, được điền trước bằng thông tin nhập ẩn.
> 
> - **Mục tiêu:** lấy mật khẩu của `administrator`, xóa `carlos`.

Trang tài khoản người dùng làm lộ thông tin password của người dùng đó

![Alt text](../image/lab10/01.png)

Lấy mật khấu của `administrator` thôi

![Alt text](../image/lab10/02.png)

login thành công, truy cập `Admin panel`

![Alt text](../image/lab10/03.png)

xóa `carlos` và solve

![Alt text](../image/lab10/04.png)

# [Lab 11: Insecure direct object references](https://portswigger.net/web-security/access-control/lab-insecure-direct-object-references)

![Yêu cầu lab 11](../image/lab11/0.png)

> - **Mô tả lab:** Lab này lưu trữ nhật ký trò chuyện của người dùng trực tiếp trên hệ thống tệp của máy chủ và truy xuất chúng bằng URL tĩnh.
> 
> - **Mục tiêu:** lấy mật khẩu của `carlos` và đăng nhập.

Test chức năng Live chat, thấy có `View transcript`, nó sẽ download cho ta 1 file và file này được tự động đặt tên `2.txt`. Vậy file `1.txt` thì ???

![Alt text](../image/lab11/01.png)

Xem thử, thì lộ password rồi

![Alt text](../image/lab11/02.png)

Đăng nhập và solve lab

![Alt text](../image/lab11/03.png)

# [Lab 12: Multi-step process with no access control on one step](https://portswigger.net/web-security/access-control/lab-multi-step-process-with-no-access-control-on-one-step)

![Yêu cầu lab 12](../image/lab12/0.png)

> - **Mô tả lab:** Bảng quản trị với quy trình gồm nhiều bước chưa hoàn thiện để thay đổi vai trò của người dùng.
> 
> - **Mục tiêu:** đăng nhập `wiener:peter`, tiến hành tự thăng cấp thành quản trị viên.

Đăng nhập `administrator:admin`, truy cập `Admin panel` để thực hiện thăng cấp quản trị cho các tài khoản

![Alt text](../image/lab12/01.png)

Khi thực hiện upgrade, sẽ thêm 1 bước confirm nữa

![Alt text](../image/lab12/02.png)

Đăng nhập `wiener:peter` và tiến hành tự nâng cấp quyền quản trị, khi ta yêu cầu thăng cấp thì không được vì không có quyền

![Alt text](../image/lab12/03.png)

Tuy nhiên ta thấy 2 Request upgrade và confirm, và dường như request confirm chỉ thêm tham số `confirmed` mà thôi, vẫn có các tham số của Request upgrade

![Alt text](../image/lab12/04.png)

Vậy ta sẽ dùng Request confirm để bypass luôn

![Alt text](../image/lab12/05.png)

Vậy là solve lab rồi

![Alt text](../image/lab12/06.png)

# [Lab 13: Referer-based access control](https://portswigger.net/web-security/access-control/lab-referer-based-access-control)

![Yêu cầu lab 13](../image/lab13/0.png)

> - **Mô tả lab:** Kiểm soát quyền truy cập vào chức năng quản trị nhất định dựa trên header `Referer`.
> 
> - **Mục tiêu:** đăng nhập `wiener:peter`, tiến hành tự thăng cấp thành quản trị viên.

Thay đổi dễ dàng là solve rồi

![Alt text](../image/lab13/01.png)

Solve lab

![Alt text](../image/lab13/02.png)
