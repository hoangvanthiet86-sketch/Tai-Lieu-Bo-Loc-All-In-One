# 01. Tổng quan và kiến trúc

## 1. Bộ lọc giải quyết bài toán gì?

AFL thực hiện hai công việc nối tiếp:

1. **Thu hẹp thị trường** bằng các điều kiện định lượng quen thuộc: MA, thanh khoản, vị trí giá, Bollinger và biến động phiên.
2. **Làm giàu danh sách đã lọc** bằng cách tính VSA, cấu trúc vùng, sự kiện Wyckoff, Phase/Family/Evidence, vùng mua, thứ tự ưu tiên và mục tiêu P&F.

Điểm quan trọng là bước 2 không được phép thay đổi tập mã của bước 1.

## 2. Stage 1 — Selection Authority

Stage 1 là nơi duy nhất gán biến `Filter`.

Một mã chỉ xuất hiện khi đồng thời đạt:

```text
lastbarinrange
AND đủ dữ liệu
AND chế độ MA hướng lên
AND KL MA20 > ngưỡng tối thiểu
AND khoảng giá so với MA20
AND khoảng khối lượng so với KL MA20
AND điều kiện thứ tự MA nếu bật
AND bộ lọc màu MA20
AND bộ lọc màu giá đóng cửa
AND BBW <= ngưỡng tối đa
```

Do dùng `Status("lastbarinrange")`, mỗi mã chỉ xuất hiện tại thanh cuối của phạm vi Analysis đang chạy.

## 3. Stage 2 — Enrichment

Stage 2 chỉ chạy khi:

```afl
LIO_RunDeepAnalysis = Nz(LastValue(Filter), 0) == 1;
```

Nếu mã không đạt Stage 1:

- Khối phân tích nặng không chạy.
- Các cột Stage 2 giữ trạng thái mặc định “không đủ/không áp dụng”.
- Stage 2 không thể đưa mã trở lại danh sách.

## 4. Các lớp bên trong Stage 2

### Stage 2A — Core VSA

Tính:

- RVOL.
- RSpread.
- Vị trí đóng cửa.
- Robust ATR.
- Tiến triển theo hướng/ATR.
- Effort versus Result.

### Stage 2B — Hành vi

Tính:

- Ứng viên No Demand/No Supply.
- Phản ứng xác nhận ở thanh kế tiếp.
- Climactic Effort.
- Absorption.
- Stopping Volume và phản ứng sau đó.

### Stage 2C — Structure/Location/Pivot

Tính ba vùng tham chiếu loại trừ thanh hiện tại:

- Ngắn hạn.
- Trung hạn.
- Dài hạn.

Đồng thời xác nhận Pivot với số thanh trái/phải do người dùng đặt.

### Stage 2D — Sự kiện theo vùng

Tính:

- Spring/Shakeout.
- Supply Test.
- Upthrust/UTAD-like.
- SOS/LPS.
- SOW/LPSY.
- Phản ứng hấp thụ tăng/giảm.

### Stage 2E — Phase/Family/Evidence

Chạy máy trạng thái nhân quả để mô tả:

- Phase A/B/C/D/E.
- Giả thuyết tích lũy, tái tích lũy, phân phối, tái phân phối.
- Bằng chứng tăng, giảm hoặc hỗn hợp.
- Bối cảnh xu hướng.

### Stage 2F — Vùng mua và xếp hạng

Chỉ tạo đề xuất trong bối cảnh tăng, một vùng rõ ràng và bằng chứng không mâu thuẫn. Kết quả gồm:

- Loại cơ hội.
- Vùng mua.
- Giá vô hiệu tham chiếu.
- Trạng thái giá so với vùng.
- Khóa sắp xếp `4 → 3 → 2 → 1 → 0`.

### Stage 2G — P&F Horizontal Count

Chỉ chạy cho trạng thái giá 3 hoặc 4, tức là:

- Gần vùng mua.
- Đang trong vùng mua.

Kết quả gồm số cột đếm ngang, box, dòng đếm, ba mức mục tiêu và dư địa đến mục tiêu kế tiếp chưa đạt.

## 5. Ba chế độ hiển thị dùng chung một logic

Chế độ hiển thị chỉ thay đổi số cột, không thay đổi công thức:

- Tóm tắt: kết quả vận hành.
- Phân tích: Tóm tắt + chẩn đoán.
- Đầy đủ: toàn bộ đầu ra kỹ thuật.

Vì vậy, cùng một mã, cùng ngày, cùng Parameters và cùng Periodicity phải có cùng giá trị ở các cột chung giữa ba chế độ.

## 6. Thứ tự sắp xếp

Tóm tắt/Phân tích sắp giảm dần theo cột 33 `Uu tien mua`, rồi theo cột 12 `Thu tu MA`.

Đầy đủ sắp giảm dần theo cột 112 `Uu tien mua`, rồi theo cột 15 `Thu tu MA`.

Phần nguyên của khóa ưu tiên giữ thứ tự nhóm. Phần thập phân chỉ sắp xếp bên trong cùng nhóm, không thể giúp nhóm thấp vượt nhóm cao hơn.

## 7. Giới hạn thiết kế

- Không phải hệ thống giao dịch hoàn chỉnh.
- Không có quản trị vốn hoặc kích thước vị thế.
- `Gia vo hieu tham chieu` là mức cấu trúc tham khảo, không tự động là stop-loss cuối cùng.
- Không đánh giá bối cảnh chỉ số thị trường chung.
- Không thay thế việc xác minh biểu đồ.
- Kết quả phụ thuộc chất lượng dữ liệu, khung thời gian và Parameters.

