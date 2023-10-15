
# [Lab 1: File path traversal, simple case](https://portswigger.net/web-security/file-path-traversal/lab-simple)

![Yêu cầu lab 1](../image/lab1/0.png)

> - **Mô tả lab:** Lỗi `Path traversal` trong hiển thị hình ảnh sản phẩm.
>
> - **Mục tiêu:** trích xuất contents file `/etc/passwd`.

Trang web chính có rất nhiều hình ảnh sản phẩm

![main](../image/lab1/01.png)

hãy bật `Images` để xem các path đến image nữa vì trang có rất nhiều image và sẽ có directory lưu trữ nó

![Alt text](../image/lab1/02.png)![Alt text](image.png)

Quan sát các Request chứa hình ảnh sản phẩm của web

![Alt text](../image/lab1/03.png)

test thôi

![Alt text](../image/lab1/04.png)

response trả về `No such file` --> directory hiện không có thư mục --> lùi thư mục đến chỗ đọc được thì thôi

![Alt text](../image/lab1/05.png)

đã tìm ra

![Alt text](../image/lab1/06.png)

xem trên web lỗi không thể hiển thị nên ta sẽ xem bằng Response trên burp suite

![Alt text](../image/lab1/07.png)

![Alt text](../image/lab1/08.png)

ok solve the lab

![Alt text](../image/lab1/09.png)

> **Test bằng Active Scan**

![Alt text](../image/lab1/10.png)

![Alt text](../image/lab1/11.png)

# [Lab 2: File path traversal, traversal sequences blocked with absolute path bypass](https://portswigger.net/web-security/file-path-traversal/lab-absolute-path-bypass)

![Yêu cầu lab 2](../image/lab2/0.png)

> - **Mô tả lab:** lỗi `Path traversal` trong hiển thị hình ảnh sản phẩm, chặn các chuỗi truyền tải nhưng coi tệp được cung cấp có liên quan đến thư mục làm việc mặc định.
>
> - **Mục tiêu:** truy xuất contents của `/etc/passwd`.

Trang web chính của lab có rất nhiều hình ảnh sản phẩm

![main web](../image/lab2/01.png)

Quan sát các Request chứa hình ảnh sản phẩm của web

![file name](../image/lab2/02.png)

Thử với đường dẫn tuyệt đối trước thì đọc được file luôn rồi

![Alt text](../image/lab2/03.png)

solve lab luôn

![solve luôn](../image/lab2/04.png)

> **Test bằng Active Scan**

![Alt text](../image/lab2/05.png)

![Alt text](../image/lab2/06.png)

# [Lab 3: File path traversal, traversal sequences stripped non-recursively](https://portswigger.net/web-security/file-path-traversal/lab-sequences-stripped-non-recursively)

![Yêu cầu lab 3](../image/lab3/0.png)

> - **Mô tả lab:** lỗi như các lab, tuy nhiên có loại bỏ các chuỗi truyền tải khỏi nội dung tệp trước khi sử dụng
>
> - **Mục tiêu:** truy xuất contents của `/etc/passwd`.

Trang web chính của lab chứa rất nhiều hình ảnh sản phẩm

![Trang web chính của lab](../image/lab3/01.png)

Quan sát các Request chứa hình ảnh sản phẩm của web

![filename](../image/lab3/02.png)

thử thêm `../` vào `filename` để lùi đến thư mục khác, ta phát hiện ảnh vẫn hiển thị bình thường, ta đoán là đã bị kiểm soát đầu vào rồi và bị replace

![Alt text](../image/lab3/03.png)

Ta sẽ đổi thành `....//` để khi mất 1 `../` ta vẫn còn 1 `../`, và ảnh đã không tồn tại `No such file`, vậy là bypass được rồi

![Alt text](../image/lab3/04.png)

Đọc `/etc/passwd` thôi

![Alt text](../image/lab3/05.png)

solve the lab

![Alt text](../image/lab3/06.png)

