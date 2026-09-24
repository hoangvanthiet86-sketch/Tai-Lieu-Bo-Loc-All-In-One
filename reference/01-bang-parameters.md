# Bảng tra cứu 39 Parameters

Bảng này phản ánh đúng AFL v1.7.0. `Stage 1` có 12 Parameters và có thể thay đổi tập mã Filter. `Stage 2` có 27 Parameters, chỉ thay đổi phân tích và đầu ra bổ sung.

## Stage 1 — 12 Parameters

| # | Tên trong Parameters | Kiểu / lựa chọn | Mặc định | Phạm vi; bước | Ảnh hưởng Filter |
|---:|---|---|---:|---|---|
| 1 | Toi thieu % Dong cua so voi MA20 | số | −100 | −100…100; 0,1 | Có |
| 2 | Toi da % Dong cua so voi MA20 | số | 100 | −100…100; 0,1 | Có |
| 3 | Toi thieu % Khoi luong so voi KL MA20 | số | −100 | −100…1000; 0,1 | Có |
| 4 | Toi da % Khoi luong so voi KL MA20 | số | 1000 | −100…1000; 0,1 | Có |
| 5 | Chi lay MA20>MA50>MA100 | Không / Có | Không | 2 lựa chọn | Có |
| 6 | Che do MA huong len | MA20; +MA50; +MA100; +MA200 | MA20 | 4 lựa chọn | Có |
| 7 | Che do hien thi ket qua | Tóm tắt; Phân tích; Đầy đủ | Tóm tắt | 3 lựa chọn | Không; chỉ hiển thị |
| 8 | Khoi luong MA20 toi thieu | số | 50.000 | 0…5.000.000; 1.000 | Có |
| 9 | Chu ky Bollinger | số thanh | 20 | 5…50; 1 | Có qua BBW |
| 10 | Do rong Bollinger toi da (%) | % | 5 | 0,1…50; 0,1 | Có |
| 11 | Loc mau MA20 (0-2) | mã 0–2 | 0 | 0…2; 1 | Có |
| 12 | Loc mau gia dong cua (0-6) | mã 0–6 | 0 | 0…6; 1 | Có |

### Mã lọc màu

`Loc mau MA20`: `0` không lọc; `1` và `2` đều là `Close >= MA20` trong v1.7.0.

`Loc mau gia dong cua`: `0` không lọc; `1` tăng >=6%; `2` tăng 3–<6%; `3` tăng >0–<3%; `4` tăng bất kỳ; `5` không tăng; `6` không đen, hiện tương đương mã 4.

## Stage 2A — Core VSA, 7 Parameters

| # | Tên trong Parameters | Mặc định | Phạm vi; bước | Tác dụng chính |
|---:|---|---:|---|---|
| 13 | VSA 2.1 So phien nhin lai khoi luong | 20 | 2…200; 1 | Mẫu nền RVOL, loại thanh hiện tại |
| 14 | VSA 2.2 So phien nhin lai bien do | 20 | 2…200; 1 | Mẫu nền RSpread, loại thanh hiện tại |
| 15 | VSA 2.3 Chu ky ATR | 14 | 2…200; 1 | ATR cho tiến triển, sự kiện, vùng mua, P&F |
| 16 | VSA 2.4 Nguong RVOL no luc cao | 1,80 | 0,01…10; 0,05 | Phân lớp nỗ lực cao |
| 17 | VSA 2.5 Nguong RVOL no luc thap | 0,75 | 0…10; 0,05 | Phân lớp nỗ lực thấp |
| 18 | VSA 2.6 Ket qua theo huong thap / ATR | 0,35 | 0…10; 0,05 | Ngưỡng kết quả thấp |
| 19 | VSA 2.7 Ket qua theo huong cao / ATR | 0,80 | 0,01…10; 0,05 | Ngưỡng kết quả cao |

Yêu cầu cấu hình: ngưỡng RVOL thấp < cao và ngưỡng kết quả thấp < cao.

