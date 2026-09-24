# 03. Thiết lập AmiBroker

## 1. Nạp công thức

1. Mở AmiBroker.
2. Mở **Analysis**.
3. Chọn tệp AFL v1.7.0.
4. Mở Formula Editor và chạy **Verify Formula**.
5. Nếu Verify không báo lỗi, quay lại Analysis và mở **Parameters**.

Verify Formula chỉ kiểm tra việc biên dịch công thức. Một AFL vẫn có thể gặp vấn đề khi Explore nếu dữ liệu, Periodicity hoặc phạm vi chạy không phù hợp. Vì vậy phải kiểm tra cả Verify và chạy thực tế.

## 2. Apply to

`Apply to` quyết định tập mã AmiBroker sẽ duyệt:

- All symbols.
- Current.
- Filter/watchlist do người dùng chọn.

Stage 1 vẫn quyết định mã nào xuất hiện, nhưng `Apply to` quyết định những mã nào được đưa vào quá trình đánh giá.

## 3. Range

Khuyến nghị vận hành hằng ngày:

- Dùng thanh gần nhất phù hợp với dữ liệu đã cập nhật.
- Bảo đảm thanh cuối không phải thanh tương lai rỗng.
- Khi đối chiếu hai AFL, dùng cùng ngày bắt đầu/kết thúc và cùng số thanh gần nhất.

`Status("lastbarinrange")` khiến mỗi mã chỉ xuất hiện tại thanh cuối của Range. Nếu Range kết thúc ở một ngày cũ, kết quả là ảnh chụp tại ngày đó.

## 4. Periodicity

AFL không khóa Daily. Tuy nhiên:

- Daily là chuẩn hồi quy chính.
- MA20 trên Daily khác MA20 trên Weekly.
- `Lookback 20` trên Weekly là 20 tuần.
- Pivot và tuổi sự kiện đều tính bằng số thanh của Periodicity.

Khi chạy khung khác Daily, phải diễn giải kết quả theo chính khung đó.

## 5. Cấu hình khởi đầu an toàn

Trong lần đầu:

| Thiết lập | Giá trị khuyến nghị ban đầu |
|---|---|
| Chế độ hiển thị | `Tom tat` |
| Chế độ MA hướng lên | `MA20` |
| Chỉ lấy MA20>MA50>MA100 | `Khong` |
| Các ngưỡng VSA | Giữ mặc định |
| Parameters Structure/Location | Giữ mặc định |
| Parameters vùng mua | Giữ mặc định |
| P&F | ATR cố định, 0.50 ATR, đảo chiều 3 ô, tối thiểu 5 cột |

Mục đích là xác nhận AFL chạy đúng trước khi siết điều kiện.

## 6. Chuyển chế độ hiển thị

### Tóm tắt

Dùng để:

- Lọc hằng ngày.
- Xem nhanh thứ tự ưu tiên.
- Tìm mã cần mở biểu đồ.

### Phân tích

Dùng khi:

- Cần biết công thức nguồn.
- Kết quả không khớp quan sát.
- Cần kiểm tra ứng viên/xác nhận.
- P&F không có mục tiêu.

### Đầy đủ

Dùng để:

- Kiểm tra cửa sổ ngắn/trung/dài.
- Kiểm tra Pivot và ngày xác nhận.
- Đối chiếu toàn bộ pipeline.

Không nên dùng 123 cột làm màn hình vận hành mặc định vì khó đọc và dễ mất trọng tâm.

## 7. Parameters được AmiBroker ghi nhớ

AmiBroker có thể giữ Parameters từ lần chạy trước. Khi nhận tệp mới:

1. Mở Parameters.
2. Kiểm tra chế độ MA, Display và P&F.
3. Nếu kết quả khác dự kiến, chụp hoặc ghi lại toàn bộ Parameters.
4. Không đối chiếu hai báo cáo khi chưa xác nhận Parameters giống nhau.

## 8. Checklist trước khi Explore

- [ ] Dữ liệu đã cập nhật đến đúng ngày.
- [ ] Apply to đúng thị trường/watchlist.
- [ ] Range kết thúc đúng thanh cần phân tích.
- [ ] Periodicity đúng.
- [ ] Parameters đúng cấu hình dự kiến.
- [ ] Verify Formula không báo lỗi.
- [ ] Chế độ hiển thị phù hợp mục tiêu.

