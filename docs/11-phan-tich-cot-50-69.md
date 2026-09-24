# 11. Chế độ Phân tích — Cột 50 đến 69

Nhóm 50–63 giải thích bằng chứng hành vi giá–khối lượng. Nhóm 64–69 cho biết AFL đã chọn vùng cấu trúc nào làm bối cảnh. Đây là cầu nối giữa VSA từng thanh và Wyckoff theo chuỗi.

## Quy ước xác nhận

AFL phân biệt ba lớp:

1. **Ứng viên tại thanh gốc** — hình dạng hiện tại giống một mẫu.
2. **Phản ứng tại thanh kế tiếp** — thị trường có xác nhận hay không.
3. **Cờ đã xác nhận** — kết quả nhị phân để các tầng sau sử dụng.

Vì vậy một dòng `ỨNG VIÊN CƠ BẢN — CHƯA XÁC NHẬN` không được đọc như tín hiệu hoàn chỉnh.

## Cột 50 — Ứng viên No Demand

Một thanh là ứng viên No Demand khi đồng thời:

```text
Close hiện tại > Close trước
AND Volume hiện tại < Volume của từng thanh trong 2 thanh trước
AND RSpread < 0,80
```

Ý nghĩa: giá nhích lên trên biên hẹp với khối lượng suy giảm. Đây mới là dấu hiệu thiếu cầu tiềm năng; cần phản ứng giảm ở thanh kế tiếp để xác nhận.

## Cột 51 — Ứng viên No Supply

```text
Close hiện tại < Close trước
AND Volume hiện tại < Volume của từng thanh trong 2 thanh trước
AND RSpread < 0,80
```

Ý nghĩa: giá lùi xuống trên biên hẹp với khối lượng suy giảm. Chỉ được xem là thiếu cung tiềm năng cho tới khi thanh kế tiếp phản ứng tăng.

## Cột 52 — Phản ứng No Demand

Nếu thanh trước là ứng viên No Demand:

```text
Close hiện tại < Close của thanh ứng viên → đã xác nhận
```

Các trạng thái có thể gồm thiếu dữ liệu, không có ứng viên trước đó, phản ứng chưa xác nhận và phản ứng giá cơ bản đã xác nhận.

## Cột 53 — Phản ứng No Supply

Nếu thanh trước là ứng viên No Supply:

```text
Close hiện tại > Close của thanh ứng viên → đã xác nhận
```

## Cột 54 — No Demand đã xác nhận

Cờ `1/0`. Giá trị `1` chỉ xuất hiện ở thanh xác nhận, không phải ở thanh gốc. Khi nhìn biểu đồ cần lùi một thanh để xem mẫu No Demand gốc.

## Cột 55 — No Supply đã xác nhận

Cờ `1/0`. Giá trị `1` nằm trên thanh phản ứng xác nhận; thanh ứng viên nằm ngay trước đó.

## Cột 56 — Nỗ lực cao trào

```text
RSpread >= 1,60
AND RVOL >= 1,80
```

Nỗ lực cao trào chỉ cho biết biên độ và khối lượng cùng rất lớn. Nó không cho biết bên mua hay bên bán chiến thắng. Phải đọc:

- hướng Close và áp lực phá biên;
- vị trí đóng cửa;
- vị trí trong vùng;
- phản ứng của các thanh sau.

## Cột 57 — Hấp thụ

```text
RVOL >= 1,80
AND |Tiến triển theo hướng ATR| <= 0,35
```

Khối lượng rất cao nhưng tiến triển đóng cửa nhỏ gợi ý có lực đối ứng hấp thụ. AFL chưa khẳng định đó là hấp thụ cung hay hấp thụ cầu ở cột này; hướng được đánh giá bằng phản ứng tăng/giảm ở cột 79–80.

## Cột 58 — Áp lực giảm

Cờ `1` khi:

```text
Low hiện tại < Low trước
OR Close hiện tại < Close trước
```

Đây là mô tả hướng quan sát, không phải kết luận phân phối.

## Cột 59 — Áp lực tăng

Cờ `1` khi:

```text
High hiện tại > High trước
OR Close hiện tại > Close trước
```

Cả cột 58 và 59 có thể cùng bằng `1` nếu thanh hiện tại mở rộng hai phía hoặc tạo diễn biến hỗn hợp.

## Cột 60 — Khối lượng chặn đà

Điều kiện chính:

```text
RVOL >= 1,80
AND có áp lực giảm
AND Vị trí đóng cửa >= 0,50
```

