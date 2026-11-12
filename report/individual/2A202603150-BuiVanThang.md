# Báo cáo thực hành Day 3 — Segmentation

- Mã học viên: 2A202603150.
- Họ tên: Bùi Văn Thắng.
- Ngày thực hành: 11/11/2026; ngày chuẩn bị bản nộp: 12/11/2026.
- Môi trường: CVAT local tại `http://localhost:8080`, phiên bản máy chủ 2.80.1.
- Công cụ: CVAT để gán nhãn và xuất semantic. Có trợ lý AI hỗ trợ cài SAM 2.1 Tiny, kiểm tra cấu trúc ZIP và biên soạn báo cáo từ thông tin đã xác nhận. SAM chưa sử dụng được trong giao diện gán nhãn tại thời điểm chuẩn bị bản nộp.

## 1. Bài đã nộp

Bản chuẩn bị nộp hiện có `submissions/easy_semantic.zip`, xuất từ CVAT theo định dạng **Segmentation mask 1.1**. Phần semantic đã làm và xuất cho 3 ảnh. Các task còn lại chưa có ZIP trong bản nộp này. Bảng dưới đây ghi trạng thái file đã chuẩn bị trong repo, không xác nhận đã gửi link trên LMS.

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | --- | ---: |
| easy_semantic | easy_semantic.zip | 3 / 3, đã xuất | 20 |
| medium_instance | chưa có | Chưa hoàn thành bản nộp / 3 | 32 |
| hard_panoptic | chưa có | Chưa hoàn thành bản nộp / 2 | 30 |
| cp1_holes | chưa có | Chưa hoàn thành bản nộp / 1 | 3 |
| cp2_slice | chưa có | Chưa hoàn thành bản nộp / 1 | 3 |
| cp5_occlusion | chưa có | Chưa hoàn thành bản nộp / 1 | 3 |
| cp3_thin | chưa có | Chưa hoàn thành bản nộp / 1 | 3 |
| cp4_curb | chưa có | Chưa hoàn thành bản nộp / 1 | 3 |
| cp6_coverage | chưa có | Chưa hoàn thành bản nộp / 1 | 3 |
| **Tổng tối đa theo đề** | | | **100** |

Ba ảnh trong ZIP semantic:

- `7ee6d192-89e2408b.jpg`.
- `817bca71-00000000.jpg`.
- `81ae7cbb-6bc63a4a.jpg`.

ZIP có mask semantic tương ứng cho cả ba ảnh, kích thước 1280 × 720. Danh sách lớp gồm `road`, `sidewalk`, `building`, `vegetation`, `sky`, cùng background của định dạng export. Không tạo ZIP rỗng cho các task chưa hoàn thành và không sửa JSON/PNG bên trong ZIP.

## 2. Một quyết định trước khi dùng gợi ý

Mục này **chưa hoàn thành đầy đủ yêu cầu của đề**. Task `medium_instance` đã được mở tại Job 6, ảnh `000000181542.jpg`, nhưng chưa có thông tin xác nhận về một object đầu tiên tự vẽ và Save để báo cáo. Vì vậy, tôi chưa ghi một quyết định gán nhãn Medium là đã thực hiện.

Chưa dùng được gợi ý SAM trong Job Medium: danh sách Model ở tab **AI assistance → Prompting** hiển thị **No data**. Không có mask SAM trong bài được xác nhận là đã giữ hoặc sửa.

Quy tắc cần áp dụng khi tiếp tục: gán phần nhìn thấy; mỗi người và mỗi phương tiện là instance riêng; không vẽ phần bị che khuất. Đây là quy tắc dự kiến áp dụng, chưa phải bằng chứng hoàn thành object đầu tiên.

## 3. Một lỗi tôi tìm thấy và sửa

- **Task/ảnh/vùng:** `medium_instance`, Job 6, ảnh `000000181542.jpg`; bảng AI assistance.
- **Loại lỗi:** lỗi kỹ thuật khi sử dụng công cụ hỗ trợ gán nhãn.
- **Bằng chứng:** sau khi mở tab Prompting, danh sách Model trống và hiện `No data`; không chọn được SAM 2.1 Tiny.
- **Các bước đã thử:** mở bảng AI assistance, chuyển từ Auto-labeling sang Prompting và kiểm tra danh sách Model. Trợ lý đã cài SAM 2.1 Tiny và kiểm thử phía máy chủ trên ảnh tổng hợp. Kiểm thử đó trả được mask, nhưng không xác nhận công cụ hoạt động trong phiên giao diện của tôi.
- **Kết quả:** lỗi hiển thị model chưa được khắc phục tại thời điểm chuẩn bị bản nộp. Cần coach hỗ trợ kiểm tra kết nối giữa CVAT và dịch vụ model. Chưa xác nhận đã báo lỗi trực tiếp cho coach.
- **Save/export:** phần semantic đã được xuất thành `easy_semantic.zip`. Chưa có bản export Medium sau sửa lỗi. Chưa có lỗi biên mask cụ thể đã sửa được xác nhận để trình bày thêm.

Trợ lý chạy kiểm tra cấu trúc bằng:

```bash
python -X utf8 scripts/inspect_submissions.py --dir submissions
```

Kết quả: `easy_semantic` báo **OK**; tám task còn lại báo **THIẾU**. `OK` chỉ xác nhận cấu trúc ZIP, tên ảnh và class phù hợp với hợp đồng bài lab, không chứng minh biên mask đúng. Chưa có điểm tự đánh giá từ reference; không tự ghi điểm cho bài.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Các câu hỏi dưới đây được tổng hợp với hỗ trợ của trợ lý AI từ ảnh Medium đã mở. Đây là các ca cần làm rõ khi tiếp tục gán nhãn, **không phải ba quyết định tự làm đã được kiểm chứng hoặc ba lỗi đã sửa**.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| `000000181542.jpg`, người phụ nữ mặc áo dài ở giữa ảnh | Gộp toàn bộ người và áo dài thành một instance `person`; hoặc bỏ các tà áo rộng khỏi mask vì dễ nhầm với nền | Quần áo đang mặc thuộc vùng của người; cần phân biệt tà áo với phần nền nhìn thấy giữa các phần cơ thể/quần áo | Dự kiến giữ các phần áo dài nhìn thấy trong instance `person`, loại nền nhìn thấy ở khe hở. Cần kiểm kỹ đường biên khi vẽ; chưa có mask hoàn tất cho ca này. |
| `000000181542.jpg`, người lái xe máy bên trái người phụ nữ | Gộp người và xe máy thành một object; hoặc tách `person` và `motorcycle` | Đây là hai đối tượng thuộc hai class khác nhau, có che khuất lẫn nhau; chỉ gán phần nhìn thấy của từng đối tượng | Dự kiến tách người và xe thành hai instance, không đoán phần xe bị người che. Chưa hoàn tất annotation cho ca này. |
| `000000181542.jpg`, xe bán tải phía sau người phụ nữ | Gán `car`; hoặc gán `truck` do có thùng hàng phía sau | Bộ nhãn Medium có cả `car` và `truck`; hình dáng thùng hàng giúp nhận diện xe bán tải, nhưng cần thống nhất quy ước phân lớp của bộ dữ liệu | Cần coach xác nhận quy ước cho xe bán tải trong task này trước khi chốt class. Chưa có nhãn hoàn tất cho ca này. |

Bản hiện tại còn thiếu phần instance, panoptic và sáu checkpoint. Báo cáo ghi nhận phần đã có cùng hạn chế thực tế, không coi các bước cài công cụ hoặc kiểm tra cấu trúc là thay thế cho việc hoàn thành gán nhãn.
