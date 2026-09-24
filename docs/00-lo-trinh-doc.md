# 00. Lộ trình đọc tài liệu

## 1. Tài liệu này được tổ chức như thế nào?

Tài liệu không bắt người dùng học toàn bộ Wyckoff/VSA trước khi chạy bộ lọc. Lộ trình đi theo chính trải nghiệm trong AmiBroker:

1. Hiểu AFL lọc gì và phân tích gì.
2. Thiết lập Parameters đúng.
3. Đọc 39 cột Tóm tắt.
4. Khi có câu hỏi, chuyển sang 48 cột chẩn đoán của chế độ Phân tích.
5. Chỉ dùng 123 cột Đầy đủ khi cần kiểm tra kỹ thuật sâu.

## 2. Mẫu giải thích thống nhất

Mỗi Parameters hoặc cột kết quả được giải thích theo tám câu hỏi:

1. **Nó là gì?**
2. **Được tính như thế nào?**
3. **Cần dữ liệu gì?**
4. **Các giá trị/trạng thái có nghĩa gì?**
5. **Đọc riêng lẻ ra sao?**
6. **Phải kết hợp với cột nào?**
7. **Sai lầm thường gặp là gì?**
8. **Khi nào cần mở chế độ Phân tích để kiểm tra?**

## 3. Ba cấp độ sử dụng

### Cấp độ 1 — Lọc nền

Mục tiêu: biết vì sao một mã xuất hiện trong danh sách.

Tập trung vào:

- Chế độ MA hướng lên.
- Thanh khoản MA20.
- Khoảng giá so với MA20.
- Khoảng khối lượng so với MA20.
- Thứ tự MA.
- Bollinger Band Width.

### Cấp độ 2 — Đọc chế độ Tóm tắt

Mục tiêu: sắp xếp ứng viên và nhận biết trường hợp cần mở biểu đồ.

Tập trung vào sáu nhóm:

1. Nhận diện và dữ liệu cơ bản.
2. Khối lượng và xu hướng.
3. VSA tóm tắt.
4. Cấu trúc Wyckoff.
5. Cơ hội và vùng mua.
6. P&F.

### Cấp độ 3 — Truy nguyên bằng chế độ Phân tích

Mục tiêu: trả lời câu hỏi “vì sao AFL kết luận như vậy?”.

Ví dụ:

- Vì sao cột `No luc / Ket qua` là “nỗ lực cao/kết quả thấp”?
- Vì sao AFL đặt mã vào Phase C?
- Spring đã có ứng viên hay đã được xác nhận?
- Vì sao mã có vùng mua nhưng chưa có mục tiêu P&F?
- Vì sao hai mã cùng nhóm 4 nhưng mã này đứng trên mã kia?

## 4. Quy tắc không được bỏ qua

Tài liệu dùng các trạng thái Wyckoff/VSA theo nghĩa mô tả bằng chứng. Không được biến chúng thành công thức giao dịch máy móc:

- `RVOL cao` không đồng nghĩa chắc chắn có cầu mạnh.
- `No Supply` không đồng nghĩa mua ngay.
- `Phase C` không đồng nghĩa chắc chắn là tích lũy.
- `SOS` không bảo đảm breakout thành công.
- `DANG TRONG VUNG MUA` không thay cho điểm dừng lỗ và quản trị vị thế.
- Mục tiêu P&F không thay cho đánh giá cung/cầu sau khi giá rời vùng.

## 5. Khi nào nên dừng ở Tóm tắt?

Có thể dừng ở Tóm tắt khi:

- Mục tiêu chỉ là tạo danh sách ứng viên.
- Các cột pha, bằng chứng và bối cảnh nhất quán.
- Vùng mua và trạng thái giá rõ ràng.
- Không cần biết chính xác sự kiện nguồn.

Phải mở Phân tích khi:

- Kết quả trái với biểu đồ quan sát.
- Family và Evidence mâu thuẫn.
- Cơ hội mua xuất hiện nhưng không rõ nguồn gốc.
- P&F báo thiếu dữ liệu, box không hợp lệ hoặc chưa đủ cột.
- Cần kiểm tra thanh ứng viên và thanh xác nhận.