## Stage 2B — Structure/Location, 10 Parameters

| # | Tên trong Parameters | Mặc định | Phạm vi; bước | Tác dụng chính |
|---:|---|---:|---|---|
| 20 | SL 3.1 So phien nhin lai ngan | 20 | 2…500; 1 | Vùng tham chiếu ngắn |
| 21 | SL 3.2 So phien nhin lai trung han | 60 | 2…500; 1 | Vùng tham chiếu trung hạn |
| 22 | SL 3.3 So phien nhin lai dai | 120 | 2…500; 1 | Vùng tham chiếu dài |
| 23 | SL 3.4 So phien ben trai Pivot | 3 | 1…20; 1 | Độ chặt phía trái Pivot |
| 24 | SL 3.5 So phien ben phai Pivot | 3 | 1…20; 1 | Số thanh chờ xác nhận Pivot |
| 25 | VSA 3.6 Khoang cach toi da giua 2 moc vung | 40 | 3…120; 1 | Giới hạn khoảng cách neo vùng |
| 26 | VSA 3.7 Do rong vung toi thieu / ATR | 1,50 | 0,25…20; 0,25 | Loại vùng quá hẹp |
| 27 | VSA 3.8 Tuoi vung toi thieu de vao dang Pha B | 4 | 1…40; 1 | Chuyển A → B theo tuổi |
| 28 | VSA 3.9 Bien vo hieu / Do rong vung | 0,50 | 0,10…3; 0,10 | Biên mở rộng để vô hiệu vùng |
| 29 | VSA 3.10 So phien xac nhan vo hieu | 2 | 1…10; 1 | Số Close liên tiếp ngoài biên |

## Stage 2C — Vùng mua, 4 Parameters

| # | Tên trong Parameters | Mặc định | Phạm vi; bước | Tác dụng chính |
|---:|---|---:|---|---|
| 30 | MUA 4.1 Do rong vung mua / ATR | 0,50 | 0,10…2; 0,05 | Độ rộng ZoneLow–ZoneHigh |
| 31 | MUA 4.2 Bien gia vo hieu / ATR | 0,25 | 0,05…1; 0,05 | Khoảng đệm mức vô hiệu |
| 32 | MUA 4.3 Khoang cach gan vung mua (%) | 3,00 | 0…20; 0,25 | Ranh giới nhóm 3 và 2 |
| 33 | MUA 4.4 Tuoi su kien toi da | 15 | 1…60; 1 | Độ mới tối đa của C/SOS/LPS |

## Stage 2D — P&F, 6 Parameters

| # | Tên trong Parameters | Kiểu / lựa chọn | Mặc định | Phạm vi; bước |
|---:|---|---|---:|---|
| 34 | PNF 5.1 Cach tinh kich thuoc o | ATR cố định; % giá; giá trị cố định | ATR cố định | 3 lựa chọn |
| 35 | PNF 5.2 Kich thuoc o / ATR | số | 0,50 | 0,10…3; 0,05 |
| 36 | PNF 5.3 Kich thuoc o (%) | % | 1,00 | 0,10…10; 0,10 |
| 37 | PNF 5.4 Kich thuoc o co dinh | đơn vị giá | 0,10 | 0,01…100; 0,01 |
| 38 | PNF 5.5 So o dao chieu | số ô | 3 | 1…5; 1 |
| 39 | PNF 5.6 So cot toi thieu | số cột X/O | 5 | 5…30; 1 |

## Kiểm tra nhanh sau khi đổi Parameters

- Thay #1–#6 hoặc #8–#12 có thể làm đổi số mã.
- Thay #7 không được làm đổi số mã; chỉ đổi số cột.
- Thay #13–#39 không được làm đổi tập mã Filter.
- Khi so hai lần chạy, phải giữ nguyên database, thời điểm dữ liệu, periodicity, Apply to và Range.

