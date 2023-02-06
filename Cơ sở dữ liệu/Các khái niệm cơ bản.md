### Lý thuyết về cơ sở dữ liệu
Là lĩnh vực của Tin học nhằm nghiên cứu các nguyên tắc, cách thức, phương pháp tổ chức dữ liệu trên các thiết bị mang tin của máy tính nhằm giúp cho người sử dụng dễ dàng thực hiện các thao tác dữ liệu một cách thuận tiện nhất

### Cơ sở dữ liệu
Tập hợp các dữ liệu có cấy trúc và liên quan với nhau được lưu trữ trên máy tính, đáp ứng nhu cầu khai thác của nhiều người dùng một cách đồng thời và được tổ chức theo một mô hình
Dữ liệu là các sự kiện có thể ghi lại được và có ý nghĩa
CSDL thường được hiểu là một tập hợp các dữ liệu được quản lý bởi một hệ quản trị cơ sở dữ liệu

---

### Hệ quản trị cơ sở dữ liệu
Là một tập hợp chương trình cho phép:
- Cho phép người dùng tạo mới các cơ sở dữ liệu và xác định các lược đồ cơ sở dữ liệu của chúng (cấu trúc logic của dữ liệu)
- Cho phép người dùng có thể truy vấn và thay đổi dữ liệu
- Hỗ trợ lưu trữ lượng lớn dữ liệu - có thể hàng terabytes - trong thời gian dài
- Có khả năng khôi phục dữ liệu
- Điều khiển truy cập dữ liệu từ nhiều người sử dụng tại cùng một thời điểm
---
### Hệ thống tệp tin
Cơ sở dữ liệu tệp: Lưu trữ thông tin của một bảng trong một tệp duy nhất, trong đó mỗi dòng chứa một bản ghi và mỗi trường được phân cách nhau bởi dấu phẩy, khoảng trắng, tab,... Không có mối quan hệ cấu trúc giữa bản ghi

**Thuận lợi**
- Dễ hiểu, dễ thực hiện
- Yêu cầu ít kỹ năng phần cứng phần mềm hỗ trợ
- Thích hợp nhất cho các cơ sở dữ liệu nhỏ
**Khó khăn**
- Dữ liệu trùng lặp nhiều
- Thao tác dữ liệu kém hiệu quả do phát sinh các dị thường
- Tốn thời gian khi truy xuất thông tin đối với các cơ sở dữ liệu lớn
---
### Mô hình dữ liệu phân cấp
Cơ sở dữ liệu phần cấp biểu diễn dữ liệu trong một cấu trúc cây, trong đó có một nút cha duy nhất cho các bản ghi

**Thuận lợi**
- Dễ hiểu các mối quan hệ giữa các bản ghi khác nhau
- Tốc độ tryu cập thông tin nhanh chóng do các đường dẫn được xác định trước

**Khó khăn**
- Không có phép thể hiện mối quan hệ nhiều - nhiều
- Bất kỳ thay đổi nào trong các mối quan hệ có thể yêu cầu tổ chức lại dữ liệu theo cách thủ công
---
### Mô hình dữ liệu mạng
Mô hình dữ liệu mạng cho phép thể hiện mối quan hệ nhiều - nhiều. Một cơ sở dữ liệu mạng có cấu trúc đồ thị cơ bản

**Thuận lợi**
- Mô hình đơn giản, dễ thực hiện
- Dễ dàng truy cập dữ liệu
- Khả năng quản lý nhiều mối quan hệ hơn: một-một, một-nhiều, nhiều-nhiều
**Khó khăn**
- Hệ thống phức tạp: mỗi và mọi bản ghi phải được duy trì với sự trợ giúp của con trỏ
- Do việc sử dụng con trỏ, việc chèn, cập nhật và xóa trở nên phức tạp hơn
- Một sự thay đổi trong cấu trúc đòi hỏi một sự thay đổi trong ứng dụng -> thiếu tính độc lập về cấu trúc
---
### Mô hình dữ liệu quan hệ
Mô hình dữ liệu quan hệ lưu trữ thông tin trong các bảng (thực thể) và cho phép các thực thể có quan hệ với nhauu thông qua một thuộc tính chung

**Thuận lợi**
- Hỗ trợ các phép toán như hợp, giao, hiệu, tích Descartes, chiếu, chọn, kết nối và chia
- Dễ dàng truy cập dữ liệu
- Sử dụng ngôn ngữ dễ hiểu
**Khó khăn**
- Phản hồi cho một truy vấn tốn nhiều thời gian và không hiệu quả nếu số lượng các bảng được thiết lập giữa các mối quan hệ tăng lên