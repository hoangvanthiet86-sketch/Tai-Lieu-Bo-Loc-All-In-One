# 04. Parameters Stage 1 — Bộ lọc nền

Stage 1 có 12 Parameters và là phần duy nhất quyết định mã có lọt vào bảng kết quả hay không.

## 1. Công thức Filter tổng quát

```text
Filter =
    thanh cuối trong Range
    AND đủ dữ liệu
    AND chế độ MA hướng lên
    AND KL MA20 > KL MA20 tối thiểu
    AND % Close/MA20 nằm trong khoảng đặt trước
    AND % Volume/KL MA20 nằm trong khoảng đặt trước
    AND điều kiện thứ tự MA nếu bật
    AND điều kiện màu MA20
    AND điều kiện màu Close
    AND BBW <= BBW tối đa
```

Mọi điều kiện là phép `AND`: chỉ một điều kiện sai cũng đủ loại mã.

---

## 2. Tối thiểu và tối đa % đóng cửa so với MA20

### Công thức

```text
CloseVsMA20Pct = 100 × (Close − MA20) / MA20
```

### Mặc định

- Tối thiểu: `-100`.
- Tối đa: `100`.

Mặc định gần như không siết điều kiện này.

### Cách đọc

- `+5`: giá đóng cửa cao hơn MA20 khoảng 5%.
- `0`: giá bằng MA20.
- `-3`: giá thấp hơn MA20 khoảng 3%.

### Tác động của thiết lập

- Nâng mức tối thiểu: loại các mã nằm quá thấp so với MA20.
- Hạ mức tối đa: loại các mã đã kéo quá xa khỏi MA20.
- Đặt khoảng hẹp quanh 0: tìm các mã đang gần MA20.

### Ví dụ

`-2 đến +5` cho phép giá nằm từ 2% dưới MA20 đến 5% trên MA20.

### Sai lầm thường gặp

- Dùng ngưỡng dương cao rồi cho rằng mã càng cao càng tốt.
- Quên rằng giá xa MA20 có thể làm tỷ lệ lợi nhuận/rủi ro kém.
- Đổi ngưỡng nhưng không ghi lại khi so sánh hai báo cáo.

---

## 3. Tối thiểu và tối đa % khối lượng so với KL MA20

### Công thức

```text
VolVsVolMA20Pct = 100 × (Volume − VolMA20) / VolMA20
```

Đây không phải RVOL. Ví dụ:

- `VolVsVolMA20Pct = 50%` tương ứng Volume bằng 1,5 lần KL MA20.
- `VolVsVolMA20Pct = -40%` tương ứng Volume bằng 0,6 lần KL MA20.

### Mặc định

- Tối thiểu: `-100`.
- Tối đa: `1000`.

Mặc định rất rộng.

### Tác động

- Tăng mức tối thiểu: ưu tiên phiên có hoạt động cao hơn bình thường.
- Giảm mức tối đa: loại các phiên có khối lượng quá đột biến.

### Cảnh báo

Khối lượng cao không tự động là cầu mạnh. Cần đọc thêm:

- Giá tăng hay giảm.
- RSpread.
- Vị trí đóng cửa.
- Tiến triển theo hướng.
- Phản ứng các thanh sau.

---

## 4. Chỉ lấy MA20 > MA50 > MA100

### Logic

```text
maOrder = MA20 > MA50 AND MA50 > MA100
```

### Lựa chọn

- `Khong`: không bắt buộc thứ tự.
- `Co`: bắt buộc thứ tự tăng.

### Điều không được hiểu nhầm

Thứ tự MA không nói các MA đang dốc lên. Một mã có thể có:

```text
MA20 > MA50 > MA100
```

nhưng MA20 đang giảm so với phiên trước. Ngược lại, cả ba MA có thể cùng hướng lên nhưng chưa xếp đúng thứ tự.

Đây là hai nhóm điều kiện độc lập:

- `Che do MA huong len`: độ dốc một thanh.
- `Chi lay MA20>MA50>MA100`: vị trí tương đối.

---

## 5. Chế độ MA hướng lên

### Công thức từng MA

```text
up20  = MA20  hiện tại > MA20  thanh trước
up50  = MA50  hiện tại > MA50  thanh trước
up100 = MA100 hiện tại > MA100 thanh trước
up200 = MA200 hiện tại > MA200 thanh trước
```

### Các chế độ

| Chế độ | Điều kiện bắt buộc | Đặc điểm |
|---|---|---|
| `MA20` | `up20` | Rộng nhất, nhạy với chuyển động ngắn hạn |
| `MA20+MA50` | `up20 AND up50` | Yêu cầu ngắn và trung hạn cùng cải thiện |
| `MA20+MA50+MA100` | Thêm `up100` | Chặt hơn, ít mã hơn |
| `MA20+MA50+MA100+MA200` | Thêm `up200` | Chặt nhất, đòi hỏi cả xu hướng dài hạn |

### Độ dốc được đo thế nào?

Đây là so sánh giữa hai thanh liên tiếp, không phải góc của đường MA và không phải hồi quy nhiều phiên.

