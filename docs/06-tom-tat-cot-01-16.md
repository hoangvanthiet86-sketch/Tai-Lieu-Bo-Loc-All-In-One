# 06. Chế độ Tóm tắt — Cột 01 đến 16

Nhóm đầu trả lời ba câu hỏi:

1. Mã nào đang được xem xét?
2. Thanh khoản và biến động hiện tại thế nào?
3. Xu hướng theo MA và vị trí dài hạn ra sao?

## Cột 01 — Mã CP

Mã chứng khoán do cơ sở dữ liệu AmiBroker cung cấp qua `Name()`.

Không tham gia tính điểm hoặc Filter; đây là cột nhận diện.

## Cột 02 — Ngành

AFL ưu tiên bốn ký tự đầu của tên Industry trong FData nếu bốn ký tự đó chuyển được thành mã số dương. Nếu không, AFL dùng `IcbID(0)`.

Ô trống không làm mã bị loại. Nó thường phản ánh dữ liệu phân ngành chưa được gán.

## Cột 03 — Ngày

Ngày/giờ của thanh cuối trong Analysis Range.

Đây là cột bắt buộc phải kiểm tra khi đối chiếu báo cáo. Hai dòng cùng mã nhưng khác ngày không phải cùng một ảnh chụp thị trường.

## Cột 04 — Đóng cửa

Giá `Close` của thanh cuối trong Range.

Giá này được dùng trong nhiều công thức:

- % thay đổi.
- Khoảng cách tới MA.
- Vị trí đóng cửa.
- Vị trí trong vùng.
- Trạng thái so với vùng mua.
- Mục tiêu P&F kế tiếp.

## Cột 05 — % TD

### Công thức

```text
% TD = 100 × (Close hiện tại − Close trước) / Close trước
```

### Cách đọc

- Dương: giá tăng so với thanh trước.
- Âm: giá giảm.
- 0: không đổi.

Không dùng riêng `% TD` để kết luận sức mạnh. Một phiên tăng mạnh có thể là SOS, Buying Climax hoặc Upthrust tùy vị trí và phản ứng sau đó.

## Cột 06 — KL MA20

### Công thức

```text
KL MA20 = MA(Volume, 20)
```

Khác với mẫu số RVOL, KL MA20 truyền thống của Stage 1 có chứa Volume hiện tại trong cửa sổ 20 thanh.

### Công dụng

- Kiểm tra thanh khoản bình quân.
- Là điều kiện Filter với `Khoi luong MA20 toi thieu`.
- Là mẫu số của `% Khoi luong so voi KL MA20` trong Stage 1.

## Cột 07 — RVOL

### Công thức

```text
RVOL = Volume hiện tại / trung bình Volume của N thanh trước
```

`N` do `VSA 2.1` quy định, mặc định 20. Thanh hiện tại bị loại khỏi mẫu số.

### Cách đọc nhanh

- `1,00`: bằng trung bình quá khứ.
- `1,80`: cao hơn khoảng 80%.
- `0,50`: bằng một nửa.

### Cách dùng đúng

RVOL đo **mức nỗ lực giao dịch**, không chỉ ra bên mua hay bên bán thắng. Luôn đọc cùng:

- Hướng giá.
- RSpread.
- Vị trí đóng cửa.
- Tiến triển theo hướng/ATR.

## Cột 08 — Trạng thái khối lượng

| Điều kiện RVOL | Trạng thái |
|---|---|
| Không hợp lệ | DỮ LIỆU KHÔNG ĐỦ |
| `< 0,50` | CỰC THẤP |
| `0,50–0,75` | THẤP |
| `> 0,75 và < 1,25` | BÌNH THƯỜNG |
| `1,25 đến < 1,80` | CAO |
| `1,80 đến < 2,50` | RẤT CAO |
| `>= 2,50` | CỰC CAO |

Các dải này là dải mô tả cố định. Hai Parameters “nỗ lực cao/thấp” dùng cho Effort/Result có thể được đặt khác với ranh giới mô tả trên.

