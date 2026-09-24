# 14. Cấu hình mẫu và cách tinh chỉnh

Các cấu hình dưới đây là điểm xuất phát để so sánh, không phải bộ tham số tối ưu hay khuyến nghị đầu tư. Hãy lưu tên cấu hình, ngày chạy, thị trường, khung thời gian và chỉ đổi một nhóm biến mỗi lần.

## 1. Hồ sơ nền để kiểm chứng

Trước khi tối ưu, tạo một hồ sơ `BASE_v1.7` dùng đúng mặc định AFL:

| Nhóm | Giá trị nền |
|---|---|
| MA hướng lên | MA20 |
| Thứ tự MA | Không |
| KL MA20 tối thiểu | 50.000 |
| Close/MA20 | −100% đến +100% |
| Volume/KL MA20 | −100% đến +1000% |
| BBW tối đa | 5% |
| VSA Volume/Spread lookback | 20 / 20 |
| ATR | 14 |
| Range Pivot trái/phải | 3 / 3 |
| Vùng mua / ATR | 0,50 |
| Gần vùng mua | 3% |
| Tuổi sự kiện | 15 thanh |
| P&F | ATR cố định; 0,50 ATR; đảo chiều 3; tối thiểu 5 cột |

Giữ hồ sơ nền để có thể quay lại khi một thay đổi tạo kết quả khó giải thích.

## 2. Ba mức lọc Stage 1

### Mức rộng — tìm sớm

| Parameter | Gợi ý bắt đầu |
|---|---|
| MA hướng lên | MA20 |
| Thứ tự MA | Không |
| Close/MA20 | −5% đến +10% |
| Lọc màu MA20 | 0 |
| Lọc màu Close | 0 |
| BBW tối đa | 8–10% |

Ưu điểm: thấy nhiều cấu trúc mới. Nhược điểm: nhiều mã cần loại thủ công, có thể còn dưới MA20 hoặc MA chưa sắp xếp.

### Mức cân bằng — danh sách theo dõi hằng ngày

| Parameter | Gợi ý bắt đầu |
|---|---|
| MA hướng lên | MA20 + MA50 |
| Thứ tự MA | tùy mục tiêu; thử Không trước |
| Close/MA20 | −2% đến +7% |
| Lọc màu MA20 | 1 nếu muốn Close ≥ MA20 |
| BBW tối đa | 5–7% |

Đây là cấu hình phù hợp để bắt đầu đánh giá tác động của Wyckoff/VSA mà không siết xu hướng dài hạn quá sớm.

### Mức chặt — ưu tiên xu hướng trưởng thành

| Parameter | Gợi ý bắt đầu |
|---|---|
| MA hướng lên | MA20 + MA50 + MA100 hoặc thêm MA200 |
| Thứ tự MA | Có |
| Close/MA20 | 0% đến +5% |
| Lọc màu MA20 | 1 |
| BBW tối đa | 5% hoặc thấp hơn |

Kết quả ít hơn nhưng có thể bỏ lỡ các mã vừa chuyển pha trước khi MA dài hạn quay lên.

## 3. Chọn ngưỡng thanh khoản

Không sao chép máy móc `50.000` giữa các nguồn dữ liệu. Cần xác nhận:

- Volume là cổ phiếu, lô hay đơn vị khác;
- thị trường Việt Nam hay Mỹ;
- quy mô vị thế dự kiến;
- cổ phiếu thường hay ETF/ADR;
- dữ liệu có điều chỉnh lịch sử hay không.

Một cách thực hành:

1. Chạy với ngưỡng nền.
2. Mở 10–20 mã ở cuối phân bố thanh khoản.
3. Ước lượng khả năng vào/ra theo quy mô vị thế thực tế.
4. Nâng ngưỡng cho tới khi loại được nhóm không thể giao dịch mà không làm mất quá nhiều ứng viên.

## 4. Khung thời gian không bị khóa Daily

v1.7.0 dùng periodicity đang chọn trong Analysis cho phần lớn phép tính. Vì vậy `20 thanh` mang nghĩa khác nhau:

| Khung | 20 thanh xấp xỉ |
|---|---|
| Daily | 20 phiên giao dịch |
| Weekly | 20 tuần |
| 60 phút | 20 thanh 60 phút |

Các ngưỡng theo “số thanh” gồm VSA lookback, ATR, tuổi sự kiện, tuổi vùng và cửa sổ retest. Khi đổi khung, không mặc định giữ nguyên ý nghĩa thời gian.

