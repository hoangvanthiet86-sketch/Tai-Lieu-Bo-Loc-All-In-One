# 15. Ví dụ thực hành

Các số dưới đây là ví dụ minh họa, không phải dữ liệu của một mã cụ thể và không phải khuyến nghị giao dịch.

## Ví dụ 1 — Lọt Filter nhưng không có đề xuất mua

Giả sử một mã có:

- MA20 và MA50 cùng hướng lên;
- KL MA20 vượt ngưỡng;
- BBW đạt;
- Phase B;
- Family chưa giải quyết;
- Evidence hỗn hợp.

Kết quả hợp lý:

| Cột | Kết quả |
|---|---|
| Cơ hội mua | KHÔNG ĐỀ XUẤT |
| Trạng thái giá mua | KHÔNG ÁP DỤNG |
| Ưu tiên mua | nhóm 0 |
| Trạng thái P&F | KHÔNG ÁP DỤNG |

Giải thích: Stage 1 quyết định mã xuất hiện; Stage 2 không bắt buộc phải tạo điểm mua cho mọi mã.

## Ví dụ 2 — Tính vùng mua Phase C

Giả sử:

- RangeLow = `42,00`;
- RangeHigh = `50,00`;
- Low thanh Spring gốc = `41,80`;
- ATR trước = `2,00`;
- độ rộng vùng mua = `0,50 ATR`;
- biên vô hiệu = `0,25 ATR`.

### Cận dưới

```text
ZoneLow = max(42,00; 41,80) = 42,00
```

### Cận trên

```text
ZoneHigh = min(50,00; 42,00 + 0,50 × 2,00)
         = 43,00
```

### Giá vô hiệu tham chiếu

```text
Invalidation = min(42,00; 41,80) − 0,25 × 2,00
             = 41,30
```

Nếu Close là `42,40`, giá nằm trong vùng mua. Nếu Close là `43,80`, khoảng cách trên ZoneHigh là:

```text
100 × (43,80 − 43,00) / 43,00 = 1,86%
```

Với ngưỡng gần vùng `3%`, mã thuộc nhóm 3.

## Ví dụ 3 — Phase D chờ retest và Phase D có LPS

### Chưa có LPS

Sau SOS, RangeHigh cũ là `50,00`, ATR trước là `2,00`:

```text
ZoneLow  = 50,00
ZoneHigh = 50,00 + 0,50 × 2,00 = 51,00
```

Setup là `PHASE D — CHỜ RETEST SAU SOS`. Mục tiêu không phải mua đuổi thanh SOS mà theo dõi phản ứng quanh cận trên cũ.

### LPS đã xác nhận

Nếu Low thanh LPS gốc là `50,50`:

```text
ZoneLow  = max(50,00; 50,50) = 50,50
ZoneHigh = 50,50 + 1,00 = 51,50
Invalidation = 50,00 − 0,25 × 2,00 = 49,50
```

P&F lúc này khóa biên phải tại thanh LPS gốc, không tiếp tục nới khoảng đếm theo các thanh sau.

## Ví dụ 4 — Tính mục tiêu P&F đếm ngang

Giả sử phép dựng P&F hợp lệ cho:

- số cột đếm ngang `N = 6`;
- Box = `1,00`;
- Reversal = `3`;
- đáy thực tế = `42,00`;
- dòng đếm = `44,00`.

### Độ mở rộng

```text
Extension = 6 × 1,00 × 3 = 18,00
```

### Ba mục tiêu

```text
Mục tiêu bảo thủ = 42,00 + 18,00 = 60,00
Mục tiêu cơ sở   = (42,00 + 44,00) / 2 + 18,00 = 61,00
Mục tiêu mở rộng = 44,00 + 18,00 = 62,00
```

Nếu Close hiện tại là `52,00`, mục tiêu kế tiếp là `60,00`:

```text
Dư địa = 100 × (60 / 52 − 1) = 15,38%
```

Nếu Close là `60,50`, mục tiêu bảo thủ đã qua; mục tiêu kế tiếp là `61,00`, không phải `60,00`.

