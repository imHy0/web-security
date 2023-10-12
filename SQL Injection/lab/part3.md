# [Lab 7: SQLi UNION attack, determining the number of columns returned by the query](https://portswigger.net/web-security/sql-injection/union-attacks/lab-determine-number-of-columns)

![Yêu cầu lab 7](../image/lab7/0.png)

> - **Mô tả lab:** vẫn như các lab trước thôi.
>
> - **Mục tiêu:** xác định số cột bằng cách thực hiện tấn công `UNION` chèn SQL trả về một hàng bổ sung chứa giá trị `null`

main web

![main web](../image/lab7/01.png)

test

![thêm '](../image/lab7/02.png)

![ok](../image/lab7/03.png)

xác định số cột sử dụng `ORDER BY`

![order by 3](../image/lab7/04.png)

![ok](../image/lab7/05.png)

add value, chèn SQL trả về một hàng bổ sung chứa giá trị `null`

![add null value](../image/lab7/06.png)

solve the lab

![solve](../image/lab7/07.png)

# [Lab 8: SQLi UNION attack, finding a column containing text](https://portswigger.net/web-security/sql-injection/union-attacks/lab-find-column-containing-text)

![Yêu cầu lab 8](../image/lab8/0.png)

> - **Mô tả lab**: giống lab trên
>
> - **Mục tiêu:** cũng như lab7 nhưng chèn SQL để trả về hàng chứa giá trị được cung cấp. → ở lab này yêu cầu là trả về `YalfhR`

test

![thêm '](../image/lab8/01.png)

vẫn là lỗi SQLi ở đây

![ok](../image/lab8/02.png)

xác định số cột sử dụng `ORDER BY`

![ok](../image/lab8/03.png)

add value, đúng vị trí mới được nha, vì có thể kiểu dữ liệu không phải string

![vị trí 1](../image/lab8/04.png)

 → lỗi
 
![vị trí 2](../image/lab8/05.png)

 → ok, solve the lab

![solve the lab](../image/lab8/06.png)

# [Lab 9: SQLi UNION attack, retrieving data from other tables](https://portswigger.net/web-security/sql-injection/union-attacks/lab-retrieve-data-from-other-tables)

![Yêu cầu lab 9](../image/lab9/0.png)

> - **Mô tả lab:** vẫn như các lab trên.
>
> - **Database:** bảng `users` chứa 2 cột `(username, password)`.
>
> - **Mục tiêu:** thực hiện `UNION` attack lấy username và password.

main web

![main](../image/lab9/01.png)

test

![thêm '](../image/lab9/02.png)

vẫn lỗi như các lab

![ok](../image/lab9/03.png)

xác định số cột sử dụng `ORDER BY`, vậy là có 2 cột

![ok](../image/lab9/04.png)

xác nhận xem có bảng `users` không, ok có rồi

![ok](../image/lab9/05.png)

lại tìm xem trong bảng trên có đúng có 2 cột `username`, `password` không.

![ok](../image/lab9/06.png)

có rồi thì lấy dữ liệu thôi

![ok](../image/lab9/07.png)

lấy được tài khoản của admin là `administrator:ea0fnxjokkdjwrgv3gmb`

![data](../image/lab9/08.png)

login and solve the lab

![solve](../image/lab9/09.png)

# [Lab 10: SQLi UNION attack, retrieving multiple values in a single column](https://portswigger.net/web-security/sql-injection/union-attacks/lab-retrieve-multiple-values-in-single-column)

![Yêu cầu lab 10](../image/lab10/0.png)

**_Hint_**

![concat1](../image/lab10/01.png)
![concat2](../image/lab10/02.png)

> - **Mô tả lab:** Lab này vẫn như lab 9.

main web

![main](../image/lab10/03.png)

test

![thêm '](../image/lab10/04.png)

vẫn lỗi bình thường như các lab

![ok](../image/lab10/05.png)

tìm số cột sử dụng `ORDER BY`, vậy là có 2 cột

![ok](../image/lab10/06.png)

tìm tên các bảng từ người dùng xem có bảng `users` không, có rồi đây

![table_name](../image/lab10/07.png)

tìm tên cột xem có đúng có 2 cột `username`, `password` không, đương nhiên có rồi

![column_name](../image/lab10/08.png)

nếu liệt kê lần lượt thì lỗi `500 Error`

![fail](../image/lab10/09.png)

có thể là do có cột không thể nhận dữ liệu là một chuỗi → ta sẽ nối thành xâu có dạng `username : password` và để ở cột thỏa mãn → tìm được tài khoản của admin rồi `administrator : pcouw8257i1bhem2cvgd`

![Alt text](../image/lab10/10.png)

login and solve the lab

![Alt text](../image/lab10/11.png)