Riêng xu hướng tuần của Stage 1 vẫn được tính bằng dữ liệu Weekly nội bộ. Trên khung không phải Daily, cần kiểm thử đặc biệt việc căn chỉnh dữ liệu tuần với periodicity đang Explore.

## 5. Tinh chỉnh VSA

### Nhiễu quá nhiều

- tăng lookback Volume/Spread;
- tăng ngưỡng RVOL nỗ lực cao;
- không hạ ngưỡng kết quả thấp chỉ để tạo thêm hấp thụ;
- giữ quan sát phản ứng xác nhận.

### Phản ứng quá chậm

- giảm vừa phải lookback;
- giảm ATR period nếu phù hợp khung;
- đối chiếu lại trên nhiều mã để tránh tối ưu theo một ví dụ.

Hai cặp ngưỡng phải luôn hợp lệ:

```text
RVOL thấp < RVOL cao
Kết quả thấp/ATR < Kết quả cao/ATR
```

## 6. Tinh chỉnh vùng cấu trúc

| Hiện tượng | Thay đổi có thể thử | Đổi lại |
|---|---|---|
| Quá nhiều Pivot nhỏ | Tăng Pivot trái/phải | Trễ hơn |
| Ít vùng được tạo | Giảm Pivot hoặc giảm độ rộng vùng tối thiểu | Nhiễu hơn |
| Vùng quá ngắn | Tăng tuổi tối thiểu Phase B | Chậm chuyển pha |
| Vùng cũ tồn tại quá lâu | Giảm biên vô hiệu hoặc số phiên xác nhận | Dễ thay vùng sớm |
| Hai mốc vùng quá xa | Giảm khoảng cách tối đa | Bỏ vùng dài |

Không đổi đồng thời Pivot, khoảng cách mốc và độ rộng/ATR; nếu kết quả biến động mạnh sẽ không biết nguyên nhân.

## 7. Tinh chỉnh vùng mua

### Vùng quá rộng

Giảm `Độ rộng vùng mua/ATR`. Kết quả: ít mã nhóm 4 hơn, vị trí vào yêu cầu chính xác hơn.

### Nhiều mã vừa vượt vùng đã bị nhóm 2

Tăng nhẹ `% gần vùng mua`, nhưng không dùng ngưỡng lớn để hợp thức hóa mua đuổi. Khoảng 3% chỉ là mặc định, không phải chuẩn chung cho mọi độ biến động.

### Setup tồn tại quá lâu

Giảm `Tuổi sự kiện tối đa`. Điều này không xóa lịch sử pha; nó chỉ ngăn sự kiện cũ tạo proposal hiện tại.

## 8. Chọn chế độ Box P&F

### ATR cố định

Phù hợp khi muốn Box thích nghi theo biến động từng mã. Đây là lựa chọn nền hợp lý để so nhiều cổ phiếu có mức giá và biến động khác nhau.

### Phần trăm giá

Phù hợp khi muốn Box tỷ lệ với giá. Cần kiểm tra cổ phiếu biến động rất cao hoặc rất thấp vì cùng 1% có thể tạo số cột khác đáng kể.

### Giá trị cố định

Chỉ nên dùng khi tập mã có cùng thang giá hoặc quy tắc box đã được xác định trước. Một Box `0,10` không có ý nghĩa tương đương với mã giá 5 và mã giá 500.

## 9. Reversal và số cột tối thiểu

- Reversal 3 là điểm xuất phát phổ biến của phiên bản này.
- Tăng Reversal thường làm ít lần đảo cột hơn.
- Giảm Reversal tạo nhiều cột hơn nhưng nhạy với nhiễu.
- Số cột tối thiểu là cổng chất lượng; không nên hạ chỉ vì một báo cáo không có mục tiêu.

Nếu hầu hết mọi mã đều báo thiếu cột, ưu tiên kiểm tra Box và khoảng đếm trước khi đổi ngưỡng tối thiểu.

## 10. Quy trình so sánh cấu hình

1. Cố định database, ngày, periodicity, Apply to và Range.
2. Chạy hồ sơ nền, lưu ảnh hoặc xuất kết quả.
3. Chỉ đổi một Parameter hoặc một nhóm logic liên quan.
4. So số mã Stage 1, phân bố nhóm 0–4 và trạng thái P&F.
5. Mở biểu đồ các mã xuất hiện/mất đi ở ranh giới.
6. Ghi kết luận rồi mới đổi tham số tiếp theo.

Không đánh giá một cấu hình chỉ bằng số lượng mã. Cần đánh giá tính giải thích được và mức nhất quán với biểu đồ.

