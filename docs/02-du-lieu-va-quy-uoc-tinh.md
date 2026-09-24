# 02. Dữ liệu và quy ước tính

## 1. Dữ liệu đầu vào

AFL dùng năm chuỗi chính:

| Ký hiệu | Ý nghĩa |
|---|---|
| `O` | Giá mở cửa |
| `H` | Giá cao nhất |
| `L` | Giá thấp nhất |
| `C` | Giá đóng cửa |
| `V` | Khối lượng |

Nhiều phép tính không dùng trực tiếp `O`, nhưng dữ liệu OHLCV vẫn phải đồng nhất và được điều chỉnh đúng theo nguồn dữ liệu của người dùng.

## 2. Thanh hiện tại và thanh trước

Trong AFL:

```afl
Ref(x, -1)
```

là giá trị của `x` tại thanh liền trước.

Ví dụ:

```afl
up20 = ma20 > Ref(ma20, -1);
```

MA20 “hướng lên” khi MA20 của thanh đang xét lớn hơn MA20 của thanh trước. Đây không phải hồi quy độ dốc nhiều phiên; chỉ là phép so sánh hai giá trị liên tiếp.

## 3. Current-bar exclusion

RVOL, RSpread và các vùng tham chiếu dùng cửa sổ quá khứ loại trừ thanh hiện tại.

Ví dụ RVOL 20:

```text
Mẫu số = trung bình Volume của 20 thanh ngay trước thanh hiện tại
RVOL = Volume hiện tại / mẫu số
```

Điều này tránh việc Volume hiện tại tự làm thay đổi chuẩn dùng để đánh giá chính nó.

Tương tự, vùng ngắn/trung/dài lấy High cao nhất và Low thấp nhất của `N` thanh trước đó, không lấy thanh hiện tại.

## 4. Lookback thay đổi theo Periodicity

`Lookback = 20` có nghĩa là 20 thanh của Periodicity đang chạy:

- Daily: 20 phiên ngày.
- Weekly: 20 tuần.
- Intraday 60 phút: 20 thanh 60 phút.

Do đó, đổi Periodicity làm thay đổi bản chất mọi MA, RVOL, ATR, Pivot và vùng. AFL cho phép chạy khung khác Daily, nhưng không được so sánh kết quả hai khung như thể chúng dùng cùng dữ liệu thời gian.

## 5. Phần trăm chênh lệch

Hai dạng thường gặp:

### So với một mốc

```text
% so với mốc = 100 × (Giá hiện tại − Mốc) / Mốc
```

Ví dụ `% Dong cua so voi MA20`:

```text
100 × (Close − MA20) / MA20
```

- Dương: Close trên MA20.
- Âm: Close dưới MA20.
- 0: Close bằng MA20.

### Thay đổi giữa hai thanh

```text
% TD = 100 × (Close hiện tại − Close trước) / Close trước
```

## 6. Chuẩn hóa theo ATR

ATR giúp so sánh các mã có mức giá và độ biến động khác nhau.

AFL dùng True Range:

```text
TR = max(
    High − Low,
    |High − Close trước|,
    |Low − Close trước|
)
```

Robust ATR được khởi tạo sau một chuỗi TR hợp lệ liên tục và sau đó làm mượt theo Wilder. Nhiều công thức dùng **ATR của thanh trước**, giúp tránh việc biến động hiện tại tự nới chuẩn của chính nó.

## 7. Giá trị `Null`, 0 và 1

Ba dạng này không giống nhau:

- `Null`: không có giá trị số hợp lệ để hiển thị.
- `0`: trạng thái không có/không đạt, hoặc mã trạng thái đầu tiên tùy cột.
- `1`: thường là có/đạt/sẵn sàng ở cột nhị phân.

Không được tự động hiểu ô trống là “xấu”. Ô trống có thể chỉ ra:

- Chưa đủ lịch sử.
- Không áp dụng cho setup hiện tại.
- Không có một vùng duy nhất để công bố.
- P&F chưa vượt qua cổng dữ liệu.

## 8. Ứng viên và xác nhận

Nhiều hành vi được xử lý hai bước:

```text
Thanh gốc tạo ứng viên → thanh sau tạo phản ứng → xác nhận hoặc không xác nhận
```

Ví dụ No Supply:

1. Thanh giảm, volume thấp hơn hai thanh trước, spread hẹp → ứng viên.
2. Thanh sau đóng cửa cao hơn thanh ứng viên → xác nhận cơ bản.

Giá trị xác nhận chỉ xuất hiện sau khi dữ liệu của thanh phản ứng tồn tại. Đây là độ trễ có chủ đích để tránh nhìn trước.

## 9. Pivot nhân quả

Với `PivotLeft = A`, `PivotRight = B`, thanh ứng viên tại vị trí `k` chỉ được xác nhận tại `k + B`.

Pivot High yêu cầu:

- Cao hơn nghiêm ngặt tất cả `A` thanh bên trái.
- Cao hơn hoặc bằng tất cả `B` thanh bên phải.

Pivot Low thực hiện đối xứng.

Do đó, Pivot không được biết ngay tại cực trị. Cột ngày xác nhận và tuổi Pivot phải được đọc cùng nhau.

## 10. Fail-closed

Khi có hai bối cảnh vùng cùng tồn tại hoặc dữ liệu không đủ, AFL không tự chọn một vùng tùy ý. Nó công bố trạng thái mơ hồ và giữ các đầu ra phụ thuộc ở `Null`/không đủ.

Đây là nguyên tắc thận trọng:

> Thà không kết luận còn hơn kết luận từ một bối cảnh không duy nhất.

