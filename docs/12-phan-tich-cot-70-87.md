# 12. Chế độ Phân tích — Cột 70 đến 87

Nhóm 70–82 cho biết sự kiện nào làm thay đổi pha và Family. Nhóm 83–87 mở các biến trung gian của phép đếm P&F. Đây là phần dùng để giải thích trực tiếp các cột 23–39 của chế độ Tóm tắt.

## Cột 70 — Spring / Shakeout

AFL so giá với cận dưới vùng và phân loại hành vi xuyên thủng rồi thu hồi. Các trạng thái chính:

- không có;
- dạng Spring;
- dạng Shakeout;
- thu hồi mơ hồ.

Ngưỡng cố định quan trọng:

- độ xuyên tối đa: `1,0 × ATR trước`;
- mức xuyên nông dùng phân loại: `0,5 × ATR trước`;
- vị trí đóng cửa tối thiểu: `0,50`.

Đây là ứng viên tại thanh gốc. Cột 71 mới cho biết phản ứng kế tiếp đã xác nhận hay chưa.

## Cột 71 — Spring đã xác nhận

Cờ `1` khi thanh sau giữ được điều kiện phản ứng tăng của Spring. Cờ nằm tại thanh xác nhận; thanh xuyên/thu hồi nằm ngay trước đó.

Spring xác nhận có thể đưa cấu trúc từ Phase B sang Phase C theo hướng tăng nếu toàn bộ bối cảnh vùng hợp lệ.

## Cột 72 — Kiểm tra cung đã xác nhận

Supply Test được tìm gần cận dưới, trong phạm vi tối đa `0,5 × ATR trước`, dựa trên ứng viên No Supply và phản ứng xác nhận.

Nó có thể là nguồn Phase C tăng thay cho Spring. Không nên dịch cột này thành “cạn cung chắc chắn”; nó là phép kiểm tra thuật toán dựa trên biên độ, khối lượng và phản ứng một thanh.

## Cột 73 — Upthrust đã xác nhận

Ứng viên xuyên cận trên nhưng đóng cửa yếu, với:

- vị trí đóng cửa tối đa `0,50`;
- độ xuyên không quá `1,0 × ATR trước`;
- phản ứng giảm ở thanh kế tiếp.

Upthrust xác nhận là bằng chứng giảm, nhưng UTAD còn cần thêm bối cảnh và tiến triển pha.

## Cột 74 — Bối cảnh dạng UTAD

Cờ này yêu cầu đồng thời:

- Upthrust đã xác nhận;
- vùng hình thành từ phía trên;
- pha đã phát triển đủ sâu;
- hướng Phase C là giảm.

Vì vậy `Upthrust đã xác nhận = 1` nhưng `Bối cảnh dạng UTAD = 0` là hoàn toàn có thể và không phải lỗi.

## Cột 75 — SOS

Sign of Strength được nhận diện khi giá phá cận trên từ vùng trước đó và đồng thời:

```text
RSpread >= 1,20
RVOL >= 1,25
Vị trí đóng cửa >= 0,60
Close vượt RangeHigh cũ
```

SOS mô tả một thanh sức mạnh ra khỏi vùng; setup mua Phase D vẫn ưu tiên chờ retest/LPS thay vì mua đuổi.

## Cột 76 — LPS đã xác nhận

Last Point of Support được tìm trong tối đa 15 thanh sau SOS. Thanh gốc LPS cần:

- Low và Close giữ trên hoặc bằng RangeHigh cũ;
- `RSpread <= 0,80`;
- `RVOL <= 1,25`.

Thanh kế tiếp xác nhận nếu giữ Low thanh gốc và đóng cửa cao hơn. Cờ `1` nằm trên thanh xác nhận; P&F khóa biên phải tại thanh gốc LPS, tức lùi một thanh.

## Cột 77 — SOW

Sign of Weakness là phiên bản giảm của SOS:

```text
RSpread >= 1,20
RVOL >= 1,25
giá phá cận dưới và đóng cửa theo hướng giảm
```

## Cột 78 — LPSY đã xác nhận

Last Point of Supply là retest yếu sau SOW, đối xứng với LPS. Đây là bằng chứng giảm phục vụ Family phân phối/tái phân phối; AFL hiện chỉ đề xuất vùng mua ở bối cảnh tăng nên LPSY không tạo `Cơ hội mua`.

## Cột 79 — Phản ứng hấp thụ tăng

Nếu thanh trước là hấp thụ và thanh hiện tại phản ứng theo hướng tăng, cờ này bằng `1`. Nó giúp gán hướng cho hiện tượng `khối lượng cao nhưng tiến triển thấp` ở cột 57.

## Cột 80 — Phản ứng hấp thụ giảm

