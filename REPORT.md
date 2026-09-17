# Báo cáo Day 5 — điền trực tiếp trong fork của bạn

**Cách dùng:** Bản cần nộp đã có sẵn ở [`REPORT.md`](../REPORT.md) trong thư mục gốc của fork; mở file đó và điền vào chỗ còn thiếu. Giữ nguyên bốn mục và bảng để coach đọc bài nhanh.

- Mã học viên theo lớp: 2A202602138
- Ngày / CVAT local: 17/09/2026
- Công cụ đã dùng: CVAT Brush, Polygon, OpenCV

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh VLearn đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

## 1. Bài đã nộp

**Bạn cần điền gì?** “File ZIP đúng tên” là tên file đã tải từ CVAT rồi đặt lại. “Hoàn thành mấy ảnh” là số ảnh đã vẽ và Save, không phải số ảnh có trong task. Chưa làm hoặc export lỗi thì ghi `chưa có`, không ghi tên ZIP rỗng. Cột điểm là **điểm tối đa của task**, không phải điểm tự chấm.

| Task            | File ZIP đúng tên     | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --------------- | --------------------- | -----------------: | ---------------------------: |
| easy_semantic   | `easy_semantic.zip`   |              3 / 3 |                           20 |
| medium_instance | `medium_instance.zip` |              3 / 3 |                           32 |
| hard_panoptic   | `hard_panoptic.zip`   |              2 / 2 |                           30 |
| cp1_holes       | `cp1_holes.zip`       |              1 / 1 |                            3 |
| cp2_slice       | `cp2_slice.zip`       |              1 / 1 |                            3 |
| cp5_occlusion   | `cp5_occlusion.zip`   |              1 / 1 |                            3 |
| cp3_thin        | `cp3_thin.zip`        |              1 / 1 |                            3 |
| cp4_curb        | `cp4_curb.zip`        |              1 / 1 |                            3 |
| cp6_coverage    | `cp6_coverage.zip`    |              1 / 1 |                            3 |
| **Tổng tối đa** |                       |                    |                      **100** |

Không tự điền điểm nếu chưa có phản hồi từ người chấm. Đã đọc được toàn bộ chín ZIP submission, không có file bên trong bị hỏng; việc ZIP đã export và đọc được là bằng chứng dữ liệu đã được lưu đủ để xuất file, nhưng không thay thế xác nhận Save trên CVAT. `easy_semantic.zip` có đủ 3 PNG mask kích thước 1280 × 720 nhưng labelmap có label ngoài class chuẩn (`bus`, `car`, `truck`, `van`). `medium_instance.zip` có đủ 3 ảnh và 62 annotation (60 polygon, 2 RLE); category ngoài class chuẩn gồm `building`, `road`, `sidewalk`, `sky`, `van`, `vegetation`. `hard_panoptic.zip` có đủ 2 ảnh và 65 annotation (62 polygon, 3 RLE); category ngoài class chuẩn là `van`. Các checkpoint đạt kiểm tra cấu trúc: `cp1_holes.zip` có 5 polygon, `cp2_slice.zip` có 15 polygon, `cp5_occlusion.zip` có 39 polygon; `cp3_thin.zip`, `cp4_curb.zip` và `cp6_coverage.zip` có đủ mask và labelmap đúng class. Notebook và code chỉ kiểm tra cấu trúc export; không có preprocessing, feature, model training, evaluation metric thực tế hoặc deployment trong dữ liệu được cung cấp. Report chưa ghi nhận việc đã báo coach.

## 2. Một quyết định trước khi dùng gợi ý

**Mục này hỏi cách bạn tự ra quyết định.** Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi mở bất kỳ đề xuất tự động nào cho object đó. “Vị trí” cần đủ để tìm lại; “quy tắc biên” là lý do dừng mask ở ranh đó, nhất là mép ảnh hoặc vật che.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: `000000181542.jpg`, xe buýt ở vùng phía trên bên trái ảnh, gần mép trái; object tương ứng trong ZIP có class `bus` và bbox `[0, 62.31, 200.47, 97.76]`. ZIP không lưu thứ tự thao tác nên không xác nhận được đây là object đầu tiên tôi tự vẽ.
- Class và quy tắc tôi dùng để chọn biên: Class `bus`; chỉ lấy phần xe buýt nhìn thấy trong ảnh, dừng tại đường biên với nền và không gộp các xe/người khác vào cùng mask. Theo hướng dẫn, không đoán phần bị che.
- Nếu dùng gợi ý sau đó: Sau khi tự vẽ object bus, tôi dùng OpenCV để hỗ trợ kiểm tra biên; tôi giữ vùng gợi ý chỉ ở phần thân xe nhìn thấy và loại phần ăn vào nền hoặc vật khác.
- Nếu không dùng gợi ý: Không áp dụng; OpenCV được dùng như công cụ hỗ trợ sau bước tự vẽ, còn quyết định class và mask thuộc về tôi.

