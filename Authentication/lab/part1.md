# [Lab 1: Username enumeration via different responses](https://portswigger.net/web-security/authentication/password-based/lab-username-enumeration-via-different-responses)

![Yêu cầu lab 1](../image/lab1/0.png)

> - **Mô tả lab:** có thể liệt kê username và brute-force password.
>
> - **Mục tiêu:** tìm username tồn tại, brute-force password và login vào tài khoản đó.

Đây là trang web chính của bài lab, ta có thể dễ dàng thấy các chức năng:

- `Home:` trở về blog

- `My account:` thông tin tài khoản

- `View post:` xem thông tin bài viết

![Alt text](../image/lab1/01.png)

Đến `My account`, khi ta chưa đăng nhập, sẽ chuyển đến trang Login, đây là nơi chúng ta cần brute-force

![Alt text](../image/lab1/02.png)

Thử đăng nhập với username và password bất kỳ ta nhận được Message `Invalid username` → tài khoản có username ‘a’ chưa tồn tại → lợi dụng nó để tìm kiếm username tồn tại

![Alt text](../image/lab1/03.png)

![Alt text](../image/lab1/04.png)

Với tài khoản có username ‘agenda’ không thấy có message → đây là tài khoản đã tồn tại

![Alt text](../image/lab1/05.png)

→ Thử đăng nhập với username ‘agenda’ ta thấy Message `Incorrect password` → lợi dụng để bypass

![Alt text](../image/lab1/06.png)

làm tương tự như username, ta cũng tìm được password

> Note: Ngoài ra ta có thể brute-force cả 2 vị trí cùng lúc, tuy nhiên payload nhiều hơn nên sẽ mất thời gian hơn rất nhiều

![Alt text](../image/lab1/07.png)

Tìm được rồi

![Alt text](../image/lab1/08.png)

login and solve

![Alt text](../image/lab1/09.png)

# [Lab 2: Username enumeration via subtly different responses](https://portswigger.net/web-security/authentication/password-based/lab-username-enumeration-via-subtly-different-responses)

![Yêu cầu lab 2](../image/lab2/0.png)

> - **Mô tả lab:** nói chung là giống **lab 1**

Bài này tương tự như Lab 1 nhưng sự khác biệt là khi đăng nhập thì message khác chỉ chung chung là `Invalid username or password` chứ không message riêng như lab 1, tuy nhiên khi tồn tại thì message này cũng không xuất hiện nên làm như lab 1 vẫn thành công.

![Alt text](../image/lab2/01.png)

tìm được username

![Alt text](../image/lab2/02.png)

tìm được password

![Alt text](../image/lab2/03.png)

login and solve the lab

![Alt text](../image/lab2/04.png)

# [Lab 3: Username enumeration via response timing](https://portswigger.net/web-security/authentication/password-based/lab-username-enumeration-via-response-timing)

![Yều cầu lab 3](../image/lab3/0.png)

> - **Mô tả lab:** có thể liệt kê username dựa trên thời gian phản hồi.
>
> - **Mục tiêu:** tìm username tồn tại, brute-force password và login vào tài khoản đó.

main web

![Alt text](../image/lab3/01.png)

đăng nhập sai thì message vẫn tương tự như các bài lab trên

![Alt text](../image/lab3/02.png)

tuy nhiên khi ta cố gắng đăng nhập sai nhiều lần → sẽ bị block một khoảng thời gian

![Alt text](../image/lab3/03.png)

Sau đó dù có đăng nhập với tài khoán chính xác thì cũng không thể đăng nhập thành công, rất có thể là do chặn IP rồi.

Tuy nhiên, web có hỗ trợ header `X-Forwarded-For`, cho phép ta giả mạo địa chỉ IP và bypass bảo vệ brute-force dựa trên IP. Khi thêm header vào thì ta đã hết bị block rồi. Vậy ta sẽ dùng nó để bypass bài này.

![Alt text](../image/lab3/04.png)

chú ý status code khi đăng nhập thành công: `302`

![Alt text](../image/lab3/05.png)

Set up tấn công, tìm username trước

![Alt text](../image/lab3/06.png)![Alt text](../image/lab3/07.png)

tìm password

![Alt text](../image/lab3/08.png)![Alt text](../image/lab3/09.png)

login and solve

![Alt text](../image/lab3/10.png)
 
