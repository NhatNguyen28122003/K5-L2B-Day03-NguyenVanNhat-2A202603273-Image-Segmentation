# Báo cáo thực hành Day 3 — Image Segmentation

- Mã học viên theo lớp: 2A202603273
- Ngày / CVAT local: 12/11/2026 · CVAT local `http://localhost:8080`
- Công cụ đã dùng: CVAT Brush/Polygon;sửa trong CVAT. Không dùng SAM.

## 1. Bài đã nộp

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | submissions/easy_semantic.zip | 3 / 3 | 20 |
| medium_instance | submissions/medium_instance.zip | 3 / 3 | 32 |
| hard_panoptic | submissions/hard_panoptic.zip | 2 / 2 | 30 |
| cp1_holes | submissions/cp1_holes.zip | 1 / 1 | 3 |
| cp2_slice | submissions/cp2_slice.zip | 1 / 1 | 3 |
| cp3_thin | submissions/cp3_thin.zip | 1 / 1 | 3 |
| cp4_curb | submissions/cp4_curb.zip | 1 / 1 | 3 |
| cp5_occlusion | submissions/cp5_occlusion.zip | 1 / 1 | 3 |
| cp6_coverage | submissions/cp6_coverage.zip | 1 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Format export: semantic (`easy_semantic`, `cp3_thin`, `cp4_curb`, `cp6_coverage`) dùng Segmentation mask 1.1; instance/panoptic dùng COCO 1.0.

## 2. Một quyết định trước khi dùng gợi ý

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: `000000181542.jpg`, người đứng ở khu giữa phía trên ảnh (quanh x≈240–265, y≈130), vẽ bằng Polygon trước khi xem gợi ý.
- Class và quy tắc tôi dùng để chọn biên: `person`; chỉ bao phần thân nhìn thấy (đầu, thân, chân), dừng biên ở mép quần áo/da, không kéo theo bóng dưới chân và không lấn sang người/xe đứng sát bên. Phần bị vật khác che thì không đoán.
- Nếu dùng gợi ý sau đó: toàn bộ Medium được vẽ tay bằng Polygon/Brush (45 object trên 3 ảnh); không dùng mask gợi ý cho Medium.
- Quyết định gán nhãn: người ngồi trên xe máy được vẽ thành hai object riêng (`person` và `motorcycle`), vì task instance hỏi “pixel thuộc vật nào”.

## 3. Một lỗi tôi tìm thấy và sửa

- Task/ảnh/vùng: `cp6_coverage` / `7daa6479-67988f3f.jpg` / chiếc xe buýt trắng ở giữa ảnh.
- Lỗi thuộc loại: sai lớp.
- Bằng chứng tôi nhìn thấy: mask gợi ý tô loang lổ màu `car` lên thân xe buýt . Class list của `cp6_coverage` chỉ có road, sidewalk, building, vegetation, sky, car, person — không có `bus`.
- Quy tắc và hành động sửa: pixel không thuộc class nào của task thì để trống. XXóa toàn bộ phần `car` nằm trên xe buýt và xe tải bên trái.
- Sau sửa đã Save và export lại chưa? Đã Save trên CVAT; export ZIP.

Kết quả tự đánh giá: 

## 4. Ba ca chưa chắc hoặc đã cân nhắc

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
