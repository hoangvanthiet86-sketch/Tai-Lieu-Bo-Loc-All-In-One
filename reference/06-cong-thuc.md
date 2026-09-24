# Công thức tra cứu

Ký hiệu: `C, H, L, V` là Close, High, Low, Volume; hậu tố `t−1` là thanh trước; mọi phép chia đều cần mẫu số hợp lệ.

## 1. Stage 1

### Đường trung bình và độ dốc

```text
MA20  = SMA(C,20)
MA50  = SMA(C,50)
MA100 = SMA(C,100)
MA200 = SMA(C,200)

UpMA(N) = MA(N)t > MA(N)t−1
MAOrder = MA20 > MA50 > MA100
```

### Giá và khối lượng

```text
%TD = 100 × (Ct − Ct−1) / Ct−1
CloseVsMA20% = 100 × (C − MA20) / MA20
CloseVsMA50% = 100 × (C − MA50) / MA50
VolMA20 = SMA(V,20)
VolVsVolMA20% = 100 × (V − VolMA20) / VolMA20
```

### Bollinger Band Width

```text
Mid = SMA(C, Period)
Upper = Mid + 2 × StDev(C, Period)
Lower = Mid − 2 × StDev(C, Period)
BBW% = 100 × (Upper − Lower) / Mid
```

### Đỉnh 52 tuần

```text
PrevWeekHigh = High của tuần trước
High52 = cao nhất của PrevWeekHigh trong 52 tuần
CloseVs52WHigh% = 100 × (C − High52) / High52
```

Tuần hiện tại bị loại khỏi High52.

### Xu hướng tuần

```text
MA10W > MA20W > MA40W
AND CloseW > MA20W
AND MA10W > MA10W của 2 tuần trước
AND MA20W > MA20W của 2 tuần trước
AND CloseW > CloseW của 4 tuần trước
AND LowW > LowW của 4 tuần trước
```

### Filter cuối

```text
Status(lastbarinrange)
AND đủ dữ liệu MA200/Volume/tuần
AND chế độ MA hướng lên
AND VolMA20 > ngưỡng tối thiểu
AND CloseVsMA20% trong [min,max]
AND VolVsVolMA20% trong [min,max]
AND thứ tự MA nếu bật
AND lọc màu MA20
AND lọc màu Close
AND BBW% <= ngưỡng
```

## 2. Core VSA

### RVOL

```text
RVOLt = Vt / Average(V của đúng N thanh hợp lệ trước t)
```

Thanh hiện tại không nằm trong mẫu nền. Nếu chưa có đủ N thanh hợp lệ, kết quả là Null.

### Relative Spread

```text
Spreadt = Ht − Lt
RSpreadt = Spreadt / Average(Spread của đúng N thanh hợp lệ trước t)
```

### Vị trí đóng cửa

```text
ClosePosition = (C − L) / (H − L)
```

Nếu `H = L`, AFL dùng `0,5`.

### True Range và ATR

```text
TR = max(H−L, |H−C trước|, |L−C trước|)
ATR = WilderAverage(TR, Period)
```

ATR chỉ sẵn sàng sau khi có đủ seed liên tiếp hợp lệ.

### Tiến triển theo hướng

```text
DirectionalProgress = (Ct − Ct−1) / ATRt−1
ResultMagnitude = |DirectionalProgress|
```

### Nỗ lực và kết quả

```text
Effort = cao nếu RVOL >= HighEffort
         thấp nếu RVOL <= LowEffort
         bình thường nếu nằm giữa

Result = cao nếu |Progress| >= HighResultATR
         thấp nếu |Progress| <= LowResultATR
         bình thường nếu nằm giữa
```

## 3. Hành vi VSA

```text
NoDemandCandidate = C>Ct−1
                    AND V< từng V của 2 thanh trước
                    AND RSpread<0,80

NoDemandConfirmed = ứng viên ở t−1 AND Ct<Ct−1
```

```text
NoSupplyCandidate = C<Ct−1
                    AND V< từng V của 2 thanh trước
                    AND RSpread<0,80

NoSupplyConfirmed = ứng viên ở t−1 AND Ct>Ct−1
```

```text
Climactic = RSpread>=1,60 AND RVOL>=1,80
Absorption = RVOL>=1,80 AND |Progress|<=0,35
DownPressure = L<Lt−1 OR C<Ct−1
UpPressure = H>Ht−1 OR C>Ct−1
StoppingVolume = RVOL>=1,80
                 AND DownPressure
                 AND ClosePosition>=0,50
```

```text
StoppingConfirmed = StoppingVolume ở t−1
                    AND Lt>=Lt−1
                    AND Ct>Ct−1
```

## 4. Vùng tham chiếu và vị trí

Với lookback N, chỉ dùng N thanh trước:

```text
UpperN = Highest(H của N thanh trước)
LowerN = Lowest(L của N thanh trước)
WidthN = UpperN − LowerN
LocationN = (Ct − LowerN) / WidthN
```

Vị trí trong vùng cấu trúc được chọn:

```text
RangePosition = (C − RangeLow) / (RangeHigh − RangeLow)
```

