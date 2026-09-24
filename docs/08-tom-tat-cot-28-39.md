# 08. Chế độ Tóm tắt — Cột 28 đến 39

Nhóm này chuyển từ nhận diện cấu trúc sang hỗ trợ hành động: cơ hội, vùng giá, thứ tự ưu tiên và mục tiêu P&F.

## Điều kiện nền để có đề xuất mua

Trước khi tạo bất kỳ setup nào, AFL yêu cầu:

```text
Có đúng 1 bối cảnh vùng
AND phân tích cuối sẵn sàng
AND vùng đang hoạt động hoặc đã kết thúc Phase E
AND Family là tích lũy hoặc tái tích lũy
AND chỉ có bằng chứng tăng
AND bối cảnh xu hướng là giả thuyết tăng
```

Do đó, mã lọt Filter không đồng nghĩa có đề xuất mua.

## Cột 28 — Cơ hội mua

### 0. KHÔNG ĐỀ XUẤT

Thiếu ít nhất một điều kiện bắt buộc. Mã vẫn nằm trong danh sách vì Stage 1 đã đạt.

### 1. PHASE C — SPRING ĐÃ XÁC NHẬN

Yêu cầu:

- Phase C đã xác nhận theo hướng tăng.
- Nguồn sự kiện C là Spring.
- Sự kiện còn trong tuổi tối đa.
- Giá nằm không quá 65% từ đáy lên đỉnh vùng.

### 2. PHASE C — KIỂM TRA CUNG ĐÃ XÁC NHẬN

Tương tự setup 1 nhưng nguồn Phase C là Supply Test.

### 3. PHASE D — CHỜ RETEST SAU SOS

- Đang Phase D tăng.
- Có SOS còn mới.
- Chưa có LPS gần đây đủ điều kiện ưu tiên hơn.

Đây là setup “chờ”, không phải khuyến nghị mua đuổi thanh SOS.

### 4. PHASE D — LPS ĐÃ XÁC NHẬN

- Đang Phase D tăng.
- Có LPS xác nhận còn mới.

### 5. PHASE E — LPS GẦN ĐÂY

- Đang Phase E tăng.
- LPS gần nhất vẫn trong tuổi tối đa.

## Cột 29 — Trạng thái giá mua

Sau khi có vùng hợp lệ:

```text
Nếu Close < ZoneLow                     → nhóm 1
Nếu ZoneLow <= Close <= ZoneHigh        → nhóm 4
Nếu Close > ZoneHigh và cách <= NearPct → nhóm 3
Nếu Close > ZoneHigh và cách > NearPct  → nhóm 2
```

| Nhóm | Hiển thị | Hành động đọc đúng |
|---:|---|---|
| 4 | ĐANG TRONG VÙNG MUA | Đủ điều kiện vị trí; vẫn cần xác minh biểu đồ |
| 3 | GẦN VÙNG MUA | Theo dõi sát, tránh mua nếu giá tiếp tục chạy xa |
| 2 | CHỜ ĐIỀU CHỈNH — KHÔNG MUA ĐUỔI | Setup còn nhưng vị trí không còn hấp dẫn |
| 1 | DƯỚI VÙNG MUA — CHỜ XÁC NHẬN LẠI | Không mặc định là rẻ; có thể cấu trúc suy yếu |
| 0 | KHÔNG ÁP DỤNG | Không có proposal hợp lệ |

## Cột 30 — Vùng mua từ

### Phase C

```text
ZoneLow = max(RangeLow, Low của thanh gốc Spring/Test)
```

### Phase D chờ retest

```text
ZoneLow = RangeHigh cũ
```

### Phase D/E có LPS

```text
ZoneLow = max(RangeHigh cũ, Low của thanh gốc LPS)
```

## Cột 31 — Vùng mua đến

### Phase C

```text
ZoneHigh = min(
    RangeHigh,
    ZoneLow + ZoneWidthATR × PriorATR
)
```

### Phase D/E

```text
ZoneHigh = ZoneLow + ZoneWidthATR × PriorATR
```

## Cột 32 — Giá vô hiệu tham chiếu

### Phase C

```text
Invalidation =
    min(RangeLow, Low thanh gốc sự kiện)
    − InvalidationATR × PriorATR
```

### Phase D/E

```text
Invalidation =
    RangeHigh cũ
    − InvalidationATR × PriorATR
```

Proposal chỉ hợp lệ khi Close vẫn cao hơn mức này.

Đây là mức tham chiếu cấu trúc, không tự động là stop-loss cuối cùng. Stop thực tế còn phụ thuộc điểm vào, quy mô vị thế, thanh khoản và kế hoạch cá nhân.

## Cột 33 — Ưu tiên mua

### Phần nguyên

Phần nguyên chính là nhóm trạng thái giá `4 → 3 → 2 → 1 → 0`.

### Nhóm 4 — trong vùng

Vị trí trong vùng mua:

```text
InsidePosition = (Close − ZoneLow) / (ZoneHigh − ZoneLow)
Proximity = 1 − InsidePosition
```

Giá gần cận dưới hơn có Proximity cao hơn.

### Nhóm 3/2 — trên vùng

```text
DistancePct = 100 × (Close − ZoneHigh) / ZoneHigh
Proximity = 1 / (1 + DistancePct / NearZonePct)
```

Giá càng gần ZoneHigh càng được xếp trên trong cùng nhóm.

### Nhóm 1 — dưới vùng

```text
DistancePct = 100 × (ZoneLow − Close) / ZoneLow
```