> **Test bằng Active Scan**

![Alt text](../image/lab3/07.png)

![Alt text](../image/lab3/08.png)

# [Lab 4: File path traversal, traversal sequences stripped with superfluous URL-decode](https://portswigger.net/web-security/file-path-traversal/lab-superfluous-url-decode)

![Yêu cầu lab 4](../image/lab4/0.png)

> - **Mô tả lab:** lỗi `Path traversal` trong hiển thị hình ảnh sản phẩm, tuy nhiên có chặn các chuỗi truyền tải đường dẫn, sau đó thực hiện giải mã URL của đầu vào trước khi sử dụng.
>
> - **Mục tiêu:** truy xuất contents của `/etc/passwd`.

Trang web chính của lab có chứa rất nhiều hình ảnh sản phẩm

![Trang web chính của lab](../image/lab4/01.png)

Quan sát các Request chứa hình ảnh sản phẩm của web

![Alt text](../image/lab4/02.png)

encode 1 lần, thì có thể bị giải mã rồi block luôn `../`

![Alt text](../image/lab4/03.png)

encode lần nữa là thành công rồi

![Alt text](../image/lab4/04.png)

bypass thôi

![Alt text](../image/lab4/05.png)

ok solve the lab

![Alt text](../image/lab4/06.png)

> **Test bằng Active Scan**

![Alt text](../image/lab4/07.png)

![Alt text](../image/lab4/08.png)

# [Lab 5: File path traversal, validation of start of path](https://portswigger.net/web-security/file-path-traversal/lab-validate-start-of-path)

![Yêu cầu lab 5](../image/lab5/0.png)

> - **Mô tả lab:** lỗi `Path traversal` trong hiển thị hình ảnh sản phẩm, tuy nhiên đường dẫn tệp phải đầy đủ và sẽ xác thực nó bắt đầu bằng đường dẫn đó.
>
> - **Mục tiêu:** truy xuất contents của `/etc/passwd`.

Trang web chính của lab có chứa rất nhiều hình ảnh sản phẩm

![main web](../image/lab5/01.png)

Quan sát các Request chứa hình ảnh sản phẩm của web

![Alt text](../image/lab5/02.png)![Alt text](image.png)

ta thấy, nó luôn bắt đầu bằng `/var/www/images/`, vậy ta sẽ thêm `../` vào sau đường dẫn này, kết quả là thành công rồi nên file sẽ không tồn tại

![Alt text](../image/lab5/03.png)

`var` và `etc` thường cùng ở `/` nên ta dùng `../../../` để đọc `/etc/passwd`

![Alt text](../image/lab5/04.png)

solve the lab

![Alt text](../image/lab5/05.png)

> **Test bằng Insertion Point**

![Alt text](../image/lab5/06.png)

![Alt text](../image/lab5/07.png)

# [Lab 6: File path traversal, validation of file extension with null byte bypass](https://portswigger.net/web-security/file-path-traversal/lab-validate-file-extension-null-byte-bypass)

![Yêu cầu lab 6](../image/lab6/0.png)

> - **Mô tả lab:** lỗi `Path traversal` trong hiển thị hình ảnh sản phẩm, tuy nhiên xác thực tên tệp được cung cấp kết thúc bằng extension dự kiến.
>
> - **Mục tiêu:** truy xuất contents của `/etc/passwd`.

Trang web chính của lab có chứa rất nhiều hình ảnh sản phẩm

![Alt text](../image/lab6/01.png)

Quan sát các Request chứa hình ảnh sản phẩm của web

![Alt text](../image/lab6/02.png)

Lab này check extension nên ta sẽ dùng `NULL BYTE` để loại bỏ nó đi và đương nhiên là vẫn phải lùi

![Alt text](../image/lab6/03.png)

solve the lab

![Alt text](../image/lab6/04.png)

> **Test bằng Active Scan**

![Alt text](../image/lab6/05.png)

![Alt text](../image/lab6/06.png)