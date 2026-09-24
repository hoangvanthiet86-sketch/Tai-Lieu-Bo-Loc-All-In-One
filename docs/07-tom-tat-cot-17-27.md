# 07. Chế độ Tóm tắt — Cột 17 đến 27

Nhóm này chuyển từ “mã có xu hướng gì?” sang “hành vi cung/cầu và cấu trúc hiện tại là gì?”.

## Cột 17 — Trạng thái biên độ

### Công thức nguồn

```text
RawSpread = High − Low
RSpread = RawSpread hiện tại / trung bình RawSpread của N thanh trước
```

### Dải trạng thái

| RSpread | Trạng thái |
|---|---|
| Không hợp lệ | DỮ LIỆU KHÔNG ĐỦ |
| `< 0,60` | RẤT HẸP |
| `0,60 đến < 0,80` | HẸP |
| `0,80 đến < 1,20` | BÌNH THƯỜNG |
| `1,20 đến < 1,60` | RỘNG |
| `>= 1,60` | RẤT RỘNG |

Spread rộng chỉ nói giá dao động mạnh hơn chuẩn; chưa nói bên nào thắng.

## Cột 18 — Vị trí đóng cửa %

### Công thức

```text
ClosePosition = (Close − Low) / (High − Low)
Cột hiển thị = 100 × ClosePosition
```

Nếu `High == Low`, AFL quy ước vị trí đóng cửa bằng 50%.

### Cách đọc

- Gần 0%: đóng cửa sát đáy thanh.
- Gần 50%: đóng giữa thanh.
- Gần 100%: đóng sát đỉnh thanh.

## Cột 19 — Trạng thái đóng cửa

| Close Position | Trạng thái |
|---|---|
| Không hợp lệ | DỮ LIỆU KHÔNG ĐỦ |
| `< 25%` | ĐÓNG CỬA YẾU |
| `25% đến < 40%` | VÙNG THẤP |
| `40% đến 60%` | VÙNG GIỮA |
| `> 60% đến 75%` | VÙNG CAO |
| `> 75%` | ĐÓNG CỬA MẠNH |

Vị trí đóng cửa phải được đọc cùng hướng giá. Một thanh giảm nhưng đóng cao có thể là phản ứng cầu; một thanh tăng nhưng đóng thấp có thể là cung xuất hiện.

## Cột 20 — Nỗ lực/Kết quả

### Nỗ lực

Được phân loại từ RVOL:

- Cao: `RVOL >= HighEffortRVOL`.
- Thấp: `RVOL <= LowEffortRVOL`.
- Bình thường: nằm giữa.

### Kết quả theo hướng

```text
DirectionalProgress = (Close − Close trước) / ATR trước
```

Phân loại dùng trị tuyệt đối:

- Cao: `abs(progress) >= ngưỡng cao`.
- Thấp: `abs(progress) <= ngưỡng thấp`.
- Bình thường: nằm giữa.

### Chín tổ hợp

| Nỗ lực | Kết quả | Cách đặt câu hỏi |
|---|---|---|
| Cao | Cao | Nỗ lực tạo kết quả; hướng giá có phù hợp bối cảnh? |
| Cao | Bình thường | Có lực đối ứng đáng kể? |
| Cao | Thấp | Nỗ lực lớn nhưng giá đi ít; nghi ngờ hấp thụ/đối ứng |
| Bình thường | Cao | Giá đi hiệu quả với nỗ lực vừa phải |
| Bình thường | Bình thường | Trạng thái cân bằng tương đối |
| Bình thường | Thấp | Ít tiến triển; cần xem vị trí |
| Thấp | Cao | Giá đi xa với volume thấp; có thể cung/cầu đối ứng cạn |
| Thấp | Bình thường | Nỗ lực thấp, kết quả vừa |
| Thấp | Thấp | Ít giao dịch và ít tiến triển |

Không có tổ hợp nào mặc định tốt hoặc xấu. Hướng của `DirectionalProgress`, vị trí trong vùng và phản ứng sau đó mới hoàn thiện ý nghĩa.

---

## Cột 21 — Trạng thái vùng

| Trạng thái | Ý nghĩa |
|---|---|
| KHÔNG CÓ BỐI CẢNH VÙNG | Chưa hình thành vùng công bố được |
| BỐI CẢNH VÙNG ĐANG HOẠT ĐỘNG | Có đúng một vùng đang theo dõi |
| BỐI CẢNH KẾT THÚC PHASE E | Cấu trúc đã rời vùng theo chuỗi Phase E |
| NHIỀU BỐI CẢNH MƠ HỒ | Có hơn một bối cảnh, AFL không chọn tùy ý |
| VÙNG ĐÃ VÔ HIỆU/BỊ THAY THẾ | Giá rời quá xa hoặc cấu trúc mới thay thế |

## Cột 22 — Vị trí trong vùng %

### Công thức

```text
RangePosition = 100 × (Close − RangeLow) / (RangeHigh − RangeLow)
```

### Cách đọc

- `< 0%`: dưới cận dưới.
- `0%`: tại cận dưới.
- `0–50%`: nửa dưới.
- `50–100%`: nửa trên.
- `100%`: tại cận trên.
- `> 100%`: trên cận trên.