Chênh lệch rất nhỏ nhưng dương vẫn được coi là hướng lên.

### Cách chọn

- Muốn phát hiện sớm: `MA20`.
- Muốn cân bằng: `MA20+MA50`.
- Muốn ưu tiên cấu trúc xu hướng trưởng thành: thêm MA100/MA200.

Không có chế độ nào tốt nhất trong mọi thị trường.

---

## 6. Chế độ hiển thị kết quả

| Giá trị | Kết quả |
|---|---:|
| `Tom tat` | 39 cột |
| `Phan tich` | 87 cột |
| `Day du` | 123 cột |

Tham số này không tác động Filter và không thay đổi kết quả tính toán; chỉ thay đổi số cột hiển thị.

---

## 7. Khối lượng MA20 tối thiểu

### Công thức

```text
VolMA20 = MA(Volume, 20)
Điều kiện = VolMA20 > minVolMA20
```

Lưu ý là dấu `>` nghiêm ngặt: bằng đúng ngưỡng vẫn không đạt.

### Mặc định

`50.000` đơn vị khối lượng.

### Ý nghĩa

Đây là bộ lọc thanh khoản bình quân, không phải khối lượng của riêng phiên hiện tại.

### Tác động

- Tăng ngưỡng: ít mã, thanh khoản cao hơn.
- Giảm ngưỡng: nhiều mã hơn nhưng tăng rủi ro trượt giá và khó thoát vị thế.

Ngưỡng phù hợp phụ thuộc thị trường, đơn vị Volume của nguồn dữ liệu và quy mô vị thế.

---

## 8. Chu kỳ Bollinger

### Công thức

```text
BBMid   = MA(Close, Period)
BBStd   = StDev(Close, Period)
BBUpper = BBMid + 2 × BBStd
BBLower = BBMid − 2 × BBStd
```

### Mặc định

`20`, phạm vi `5–50`.

### Tác động

- Chu kỳ ngắn: BBW phản ứng nhanh, nhiễu hơn.
- Chu kỳ dài: ổn định hơn, chậm hơn.

---

## 9. Độ rộng Bollinger tối đa

### Công thức

```text
BBW = 100 × (BBUpper − BBLower) / BBMid
```

### Điều kiện Filter

```text
BBW <= ngưỡng tối đa
```

### Mặc định

`5%`.

### Ý nghĩa

Ngưỡng thấp ưu tiên trạng thái co hẹp biến động. Tuy nhiên co hẹp không cho biết hướng phá vỡ.

### Tác động

- Giảm BBW tối đa: ít mã, nền chặt hơn.
- Tăng BBW tối đa: nhiều mã, chấp nhận biến động rộng hơn.

---

## 10. Lọc màu MA20 (0–2)

### Logic hiện tại

```text
ma20Green    = Close >= MA20
ma20NotBlack = ma20Green
```

| Mã | Điều kiện |
|---:|---|
| 0 | Không lọc |
| 1 | Close ≥ MA20 |
| 2 | Close ≥ MA20 |

Trong v1.7.0, chế độ 1 và 2 **đang tương đương về logic**. Tên “không đen” được giữ từ giao diện gốc nhưng biến hiện tại được gán bằng `ma20Green`.

Tài liệu phải công bố rõ để người dùng không kỳ vọng hai kết quả khác nhau.

---

## 11. Lọc màu giá đóng cửa (0–6)

Màu được suy ra từ `% TD`:

| Mã | Điều kiện |
|---:|---|
| 0 | Không lọc |
| 1 | Tím: `% TD >= 6%` |
| 2 | Hồng: `3% <= % TD < 6%` |
| 3 | Xanh: `0 < % TD < 3%` |
| 4 | Tăng bất kỳ: `% TD > 0` |
| 5 | Không tăng: `% TD <= 0` |
| 6 | Không đen: tím hoặc hồng hoặc xanh, tức `% TD > 0` |

Trong v1.7.0, chế độ 4 và 6 **tương đương về tập mã** vì cả hai đều là `% TD > 0`.

---

## 12. Đủ dữ liệu

Stage 1 yêu cầu có giá trị hợp lệ cho:

- MA200 hiện tại và trước đó.
- KL MA20.
- Close trước.
- Đỉnh 52 tuần loại trừ tuần hiện tại.
- Trạng thái xu hướng tuần.

Vì vậy dù chọn chỉ MA20, mã vẫn cần đủ lịch sử để tính MA200 và dữ liệu tuần. Một mã mới niêm yết có thể không xuất hiện dù MA20 đã tồn tại.

## 13. Thứ tự điều chỉnh Parameters

Khi muốn giảm số mã, nên siết theo thứ tự:

1. Thanh khoản tối thiểu.
2. Chế độ MA hướng lên.
3. Thứ tự MA.
4. Khoảng giá so với MA20.
5. BBW.
6. Khối lượng phiên hiện tại.
7. Màu Close.

Không nên thay nhiều tham số cùng lúc vì sẽ khó xác định điều kiện nào làm thay đổi tập mã.