Giá bị ép xuống nhưng đóng cửa từ giữa biên trở lên trong bối cảnh khối lượng rất cao. Đây là ứng viên stopping volume, chưa đủ để khẳng định đáy.

## Cột 61 — Phản ứng chặn đà

Tại thanh sau ứng viên stopping volume:

```text
Low hiện tại >= Low của thanh gốc
AND Close hiện tại > Close của thanh gốc
```

Nếu không giữ được Low hoặc Close không cải thiện, phản ứng chưa xác nhận.

## Cột 62 — Phản ứng chặn đà đã xác nhận

Cờ `1/0` phục vụ tầng cấu trúc. Cờ nằm ở thanh xác nhận; thanh stopping volume gốc nằm ngay trước đó.

## Cột 63 — Hành vi sẵn sàng

- `1`: tầng hành vi đã có đủ dữ liệu cần thiết.
- `0`: chưa đủ nền để các bằng chứng hành vi được tin cậy.

Không suy diễn `0` thành “không có hành vi”; `0` nghĩa là chưa thể đánh giá chắc bằng thuật toán hiện tại.

---

## Cột 64 — Phía hình thành vùng

| Mã ý nghĩa | Diễn giải |
|---|---|
| Không có/mơ hồ | Không chọn được đúng một vùng |
| Phía dưới | Vùng được hình thành từ neo đáy và bằng chứng chặn giảm/hấp thụ |
| Phía trên | Vùng được hình thành từ neo đỉnh và bằng chứng chặn tăng/phân phối |

Đây là nguồn hình thành vùng, không phải dự báo cuối cùng. Một vùng từ phía dưới vẫn có thể thất bại; một vùng từ phía trên vẫn cần bằng chứng giảm để thành giả thuyết phân phối.

## Cột 65 — Cận dưới vùng

Giá cận dưới của vùng cấu trúc được chọn. Các phép tính vị trí, Spring, SOW, vùng mua và P&F đều có thể phụ thuộc vào mốc này.

Nếu cột trống/không hợp lệ, kiểm tra:

- `Cấu trúc sẵn sàng`;
- số lượng bối cảnh hợp lệ;
- tuổi Pivot và cửa sổ range;
- các Parameter ngắn/trung/dài hạn.

## Cột 66 — Cận trên vùng

Giá cận trên của vùng được chọn. Đây là mốc tham chiếu cho Upthrust, SOS, LPS và vùng mua Phase D/E.

## Cột 67 — Tuổi vùng

Số thanh đã trôi qua từ khi vùng bắt đầu. Tuổi vùng được dùng để:

- xác định vùng đã đủ thời gian chuyển từ Phase A sang B chưa;
- kiểm tra vùng có vượt tuổi tối đa không;
- đánh giá độ dài của khoảng P&F.

Tuổi lớn không tự động tốt hơn. Vùng quá già có thể bị hết hiệu lực theo Parameter tương ứng.

## Cột 68 — Xu hướng trước

| Trạng thái | Ý nghĩa |
|---|---|
| Dữ liệu không đủ | Không đủ chuỗi để đánh giá |
| Tăng | Xu hướng trước vùng nghiêng tăng |
| Giảm | Xu hướng trước vùng nghiêng giảm |
| Hỗn hợp | Không thống nhất |

Xu hướng trước hỗ trợ phân biệt tích lũy/tái tích lũy và phân phối/tái phân phối, nhưng không tự quyết định Family.

## Cột 69 — Cấu trúc sẵn sàng

- `1`: vùng có đủ mốc, biên và dữ liệu để tầng sự kiện xử lý.
- `0`: tầng sự kiện phải fail-closed, tức không tự dựng sự kiện từ dữ liệu không chắc chắn.

## Ví dụ truy nguyên

Giả sử cột Tóm tắt cho thấy `Bằng chứng = CHỈ TĂNG`, nhưng bạn nghi ngờ:

1. Xem cột 50–55 để tìm No Supply đã xác nhận.
2. Xem 57 và 79 để tìm hấp thụ có phản ứng tăng.
3. Xem 60–62 để tìm stopping volume có phản ứng giữ đáy.
4. Xem 64–69 để xác nhận các bằng chứng ấy được đặt trong đúng một vùng hợp lệ.
5. Sau đó mới xem sự kiện Wyckoff ở cột 70–82.

Điểm cốt lõi: một mẫu VSA đúng hình dạng nhưng nằm sai vị trí cấu trúc có ý nghĩa yếu hơn một chuỗi bằng chứng nhất quán trong vùng.