Phiên bản giảm của cột 79. Hai cờ phản ứng giúp tránh gắn nhãn hấp thụ tăng/giảm ngay trên thanh nỗ lực khi chưa thấy thị trường trả lời.

## Cột 81 — Sự kiện sẵn sàng

- `1`: tầng sự kiện có đủ dữ liệu, vùng và lõi hành vi.
- `0`: không nên diễn giải các cờ Spring/SOS/LPS như một chuỗi hoàn chỉnh.

## Cột 82 — Phân tích cuối sẵn sàng

Đây là cổng cuối trước khi gán pha, Family, Evidence và đề xuất mua. Nếu bằng `0`, các tầng sau phải trả trạng thái thiếu dữ liệu hoặc không áp dụng thay vì suy đoán.

---

## Cột 83 — P&F số phiên khoảng đếm

Số thanh giá từ biên trái đến biên phải của đoạn nguyên nhân được dùng để dựng P&F.

Đây không phải số cột X/O. Hai đại lượng khác nhau:

- số phiên cho biết độ dài dữ liệu đầu vào;
- cột 35 `P&F số cột đếm ngang` cho biết số cột X/O sau khi biến đổi.

Một khoảng có nhiều phiên nhưng ít cột P&F nếu giá ít đảo chiều đủ `Reversal × Box`.

## Cột 84 — P&F kích thước ô

Box được xác định một lần tại biên phải rồi giữ cố định trong toàn bộ đoạn đếm.

### ATR cố định

```text
Box = ATR tại biên phải × Hệ số ATR
```

### Phần trăm giá

```text
Box = Dòng đếm thô × Tỷ lệ % / 100
```

### Giá trị cố định

```text
Box = giá trị người dùng nhập
```

Nếu Box không hữu hạn hoặc `<= 0`, trạng thái P&F là `KÍCH THƯỚC Ô KHÔNG HỢP LỆ`.

## Cột 85 — P&F dòng đếm

Mốc neo tùy setup:

| Setup | Dòng đếm thô |
|---|---|
| Phase C Spring | Giá cơ sở Spring |
| Phase C Supply Test | Giá cơ sở Test |
| Phase D chờ retest | RangeHigh cũ |
| Phase D/E LPS | Giá cơ sở LPS |

Dòng thô được làm tròn tới bội số Box gần nhất, sau đó bị chặn để không thấp hơn đáy thực tế của đoạn đếm.

## Cột 86 — P&F đáy thực tế

```text
ActualRangeLow = min(
    cận dưới vùng đã chọn,
    tất cả Low trong khoảng đếm
)
```

Mốc này sửa tình huống giá đã xuyên thấp hơn cận vùng ban đầu. Mục tiêu bảo thủ neo tại đáy thực tế, không mù quáng neo vào cận dưới cũ.

## Cột 87 — P&F tuổi biên phải

Số thanh từ hiện tại lùi về biên phải phép đếm:

- Setup Phase C: thường bằng `0`, vì mục tiêu đang tạm tính đến thanh hiện tại.
- Phase D chờ LPS: thường bằng `0` và còn thay đổi.
- Setup có LPS: biên phải khóa tại thanh gốc LPS, nên tuổi thường lớn hơn `0`.

Nếu LPS được xác nhận ở thanh hiện tại, biên phải là thanh ngay trước đó nên tuổi biên phải phản ánh việc khóa ở thanh gốc, không phải thanh xác nhận.

## Cách kiểm tra trường hợp “chưa đủ cột đếm ngang”

Đọc theo trình tự:

1. Cột 83 có số phiên hợp lý không?
2. Cột 84 Box có quá lớn so với dao động vùng không?
3. Cột 85 dòng đếm có nằm trong vùng giá hợp lý không?
4. Cột 86 có phản ánh đáy xuyên thủng thực tế không?
5. Cột 87 cho biết phép đếm tạm tính hay đã khóa.
6. So cột 35 với `Số cột PNF tối thiểu`.

Không giảm ngưỡng số cột chỉ để buộc hệ thống sinh mục tiêu. Trước tiên cần kiểm tra Box, Reversal và khoảng đếm có phù hợp với thị trường/khung thời gian hay không.

## Từ sự kiện đến mục tiêu

```text
Sự kiện xác nhận
→ Pha và Family nhất quán
→ Setup mua hợp lệ
→ Giá ở nhóm 3 hoặc 4
→ Chọn khoảng đếm và Box
→ Dựng cột X/O
→ Kiểm tra số cột tối thiểu
→ Tính ba mức mục tiêu
```

P&F nằm cuối chuỗi. Nếu cấu trúc hoặc setup không hợp lệ, việc có một con số mục tiêu không làm giao dịch trở nên hợp lệ.

