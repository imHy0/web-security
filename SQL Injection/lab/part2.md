# [Lab 3: SQLi attack, querying the database type and version on Oracle](https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-oracle)

![Yêu cầu lab 3](../image/lab3/0.png)

**_Hint_**

![Lấy version on Oracle](../image/lab3/01.png)

> - **Mô tả lab**: Lỗi SQLi tại ***category filter***. Có thể sử dụng `UNION` attack để trả về kết quả của query.
>
> - **Database:** Oracle
>
> - **Mục tiêu:** Hiển thị version và database nhận về chuỗi `'Oracle Database 11g Express Edition Release 11.2.0.2.0 - 64bit Production, PL/SQL Release 11.2.0.2.0 - Production, CORE 11.2.0.2.0 Production, TNS for Linux: Version 11.2.0.2.0 - Production, NLSRTL Version 11.2.0.2.0 - Production'`

![Alt text](../image/lab3/02.png)

test thử lỗi SQL

![Alt text](../image/lab3/03.png)

lỗi `500 Error` rồi

![Alt text](../image/lab3/04.png)

thêm `--` hết lỗi rồi

![Alt text](../image/lab3/05.png)

ta cần xác định số cột để có thể sử dụng `UNION` attack để trích xuất dữ liệu

![Alt text](../image/lab3/06.png)

ok, vậy là có 2 cột, giờ ta sẽ lấy version

![Query get version on Oracle](../image/lab3/07.png)

solve the lab

![Alt text](../image/lab3/08.png)

> Test bằng tool thử nhỉ, sử dụng Scan để quét lỗi

![Alt text](../image/lab3/09.png)

![Alt text](../image/lab3/10.png)

# [Lab 4: SQLi attack, querying the database type and version on MySQL and Microsoft](https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-mysql-microsoft)

![Yêu cầu lab 4](../image/lab4/0.png)

**_Hint_**

![Get dabatase version on MySQL](../image/lab4/01.png)![Get dabatase version on Mycrosoft](../image/lab4/02.png)

> - **Mô tả lab:** tương tự **lab3** nhưng khác database.
>
> - **Database:** MySQL và Microsoft.
>
> - **Yêu cầu:** nhận được chuỗi `8.0.34-0ubuntu0.20.04.1`

như **lab3** thôi, solve the lab

![Alt text](../image/lab4/03.png)

![Alt text](../image/lab4/04.png)

> Scan thì cũng tương tự **lab3**

# [Lab 5: SQLi attack, listing the database contents on non-Oracle databases](https://portswigger.net/web-security/sql-injection/examining-the-database/lab-listing-database-contents-non-oracle)

![Yêu cầu lab 5](../image/lab5/0.png)

**_Hint_**

![Listing database contents on non-Oracle databases](../image/lab5/01.png)

> - **Mô tả lab:** vẫn lỗi như 2 lab trên.
>
> - **Database:** non-Oracle, table (username, password), xác định tên của bảng này và lấy thông tin.

Xác định vị trí lỗi

![Alt text](../image/lab5/02.png)

đích thị là lỗi SQLi

![Alt text](../image/lab5/03.png)

vẫn là xác định số cột để sử dụng `UNION` attack → xác định được có 2 cột

![Alt text](../image/lab5/04.png)

Lấy tên các bảng sử dụng `UNION` attack: `UNION SELECT table_name,NULL FROM information_schema.tables` , sử dụng điều kiện `WHERE table_schema='public'` để chỉ trích xuất các bảng của người dùng

![Alt text](../image/lab5/05.png)

Tìm được tên bảng `users_arryev`

![Alt text](../image/lab5/06.png)

Lấy tên các cột từ bảng vừa tìm được

![Alt text](../image/lab5/07.png)

2 cột `password_dlrfug` và `username_swlkik`

![Alt text](../image/lab5/08.png)

giờ là lấy dữ liệu

![Alt text](../image/lab5/09.png)

lấy được tài khoản của admin là `administrator:vspma1lmgqdohrel5t73`

![Alt text](../image/lab5/10.png)

login and solve the lab

![Alt text](../image/lab5/11.png)

# [Lab 6: SQLi attack, listing the database contents on Oracle](https://portswigger.net/web-security/sql-injection/examining-the-database/lab-listing-database-contents-oracle)
![Yêu cầu lab 6](../image/lab6/0.png)

**_Hint_**

![Listing database contents on Oracle](../image/lab6/01.png)

> - **Mô tả lab:** vẫn tương tự lab 5, chỉ khác database, ở lab này là `Oracle`.

main web, ta thấy chức năng lỗi `catogory filter`

![Alt text](../image/lab6/02.png)

test

![Alt text](../image/lab6/03.png)

ok, đích thực là lỗi SQLi rồi

![Alt text](../image/lab6/04.png)

tìm số cột, vẫn là có 2 cột

![Alt text](../image/lab6/05.png)

tìm các bảng từ người dùng

![Alt text](../image/lab6/06.png)

tìm được table `USERS_BFEMJV`

![Alt text](../image/lab6/07.png)

tìm được tên các cột

![Alt text](../image/lab6/08.png)

2 cột là `USERNAME_IHWYMM` và `PASSWORD_OKJFKD`

![Alt text](../image/lab6/09.png)

lấy dữ liệu

![Alt text](../image/lab6/10.png)

lấy được tài khoản của admin là `administrator:nz3k2k4ibc83tph6ql42`

![Alt text](../image/lab6/11.png)

login and solve the lab

![Alt text](../image/lab6/12.png)
