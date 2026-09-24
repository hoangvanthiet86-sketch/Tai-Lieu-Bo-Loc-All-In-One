# 10. Chế độ Phân tích — Cột 40 đến 49

Chế độ `Phan tich` giữ nguyên 39 cột Tóm tắt rồi nối thêm 48 cột chẩn đoán. Nhóm 40–49 trả lời hai câu hỏi đầu tiên: số liệu nền nào tạo ra kết quả Stage 1, và lõi VSA có đủ dữ liệu để tính hay không.

## Cách dùng nhóm cột này

Không đọc các cột 40–49 như một bộ tín hiệu mua bán độc lập. Hãy dùng chúng để truy nguyên:

- vì sao mã vượt điều kiện MA và thanh khoản;
- thân nến đang nằm ở đâu trong biên ngày;
- khối lượng và biên độ đang lớn hay nhỏ so với quá khứ;
- biến `Tiến triển theo hướng ATR` có tương xứng với nỗ lực không;
- lõi VSA có thực sự sẵn sàng hay chỉ đang trả trạng thái thiếu dữ liệu.

## Cột 40 — MA20

```text
MA20 = trung bình đơn giản của Close trong 20 thanh gần nhất
```

Đây là giá trị đường MA20, không phải độ dốc. Điều kiện “MA20 hướng lên” được kiểm tra riêng:

```text
MA20 hiện tại > MA20 của thanh liền trước
```

Màu chữ của cột đi theo lựa chọn `Mau MA20`, nhưng màu không thay đổi phép lọc.

## Cột 41 — MA50

```text
MA50 = MA(Close, 50)
```

Cột này giúp kiểm tra trực tiếp khoảng cách giữa giá, MA20 và MA50. Chế độ `MA20 + MA50` chỉ yêu cầu cả hai đường có giá trị hiện tại lớn hơn giá trị thanh trước; không yêu cầu MA20 nằm trên MA50.

## Cột 42 — MA100

```text
MA100 = MA(Close, 100)
```

Trong chế độ `MA20 + MA50 + MA100`, cả ba độ dốc phải dương. Thứ tự `MA20 > MA50 > MA100` là điều kiện khác, hiển thị ở cột `Thứ tự MA` và được điều khiển bởi Parameter riêng.

## Cột 43 — % Đóng cửa so với Cao nhất

Tên cột được giữ theo AFL, nhưng công thức thực tế là:

```text
100 × Close / High
```

Do đó:

- `100` nghĩa là Close bằng High của chính thanh hiện tại;
- `99` nghĩa là Close bằng 99% giá High;
- đây **không phải** “Close thấp hơn High bao nhiêu phần trăm”.

Ví dụ `High = 50`, `Close = 49` thì kết quả là `98`, không phải `-2`.

Chỉ số này khác `Vị trí đóng cửa %` ở cột 18. Cột 18 chuẩn hóa Close trên toàn bộ biên Low–High của thanh, còn cột 43 lấy tỷ lệ trực tiếp Close/High.

## Cột 44 — % Đóng cửa so với Thấp nhất

Công thức thực tế:

```text
100 × (Close − Low) / Low
```

Nó cho biết Close cao hơn Low của chính thanh hiện tại bao nhiêu phần trăm. Đây cũng không phải vị trí Close trên biên nến.

Ví dụ `Low = 40`, `Close = 42`:

```text
100 × (42 − 40) / 40 = 5%
```

## Cột 45 — % Khối lượng so với KL MA20

```text
100 × (Volume − MA20(Volume)) / MA20(Volume)
```

- `0%`: Volume bằng KL MA20.
- `50%`: Volume cao hơn KL MA20 một nửa.
- `−30%`: Volume thấp hơn KL MA20 30%.

Cột này thuộc Stage 1 và dùng MA20 có bao gồm thanh hiện tại. Nó khác `RVOL`: RVOL so Volume hiện tại với trung bình của đúng N thanh hợp lệ **trước đó**, loại trừ thanh hiện tại.

