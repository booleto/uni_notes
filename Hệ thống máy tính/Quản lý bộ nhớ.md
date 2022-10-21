### Địa chỉ tuyến tính:
Nhận xét:
- Địa chỉ tuyến tính được sử dụng bởi bộ nhớ vật lý (RAM)
- Nhược điểm: địa chỉ tuyến tính không phù hợp cho hệ điều hành dịch chuyển vùng nhớ của các tiến trình
Nếu sử dụng địa chỉ tuyến tính: các tham chiếu địa chỉ trong 1 chương trình trỏ tới 1 vị trí tuyệt đối trong bộ nhớ, không thay đổi khi dịch chuyển vùng nhớ

### Địa chỉ phân đoạn
Compiler biên dịch chương trình sang mã máy và sắp các đoạn chương trình vào trong các segment
Các địa chỉ tham chiếu trong 1 segment đều là các địa chỉ offset
Khi chạy chương trình, hệ điều hành sẽ tải các segment chương trình đặt vào các vị trí khác nhau, khi đó địa chỉ của mỗi segment mới được xác định

------[code segment]---[data segment]---[stack segment]---[heap segment]----
code segment: chứa mã chương trình
data segment: chữa dữ liệu tổng thể
stack segment: lưu giá trị các tham số vào các biến cục bộ
heap segment: không gian cấp phát động

##### Chuyển đổi địa chỉ segment offset sang địa chỉ tuyến tính
Các địa chỉ segment trỏ tới 1 dòng trong bảng segment
Trường Limit: giới hạn độ dài của 1 segment
Trường Base: địa chỉ tuyến tính byte đầu segment
Địa chỉ tuyến tính = Base + offset với điều kiện offset < limit

VD: cho địa chỉ seg.offset 16b
segment: A0B6
offset: 120F
Có địa chỉ tuyến tính là 10 $\times$ A0B6 + 120F = A1D6F

##### Địa chỉ phân trang
Trang nhớ là các đơn vị cấp phát nhỏ nhất
Phương pháp địa chỉ phân trang được sử dụng trong việc cấp phát/giải phóng bộ nhớ và quản lý bộ nhớ ảo

# Giải phóng bộ nhớ
### Bản đồ bit
Nhận xét: thao tác cấp phát "khá" tốn thời gian

### Danh sách móc nối

### Quản lý bộ nhớ theo khối có kích thước bội mũ 2