Khoảng cách càng nhỏ càng được xếp trên, nhưng nhóm 1 vẫn luôn dưới nhóm 2.

### Độ mới sự kiện

```text
Freshness = 1 / (1 + EventAge / MaxEventAge)
```

Nguồn tuổi:

- Setup 1/2: tuổi sự kiện C.
- Setup 3: tuổi SOS.
- Setup 4/5: tuổi LPS.

### Phần thập phân

```text
Fraction = 0,890 × Proximity + 0,009 × Freshness
SortKey = Group + Fraction
```

Khoảng cách giá là yếu tố chính; tuổi sự kiện chỉ phá hòa nhẹ. Fraction tối đa nhỏ hơn 0,9 nên nhóm thấp không thể vượt nhóm cao.

### Nhóm 0

Nhóm 0 vẫn được xếp nội bộ theo mức hoàn thiện cấu trúc và khoảng cách đến vùng theo dõi dự kiến, nhưng không biến thành đề xuất mua.

---

## Cột 34 — Trạng thái P&F

| Trạng thái | Ý nghĩa |
|---|---|
| KHÔNG ÁP DỤNG | Không ở nhóm giá 3/4 hoặc không có setup |
| THIẾU DỮ LIỆU/MỐC ĐẾM | Thiếu vùng, tuổi, mốc hoặc biên phải |
| KÍCH THƯỚC Ô KHÔNG HỢP LỆ | Box không hữu hạn hoặc <= 0 |
| CHƯA ĐỦ CỘT ĐẾM NGANG | Dựng được P&F nhưng số cột dưới ngưỡng |
| TẠM TÍNH — PHASE C | Dùng biên phải hiện tại vì cấu trúc chưa khóa bằng LPS |
| TẠM TÍNH — CHỜ LPS XÁC NHẬN | Sau SOS nhưng chưa có LPS; mục tiêu còn thay đổi |
| ĐÃ KHÓA TẠI LPS | Biên phải khóa tại thanh gốc LPS |
| MỤC TIÊU P&F ĐÃ HOÀN THÀNH | Không còn mục tiêu riêng biệt nào cao hơn Close |

## Cột 35 — P&F số cột đếm ngang

Đây là tổng số cột X/O trong đoạn nguyên nhân từ biên trái vùng đến biên phải phép đếm.

Không còn là “số cột cắt đúng một dòng giá”. Dòng đếm là mốc neo mục tiêu, còn chiều rộng nguyên nhân dùng toàn bộ số cột của đoạn.

## Xác định biên phải

- Setup 1/2 Phase C: thanh hiện tại, mục tiêu tạm tính.
- Setup 3 sau SOS: thanh hiện tại, tạm tính chờ LPS.
- Setup 4/5: thanh gốc LPS, tức một thanh trước thanh xác nhận LPS.

## Kích thước ô

Tùy Parameters:

```text
ATR cố định:  Box = ATR tại biên phải × hệ số
% giá:        Box = RawCountLine × tỷ lệ %
Giá trị cố định: Box = giá trị người dùng nhập
```

Box được giữ cố định trong toàn bộ phép đếm.

## Dòng đếm

- Setup 1/2: giá cơ sở Spring/Test.
- Setup 3: RangeHigh cũ.
- Setup 4/5: giá cơ sở LPS.

Dòng thô được làm tròn tới hàng box gần nhất và không được thấp hơn đáy thực tế của khoảng đếm.

## Cột 36 — Mục tiêu P&F bảo thủ

```text
Extension = ColumnCount × Box × Reversal
TargetLow = ActualRangeLow + Extension
```

## Cột 37 — Mục tiêu P&F cơ sở

```text
MidAnchor = (ActualRangeLow + CountLine) / 2
TargetMid = MidAnchor + Extension
```

## Cột 38 — Mục tiêu P&F mở rộng

```text
TargetHigh = CountLine + Extension
```

## Loại bỏ mục tiêu trùng

Ba mục tiêu được so sánh sau khi làm tròn hai chữ số thập phân:

- Mục tiêu bảo thủ luôn hiển thị khi phép đếm hợp lệ.
- Mục tiêu cơ sở chỉ hiển thị nếu lớn hơn mục tiêu trước.
- Mục tiêu mở rộng chỉ hiển thị nếu lớn hơn mục tiêu riêng biệt cuối cùng.

Vì vậy ô trống ở mục tiêu cơ sở/mở rộng có thể chỉ có nghĩa là mức đó trùng sau làm tròn, không phải lỗi.

## Cột 39 — Dư địa P&F kế tiếp %

AFL chọn mục tiêu riêng biệt đầu tiên còn cao hơn Close sau khi làm tròn hai chữ số:

```text
Upside = 100 × (NextTarget / Close − 1)
```

Nếu cả ba mục tiêu đã đạt, trạng thái chuyển thành `MỤC TIÊU P&F ĐÃ HOÀN THÀNH` và dư địa để trống.

## Checklist cột 28–39

- [ ] Setup có dựa trên bằng chứng tăng duy nhất không?
- [ ] Giá đang ở nhóm 4, 3, 2 hay 1?
- [ ] Vùng mua có hợp lý trên biểu đồ không?
- [ ] Giá vô hiệu còn giữ được không?
- [ ] P&F đang tạm tính hay đã khóa?
- [ ] Số cột đã đạt ngưỡng tối thiểu chưa?
- [ ] Dư địa dùng mục tiêu kế tiếp, không mặc định là mục tiêu bảo thủ.