## 3. Một lỗi tôi tìm thấy và sửa

**Chọn một lỗi có thật trong bài**, không cần lỗi lớn nhất. Nếu chưa sửa được do công cụ lỗi, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: `easy_semantic.zip`, `medium_instance.zip` và `hard_panoptic.zip`; lỗi nằm trong labelmap/category của export, không xác định được vùng ảnh cụ thể từ dữ liệu ZIP.
- Lỗi thuộc loại: sai lớp.
- Bằng chứng tôi nhìn thấy: Script QC và nội dung ZIP báo label/category không thuộc `classes.json`: Easy có `bus`, `car`, `truck`, `van`; Medium có `building`, `road`, `sidewalk`, `sky`, `van`, `vegetation`; Hard có `van`. Ba ZIP vẫn đọc được về mặt kỹ thuật.
- Quy tắc và hành động sửa: Tôi đối chiếu lại `classes.json`; label/category ngoài danh sách phải được sửa trong CVAT, sau đó bấm Save và export lại đúng format. Không sửa trực tiếp JSON/PNG trong ZIP.
- Sau sửa đã Save và export lại chưa? Chưa export lại sau khi phát hiện lỗi; các ZIP tier hiện tại vẫn báo lỗi class/category khi chạy script QC.

Nếu bạn **đã xem Summary tự đánh giá trên GitHub Actions hoặc tự chạy script**, ghi ngắn một kết quả liên quan lỗi vừa sửa (ví dụ task, metric trước/sau nếu có): Script QC đã chạy và báo lỗi class/category như trên; chưa có điểm. Notebook không tính IoU, PQ hoặc điểm khi chưa có reference. Scorecard ba tier tối đa **82**, không phải điểm cuối trên 100. Không tự ghi PASS/top 3/bonus; người phụ trách xác nhận theo tiêu chí lớp. Không đưa file ground truth vào fork.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

**“Ca” là một vùng cụ thể khiến bạn phải dừng lại và chọn cách hiểu**, không nhất thiết là ba lỗi. Với mỗi dòng, ghi vị trí, hai khả năng đã cân nhắc, dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach.

| Ảnh/vị trí | Hai cách hiểu có thể                                                                           | Quy tắc/chứng cứ                                                                                                                  | Quyết định hoặc câu hỏi cho coach                                                                                    |
| ---------- | ---------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| 1          | `cp1_holes`, ảnh `000000144300.jpg`: kính/khe là lỗ cần khoét hay vẫn nằm trong mask vật       | ZIP có 5 polygon với các class `car`, `motorcycle`, `person`; hướng dẫn checkpoint yêu cầu kính/lỗ nằm trong mask, không tự khoét | Tôi giữ kính/khe bên trong mask của vật, không tạo vùng rỗng nếu không có quy tắc riêng yêu cầu khoét.               |
| 2          | `cp2_slice`, ảnh `000000017627.jpg`: hai xe cùng class đứng sát nhau là một hay hai instance   | ZIP có 15 polygon, gồm 11 `car`, 1 `bus` và 3 `person`; quy tắc instance yêu cầu mỗi vật đếm được là một mask riêng               | Tôi tách từng xe thành một instance riêng, kể cả khi hai xe cùng class và đứng sát nhau.                             |
| 3          | `cp5_occlusion`, ảnh `000000336232.jpg`: vật bị che thành nhiều phần là một hay nhiều instance | ZIP có 39 polygon, gồm 33 `car`, 2 `bus`, 1 `motorcycle` và 3 `person`; hướng dẫn yêu cầu vật bị che vẫn là một instance          | Tôi giữ các phần nhìn thấy của cùng một vật trong một instance, chỉ vẽ phần nhìn thấy và không tô xuyên vùng bị che. |
