# SQL injection (SQLi) là gì?
SQL injection (SQLi) là một lỗ hổng bảo mật web cho phép kẻ tấn công can thiệp vào các truy vấn mà ứng dụng thực hiện với cơ sở dữ liệu của nó. Điều này có thể cho phép kẻ tấn công xem dữ liệu mà họ không thể truy cập được, bao gồm dữ liệu của người dùng khác hoặc bất kỳ dữ liệu nào mà ứng dụng có thể truy cập. Trong nhiều trường hợp, kẻ tấn công có thể sửa đổi hoặc xóa dữ liệu này, gây ra các thay đổi vĩnh viễn đối với nội dung hoặc hành vi của ứng dụng.<br>Trong một số tình huống, kẻ tấn công có thể leo thang cuộc tấn công SQL injection để xâm nhập vào máy chủ hoặc cơ sở hạ tầng phía sau. Điều này cũng có thể cho phép chúng thực hiện các cuộc tấn công từ chối dịch vụ (DoS).

# Cách nhận biết lỗ hổng SQL injection
Bạn có thể phát hiện SQL injection thủ công bằng cách kiểm tra một cách có hệ thống từng điểm nhập liệu trong ứng dụng. Để làm điều này, bạn có thể:

- Gửi ký tự dấu nháy đơn `'` và kiểm tra lỗi hoặc bất thường.
- Sử dụng cú pháp SQL đặc biệt để so sánh giá trị ban đầu và giá trị thay đổi.
- Kiểm tra các điều kiện Boolean như `OR 1=1` và `OR 1=2` và xem phản hồi khác nhau.
- Sử dụng các payloads để gây ra độ trễ thời gian và kiểm tra sự khác biệt trong thời gian phản hồi.
- Dùng các payloads OAST để kích hoạt tương tác mạng và giám sát các tương tác kết quả.

Ngoài ra, bạn có thể sử dụng Burp Scanner để phát hiện các lỗ hổng SQL injection một cách nhanh chóng và đáng tin cậy.

Hầu hết các lỗ hổng SQL injection xảy ra trong mệnh đề `WHERE` của truy vấn SELECT. Tuy nhiên, chúng có thể xuất hiện ở bất kỳ vị trí nào trong truy vấn và trong các loại truy vấn khác nhau. Một số vị trí phổ biến khác mà SQL injection xuất hiện bao gồm:

- Trong câu lệnh `UPDATE`, bên trong các giá trị được cập nhật hoặc mệnh đề WHERE.
- Trong câu lệnh `INSERT`, bên trong các giá trị được chèn.
- Trong câu lệnh `SELECT`, bên trong tên bảng hoặc tên cột.
- Trong câu lệnh `SELECT`, bên trong mệnh đề `ORDER BY`.

<h1>Thu thập thông tin ẩn</h1>
Hãy tưởng tượng một ứng dụng mua sắm hiển thị sản phẩm theo các danh mục khác nhau. Khi người dùng nhấp vào danh mục Quà tặng, trình duyệt của họ yêu cầu URL:

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

Điều kiện `released = 1` được sử dụng để ẩn sản phẩm chưa phát hành. Từ đó có thể cho rằng các sản phẩm chưa phát hành sẽ có điều kiện là `released = 0`.

Khi một ứng dụng không có bất cứ cơ chế phòng thủ nào để chống lại SQL injection, kẻ tấn công có thể truy cập ứng dụng bằng đường link:

```
https://insecure-website.com/products?category=Gifts'--
```

Kết quả là một truy vấn SQL được thực hiện:

```sql
SELECT * FROM products WHERE category = 'Gifts'--' AND released = 1
```

```--``` là dấu được sử dụng để đánh dấu phần bình luận trong ngôn ngữ SQL, tương tự với việc mọi thứ đứng sau nó trong một truy vấn SQL sẽ được coi như một bình luận và không được thực thi. Trong ví dụ này, kẻ tấn công đã loại bỏ được điều kiện ```AND released = 1``` và lấy ra toàn bộ thông tin thuộc `category = Gifts`, bao gồm cả những sản phẩm chưa được phát hành.

Phát triển từ phương pháp này, kẻ tấn công có thể truy cập vào ứng dụng bằng một đường link tương tự để có thể lấy được thông tin về mọi sản phẩm thuộc bất kỳ danh mục nào hoặc đã phát hành hay chưa:

```
https://insecure-website.com/products?category=Gifts'+OR+1=1--
```

Ứng dụng sẽ thực hiện truy vấn SQL:

```sql
SELECT * FROM products WHERE category = 'Gifts' OR 1=1--' AND released = 1
```

Truy vấn này trả về mọi kết quả có `category = Gifts` hoặc `1=1`, hay mệnh đề `true` và truy vấn trả về mọi kết quả.

> ## Lưu ý
> Khi chèn điều kiện OR 1=1 vào một truy vấn SQL, ngay cả khi điều này có vẻ vô hại trong ngữ cảnh mà bạn đang chèn vào, thì việc các ứng dụng sử dụng dữ liệu từ một yêu cầu duy nhất trong nhiều truy vấn khác nhau là điều rất phổ biến. Nếu điều kiện của bạn được áp dụng cho một câu lệnh UPDATE hoặc DELETE, ví dụ như vậy, nó có thể gây ra mất dữ liệu ngoài ý muốn.