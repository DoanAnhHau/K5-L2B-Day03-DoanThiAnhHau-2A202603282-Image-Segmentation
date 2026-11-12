# Báo cáo thực hành Day 3 — điền trực tiếp trong fork của bạn


- Mã học viên theo lớp: 2A202603282
- Ngày / CVAT local: 11/11/2026
- Công cụ đã dùng: CVAT local

## 1. Bài đã nộp
| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | easy_semantic | 3 / 3 | 20 |
| medium_instance | medium_instance | 3 / 3 | 32 |
| hard_panoptic | hard_panoptic | 2 / 2 | 30 |
| cp1_holes | cp1_holes | 1 / 1 | 3 |
| cp2_slice | cp2_slice | 1 / 1 | 3 |
| cp5_occlusion | cp5_occlusion | 1 / 1 | 3 |
| cp3_thin | cp3_thin | 1 / 1 | 3 |
| cp4_curb | cp4_curb | 1 / 1 | 3 |
| cp6_coverage | cp6_coverage | 1 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

## 2. Một quyết định trước khi dùng gợi ý

- Ảnh, vị trí và object Medium đầu tiên tự vẽ : ảnh `000000181542.jpg`, người phụ nữ mặc áo dài sáng màu đứng ở chính giữa ảnh. 

- Class và quy tắc tôi dùng để chọn biên: Class Person. Xác định đường biên theo hình dáng thực tế của người, bao gồm phần đầu, tóc, thân, hai tay, tà áo dài và hai chân. Khi vẽ polygon, tôi đặt các điểm theo đường viền bên ngoài cơ thể, cố gắng bám sát từng chi tiết và không lấy phần xe máy, mặt đường hoặc các phương tiện phía sau vào vùng annotation.

Nếu dùng gợi ý sau đó: Chưa sử dụng gợi ý tự động cho đối tượng này.

Nếu không dùng gợi ý: Không dùng. Tôi tự xác định đường biên dựa trên hình ảnh gốc. Tôi lựa chọn người phụ nữ đứng giữa vì hình dáng cơ thể tương đối rõ, đồng thời có một số vị trí tiếp giáp với xe máy và nền đường cần chú ý khi xác định ranh giới.

## 3. Một lỗi tôi tìm thấy và sửa

- Task/ảnh/vùng: `cp3_thin`, `839f7736-abe28069.jpg`, một cột/biển báo mảnh trong ảnh.
- Lỗi thuộc loại: phủ vùng, mask semantic bỏ sót một đoạn cấu trúc mảnh.
- Bằng chứng tôi nhìn thấy: khi phóng to ảnh, cấu trúc còn nhìn thấy liên tục nhưng mask bị đứt ở một đoạn.
- Quy tắc và hành động sửa : dùng nét cọ 2–3 px theo hướng dẫn checkpoint, nối phần mask bị thiếu và kiểm tra lại hai đầu đoạn.
- Sau sửa đã Save và export lại chưa?: đã
- Summary tự đánh giá/script: đã hoàn thành

## 4. Ba ca chưa chắc hoặc đã cân nhắc

> **BẢN MẪU GIẢ ĐỊNH — các vị trí và cách hiểu dưới đây là gợi ý để bạn đối chiếu với ảnh và quyết định thật của mình.**

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| `000000181542.jpg`, giữa ảnh, người đi bộ sát xe máy | Người là một vùng mask liên tục phía sau xe / chỉ tô phần cơ thể nhìn thấy | Xe che khuất một phần cơ thể; ranh nhìn thấy phân định được phần cần gán | Chỉ mask phần nhìn thấy, không nối xuyên qua  xe |
| `000000181542.jpg`, nửa phải, các xe máy chồng lấp | Một instance bị che khuất / nhiều xe máy riêng biệt | Có thể lần theo các thân xe và bánh xe riêng; checkpoint occlusion yêu cầu đếm instance đúng và giữ mask của vật bị che thành một mas| Tách theo từng xe nhận diện được |
| `000000373353.jpg`, vùng các xe ở gần nhau | Các vùng xe chạm nhau là một vật / là nhiều xe riêng | Medium là instance segmentation; cần dựa vào đường viền từng xe thay vì chỉ lớp `car` chung | Giữ mask riêng cho từng xe |
