# 16. Xử lý lỗi và kiểm thử

Chương này tách ba loại vấn đề: lỗi biên dịch, lỗi khi Explore và kết quả chạy được nhưng không đúng kỳ vọng.

## 1. Verify Formula không lỗi nhưng Explore báo lỗi

`Verify Formula` chủ yếu kiểm tra cú pháp và khả năng biên dịch. Khi Explore, AFL mới nhận periodicity, Range, dữ liệu từng mã, Parameters đã lưu và ngữ cảnh Analysis. Vì vậy chạy thực tế có thể phát hiện lỗi mà Verify không thấy.

Checklist:

1. Xác nhận đúng tên file và phiên bản `v1.7.0_PNF_DemNgang_XepHang`.
2. Đóng/mở lại Analysis hoặc chọn lại AFL để tránh đang chạy bản cũ.
3. Kiểm tra periodicity, Apply to và Range.
4. Bấm Parameters và đối chiếu giá trị đang lưu; AmiBroker có thể giữ cấu hình cũ theo Analysis.
5. Kiểm tra database có đủ High, Low, Close, Volume và lịch sử.
6. Mở `Details` và ghi nguyên văn thông báo, dòng và cột lỗi.

### Lỗi `INVALID ANALYSIS PERIODICITY`

Thông báo buộc đặt Daily thuộc một bản AFL cũ từng khóa periodicity. v1.7.0 hiện tại không còn lệnh chặn chỉ chạy Daily. Nếu vẫn thấy lỗi này, Analysis đang nạp file cũ hoặc một bản sao khác, không phải mã v1.7.0 trong tài liệu.

## 2. Không có kết quả

Kiểm tra theo thứ tự từ ngoài vào trong:

- `Apply to` có đúng watchlist/market không?
- Range có bao gồm thanh có dữ liệu không?
- dữ liệu có ít nhất đủ MA200 và dữ liệu tuần không?
- KL MA20 có vượt ngưỡng nghiêm ngặt `>` không?
- BBW có nhỏ hơn hoặc bằng mức tối đa không?
- màu MA20/Close có đang khóa ngoài ý muốn không?
- khoảng Close/MA20 và Volume/KL MA20 có bị đặt ngược không?
- chế độ MA có quá chặt không?

Thử hồ sơ nền, sau đó bật lại từng điều kiện để tìm cổng loại mã.

## 3. Số mã khác giữa Tóm tắt và Phân tích

Với cùng dữ liệu, ngày, Range và Parameters, ba chế độ hiển thị phải dùng cùng Stage 1 Filter. Chế độ hiển thị chỉ thay số cột.

Nếu số mã khác:

1. Chụp lại toàn bộ Parameters trước mỗi lần chạy.
2. Xác nhận cùng periodicity và Range.
3. Xác nhận dữ liệu không vừa được cập nhật giữa hai lần.
4. Kiểm tra đang dùng cùng một Analysis và cùng file AFL.
5. So cột mã và `Ưu tiên mua`, không chỉ nhìn số dòng đang cuộn.

## 4. Kiểm thử tính bao hàm của chế độ MA

Giữ mọi tham số khác giống hệt. Kỳ vọng toán học:

```text
MA20+50+100+200 ⊆ MA20+50+100 ⊆ MA20+50 ⊆ MA20
```

Nếu quan hệ này không đúng, trước tiên kiểm tra dữ liệu/Range/Parameters giữa các lần chạy. Các chế độ chặt hơn chỉ thêm phép `AND`, nên không thể tạo mã mới so với chế độ rộng hơn trên cùng một snapshot.

## 5. Kiểm thử ranh giới toán tử

Các điểm nên thử có chủ ý:

| Điều kiện | Toán tử đáng chú ý |
|---|---|
| KL MA20 tối thiểu | `>`; bằng ngưỡng không đạt |
| BBW tối đa | `<=`; bằng ngưỡng đạt |
| Khoảng Close/MA20 | nằm trong cả min và max |
| Trong vùng mua | bao gồm cả ZoneLow và ZoneHigh |
| Gần vùng mua | khoảng cách `<=` ngưỡng |
| Số cột P&F | phải `>=` ngưỡng tối thiểu |

Kiểm thử ranh giới giúp phân biệt lỗi logic với kỳ vọng sai về `>` và `>=`.

## 6. Stage 2 không được thay đổi Filter

Một kiểm thử hồi quy quan trọng:

1. Chạy và lưu danh sách mã với cấu hình Stage 1 cố định.
2. Thay mạnh một Parameter Stage 2, ví dụ P&F Box hoặc tuổi sự kiện.
3. Chạy lại.
4. Danh sách mã phải giữ nguyên; chỉ các cột Wyckoff/VSA/vùng mua/P&F thay đổi.

