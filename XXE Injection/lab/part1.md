# [Lab 1: Exploiting XXE using external entities to retrieve files](https://portswigger.net/web-security/xxe/lab-exploiting-xxe-to-retrieve-files)

![Yêu cầu lab 1](../image/lab1/0.png)

> - **Mô tả lab:** Tính năng `Check stock` để phân tích dữ liệu đầu vào XML và trả về mọi giá trị không mong muốn trong phản hồi.
> 
> - **Mục tiêu:** chèn thực thể XML đọc file `/etc/passwd`.

Chức năng `Check stock` với đầu vào XML

![Alt text](../image/lab1/01.png)

Chèn thực thể đọc file `/etc/passwd`

- Khai báo thực thể để đọc file `/etc/passwd`: `<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>`

- gọi thực thế: `&xxe;`

![Alt text](../image/lab1/02.png)

solve lab

![Alt text](../image/lab1/03.png)

> **Test bằng Active Scan**

![Alt text](../image/lab1/04.png)

![Alt text](../image/lab1/05.png)

# [Lab 2: Exploiting XXE to perform SSRF attacks](https://portswigger.net/web-security/xxe/lab-exploiting-xxe-to-perform-ssrf)

![Yêu cầu lab 2](../image/lab2/0.png)

> - **Mô tả lab:** Tính năng `Check stock` để phân tích dữ liệu đầu vào XML và trả về mọi giá trị không mong muốn trong phản hồi. Máy chủ lab đang chạy endpoint `EC2 metadata` tại URL `http://169.254.169.254/`, endpoint này có thể được sử dụng để truy xuất dữ liệu về phiên bản, một trong số đó có thể nhạy cảm.
> 
> - **Mục tiêu:** Khai thác lỗ hổng XXE để thực hiện tấn công SSRF nhằm lấy khóa truy cập bí mật IAM từ endpoint trên.

Chức năng `Check stock` với đầu vào XML

![Alt text](../image/lab2/01.png)

Chèn XML để thực hiện tấn công SSRF với URL mục tiêu: `http://169.254.169.254/`

![Alt text](../image/lab2/02.png)

Truy cập đến endpoint trên, lại tìm ra endpoint mới là `meta-data`

![Alt text](../image/lab2/03.png)

Tìm thấy khóa truy cập `IAM`

![Alt text](../image/lab2/04.png)

![Alt text](../image/lab2/05.png)

![Alt text](../image/lab2/06.png)

![Alt text](../image/lab2/07.png)

solve the lab

![Alt text](../image/lab2/08.png)

> **Test bằng Active Scan**

![Alt text](../image/lab2/09.png)

![Alt text](../image/lab2/10.png)
