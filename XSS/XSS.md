# Cross-site scripting  
Trong phần này, chúng ta sẽ giải thích về cross-site scripting, mô tả các loại lỗ hổng cross-site scripting khác nhau, và chỉ ra cách tìm và ngăn chặn cross-site scripting.

# Cross-site scripting (XSS) là gì?  
Cross-site scripting (còn được gọi là XSS) là một lỗ hổng bảo mật web cho phép kẻ tấn công can thiệp vào các tương tác mà người dùng thực hiện với một ứng dụng có lỗ hổng. Nó cho phép kẻ tấn công vượt qua same origin policy, vốn được thiết kế để tách biệt các trang web khác nhau. Lỗ hổng cross-site scripting thường cho phép kẻ tấn công giả mạo làm người dùng nạn nhân để thực hiện bất kỳ hành động nào mà người dùng có thể thực hiện và truy cập vào bất kỳ dữ liệu nào của người dùng. Nếu người dùng có quyền truy cập cao trong ứng dụng, kẻ tấn công có thể giành quyền kiểm soát hoàn toàn chức năng và dữ liệu của ứng dụng.

# XSS hoạt động như thế nào?  
Cross-site scripting hoạt động bằng cách thao túng một trang web dễ bị tổn thương để nó trả về mã JavaScript độc hại cho người dùng. Khi mã độc hại được thực thi trong trình duyệt của nạn nhân, kẻ tấn công có thể hoàn toàn kiểm soát tương tác của họ với ứng dụng.

# XSS proof of concept  
Bạn có thể xác nhận hầu hết các loại lỗ hổng XSS bằng cách truyền vào một payload khiến trình duyệt của bạn thực thi một số mã JavaScript tùy ý. Việc sử dụng hàm `alert()` cho mục đích này đã trở nên phổ biến từ lâu vì nó ngắn gọn, vô hại và rất dễ nhận ra khi nó được gọi thành công. Trên thực tế, bạn sẽ giải quyết phần lớn các phòng thí nghiệm XSS của chúng tôi bằng cách gọi `alert()` trong trình duyệt của nạn nhân giả lập.

Tuy nhiên, nếu bạn sử dụng Chrome, có một trở ngại nhỏ. Từ phiên bản 92 trở đi (20 tháng 7, 2021), các iframe chéo nguồn bị ngăn không gọi được `alert()`. Vì chúng được sử dụng để xây dựng một số cuộc tấn công XSS nâng cao, thỉnh thoảng bạn sẽ cần sử dụng payload PoC thay thế. Trong trường hợp này, chúng tôi khuyến nghị sử dụng hàm `print()`.

Vì nạn nhân giả lập trong các phòng thí nghiệm của chúng tôi sử dụng Chrome, chúng tôi đã sửa đổi các phòng thí nghiệm bị ảnh hưởng để chúng cũng có thể được giải quyết bằng `print()`. Chúng tôi đã chỉ ra điều này trong các hướng dẫn liên quan.

# Các loại tấn công XSS là gì?  
Có ba loại tấn công XSS chính, đó là:

- XSS phản hồi (Reflected XSS), nơi mã độc hại đến từ yêu cầu HTTP hiện tại.
- XSS lưu trữ (Stored XSS), nơi mã độc hại đến từ cơ sở dữ liệu của trang web.
- XSS dựa trên DOM (DOM-based XSS), nơi lỗ hổng tồn tại trong mã phía máy khách thay vì phía máy chủ.

## XSS phản hồi  
XSS phản hồi là loại cross-site scripting đơn giản nhất. Nó xảy ra khi một ứng dụng nhận dữ liệu trong một yêu cầu HTTP và bao gồm dữ liệu đó trong phản hồi ngay lập tức theo cách không an toàn.

Đây là một ví dụ đơn giản về lỗ hổng XSS phản hồi:

```
https://insecure-website.com/status?message=All+is+well.
<p>Status: All is well.</p>
```
Ứng dụng không xử lý thêm dữ liệu, vì vậy kẻ tấn công có thể dễ dàng tạo một cuộc tấn công như sau:

```
https://insecure-website.com/status?message=<script>/*+Bad+stuff+here...+*/</script>
<p>Status: <script>/* Bad stuff here... */</script></p>
```
Nếu người dùng truy cập URL do kẻ tấn công tạo, mã độc sẽ được thực thi trong trình duyệt của người dùng, trong ngữ cảnh của phiên làm việc của người dùng với ứng dụng. Khi đó, mã này có thể thực hiện bất kỳ hành động nào và truy xuất bất kỳ dữ liệu nào mà người dùng có quyền truy cập.

## XSS lưu trữ  
XSS lưu trữ (còn được gọi là XSS cố định hoặc XSS bậc hai) xảy ra khi một ứng dụng nhận dữ liệu từ một nguồn không tin cậy và bao gồm dữ liệu đó trong các phản hồi HTTP sau này theo cách không an toàn.

Dữ liệu có thể được gửi đến ứng dụng qua các yêu cầu HTTP, ví dụ như các bình luận trên bài viết blog, biệt danh người dùng trong phòng trò chuyện, hoặc chi tiết liên hệ trong đơn đặt hàng của khách hàng. Trong các trường hợp khác, dữ liệu có thể đến từ các nguồn không tin cậy khác, ví dụ như ứng dụng webmail hiển thị các tin nhắn nhận qua SMTP, ứng dụng tiếp thị hiển thị bài đăng mạng xã hội, hoặc ứng dụng giám sát mạng hiển thị dữ liệu gói từ lưu lượng mạng.

## XSS dựa trên DOM  
XSS dựa trên DOM (còn được gọi là DOM XSS) xảy ra khi một ứng dụng chứa một số mã JavaScript phía máy khách xử lý dữ liệu từ một nguồn không tin cậy theo cách không an toàn, thường là bằng cách ghi dữ liệu trở lại DOM.

Trong ví dụ sau, một ứng dụng sử dụng một số JavaScript để đọc giá trị từ một trường đầu vào và ghi giá trị đó vào một phần tử trong HTML:

```
var search = document.getElementById('search').value;
var results = document.getElementById('results');
results.innerHTML = 'You searched for: ' + search;
```
Nếu kẻ tấn công có thể kiểm soát giá trị của trường đầu vào, họ có thể dễ dàng tạo ra một giá trị độc hại khiến mã của họ thực thi:

```
You searched for: <img src=1 onerror='/* Bad stuff here... */'>
```
Thông thường, trường đầu vào sẽ được điền từ một phần của yêu cầu HTTP, chẳng hạn như tham số chuỗi truy vấn URL, cho phép kẻ tấn công thực hiện cuộc tấn công bằng cách sử dụng một URL độc hại, tương tự như XSS phản hồi.