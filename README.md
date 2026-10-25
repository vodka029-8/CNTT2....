### Phần 1: Phân tích 5 Quy tắc Vàng vi phạm trong bảng tính

Bảng dữ liệu quản lý thiết bị IoT hiện tại đang vi phạm các nguyên tắc thiết kế cơ sở dữ liệu trên bảng tính (Database Rules) sau:

1. **Vi phạm quy tắc "Hàng tiêu đề đơn giản & Duy nhất (No Merged Cells in Headers)":**
* *Mô tả lỗi:* Các ô bị gộp (Merge Cell) ở hàng tiêu đề khiến cho cấu trúc hàng cột bị mất tính đồng nhất. Khi đó, tính năng AutoFilter của Excel không thể nhận diện chính xác phạm vi tiêu đề của từng cột.


2. **Vi phạm quy tắc "Không để hàng trống (No Blank Rows/Columns within Data)":**
* *Mô tả lỗi:* Sự tồn tại của các hàng trống nằm giữa vùng dữ liệu khiến Excel hiểu nhầm rằng bảng dữ liệu đã kết thúc tại hàng trống đó. Đây là nguyên nhân trực tiếp làm cho nút Filter biến mất hoặc chỉ lọc được một phần dữ liệu phía trên.


3. **Vi phạm quy tắc "Tính đồng nhất và sạch sẽ của dữ liệu (Data Cleanliness & Consistency)":**
* *Mô tả lỗi:* Cột "Trạng thái" chứa dữ liệu không đồng nhất do lỗi khoảng trắng thừa (khoảng trắng vô hình trước hoặc sau chữ). Khi bật Filter, Excel phân loại giá trị có khoảng trắng và không có khoảng trắng thành hai nhóm riêng biệt (`hỏng ` và `hỏng`), gây sai lệch thống kê.



---

### Phần 2: Hướng dẫn hoàn thiện & Xử lý dữ liệu

#### Bước 1: Xử lý gỡ bỏ Merge Cell và Xóa hàng trống

* **Gỡ Merge Cell:** Bôi đen khu vực tiêu đề đang bị gộp, nhấn vào icon **Merge & Center** trên thanh công cụ để hủy gộp, sau đó điền lại tiêu đề cho các cột tương ứng nếu cần.
* **Xóa hàng trống:** Rà soát và xóa bỏ toàn bộ các hàng hoặc cột trống ngắt quãng nằm giữa bảng dữ liệu để đảm bảo bảng dữ liệu liền mạch từ trên xuống dưới.

#### Bước 2: Làm sạch cột "Trạng thái" bằng hàm TRIM

Để loại bỏ các khoảng trắng thừa vô hình khiến Filter không khớp, bạn thực hiện như sau:

1. Chèn một cột phụ cạnh cột "Trạng thái" cũ (ví dụ: đặt tên là *Trạng_thái_chuẩn*).
2. Tại ô đầu tiên của cột phụ, nhập công thức:
```excel
=TRIM(C2)

```


*(Giả sử cột trạng thái cũ nằm ở cột C).*
3. Kéo công thức xuống cho toàn bộ các dòng dữ liệu.
4. Copy toàn bộ cột phụ vừa làm sạch, sau đó **Paste Values** đè ngược lại vào cột "Trạng thái" gốc để cố định dữ liệu, rồi xóa cột phụ đi.

#### Bước 3: Kích hoạt AutoFilter hoàn chỉnh

* Bôi đen toàn bộ vùng dữ liệu (hoặc chọn một ô bất kỳ trong bảng).
* Trên thanh Ribbon, chọn **Data** > nhấn **Filter** (hoặc dùng phím tắt `Ctrl + Shift + L`). Lúc này nút Filter sẽ hiển thị đầy đủ ở tất cả các cột và gom nhóm dữ liệu trạng thái "hỏng" một cách chính xác tuyệt đối.
