# 09. Quy trình đọc chế độ Tóm tắt

Không nên đọc 39 cột như 39 tín hiệu độc lập. Hãy đọc theo sáu cổng quyết định.

## Cổng 1 — Xác nhận dữ liệu

Kiểm tra:

- Ngày có đúng không?
- Close và KL MA20 có hợp lý không?
- Có ô “DỮ LIỆU KHÔNG ĐỦ” bất thường không?

Nếu dữ liệu sai, dừng phân tích.

## Cổng 2 — Hiểu vì sao mã lọt Stage 1

Đọc:

- `% Close/MA20`.
- `% Close/MA50`.
- Thứ tự MA.
- MA200 dốc lên.
- Tăng tuần.
- BBW.

Đối chiếu với Parameters đang dùng. Ví dụ chọn `MA20+MA50`, không được mặc định MA100/MA200 cũng đang lên.

## Cổng 3 — Đọc nỗ lực và kết quả

Đọc cùng lúc:

- RVOL/trạng thái khối lượng.
- Trạng thái biên độ.
- Vị trí đóng cửa.
- Nỗ lực/kết quả.

Ba câu hỏi:

1. Nỗ lực lớn hay nhỏ?
2. Giá di chuyển nhiều hay ít so với ATR?
3. Giá đóng ở đâu trong thanh?

Ví dụ RVOL cao + spread rộng + đóng mạnh + Close tăng có ý nghĩa khác RVOL cao + spread rộng + đóng yếu.

## Cổng 4 — Đọc vị trí cấu trúc

Đọc:

- Trạng thái vùng.
- Vị trí trong vùng.
- Pha.
- Family.
- Evidence.
- Bối cảnh xu hướng.

Ưu tiên tính nhất quán. Nếu trạng thái mâu thuẫn, chuyển Phân tích thay vì tự chọn cột mình thích.

## Cổng 5 — Đọc cơ hội và vùng mua

Theo thứ tự:

1. Có setup hay không?
2. Setup dựa vào Spring/Test, SOS hay LPS?
3. Giá nằm ở đâu so với vùng?
4. Mức vô hiệu còn giữ không?
5. SortKey thuộc nhóm nào?

Không bỏ qua mã nhóm 0 nếu mục tiêu là theo dõi sớm, nhưng không coi nhóm 0 là đề xuất mua.

## Cổng 6 — Đọc P&F

Kiểm tra:

- Có áp dụng không?
- Thiếu dữ liệu, box lỗi hay thiếu cột?
- Mục tiêu tạm tính hay đã khóa?
- Mục tiêu nào là mục tiêu kế tiếp?
- Dư địa có đủ hấp dẫn so với rủi ro cấu trúc không?

P&F đứng cuối quy trình vì mục tiêu giá không thể sửa một setup cấu trúc yếu.

## Mẫu ghi chú một dòng

```text
[Mã] — MA [chế độ]; thanh khoản [mức]; VSA [nỗ lực/kết quả];
vùng [trạng thái + vị trí]; Wyckoff [pha/family/evidence];
setup [loại]; giá [nhóm]; P&F [trạng thái + mục tiêu kế tiếp].
```

## Ví dụ diễn giải

```text
ABC — MA20 và MA50 cùng lên, KL MA20 đạt; RVOL cao nhưng kết quả
theo hướng thấp nên có lực đối ứng; giá ở nửa dưới vùng, Phase C,
giả thuyết tích lũy và chỉ có bằng chứng tăng; Spring đã xác nhận;
giá đang trong vùng mua nhóm 4; P&F mới 4 cột nên chưa có mục tiêu.
```

Kết luận đúng không phải “mua ABC”, mà là:

> ABC là ứng viên cần mở biểu đồ; vị trí vùng mua phù hợp nhưng nỗ lực/kết quả đang có lực đối ứng và P&F chưa đủ chiều rộng.

## Khi nào chuyển sang Phân tích?

- Nỗ lực/kết quả khó hiểu → xem cột 46–49.
- Nghi No Supply/No Demand → xem cột 50–55.
- Không rõ vùng → xem cột 64–69.
- Không rõ nguồn pha → xem cột 70–82.
- P&F thiếu mục tiêu → xem cột 83–87.

