# Báo cáo Day 5 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên VLearn. Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](reports/REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.

- Mã học viên theo lớp: 2A202602138
- Ngày / CVAT local: 17/09/2026 /
- Công cụ đã dùng: CVAT

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh VLearn đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

| Task            | File ZIP đúng tên     | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --------------- | --------------------- | -----------------: | ---------------------------: |
| easy_semantic   | `easy_semantic.zip`   |              3 / 3 |                           20 |
| medium_instance | `medium_instance.zip` |              3 / 3 |                           32 |
| hard_panoptic   | `hard_panoptic.zip`   |              2 / 2 |                           30 |
| cp1_holes       | chưa có               |              0 / 1 |                            3 |
| cp2_slice       | chưa có               |              0 / 1 |                            3 |
| cp5_occlusion   | chưa có               |              0 / 1 |                            3 |
| cp3_thin        | chưa có               |              0 / 1 |                            3 |
| cp4_curb        | chưa có               |              0 / 1 |                            3 |
| cp6_coverage    | chưa có               |              0 / 1 |                            3 |
| **Tổng tối đa** |                       |                    |                      **100** |

Đã đọc được toàn bộ ba ZIP tier, không có file bên trong bị hỏng. `easy_semantic.zip` có đủ 3 PNG mask kích thước 1280 × 720 nhưng labelmap có label ngoài class chuẩn (`bus`, `car`, `truck`, `van`). `medium_instance.zip` có đủ 3 ảnh và 62 annotation (60 polygon, 2 RLE); category ngoài class chuẩn gồm `building`, `road`, `sidewalk`, `sky`, `van`, `vegetation`. `hard_panoptic.zip` có đủ 2 ảnh và 65 annotation (62 polygon, 3 RLE); category ngoài class chuẩn là `van`. Chưa có thông tin trong dữ liệu được cung cấp về trạng thái Save hoặc việc đã báo coach.

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: Chưa có thông tin trong dữ liệu được cung cấp.
- Class và quy tắc tôi dùng để chọn biên: Chưa có thông tin trong dữ liệu được cung cấp. Theo hướng dẫn, chỉ vẽ phần nhìn thấy và không đoán biên phía sau vật che.
- Nếu dùng gợi ý sau đó: vùng gợi ý sai/đúng, hành động sửa/giữ và lý do: Chưa có thông tin trong dữ liệu được cung cấp.
- Nếu không dùng gợi ý: Chưa có thông tin trong dữ liệu được cung cấp.

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: `easy_semantic.zip`, `medium_instance.zip` và `hard_panoptic.zip`; lỗi nằm trong labelmap/category của export, không xác định được vùng ảnh cụ thể từ dữ liệu ZIP.
- Lỗi thuộc loại: sai lớp.
- Bằng chứng tôi nhìn thấy: Script QC và nội dung ZIP báo label/category không thuộc `classes.json`: Easy có `bus`, `car`, `truck`, `van`; Medium có `building`, `road`, `sidewalk`, `sky`, `van`, `vegetation`; Hard có `van`. Ba ZIP vẫn đọc được về mặt kỹ thuật.
- Quy tắc và hành động sửa: Chưa có thông tin trong dữ liệu được cung cấp về việc đã sửa trong CVAT. Cần kiểm đúng `classes.json`, chỉnh label trong CVAT, bấm Save rồi export lại đúng format; không sửa trực tiếp JSON/PNG trong ZIP.
- Sau sửa đã Save và export lại chưa? Chưa có thông tin trong dữ liệu được cung cấp.

Nếu bạn **đã xem Summary tự đánh giá trên GitHub Actions hoặc tự chạy script**, ghi ngắn một kết quả liên quan lỗi vừa sửa (ví dụ task, metric trước/sau nếu có): Chưa có điểm; script QC chỉ xác nhận lỗi class/category, không tính metric. Scorecard ba tier tối đa **82**, không phải điểm cuối trên 100. Không tự ghi PASS/top 3/bonus; người phụ trách xác nhận theo tiêu chí lớp. Không đưa file ground truth vào fork.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể                                                                                         | Quy tắc/chứng cứ                                                                                                                    | Quyết định hoặc câu hỏi cho coach                                                          |
| ---------- | ------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| 1          | `medium_instance`, ảnh `000000181542.jpg`: các object cùng class có thể là một mask hay nhiều instance riêng | ZIP có 4 `car`, 8 `motorcycle` và 10 `person`; quy tắc task yêu cầu mỗi vật đếm được là một mask                                    | Chưa có thông tin trong dữ liệu được cung cấp về quyết định cụ thể hoặc câu hỏi cho coach. |
| 2          | `hard_panoptic`, ảnh `000000460147.jpg`: vùng nền là stuff hay một vật đếm được là thing                     | ZIP có `road`, `sidewalk`, `building`, `vegetation`, `sky` cùng `car`, `truck`, `van`; `classes.json` quy định năm lớp đầu là stuff | Chưa có thông tin trong dữ liệu được cung cấp về quyết định cụ thể hoặc câu hỏi cho coach. |
| 3          | `easy_semantic`, ảnh `7ee6d192-89e2408b.jpg`: pixel đen là background/unlabeled hay một vùng cần gán class   | PNG mask có 133273 pixel màu background và các màu road, sky, vegetation; labelmap cũng khai báo background                         | Chưa có thông tin trong dữ liệu được cung cấp về quyết định cụ thể hoặc câu hỏi cho coach. |
