# 05. Parameters Stage 2 — VSA, cấu trúc, vùng mua và P&F

Stage 2 có 27 Parameters. Chúng chỉ thay đổi các cột phân tích; không thêm hoặc loại mã khỏi `Filter`.

## A. Core VSA — 7 Parameters

### 1. VSA 2.1 — Số phiên nhìn lại khối lượng

- Mặc định: `20`.
- Phạm vi: `2–200`.

Mẫu số RVOL là trung bình Volume của đúng `N` thanh hợp lệ ngay trước thanh hiện tại.

```text
RVOL = Volume hiện tại / Average(Volume của N thanh trước)
```

- N nhỏ: nhạy với thay đổi gần đây.
- N lớn: nền so sánh ổn định hơn nhưng phản ứng chậm.

Nếu không có đủ `N` thanh volume hợp lệ, RVOL là `Null` và VSA lõi chưa sẵn sàng.

### 2. VSA 2.2 — Số phiên nhìn lại biên độ

- Mặc định: `20`.
- Phạm vi: `2–200`.

```text
RSpread = (High − Low) hiện tại / Average(High − Low của N thanh trước)
```

Thanh hiện tại không nằm trong mẫu số.

### 3. VSA 2.3 — Chu kỳ ATR

- Mặc định: `14`.
- Phạm vi: `2–200`.

ATR được dùng để chuẩn hóa tiến triển giá, độ xuyên biên, độ rộng vùng, vùng mua và box P&F khi chọn chế độ ATR.

Tăng chu kỳ làm ATR chậm hơn; giảm chu kỳ làm ATR nhạy hơn.

### 4. VSA 2.4 — Ngưỡng RVOL nỗ lực cao

- Mặc định: `1,80`.
- Phạm vi: `0,01–10`.

```text
HighEffort = RVOL >= ngưỡng cao
```

Tăng ngưỡng làm nhãn “nỗ lực cao” hiếm hơn.

### 5. VSA 2.5 — Ngưỡng RVOL nỗ lực thấp

- Mặc định: `0,75`.
- Phạm vi: `0–10`.

```text
LowEffort = RVOL <= ngưỡng thấp
```

Điều kiện cấu hình hợp lệ:

```text
LowEffortRVOL < HighEffortRVOL
```

Nếu đặt ngược hoặc bằng nhau, cột Effort/Result báo cấu hình không hợp lệ.

### 6. VSA 2.6 — Kết quả theo hướng thấp/ATR

- Mặc định: `0,35`.

```text
DirectionalProgress = (Close − Close trước) / ATR trước
LowResult = |DirectionalProgress| <= 0,35
```

### 7. VSA 2.7 — Kết quả theo hướng cao/ATR

- Mặc định: `0,80`.

```text
HighResult = |DirectionalProgress| >= 0,80
```

Giữa hai ngưỡng là kết quả bình thường. Cấu hình hợp lệ yêu cầu ngưỡng thấp nhỏ hơn ngưỡng cao.

---

## B. Structure/Location — 10 Parameters

### 8–10. Lookback ngắn, trung và dài

| Parameters | Mặc định | Phạm vi |
|---|---:|---:|
| SL 3.1 ngắn | 20 | 2–500 |
| SL 3.2 trung | 60 | 2–500 |
| SL 3.3 dài | 120 | 2–500 |

Với mỗi cửa sổ `N`:

```text
Upper = High cao nhất của N thanh trước
Lower = Low thấp nhất của N thanh trước
Width = Upper − Lower
Location = (Close hiện tại − Lower) / Width
```

Ba cửa sổ độc lập, không tự chọn một cửa sổ “thắng”. Chúng chủ yếu xuất hiện trong chế độ Đầy đủ để chẩn đoán vị trí.

### 11. SL 3.4 — Số phiên bên trái Pivot

- Mặc định: `3`.
- Phạm vi: `1–20`.

Pivot High phải cao hơn nghiêm ngặt `A` thanh bên trái; Pivot Low phải thấp hơn nghiêm ngặt `A` thanh bên trái.

Tăng `A` làm Pivot quan trọng hơn nhưng ít hơn.

### 12. SL 3.5 — Số phiên bên phải Pivot

- Mặc định: `3`.
- Phạm vi: `1–20`.

Pivot chỉ được xác nhận sau `B` thanh bên phải.

- Tăng `B`: ít nhiễu hơn, độ trễ xác nhận lớn hơn.
- Giảm `B`: nhanh hơn, nhiều Pivot hơn.

### 13. VSA 3.6 — Khoảng cách tối đa giữa hai mốc vùng

- Mặc định: `40` thanh.
- Phạm vi: `3–120`.

Hai mốc cấu trúc dùng để đóng băng một Trading Range phải cách nhau không quá giới hạn này.

### 14. VSA 3.7 — Độ rộng vùng tối thiểu/ATR

- Mặc định: `1,50 ATR`.
- Phạm vi: `0,25–20`.

```text
RangeWidth >= MinRangeWidthATR × ATR tại mốc
```

- Tăng: chỉ giữ vùng rộng, rõ hơn.
- Giảm: nhận nhiều vùng nhỏ hơn, tăng nguy cơ nhiễu.

Nếu ATR tại mốc chưa hợp lệ, mã cho phép đánh giá theo độ rộng giá dương thay vì loại tuyệt đối.

### 15. VSA 3.8 — Tuổi vùng tối thiểu để vào dạng Phase B

- Mặc định: `4` thanh.
- Phạm vi: `1–40`.

Một vùng mới bắt đầu ở Phase A. Khi tuổi vùng đạt ngưỡng mà chưa có sự kiện chuyển pha, nó sang Phase B.