## 5. Pivot nhân quả

Pivot High tại thanh gốc chỉ được xác nhận sau B thanh bên phải:

```text
High gốc > từng High của A thanh bên trái
AND High gốc >=/vượt các High của B thanh bên phải theo quy tắc mã
```

Pivot Low đối xứng. Giá trị không được công bố như Pivot xác nhận trước khi đủ B thanh bên phải; điều này tránh dùng tương lai trong trạng thái lịch sử.

## 6. Biên vô hiệu vùng

```text
RangeWidth = RangeHigh − RangeLow
UpperInvalid = RangeHigh + Fraction × RangeWidth
LowerInvalid = RangeLow − Fraction × RangeWidth
```

Vùng Phase A–C bị vô hiệu khi Close nằm liên tiếp ngoài cùng một phía đủ số thanh `InvalidationBars`.

## 7. Sự kiện Wyckoff chính

### Spring/Shakeout

Ứng viên xuyên cận dưới không quá `1 ATR trước`, sau đó thu hồi với Close Position tối thiểu `0,50`; `0,50 ATR` là ngưỡng phân biệt xuyên nông trong phân loại.

### Supply Test

No Supply candidate gần cận dưới trong `0,50 ATR trước`, sau đó có phản ứng No Supply xác nhận và giữ đáy.

### Upthrust

Ứng viên xuyên cận trên không quá `1 ATR trước`, Close Position không quá `0,50`, sau đó có phản ứng giảm.

### SOS

```text
RSpread>=1,20
AND RVOL>=1,25
AND ClosePosition>=0,60
AND Close phá RangeHigh cũ
```

### LPS

Trong tối đa 15 thanh sau SOS:

```text
Low>=RangeHigh cũ
AND Close>=RangeHigh cũ
AND RSpread<=0,80
AND RVOL<=1,25
```

Thanh sau xác nhận khi giữ Low thanh gốc và Close cao hơn. SOW/LPSY dùng logic đối xứng theo hướng giảm.

## 8. Vùng mua

### Phase C

```text
ZoneLow = max(RangeLow, EventOriginLow)
ZoneHigh = min(RangeHigh, ZoneLow + ZoneWidthATR×PriorATR)
Invalidation = min(RangeLow, EventOriginLow)
               − InvalidationATR×PriorATR
```

### Phase D chờ retest

```text
ZoneLow = OldRangeHigh
ZoneHigh = ZoneLow + ZoneWidthATR×PriorATR
Invalidation = OldRangeHigh − InvalidationATR×PriorATR
```

### Phase D/E có LPS

```text
ZoneLow = max(OldRangeHigh, LPSOriginLow)
ZoneHigh = ZoneLow + ZoneWidthATR×PriorATR
Invalidation = OldRangeHigh − InvalidationATR×PriorATR
```

### Nhóm giá

```text
C < ZoneLow                         → 1
ZoneLow <= C <= ZoneHigh            → 4
C > ZoneHigh và Distance%<=NearPct  → 3
C > ZoneHigh và Distance%>NearPct   → 2
không có setup                      → 0
```

## 9. Khóa xếp hạng

```text
Freshness = 1 / (1 + EventAge/MaxEventAge)
Fraction = 0,890×Proximity + 0,009×Freshness
SortKey = Group + Fraction
```

Với nhóm 4, `Proximity` cao khi Close gần ZoneLow. Với nhóm 3/2, nó cao khi Close gần ZoneHigh. Phần lẻ tối đa dưới 0,9 nên không thể đảo thứ tự nhóm nguyên.

## 10. Point & Figure Horizontal Count

### Box

```text
ATR mode:   Box = ATR tại biên phải × ATRFactor
% mode:     Box = RawCountLine × BoxPct/100
Fixed mode: Box = BoxFixed
```

Box được đóng băng cho toàn bộ khoảng đếm.

### Dựng cột X/O

- Cột X tăng từng Box khi High đủ cao.
- Chỉ đảo X→O khi giá giảm tối thiểu `Reversal×Box` từ cực trị X.
- Cột O giảm từng Box khi Low đủ thấp.
- Chỉ đảo O→X khi giá tăng tối thiểu `Reversal×Box` từ cực trị O.
- `HorizontalColumnCount` là tổng số cột X/O của đoạn, không phải số ô trên một hàng.

### Dòng đếm và đáy

```text
CountLine = RawCountLine làm tròn tới Box gần nhất
CountLine = max(CountLine, ActualRangeLow)

ActualRangeLow = min(RangeLow, mọi Low trong khoảng đếm)
```

### Mục tiêu

```text
Extension = ColumnCount × Box × Reversal
TargetLow  = ActualRangeLow + Extension
TargetMid  = (ActualRangeLow + CountLine)/2 + Extension
TargetHigh = CountLine + Extension
```

Mục tiêu được khử trùng sau khi làm tròn hai chữ số. Mục tiêu kế tiếp là mức riêng biệt đầu tiên lớn hơn Close đã làm tròn.

```text
UpsideNext% = 100 × (NextTarget/Close − 1)
```