## Cột 46 — RSpread

```text
RSpread = (High − Low) hiện tại
          / trung bình (High − Low) của N thanh hợp lệ trước đó
```

Trong đó N là `So phien trung binh nen VSA`. Thanh hiện tại bị loại khỏi mẫu nền.

| RSpread | Phân loại ở cột 17 |
|---:|---|
| thiếu dữ liệu | DỮ LIỆU KHÔNG ĐỦ |
| `< 0,60` | RẤT HẸP |
| `< 0,80` | HẸP |
| `< 1,20` | BÌNH THƯỜNG |
| `< 1,60` | RỘNG |
| `>= 1,60` | RẤT RỘNG |

RSpread lớn không tự động là tăng hoặc giảm. Cần đọc thêm hướng giá, vị trí đóng cửa, RVOL và phản ứng kế tiếp.

## Cột 47 — Tiến triển theo hướng ATR

```text
DirectionalProgress = (Close hiện tại − Close trước) / ATR trước
```

ATR dùng giá trị của thanh trước để tránh lấy biến động hiện tại làm mẫu số cho chính nó.

- số dương: Close tiến lên;
- số âm: Close lùi xuống;
- trị tuyệt đối lớn: dịch chuyển lớn so với biến động nền;
- gần 0: nỗ lực giá theo hướng đóng cửa nhỏ.

Trong đánh giá nỗ lực/kết quả, AFL dùng trị tuyệt đối của đại lượng này. Vì vậy cột 47 phải được đọc để biết hướng; cột `Nỗ lực / Kết quả` chỉ cho biết mức kết quả cao, bình thường hay thấp.

## Cột 48 — Cổng VSA

Cột số nhị phân:

- `1`: mã đã qua Stage 1 tại thanh đang Explore; Stage 2 được phép chạy.
- `0`: Stage 2 không chạy cho thanh đó.

Nguyên tắc kiến trúc là Wyckoff/VSA chỉ phân tích sâu các mã đã lọt bộ lọc gốc. Cổng này giúp xác minh nguyên tắc đó trong chế độ Phân tích.

## Cột 49 — VSA lõi sẵn sàng

- `1`: RVOL, RSpread, Close Position, ATR và các thành phần lõi cần thiết đã sẵn sàng.
- `0`: ít nhất một chuỗi lõi chưa đủ dữ liệu hoặc không hợp lệ.

`Cổng VSA = 1` chưa đảm bảo `VSA lõi sẵn sàng = 1`. Một mã có thể qua Stage 1 nhưng vẫn thiếu lịch sử hợp lệ cho cửa sổ VSA/ATR.

## Đối chiếu nhanh khi kết quả bất thường

| Hiện tượng | Cột cần xem | Cách hiểu |
|---|---|---|
| Qua chế độ MA nhưng nghi độ dốc sai | 40–42 và đồ thị | So giá trị hiện tại với thanh trước, không chỉ so thứ tự các MA |
| % so với High gần 100 ở hầu hết mã | 43 | Đây là `100 × Close/High`, hoàn toàn có thể gần 100 |
| KL so MA20 cao nhưng RVOL không cao | 45 và cột 7 | Hai mẫu nền khác nhau; RVOL loại thanh hiện tại |
| RSpread lớn nhưng giá ít đi | 46–47 | Có thể là nỗ lực lớn/kết quả thấp, cần xem hấp thụ |
| Các cột sâu trống/thiếu dữ liệu | 48–49 | Kiểm tra cổng Stage 2 và độ sẵn sàng lõi |

## Nguyên tắc kết luận

Không kết luận “mua” từ MA, RVOL hay RSpread. Nhóm 40–49 chỉ mô tả nền đo lường. Ý nghĩa cung–cầu bắt đầu rõ hơn ở cột 50–63 và chỉ trở thành giả thuyết cấu trúc khi ghép với vùng, sự kiện, pha và Family.