### 16. VSA 3.9 — Biên vô hiệu/độ rộng vùng

- Mặc định: `0,50`.

Biên mở rộng:

```text
Upper invalidation = RangeHigh + 0,50 × RangeWidth
Lower invalidation = RangeLow  − 0,50 × RangeWidth
```

Đây là biên dùng để xác định giá đã rời quá xa một vùng A/B/C cũ, không phải stop-loss giao dịch.

### 17. VSA 3.10 — Số phiên xác nhận vô hiệu

- Mặc định: `2`.

Giá phải đóng cửa liên tiếp ngoài cùng một phía của biên mở rộng đủ số thanh này để vùng Phase A–C bị chuyển sang trạng thái kết thúc/bị thay thế.

---

## C. Vùng mua — 4 Parameters

### 18. MUA 4.1 — Độ rộng vùng mua/ATR

- Mặc định: `0,50 ATR`.

```text
ZoneHigh = ZoneLow + 0,50 × ATR trước
```

Riêng Phase C, ZoneHigh không vượt RangeHigh.

- Tăng: vùng mua rộng hơn, nhiều mã ở trạng thái “trong vùng”.
- Giảm: vùng hẹp, yêu cầu giá chính xác hơn.

### 19. MUA 4.2 — Biên giá vô hiệu/ATR

- Mặc định: `0,25 ATR`.

Phase C:

```text
Invalidation = min(RangeLow, Low thanh gốc sự kiện) − hệ số × ATR
```

Phase D/E:

```text
Invalidation = RangeHigh cũ − hệ số × ATR
```

Giá phải cao hơn mức này để đề xuất còn hợp lệ.

### 20. MUA 4.3 — Khoảng cách gần vùng mua

- Mặc định: `3%`.

Nếu Close cao hơn ZoneHigh nhưng khoảng cách không vượt 3%, trạng thái là `GAN VUNG MUA`. Vượt quá mức này chuyển thành `CHO DIEU CHINH - KHONG MUA DUOI`.

### 21. MUA 4.4 — Tuổi sự kiện tối đa

- Mặc định: `15` thanh.

Giới hạn độ mới của:

- Spring/Supply Test cho Phase C.
- SOS cho setup chờ retest.
- LPS cho Phase D/E.

Tăng giá trị giữ setup lâu hơn; giảm giá trị yêu cầu tín hiệu mới hơn.

---

## D. Point & Figure — 6 Parameters

### 22. PNF 5.1 — Cách tính kích thước ô

Ba lựa chọn:

1. `ATR co dinh`.
2. `Phan tram gia`.
3. `Gia tri co dinh`.

“Cố định” nghĩa là box được tính một lần tại biên phải phép đếm rồi giữ nguyên trong toàn bộ đồ thị P&F của mã đó.

### 23. PNF 5.2 — Kích thước ô/ATR

- Mặc định: `0,50`.

```text
Box = ATR tại biên phải × 0,50
```

Chỉ có tác dụng khi chọn `ATR co dinh`.

### 24. PNF 5.3 — Kích thước ô (%)

- Mặc định: `1%`.

```text
Box = RawCountLine × 1% / 100
```

Chỉ có tác dụng khi chọn `Phan tram gia`.

### 25. PNF 5.4 — Kích thước ô cố định

- Mặc định: `0,10` đơn vị giá.

Chỉ có tác dụng khi chọn `Gia tri co dinh`. Cần đặc biệt cẩn thận khi dùng chung cho mã giá 5 và mã giá 500.

### 26. PNF 5.5 — Số ô đảo chiều

- Mặc định: `3`.
- Phạm vi: `1–5`.

Đang ở cột X, P&F chỉ đảo sang O khi giá giảm ít nhất số box này từ cực trị cột X. Cột O thực hiện đối xứng.

- Reversal lớn: ít cột, bỏ nhiễu nhỏ, mục tiêu có thể thay đổi mạnh.
- Reversal nhỏ: nhiều cột, nhạy hơn.

### 27. PNF 5.6 — Số cột tối thiểu

- Mặc định và giá trị nhỏ nhất: `5`.
- Phạm vi: `5–30`.

Nếu số cột đếm ngang nhỏ hơn ngưỡng, trạng thái là `CHUA DU COT DEM NGANG` và không xuất mục tiêu.

## E. Ngưỡng cố định không có trong Parameters

Một số ngưỡng được khóa trong mã v1.7.0:

| Logic | Ngưỡng |
|---|---:|
| Spring xuyên tối đa | 1,00 ATR |
| Spring xuyên nông | 0,50 ATR |
| Close tối thiểu của Spring | 0,50 |
| Close tối đa của Upthrust | 0,50 |
| SOS: RSpread tối thiểu | 1,20 |
| SOS: RVOL tối thiểu | 1,25 |
| SOW: RSpread tối thiểu | 1,20 |
| SOW: RVOL tối thiểu | 1,25 |
| LPS/LPSY: RSpread tối đa | 0,80 |
| LPS/LPSY: RVOL tối đa | 1,25 |
| Thời gian retest tối đa | 15 thanh |
| Climactic: RSpread/RVOL | 1,60 / 1,80 |
| Absorption: RVOL tối thiểu | 1,80 |
| Absorption: tiến triển tối đa | 0,35 ATR |
| Stopping Volume: RVOL tối thiểu | 1,80 |
| Stopping Volume: Close Position tối thiểu | 0,50 |

Các ngưỡng này được tài liệu hóa để người dùng hiểu kết quả nhưng không thể thay đổi từ cửa sổ Parameters trong phiên bản hiện tại.