Nếu số mã đổi, kiểm tra xem một Parameter Stage 1 có bị thay cùng lúc hoặc dữ liệu đã cập nhật hay không.

## 7. Kiểm thử thứ tự sắp xếp

Kỳ vọng chính:

```text
nhóm 4 → nhóm 3 → nhóm 2 → nhóm 1 → nhóm 0
```

Trong cùng nhóm 4, giá gần ZoneLow hơn được ưu tiên. Trong nhóm 3/2, giá gần ZoneHigh hơn được ưu tiên. Độ mới sự kiện chỉ là thành phần phụ.

Các chế độ dùng khóa số `Ưu tiên mua`, không sắp chữ theo cột trạng thái. Vì vậy không dùng thứ tự alphabet để đánh giá đúng/sai.

## 8. Kiểm thử P&F

Với một mã nhóm 3/4, ghi lại:

- loại setup;
- tuổi sự kiện;
- số phiên khoảng đếm;
- Box;
- Reversal;
- dòng đếm;
- đáy thực tế;
- tuổi biên phải;
- số cột X/O;
- ba mục tiêu và dư địa.

Sau đó kiểm tra:

```text
Extension = số cột × Box × Reversal
TargetLow = đáy thực tế + Extension
TargetMid = (đáy thực tế + dòng đếm)/2 + Extension
TargetHigh = dòng đếm + Extension
```

Cho phép sai khác hiển thị do làm tròn. Mục tiêu trùng ở hai chữ số phải được bỏ khỏi các ô sau.

## 9. Khi tất cả mã đều thiếu cột P&F

Đây là dấu hiệu cần kiểm tra hệ thống, không nên mặc định là ngẫu nhiên:

1. Xem số phiên khoảng đếm có đồng loạt rất ngắn không.
2. Xem Box có bất thường do ATR, tỷ lệ %, hoặc đơn vị giá không.
3. Xem tuổi biên phải có đúng với loại setup không.
4. Kiểm tra dữ liệu High/Low và điều chỉnh giá.
5. Thử một cấu hình P&F khác trên cùng snapshot, chỉ đổi Box.
6. So số cột trước/sau; không đổi đồng thời Reversal và ngưỡng tối thiểu.

## 10. Kiểm thử khung thời gian

v1.7.0 không khóa Daily, nhưng việc chạy được không đồng nghĩa mọi bộ tham số đều phù hợp mọi khung.

Với mỗi periodicity mới:

1. Chạy Tóm tắt và ghi số mã.
2. Kiểm tra 10 mã bằng biểu đồ cùng periodicity.
3. Xác nhận ý nghĩa của 20/60/120 thanh.
4. Kiểm tra dữ liệu tuần nội bộ của Stage 1 được căn đúng.
5. Đánh giá lại tuổi sự kiện, Pivot và P&F.

Không so trực tiếp kết quả Daily và Weekly như hai lần chạy cùng một mô hình thời gian.

## 11. Bộ kiểm thử hồi quy tối thiểu cho phiên bản mới

- [ ] Verify Formula không lỗi.
- [ ] Tóm tắt có đúng 39 cột.
- [ ] Phân tích có đúng 87 cột và 39 cột đầu giống Tóm tắt.
- [ ] Đầy đủ có đúng 123 cột.
- [ ] Ba chế độ có cùng danh sách mã với cùng cấu hình.
- [ ] Quan hệ bao hàm bốn chế độ MA đúng.
- [ ] Đổi Stage 2 không đổi tập mã Filter.
- [ ] Nhóm sắp xếp theo 4, 3, 2, 1, 0.
- [ ] Mục tiêu P&F trùng được khử trùng.
- [ ] Setup LPS khóa biên phải tại thanh gốc.
- [ ] Mã dưới vùng không được xếp trên mã trong/gần vùng.
- [ ] Dữ liệu thiếu trả trạng thái fail-closed, không tạo mục tiêu giả.

## 12. Mẫu báo lỗi có thể tái hiện

```text
Phiên bản AFL:
AmiBroker phiên bản:
Database/thị trường:
Mã và ngày:
Periodicity:
Apply to / Range:
Toàn bộ Parameters hoặc file APX:
Chế độ hiển thị:
Kết quả thực tế:
Kết quả kỳ vọng:
Thông báo lỗi + dòng/cột:
Ảnh các cột liên quan:
Dữ liệu OHLCV quanh sự kiện:
Các bước tái hiện từ đầu:
```

Báo cáo chỉ có ảnh một phần bảng thường chưa đủ để phân biệt lỗi dữ liệu, cấu hình, phiên bản và công thức.

