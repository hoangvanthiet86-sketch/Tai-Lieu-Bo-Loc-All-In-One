# Từ điển 48 cột bổ sung của chế độ Phân tích

Chế độ `Phan tich` có 87 cột: 39 cột Tóm tắt ở trước và 48 cột dưới đây ở sau.

| # toàn bảng | Tên cột | Loại | Mục đích chẩn đoán |
|---:|---|---|---|
| 40 | MA20 | giá | Giá trị MA20 thô |
| 41 | MA50 | giá | Giá trị MA50 thô |
| 42 | MA100 | giá | Giá trị MA100 thô |
| 43 | % Dong cua so voi Cao nhat | % | Thực tế `100×Close/High` của thanh hiện tại |
| 44 | % Dong cua so voi Thap nhat | % | `100×(Close−Low)/Low` |
| 45 | % Khoi luong so voi KL MA20 | % | Chênh Volume với MA20 Volume |
| 46 | RSpread | tỷ lệ | Biên hiện tại / trung bình N biên trước |
| 47 | Tien trien theo huong ATR | ATR | `(Close−Close trước)/ATR trước` |
| 48 | Cong VSA | 1/0 | Stage 1 cho phép phân tích sâu |
| 49 | VSA loi san sang | 1/0 | Các chuỗi lõi VSA hợp lệ |
| 50 | Ung vien No Demand | chữ | Up-close, volume thấp hơn 2 thanh trước, spread hẹp |
| 51 | Ung vien No Supply | chữ | Down-close, volume thấp hơn 2 thanh trước, spread hẹp |
| 52 | Phan ung No Demand | chữ | Phản ứng thanh kế tiếp |
| 53 | Phan ung No Supply | chữ | Phản ứng thanh kế tiếp |
| 54 | No Demand da xac nhan | 1/0 | Cờ xác nhận trên thanh phản ứng |
| 55 | No Supply da xac nhan | 1/0 | Cờ xác nhận trên thanh phản ứng |
| 56 | No luc cao trao | chữ | RSpread >=1,60 và RVOL >=1,80 |
| 57 | Hap thu | chữ | RVOL >=1,80, tiến triển tuyệt đối <=0,35 ATR |
| 58 | Ap luc giam | 1/0 | Low thấp hơn hoặc Close giảm |
| 59 | Ap luc tang | 1/0 | High cao hơn hoặc Close tăng |
| 60 | Khoi luong chan da | chữ | RVOL cao, áp lực giảm, Close Position >=0,50 |
| 61 | Phan ung chan da | chữ | Thanh kế tiếp giữ Low và cải thiện Close |
| 62 | Phan ung chan da da xac nhan | 1/0 | Cờ xác nhận phản ứng chặn đà |
| 63 | Hanh vi san sang | 1/0 | Tầng hành vi đủ dữ liệu |
| 64 | Phia hinh thanh vung | chữ | Vùng suy ra từ cận dưới/cận trên hoặc mơ hồ |
| 65 | Can duoi vung | giá | RangeLow được chọn |
| 66 | Can tren vung | giá | RangeHigh được chọn |
| 67 | Tuoi vung | thanh | Số thanh từ khởi đầu vùng |
| 68 | Xu huong truoc | chữ | Tăng, giảm, hỗn hợp hoặc thiếu |
| 69 | Cau truc san sang | 1/0 | Vùng đủ điều kiện cho sự kiện |
| 70 | Spring / Shakeout | chữ | Phân loại xuyên/thu hồi cận dưới |
| 71 | Spring da xac nhan | 1/0 | Phản ứng tăng sau thanh Spring |
| 72 | Kiem tra cung da xac nhan | 1/0 | No Supply/Test gần cận dưới được xác nhận |
| 73 | Upthrust da xac nhan | 1/0 | Phản ứng giảm sau xuyên cận trên |
| 74 | Boi canh dang UTAD | 1/0 | Upthrust + vùng trên + pha/hướng phù hợp |
| 75 | SOS | 1/0 | Phá cận trên với spread, volume, close mạnh |
| 76 | LPS da xac nhan | 1/0 | Retest sau SOS được xác nhận |
| 77 | SOW | 1/0 | Phá cận dưới với spread/volume mạnh |
| 78 | LPSY da xac nhan | 1/0 | Retest sau SOW được xác nhận |
| 79 | Phan ung hap thu tang | 1/0 | Phản ứng tăng sau hấp thụ |
| 80 | Phan ung hap thu giam | 1/0 | Phản ứng giảm sau hấp thụ |
| 81 | Su kien san sang | 1/0 | Tầng sự kiện đủ dữ liệu |
| 82 | Phan tich cuoi san sang | 1/0 | Cổng cuối của Phase/Family/Buy |
| 83 | PNF so phien khoang dem | thanh | Độ dài dữ liệu đầu vào P&F |
| 84 | PNF kich thuoc o | giá | Box cố định của phép đếm |
| 85 | PNF dong dem | giá | CountLine đã làm tròn và chặn đáy |
| 86 | PNF day thuc te | giá | Min của RangeLow và Low trong khoảng |
| 87 | PNF tuoi bien phai | thanh | Khoảng cách từ hiện tại tới biên phải |

Xem giải thích: [cột 40–49](../docs/10-phan-tich-cot-40-49.md), [cột 50–69](../docs/11-phan-tich-cot-50-69.md), [cột 70–87](../docs/12-phan-tich-cot-70-87.md).

