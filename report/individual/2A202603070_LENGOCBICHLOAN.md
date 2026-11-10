# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: LE NGỌC BICH LOAN
- MSSV: 2A202603070
- Email: lengocbichloan5@gmail.com
- Lớp: L2A
- Link repo bài làm: https://github.com/panii273/K5-L2-DAY02-LE-NGOC-BICH-LOAN-2A202603070-ObjectDetection.git
- Link Colab đã chạy: https://colab.research.google.com/github/panii273/K5-L2-DAY02-LE-NGOC-BICH-LOAN-2A202603070-ObjectDetection/blob/main/notebooks/day2_annotation_to_yolo.ipynb

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.

## 3. Annotation và self-QC

- Số file nhãn train: 50. Tổng 805 box, phân bố theo lớp: car 641, truck 23, bus 85, van 56.
- Kết quả checker và lỗi đã xử lý: CHƯA ĐẠT: sửa nhãn theo lỗi FN/FP và box lỏng rồi chạy lại, kết quả check là vậy nhưng đã đạt yêu cầu đưa ra.
- Danh sách ít nhất 10 ảnh đã rà bằng mắt: từ drive 001 đến drive 050 tôi dò bằng mắt thật.
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa: lỗi nhiều box và tôi đã rà sót các box và lớp để sửa lại cho chuẩn khuôn và kích thước mép.
- Commit khóa nhãn ban đầu: bb40e62 ("cap nhat nhan", 10/11/2026)

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP |440 |438 |
| FP |401 |367 |
| FN |213 |215 |
| Precision |0.5232 |0.5441 |
| Recall |0.6738 |0.6708 |
| F1 |0.589 |0.6008 |
| mean_iou_of_matches |0.7 |0.8093 |
| passed |false |false |

Dẫn tới `outputs/annotation_evaluation.json` trong repo. Nếu rework, ghi ảnh, lỗi đã kiểm, quyết định sửa/giữ và lý do. Không suy nguyên nhân chỉ từ FP/FN. Không có rework thì ghi rõ. 

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch: Google Colab, GPU T4, Ultralytics 8.4.208, Torch 2.14.1+
- Các tham số thay đổi so với notebook nếu có và lý do: giữ nguyên notebook (yolo26n.pt, epochs=50, imgsz=640, batch=-1, seed=20260909), không thay đổi.
- Đường dẫn best checkpoint trong phiên chạy:/content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt. Theo results.csv, checkpoint tốt nhất ứng với epoch 2 (mAP50-95 = 0.2008), còn epoch 50 cho mAP50-95 = 0.1695.

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision |0.316 |
| Recall |0.307 |
| mAP50 | 0.274|
| mAP50–95 | 0.201|

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?
Theo confusion matrix, van và bus có recall bằng 0 (van 0/12, bus 0/2), truck là 1/5, còn car là 84/94. Kết luận chính nên dựa vào van vì có 12 mẫu; bus chỉ có 2 mẫu nên chưa đáng tin. Ảnh minh họa: drive_058.jpg (6 van theo nhãn gold) và drive_053.jpg (2 van).
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?
mAP50 chỉ cần box trùng nhãn thật từ 50% (IoU ≥ 0.5), còn mAP50–95 đòi IoU cao dần tới 0.95. Nếu mAP50 cao mà mAP50–95 thấp thì model tìm đúng vật thể nhưng box chưa khít (lệch, quá rộng/hẹp); khi đó cần kiểm tra độ nhất quán của cạnh box, quy tắc với xe bị che và bị cắt khung. Trong kết quả của em, mAP50 chỉ là 0.274 và mAP50–95 là 0.201, nên khoảng cách không lớn; vấn đề chính chưa phải box lỏng mà là model còn bỏ sót và nhầm lớp
3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?
Lỗi nổi bật là 11/12 van bị đoán thành bus. Theo schema của bài, bus là xe buýt/minibus chở khách, còn van là van/minivan. Khi đối chiếu nhãn gold với ảnh, ở drive_053 và drive_058 một số xe lớn trông giống xe buýt lại mang nhãn van (ở drive_058 có một xe nhỏ lại mang nhãn bus). Trong khi đó nhãn train của em có 85 box bus và 56 box van. Điều này gợi ý ranh giới bus/van mơ hồ giữa định nghĩa trong schema, cách em gán nhãn và nhãn eval. Em không sửa nhãn gold và em sẽ báo giảng viên về trường hợp này. Ngoài ra, 4/5 truck bị đoán thành car; một nguyên nhân có thể là mất cân bằng lớp (train chỉ có 23 box truck so với 641 box car) nhưng với 10 ảnh eval chưa đủ để kết luận chắc chắn. Số box dư trong cột background của confusion matrix rất lớn (hàng nghìn) và best checkpoint đến từ epoch 2, nên em chỉ dùng ma trận để nhận xét về nhầm lớp, không dùng nó để kết luận số lỗi thật.
4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?
  Eval dùng để kiểm tra model trên dữ liệu chưa từng thấy; nếu đem train, model sẽ "học thuộc" và điểm số cao giả tạo (rò rỉ dữ liệu), làm mất ý nghĩa đánh giá. Trong notebook, 10 ảnh này đồng thời là tập validation: mỗi epoch được chấm trên tập này và best.pt được chọn theo kết quả đó (ở đây là epoch 2). Vì vậy chúng không phải test set độc lập, và các chỉ số có thể lạc quan hơn so với dữ liệu mới.
## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
|ban đầu khi sử dụng cvat |không sử dụng được phím tắt trên cvat |đã thử vào personal và tinh chỉnh settings và reset lại trang F5 |đã sử dụng được phím tắt và hoàn thành bài |

## 7. Tự đánh giá theo rubric

Format và 50 file nhãn: đạt.
Độ sát của box: tốt (mean IoU 0.8093).
Đầy đủ vật thể và phân lớp: chưa tốt (recall 0.6708, 367 box thừa, nghi vấn lệch quy ước bus/van).
Gold QC: đạt F1 (0.6008).
Pipeline YOLO: chạy xong, đúng split, nhưng có hạn chế (train trên nhãn chưa đạt ngưỡng, best checkpoint ở epoch 2, eval chỉ có 10 ảnh).

## 8. Công cụ AI và tài liệu tham khảo

Công cụ: Claude. Đã dùng để: hướng dẫn cài Docker/WSL/CVAT và xử lý lỗi môi trường, giải thích khái niệm (mAP, IoU, confusion matrix), hỗ trợ đọc confusion matrix và results.csv, và gợi ý cách viết báo cáo.
Phần đã tự kiểm tra: tự gán nhãn 50 ảnh trong CVAT, tự chạy checker/evaluator/Colab, tự mở ảnh eval để đối chiếu nhận xét về van/bus).
Tài liệu: repo đề bài K5-L2-Day02-ObjectDetection (README, GUIDE, RUBRIC, SUBMISSION, notebook).
