# 1. Burp Proxy

## 1.1. Overview

Burp Proxy cho phép bạn can thiệp vào các gói tin HTTP giữa Burp's Browser và server của các trang web. Điều này giúp ta hiểu rõ được cách hoạt động của các trang web cũng như cấu trúc của các gói tin khi ta thao tác. Không chỉ vậy, Burp Suite cũng cho phép ta sửa đổi gói tin trước khi chuyển tiếp chúng.

## 1.2. Tutorial
1. Chọn tab <b>Proxy > Intercept</b>.<br>
2. Nhấn <b>"Intercept is off"</b> để chuyển thành <b>"Intercept is on".</b><br>
<image src="images/1.png"><br>
3. Nhấn <b>Open Browser</b> để mở Burp's Browser.<br>
4. Điều chỉnh kích cỡ cửa sổ để hiển thị cả Burp Suite và Burp's Browser lên màn hình.<br>
5. Từ giờ cho đến khi tắt Inception hoặc đóng Burp's Browser, khi thao tác trên Burp's Browser, các gói tin mà server gửi cho máy sẽ được Burp Suite giữ lại và hiển thị cho người dùng trước khi được chuyển tiếp cho máy xử lý.<br>
<image src="images/2.png"><br>

## 1.3. Usage
1. Tìm hiểu cấu trúc gói tin qua lại giữa máy tính và server<br>
2. Thực hiện các cuộc tấn công HTTP Request Smuggling, SQL Injection, Cross-Site Scripting, Session Hijacking, ...<br>
3. Chủ động ngăn chặn các cuộc tấn công bằng gói tin<br>

## 1.4. Additional Features
1. HTTP history: Hiển thị mọi gói tin HTTP đã được thông qua bởi Burp Suite, dù Intercept có đang bật hay không, đồng thời cung cấp các tuỳ chọn như Scan (Quét tuỳ chọn các gói tin), Send to (Chuyển gói tin cho các công cụ khác trên Burp Suite) và một số tuỳ chọn khác.<br>
<image src="images/3.png"><br>
2. WebSockets history: Tương tự như "HTTP history" nhưng hiển thị các gói tin WebSockets.
3. Proxy settings: Cá nhân hoá các quy tắc xử lý phù hợp với người dùng. Ở đây người dùng có thể thêm, sửa hoặc xoá các quy tắc để Burp Suite tuân theo, đồng thời có thể enable/disable một số feature của Burp Suite.<br>
<image src="images/4.png"><br>

# 2. Burp Intruder

## 2.1. Overview
Burp Intruder là công cụ cho phép tự động hoá việc gửi các gói tin HTTP một cách liên tục, trong đó mỗi gói tin chứa thông tin khác nhau, thường được sử dụng như một phương pháp Brute Force.

## 2.2. Tutorial
1. Chọn tab <b>Proxy > HTTP history/WebSockets history</b>.<br>
2. Chọn gói tin muốn can thiệp, chuột phải và chọn "Send to Intruder".<br>
<image src="images/5.png"><br>
3. Chọn tab <b>Intruder > Positions</b>.<br>
4. Chọn kiểu tấn công trong mục "Choose an attack type".<br>
5. Chỉnh sửa địa chỉ của "Target" (nếu cần) và nội dung của gói tin.<br>
6. Di chuyển tới tab <b>Intruder > Payloads</b>.<br>
7. Chỉnh sửa "Payload sets" phù hợp với nhu cầu, thêm nội dung vào "Payload settings", và thêm quy tắc xử lý vào "Payload processing" (nều có).<br>
<image src="images/6.png"><br>
9. Nhấn "Start attack", Burp Suite sẽ gửi gói tin đến server dựa trên "Attack type" đã được người dùng chọn.<br>

## 2.3. Usage
1. Brute Force tài khoản hoặc mật khẩu<br>
2. Tấn công DoS<br>
3. Tận dụng Session ID để thực hiện nhiều tác vụ khác nhau<br>

# 3. Burp Repeater

## 3.1. Overview
Burp Repeater là công cụ hỗ trợ gửi đi các gói tin HTTP hoặc WebSocket nhiều lần để so sánh sự khác biệt về phản hồi trong mỗi lần. Khác với Intruder, Burp Repeater chú trọng vào phản hồi của từng gói tin được gửi đi, giúp người dùng có một cái nhìn chi tiết hơn về cách phản hồi của server.

## 3.2. Tutorial
1. Chọn tab <b>Proxy > HTTP history/WebSockets history</b>.<br>
2. Chọn gói tin muốn can thiệp, chuột phải và chọn "Send to Repeater".<br>
3. Di chuyển đến tab <b>Repeater</b>.<br>
4. Gói tin được gửi từ Proxy sẽ hiện ở bên trái, phản hồi sau khi người dùng gửi gói tin sẽ hiển thị ở bên phải.<br>
<image src="images/7.png"><br>
5. Chỉnh sửa các thông tin đúng với nhu cầu của bản thân, sau đó nhấn "Send".
6. Mỗi lần nhấn "Send", khu vực hiển thị phản hồi sẽ được làm mới, có thể chọn các phím mũi tên "<" và ">" để di chuyển qua lại giữa các gói tin trước đó nhằm tự đánh giá sự khác biệt, ngoài ra cũng có thể chọn các chế độ hiển thị khác như "Raw" hoặc "Hex".

## 3.3. Usage
1. Cho phép người dùng tự can thiệp nhiều hơn, xem xét kết quả một cách kỹ lưỡng hơn so với Intruder.
2. Gửi các gói tin để tìm hiểu về các rủi ro liên quan đến dữ liệu nhập vào.
3. Gửi các gói tin để lỗ hổng liên quan đến các tác vụ nhiều thao tác hoặc liên quan đến việc duy trì kết nối với server.