# [Lab 4: Broken brute-force protection, IP block](https://portswigger.net/web-security/authentication/password-based/lab-broken-bruteforce-protection-ip-block)

![Yêu cầu lab 4](../image/lab4/0.png)

> - **Mô tả lab:** lỗ hổng trong cơ chế bảo vệ brute-force password.
>
> - **Mục tiêu:** tìm password của `carlos` và login.

![Trang web chính](../image/lab4/01.png)

Các message báo lỗi

![Alt text](../image/lab4/02.png)![Alt text](../image/lab4/03.png)

Sau khi đăng nhập sai quá 3 lần, IP sẽ bị block trong 1 khoảng thời gian.

![Alt text](../image/lab4/04.png)

Tuy nhiên thời gian này ngắn quá, sau khoảng thời gian này ta có thể đăng nhập lại bình thường. Nếu đợi để brute-force thì khá lâu, ta thử 1 lần đăng nhập thất bại và sau đó sẽ là 1 lần đăng nhập đúng thì vẫn đăng nhập được bình thường nên ta sẽ sử dụng cách này để tìm password của `carlos`

Khi đăng nhập chính xác, chú ý status code `302`

![Alt text](../image/lab4/05.png)

Set up tấn công

- tạo trước 1 list username password: xen kẽ các payload 

![Alt text](../image/lab4/06.png)

- set up Intruder

![Alt text](../image/lab4/07.png)

- tìm thấy password

![Alt text](../image/lab4/08.png)

Login and solve

![Alt text](../image/lab4/09.png)

# [Lab 5: Username enumeration via account lock](https://portswigger.net/web-security/authentication/password-based/lab-username-enumeration-via-account-lock)

![Yêu cầu lab 5](../image/lab5/0.png)

> - **Mô tả lab:** lỗi liệt kê username, sử dụng khóa tài khoản tuy nhiên có lỗi logic.
>
> - **Mục tiêu:** liệt username tồn tại, brute-force password và login vào tài khoản đó.

![main web](../image/lab5/01.png)

Thử đăng nhập với tài khoản bất kỳ nhưng không thấy có message nào ngoài `Invalid username or password` bởi các tài khoản này đều không hề tồn tại. Giờ ta cần phải đi tìm các tài khoản tồn tại để solve lab.

![Alt text](../image/lab5/02.png)

Thử brute-force cả 2 position `username` và `password` luôn và Extract dữ liệu theo thông báo warning

![Alt text](../image/lab5/03.png)

thì phát hiện có 1 tài khoản không hề có warning gì

![Alt text](../image/lab5/04.png)

Đăng nhập thành công → solve lab

![Alt text](../image/lab5/05.png)

Tuy nhiên test như trên thì hơn khoảng 10000 Request nên rất lâu. Ta nên tìm `username` trước.

![Alt text](../image/lab5/06.png)

Ta tìm được 1 tài khoản có message khác với các tài khoản còn lại `You have made too many incorrect login attempts. Please try again in 1 minute(s).` Chắc hẳn đây là tài khoản tồn tại cần tìm rồi.

![Alt text](../image/lab5/07.png)

Sau đó tương tự như trên nhưng chỉ cần brute-force cho password là ra.

![Alt text](../image/lab5/08.png)

# [Lab 6: Broken brute-force protection, multiple credentials per request](https://portswigger.net/web-security/authentication/password-based/lab-broken-brute-force-protection-multiple-credentials-per-request)

![Yêu cầu lab 6](../image/lab6/0.png)

> - **Mô tả lab:** lỗ hổng trong cơ chế bảo vệ brute-force.
>
> - **Mục tiêu:** tìm password của `carlos` và login.

![Trang web chính](../image/lab6/01.png)

vẫn là những message quen thuộc

![Alt text](../image/lab6/02.png)

và vẫn bị block khi nhập sai quá nhiều

![Alt text](../image/lab6/03.png)

Quan sát Request thì thấy bài này body là một đoạn `Javascript` chứ không phải `key=value` như các bài trước

![Alt text](../image/lab6/04.png)

thử thay bằng mảng các password coi có bị kiểm tra gì không và kết quả cho thấy là nó vẫn duyệt bình thường

![Alt text](../image/lab6/05.png)

Thử với `carlos` coi có login thành công không

![Alt text](../image/lab6/06.png)

Thành công rồi solve lab thôi

![Alt text](../image/lab6/07.png)