## Ví dụ 5 — Vì sao ba mục tiêu có thể không đủ ba ô

Giả sử Box và cách làm tròn khiến:

```text
TargetLow  = 25,004 → 25,00
TargetMid  = 25,003 → 25,00
TargetHigh = 25,006 → 25,01
```

Sau khử trùng ở hai chữ số:

- bảo thủ hiển thị `25,00`;
- cơ sở để trống vì trùng;
- mở rộng hiển thị `25,01`.

Ô trống ở đây là chủ ý, không phải thiếu dữ liệu. Trạng thái P&F và số cột vẫn phải hợp lệ.

## Ví dụ 6 — “Gần vùng mua” nhưng không có mục tiêu P&F

Một mã có thể ở nhóm 3 nhưng chưa có mục tiêu nếu:

- thiếu mốc đếm hoặc dữ liệu;
- Box không hợp lệ;
- số cột X/O nhỏ hơn ngưỡng tối thiểu;
- tất cả mục tiêu riêng biệt đã thấp hơn hoặc bằng Close.

Quy trình kiểm tra:

1. Xem `Trạng thái P&F`.
2. Nếu thiếu cột, xem số phiên, Box, dòng đếm, đáy thực tế và tuổi biên phải.
3. Nếu hoàn thành, kiểm tra Close đã vượt các mục tiêu hay chưa.
4. Nếu không áp dụng dù nhóm giá là 3/4, kiểm tra phiên bản và báo cáo đầy đủ.

## Ví dụ 7 — Tất cả mã đều “chưa đủ cột”

Nếu chỉ một vài mã thiếu cột, đó có thể là đặc điểm cấu trúc. Nếu gần như toàn bộ danh sách cùng thiếu:

- Box ATR có thể quá lớn;
- Reversal có thể làm quá ít lần đảo cột;
- khoảng đếm bị ngắn hoặc chọn sai biên phải;
- đang dùng cấu hình lưu từ thị trường khác;
- dữ liệu lịch sử trong Analysis không đủ.

Không nên kết luận bằng mắt rằng thuật toán sai hoặc giảm ngay số cột tối thiểu. Chuyển sang Phân tích, so các cột 83–87 trên nhiều mã, rồi thay một Parameter mỗi lần.

## Ví dụ 8 — Volume cao không đồng nghĩa cầu thắng

Giả sử:

```text
RVOL = 2,10
RSpread = 1,75
DirectionalProgress = 0,12 ATR
Close Position = 0,48
```

AFL có thể ghi nỗ lực cao trào và kết quả hướng thấp/hấp thụ. Chưa thể kết luận tăng vì:

- khối lượng cao chỉ nói có giao dịch mạnh;
- giá tiến rất ít;
- Close không nằm mạnh ở phần trên;
- cần phản ứng các thanh sau và vị trí trong vùng.

## Ví dụ 9 — Evidence hỗn hợp

Một vùng từng có Spring xác nhận nhưng sau đó xuất hiện Upthrust/SOW. Khi cả bằng chứng tăng và giảm còn hiệu lực:

- `Bằng chứng = HỖN HỢP`;
- Family có thể là hỗn hợp/xung đột;
- bối cảnh hướng không còn chỉ tăng;
- đề xuất mua bị chặn.

Đây là hành vi fail-closed có chủ ý. Không nên chỉ giữ Spring vì nó xuất hiện trước và bỏ qua bằng chứng giảm mới hơn.

## Bài tập tự kiểm tra

Với mỗi dòng nhóm 3/4, hãy trả lời được năm câu:

1. Setup bắt nguồn từ sự kiện nào và thanh gốc ở đâu?
2. Vì sao Family là tích lũy hay tái tích lũy?
3. Có bằng chứng giảm nào đang tồn tại không?
4. Vùng mua và mức vô hiệu được tính từ mốc nào?
5. Mục tiêu P&F đang tạm tính hay đã khóa?

Nếu chưa trả lời được, chuyển sang chế độ Phân tích trước khi dùng kết quả.