## Cột 09 — RSI(14)

AFL dùng hàm `RSI(14)` của AmiBroker.

### Cách sử dụng trong tài liệu này

RSI chỉ là mô tả động lượng tương đối:

- Không coi RSI cao là lệnh bán tự động.
- Không coi RSI thấp là lệnh mua tự động.
- Ưu tiên cấu trúc, vị trí và volume hơn ngưỡng RSI máy móc.

RSI không tham gia trực tiếp vào `Filter` của v1.7.0.

## Cột 10 — % Đóng cửa so với MA20

```text
100 × (Close − MA20) / MA20
```

Cột này vừa hiển thị vừa tham gia Filter theo khoảng tối thiểu/tối đa.

Ví dụ `4,5` nghĩa là Close cao hơn MA20 4,5%.

## Cột 11 — % Đóng cửa so với MA50

```text
100 × (Close − MA50) / MA50
```

Cột này không có ngưỡng lọc riêng trong Parameters hiện tại, nhưng giúp phân biệt:

- Giá chỉ vừa hồi trên MA20.
- Giá đã nằm trên cả MA20 và MA50.
- Giá đang dưới MA50 dù MA20 vừa hướng lên.

## Cột 12 — Thứ tự MA

Hiển thị `1` khi:

```text
MA20 > MA50 > MA100
```

Nếu không đạt, ô để trống.

Khi `Chi lay MA20>MA50>MA100 = Co`, cột này phải là 1 đối với mọi mã trong bảng.

## Cột 13 — MA200 dốc lên

Hiển thị `1` khi:

```text
MA200 hiện tại > MA200 thanh trước
```

Nếu chọn chế độ MA đến MA200, mọi mã lọt Filter phải có cột này bằng 1.

## Cột 14 — Tăng tuần

Được tính trên dữ liệu tuần, sau đó mở rộng về thanh đang hiển thị.

Điều kiện đồng thời:

```text
MA10W > MA20W > MA40W
CloseW > MA20W
MA10W > MA10W của 2 tuần trước
MA20W > MA20W của 2 tuần trước
CloseW > CloseW của 4 tuần trước
LowW > LowW của 4 tuần trước
```

Hiển thị `1` khi đạt, để trống khi không đạt.

Cột này không nằm trong Filter cuối cùng; nó là thông tin bối cảnh.

## Cột 15 — % Đóng cửa so với Đỉnh 52T

### Mốc tham chiếu

AFL lấy đỉnh cao nhất của 52 tuần trước, loại trừ tuần hiện tại.

### Công thức

```text
100 × (Close − High52WExclCurrentWeek) / High52WExclCurrentWeek
```

Thông thường giá trị bằng hoặc dưới 0:

- `0`: sát đỉnh tham chiếu.
- `-10`: thấp hơn đỉnh khoảng 10%.
- Dương: giá hiện tại đã vượt đỉnh 52 tuần trước.

## Cột 16 — BBW (%)

### Công thức

```text
Mid   = MA(Close, bbPeriod)
Upper = Mid + 2 × StDev
Lower = Mid − 2 × StDev
BBW   = 100 × (Upper − Lower) / Mid
```

### Quan hệ với Filter

Mã chỉ lọt khi:

```text
BBW <= Do rong Bollinger toi da
```

### Ý nghĩa

BBW thấp mô tả biến động co hẹp, nhưng không xác định hướng phá vỡ. Phải đọc tiếp VSA và cấu trúc.

## Checklist nhóm cột 01–16

- [ ] Ngày dữ liệu đúng.
- [ ] KL MA20 phù hợp quy mô giao dịch.
- [ ] RVOL được đọc cùng trạng thái giá.
- [ ] Chế độ MA và cột thứ tự MA không bị nhầm lẫn.
- [ ] Khoảng cách tới MA không quá xa so với chiến lược.
- [ ] BBW thấp không được hiểu là tín hiệu tăng chắc chắn.

