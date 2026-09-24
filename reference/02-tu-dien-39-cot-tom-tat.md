# Từ điển 39 cột Tóm tắt

| # | Tên cột | Loại | Nguồn / cách đọc ngắn |
|---:|---|---|---|
| 1 | Ma CP | chữ | Mã AmiBroker `Name()` |
| 2 | Nganh | chữ | Mã ngành từ database |
| 3 | Ngay | ngày giờ | Thanh cuối trong Analysis Range |
| 4 | Dong cua | giá | Close |
| 5 | % TD | % | `100 × (Close/Close trước − 1)` |
| 6 | KL MA20 | khối lượng | `MA(Volume,20)`; có gồm thanh hiện tại |
| 7 | RVOL | tỷ lệ | Volume / trung bình N thanh trước, loại hiện tại |
| 8 | Trang thai khoi luong | chữ | Phân lớp RVOL |
| 9 | RSI(14) | điểm | RSI chuẩn 14 thanh |
| 10 | % Dong cua so voi MA20 | % | `100 × (Close−MA20)/MA20` |
| 11 | % Dong cua so voi MA50 | % | `100 × (Close−MA50)/MA50` |
| 12 | Thu tu MA | 1/trống | 1 khi `MA20>MA50>MA100` |
| 13 | MA200 doc len | 1/trống | 1 khi MA200 hiện tại > thanh trước |
| 14 | Tang tuan | 1/trống | Cấu trúc xu hướng tuần đạt toàn bộ điều kiện |
| 15 | % Dong cua so voi Dinh 52T | % | Khoảng cách Close với đỉnh 52 tuần, loại tuần hiện tại |
| 16 | BBW (%) | % | `100 × (Upper−Lower)/Mid`, Bollinger ±2 SD |
| 17 | Trang thai bien do | chữ | Phân lớp RSpread |
| 18 | Vi tri dong cua % | % | `100 × (Close−Low)/(High−Low)`; nến zero-range = 50% |
| 19 | Trang thai dong cua | chữ | Phân lớp vị trí đóng cửa |
| 20 | No luc / Ket qua | chữ | RVOL so với trị tuyệt đối tiến triển/ATR |
| 21 | Trang thai vung | chữ | Không có, hoạt động, Phase E, mơ hồ hoặc vô hiệu |
| 22 | Vi tri trong vung % | % | `100 × (Close−RangeLow)/(RangeHigh−RangeLow)` |
| 23 | Pha Wyckoff | chữ | A, B, ứng viên C, C, D, E hoặc vô hiệu |
| 24 | Phat trien cau truc | chữ | Mức phát triển giải thích pha |
| 25 | Gia thuyet ho | chữ | Accumulation/Distribution/Reaccumulation/Redistribution… |
| 26 | Bang chung | chữ | Chỉ tăng, chỉ giảm, hỗn hợp hoặc thiếu |
| 27 | Boi canh xu huong | chữ | Giả thuyết tăng, giảm, hỗn hợp hoặc chưa xác định |
| 28 | Co hoi mua | chữ | Setup 0–5 dựa trên Phase C/D/E |
| 29 | Trang thai gia mua | chữ | Nhóm 0–4 theo vị trí Close với vùng mua |
| 30 | Vung mua tu | giá | ZoneLow theo setup |
| 31 | Vung mua den | giá | ZoneHigh theo setup và ATR |
| 32 | Gia vo hieu tham chieu | giá | Mức mất hiệu lực cấu trúc tham chiếu |
| 33 | Uu tien mua | số | Khóa xếp hạng: nhóm + độ gần + độ mới |
| 34 | Trang thai PNF | chữ | Không áp dụng, lỗi, tạm tính, khóa hoặc hoàn thành |
| 35 | PNF so cot dem ngang | số | Tổng cột X/O trong đoạn nguyên nhân |
| 36 | Muc tieu PNF bao thu | giá | `ActualLow + N×Box×Reversal` |
| 37 | Muc tieu PNF co so | giá | `MidAnchor + N×Box×Reversal` |
| 38 | Muc tieu PNF mo rong | giá | `CountLine + N×Box×Reversal` |
| 39 | Du dia PNF ke tiep % | % | Dư địa tới mục tiêu riêng biệt đầu tiên > Close |

## Nhóm chức năng

- Cột 1–6: nhận diện và thanh khoản nền.
- Cột 7–20: xu hướng và VSA tóm tắt.
- Cột 21–27: vùng, pha, Family và Evidence.
- Cột 28–33: setup, vùng mua và xếp hạng.
- Cột 34–39: P&F.

Chi tiết đầy đủ: [cột 01–16](../docs/06-tom-tat-cot-01-16.md), [cột 17–27](../docs/07-tom-tat-cot-17-27.md), [cột 28–39](../docs/08-tom-tat-cot-28-39.md).

