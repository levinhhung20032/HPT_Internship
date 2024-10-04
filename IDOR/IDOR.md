# Tham chiếu đối tượng trực tiếp không an toàn (IDOR)  
Trong phần này, chúng ta sẽ giải thích về tham chiếu đối tượng trực tiếp không an toàn (IDOR) và mô tả một số lỗ hổng phổ biến.

# Tham chiếu đối tượng trực tiếp không an toàn (IDOR) là gì?  
Tham chiếu đối tượng trực tiếp không an toàn (IDOR) là một loại lỗ hổng kiểm soát truy cập phát sinh khi một ứng dụng sử dụng đầu vào do người dùng cung cấp để truy cập trực tiếp vào các đối tượng. Thuật ngữ IDOR được phổ biến bởi sự xuất hiện của nó trong danh sách OWASP Top Ten 2007. Tuy nhiên, đây chỉ là một ví dụ trong nhiều lỗi triển khai kiểm soát truy cập có thể dẫn đến việc bỏ qua các kiểm soát truy cập. Lỗ hổng IDOR thường liên quan đến leo thang đặc quyền ngang, nhưng cũng có thể xảy ra với leo thang đặc quyền dọc.

# Ví dụ về IDOR  
Có nhiều ví dụ về lỗ hổng kiểm soát truy cập trong đó giá trị tham số do người dùng kiểm soát được sử dụng để truy cập trực tiếp vào tài nguyên hoặc chức năng.

## Lỗ hổng IDOR với tham chiếu trực tiếp đến các đối tượng trong cơ sở dữ liệu  
Hãy xem xét một trang web sử dụng URL sau để truy cập vào trang tài khoản khách hàng, bằng cách truy xuất thông tin từ cơ sở dữ liệu:

```
https://insecure-website.com/customer_account?customer_number=132355
```

Ở đây, số tài khoản khách hàng được sử dụng trực tiếp làm chỉ mục bản ghi trong các truy vấn thực hiện trên cơ sở dữ liệu. Nếu không có kiểm soát bổ sung nào, kẻ tấn công có thể đơn giản thay đổi giá trị `customer_number`, bỏ qua các kiểm soát truy cập để xem hồ sơ của khách hàng khác. Đây là một ví dụ về lỗ hổng IDOR dẫn đến leo thang đặc quyền ngang.

Kẻ tấn công có thể thực hiện leo thang đặc quyền ngang hoặc dọc bằng cách thay đổi người dùng thành một người có đặc quyền cao hơn trong khi bỏ qua các kiểm soát truy cập. Những khả năng khác bao gồm khai thác rò rỉ mật khẩu hoặc sửa đổi các tham số khi kẻ tấn công đã truy cập được vào trang tài khoản của người dùng.

## Lỗ hổng IDOR với tham chiếu trực tiếp đến các tệp tĩnh  
Lỗ hổng IDOR thường xảy ra khi các tài nguyên nhạy cảm được lưu trong các tệp tĩnh trên hệ thống tệp máy chủ. Ví dụ, một trang web có thể lưu trữ các bản sao tin nhắn trò chuyện trên đĩa bằng cách sử dụng tên tệp tăng dần, và cho phép người dùng truy xuất chúng bằng cách truy cập URL như sau:

```
https://insecure-website.com/static/12144.txt
```

Trong tình huống này, kẻ tấn công có thể đơn giản sửa đổi tên tệp để truy xuất bản ghi do người dùng khác tạo và có thể thu thập thông tin đăng nhập hoặc dữ liệu nhạy cảm khác.