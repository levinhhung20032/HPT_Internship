<h1>SQL injection (SQLi) là gì?</h1>
<p align = justify>SQL injection (SQLi) là một lỗ hổng bảo mật web cho phép kẻ tấn công can thiệp vào các truy vấn mà ứng dụng thực hiện với cơ sở dữ liệu của nó. Điều này có thể cho phép kẻ tấn công xem dữ liệu mà họ không thể truy cập được, bao gồm dữ liệu của người dùng khác hoặc bất kỳ dữ liệu nào mà ứng dụng có thể truy cập. Trong nhiều trường hợp, kẻ tấn công có thể sửa đổi hoặc xóa dữ liệu này, gây ra các thay đổi vĩnh viễn đối với nội dung hoặc hành vi của ứng dụng.<br>Trong một số tình huống, kẻ tấn công có thể leo thang cuộc tấn công SQL injection để xâm nhập vào máy chủ hoặc cơ sở hạ tầng phía sau. Điều này cũng có thể cho phép chúng thực hiện các cuộc tấn công từ chối dịch vụ (DoS).</p>

<h1>Cách nhận biết lỗ hổng SQL injection</h1>
<p align=justify>Bạn có thể phát hiện SQL injection thủ công bằng cách kiểm tra một cách có hệ thống từng điểm nhập liệu trong ứng dụng. Để làm điều này, bạn có thể:
<ul>
<li>Gửi ký tự dấu nháy đơn `'` và kiểm tra lỗi hoặc bất thường.</li>
<li>Sử dụng cú pháp SQL đặc biệt để so sánh giá trị ban đầu và giá trị thay đổi.</li>
<li>Kiểm tra các điều kiện Boolean như `OR 1=1` và `OR 1=2` và xem phản hồi khác nhau.</li>
<li>Sử dụng các payloads để gây ra độ trễ thời gian và kiểm tra sự khác biệt trong thời gian phản hồi.</li>
<li>Dùng các payloads OAST để kích hoạt tương tác mạng và giám sát các tương tác kết quả.</li>
</ul>
Ngoài ra, bạn có thể sử dụng Burp Scanner để phát hiện các lỗ hổng SQL injection một cách nhanh chóng và đáng tin cậy.

Hầu hết các lỗ hổng SQL injection xảy ra trong mệnh đề WHERE của truy vấn SELECT. Tuy nhiên, chúng có thể xuất hiện ở bất kỳ vị trí nào trong truy vấn và trong các loại truy vấn khác nhau. Một số vị trí phổ biến khác mà SQL injection xuất hiện bao gồm:
</ul>
<li>Trong câu lệnh UPDATE, bên trong các giá trị được cập nhật hoặc mệnh đề WHERE.</li>
<li>Trong câu lệnh INSERT, bên trong các giá trị được chèn.</li>
<li>Trong câu lệnh SELECT, bên trong tên bảng hoặc tên cột.</li>
<li>Trong câu lệnh SELECT, bên trong mệnh đề ORDER BY.</li>
</ul></p>

<h1>Thu thập thông tin ẩn</h1>
<p align=justify>Hãy tưởng tượng một ứng dụng mua sắm hiển thị sản phẩm theo các danh mục khác nhau. Khi người dùng nhấp vào danh mục Quà tặng, trình duyệt của họ yêu cầu URL:

```
https://insecure-website.com/products?category=Gifts
```

Điều này khiến ứng dụng thực hiện một truy vấn SQL để lấy chi tiết về các sản phẩm liên quan từ cơ sở dữ liệu:

```sql
SELECT * FROM products WHERE category = 'Gifts' AND released = 1
```

Truy vấn này yêu cầu cơ sở dữ liệu trả về:

- Tất cả chi tiết (*)
- Từ bảng products
- Nơi category là Gifts
- Và released là 1

Điều kiện `released = 1` được sử dụng để ẩn sản phẩm chưa phát hành.</p>

<h1></h1>
<p align=justify></p>