Giá ngoài 0–100% không phải lỗi; nó cho biết giá đã xuyên hoặc rời vùng.

## Cột 23 — Pha Wyckoff

| Mã logic | Hiển thị | Cách hiểu |
|---:|---|---|
| 0 | BỐI CẢNH KHÔNG ĐỦ | Chưa đủ một vùng duy nhất |
| 1 | ĐANG PHASE A | Mới hình thành vùng từ bằng chứng chặn đà |
| 2 | ĐANG PHASE B | Vùng đã đủ tuổi, còn phát triển nguyên nhân |
| 3 | ỨNG VIÊN PHASE C | Có sự kiện kiểm tra cuối tiềm năng, chưa xác nhận |
| 4 | ĐANG PHASE C | Phản ứng sau sự kiện đã xác nhận hướng kiểm tra |
| 5 | ĐANG PHASE D | Đã có hướng chi phối qua SOS/SOW hoặc LPS/LPSY |
| 6 | ĐANG PHASE E | Chuỗi phá vỡ–retest–tiếp diễn đã hoàn chỉnh |
| 7 | VÙNG ĐÃ VÔ HIỆU/BỊ THAY THẾ | Bối cảnh cũ không còn được dùng |

Pha là trạng thái của máy cấu trúc, không phải nhãn vẽ tay tuyệt đối.

## Cột 24 — Phát triển cấu trúc

Cột này dùng cùng mã pha nhưng đổi sang ngôn ngữ mô tả:

| Pha | Phát triển cấu trúc |
|---:|---|
| 1 | CHẶN ĐÀ/HÌNH THÀNH VÙNG |
| 2 | PHÁT TRIỂN VÙNG |
| 3 | ĐANG ĐÁNH GIÁ PHÉP THỬ CUỐI |
| 4 | PHÉP THỬ CUỐI + TIẾP DIỄN |
| 5 | HƯỚNG CHI PHỐI |
| 6 | THOÁT VÙNG/MỞ RỘNG XU HƯỚNG |
| 7 | KẾT THÚC/BỊ THAY THẾ |

Hai cột 23 và 24 không phải hai phép tính khác nhau; cột 24 diễn giải phát triển cấu trúc dễ đọc hơn.

## Cột 25 — Giả thuyết họ

| Trạng thái | Ý nghĩa |
|---|---|
| CHƯA XÁC ĐỊNH Ở VÙNG DƯỚI | Vùng khởi phát từ mốc dưới nhưng chưa xác nhận hướng |
| CHƯA XÁC ĐỊNH Ở VÙNG TRÊN | Vùng khởi phát từ mốc trên nhưng chưa xác nhận hướng |
| GIẢ THUYẾT TÍCH LŨY | Trước đó giảm, sau đó xác nhận hướng tăng |
| GIẢ THUYẾT TÁI PHÂN PHỐI | Trước đó giảm, sau đó xác nhận hướng giảm |
| GIẢ THUYẾT PHÂN PHỐI | Trước đó tăng, sau đó xác nhận hướng giảm |
| GIẢ THUYẾT TÁI TÍCH LŨY | Trước đó tăng, sau đó xác nhận hướng tăng |
| BẰNG CHỨNG HỖN HỢP/MÂU THUẪN | Tồn tại cả dấu vết tăng và giảm hoặc nhiều bối cảnh |
| BỐI CẢNH CHƯA XÁC ĐỊNH | Xu hướng trước không đủ rõ |

Từ “giả thuyết” là chủ ý: AFL đang suy luận từ bằng chứng, không tuyên bố chắc chắn ý đồ của dòng tiền lớn.

## Cột 26 — Bằng chứng

Máy trạng thái ghi nhớ việc đã xuất hiện bằng chứng tăng/giảm trong vùng.

### Bằng chứng tăng gồm

- Spring xác nhận.
- Supply Test xác nhận.
- SOS.
- LPS xác nhận.
- No Supply xác nhận.
- Phản ứng hấp thụ tăng.

### Bằng chứng giảm gồm

- Upthrust xác nhận.
- SOW.
- LPSY xác nhận.
- No Demand xác nhận.
- Phản ứng hấp thụ giảm.

### Trạng thái

- Bằng chứng không đủ.
- Chỉ có bằng chứng tăng.
- Chỉ có bằng chứng giảm.
- Bằng chứng hỗn hợp.

## Cột 27 — Bối cảnh xu hướng

Được suy ra từ Family:

- Chưa xác định.
- Giả thuyết tăng.
- Giả thuyết giảm.
- Hỗn hợp/mâu thuẫn.

Đây là cột tổng hợp nhanh. Khi cần biết nguyên nhân, phải đọc Family, Evidence và các cột sự kiện trong chế độ Phân tích.

## Cách kết hợp cột 21–27

Một chuỗi nhất quán tăng có thể là:

```text
Vùng đang hoạt động
→ Phase C hoặc D
→ Giả thuyết tích lũy/tái tích lũy
→ Chỉ có bằng chứng tăng
→ Bối cảnh giả thuyết tăng
```

Nếu Family tăng nhưng Evidence hỗn hợp, không bỏ qua mâu thuẫn. Mở chế độ Phân tích để xem sự kiện giảm nào đã được ghi nhận.

