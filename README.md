# Hướng dẫn Bộ lọc All In One + Wyckoff/VSA

Bộ tài liệu này giải thích cách thiết lập, cách tính và cách sử dụng kết quả của AFL:

`Loc_AllInOne_Wyckoff_Fast_v1.7.0_PNF_DemNgang_XepHang.afl`

Mục tiêu của tài liệu là giúp người dùng đi từ mức cơ bản — chạy được bộ lọc và hiểu 39 cột Tóm tắt — đến mức nâng cao — truy nguyên 48 cột chẩn đoán của chế độ Phân tích, hiểu vùng mua, thứ tự ưu tiên và mục tiêu Point & Figure (P&F).

> **Lưu ý quan trọng:** AFL là công cụ lọc và hỗ trợ phân tích. AFL không gán `Buy`, `Sell`, `Short`, `Cover`; không thay thế việc đọc biểu đồ, quản trị rủi ro và lập kế hoạch giao dịch.

## Bắt đầu nhanh

1. Mở AFL trong AmiBroker Formula Editor và chạy **Verify Formula**.
2. Mở **Analysis**, chọn AFL, chọn thị trường cần lọc và đặt **Range** phù hợp.
3. Giữ `Che do hien thi ket qua = Tom tat` trong lần chạy đầu.
4. Chọn `Che do MA huong len` theo mức độ chặt mong muốn.
5. Chạy **Explore**.
6. Đọc kết quả theo thứ tự:
   - Thanh khoản và xu hướng.
   - VSA tóm tắt.
   - Vùng và pha Wyckoff.
   - Cơ hội, vùng mua và ưu tiên.
   - Trạng thái và mục tiêu P&F.
7. Chỉ chuyển sang `Phan tich` khi cần giải thích vì sao một kết quả Tóm tắt xuất hiện.

## Ba chế độ hiển thị

| Chế độ | Số cột | Mục đích |
|---|---:|---|
| `Tom tat` | 39 | Vận hành hằng ngày và xếp hạng ứng viên |
| `Phan tich` | 87 | Giữ nguyên 39 cột Tóm tắt và bổ sung 48 cột chẩn đoán |
| `Day du` | 123 | Kiểm tra kỹ thuật toàn bộ chuỗi tính toán |

## Lộ trình đọc khuyến nghị

### Người mới sử dụng

1. [Lộ trình đọc tài liệu](docs/00-lo-trinh-doc.md)
2. [Tổng quan và kiến trúc](docs/01-tong-quan-va-kien-truc.md)
3. [Dữ liệu và quy ước tính](docs/02-du-lieu-va-quy-uoc-tinh.md)
4. [Thiết lập AmiBroker](docs/03-thiet-lap-amibroker.md)
5. [Parameters Stage 1](docs/04-parameters-stage-1.md)
6. [Tóm tắt: cột 01–16](docs/06-tom-tat-cot-01-16.md)
7. [Tóm tắt: cột 17–27](docs/07-tom-tat-cot-17-27.md)
8. [Tóm tắt: cột 28–39](docs/08-tom-tat-cot-28-39.md)
9. [Quy trình đọc chế độ Tóm tắt](docs/09-quy-trinh-doc-tom-tat.md)

### Người cần phân tích sâu

1. [Parameters Stage 2](docs/05-parameters-stage-2.md)
2. [Phân tích: cột 40–49](docs/10-phan-tich-cot-40-49.md)
3. [Phân tích: cột 50–69](docs/11-phan-tich-cot-50-69.md)
4. [Phân tích: cột 70–87](docs/12-phan-tich-cot-70-87.md)
5. [Truy nguyên và kết hợp kết quả](docs/13-truy-nguyen-va-ket-hop.md)
6. [Cấu hình mẫu](docs/14-cau-hinh-mau.md)
7. [Ví dụ thực hành](docs/15-vi-du-thuc-hanh.md)
8. [Xử lý lỗi và kiểm thử](docs/16-xu-ly-loi-va-kiem-thu.md)

## Tài liệu tra cứu

- [Bảng toàn bộ 39 Parameters](reference/01-bang-parameters.md)
- [Từ điển 39 cột Tóm tắt](reference/02-tu-dien-39-cot-tom-tat.md)
- [Từ điển 48 cột bổ sung của Phân tích](reference/03-tu-dien-48-cot-phan-tich.md)
- [Danh sách 123 cột Đầy đủ](reference/04-che-do-day-du-123-cot.md)
- [Mã và trạng thái](reference/05-ma-trang-thai.md)
- [Công thức](reference/06-cong-thuc.md)
- [Sơ đồ nguồn → kết quả](reference/07-so-do-nguon-ket-qua.md)

## Nguyên tắc đọc kết quả

- Bối cảnh quan trọng hơn một tín hiệu đơn lẻ.
- Volume lớn chỉ cho biết nỗ lực lớn; chưa cho biết bên mua hay bên bán thắng.
- Một ứng viên sự kiện chưa phải sự kiện đã xác nhận.
- Một pha Wyckoff là giả thuyết cấu trúc dựa trên bằng chứng hiện có, không phải sự thật chắc chắn.
- `DANG TRONG VUNG MUA` chỉ mô tả vị trí giá so với vùng được tính; không phải lệnh mua.
- Mục tiêu P&F là phép chiếu theo cấu trúc nguyên nhân; không phải cam kết giá sẽ đạt mục tiêu.

## Phiên bản tài liệu

Tài liệu hiện tại được viết cho AFL **v1.7.0 PNF Đếm Ngang**. Khi công thức AFL thay đổi, tài liệu và bảng tra cứu phải được cập nhật cùng phiên bản.

