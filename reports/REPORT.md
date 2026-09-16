# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Tô Quang Hưng   Nhóm: Solo Ngày: 16/09/2026

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

<!-- Lấy số từ reports/visibility_report.md hoặc outputs/visibility_report.json sau Chặng 4.
Số ảnh phải là 20; số skeleton là tổng số người trong 20 ảnh. Thời gian trung bình = tổng
thời gian gán / 20. -->

| Chỉ số                         |      Giá trị |
| -------------------------------- | -------------: |
| Số ảnh đã gán               |             20 |
| Số skeleton                     |             29 |
| v=2 / v=1 / v=0                  | 340 / 122 / 31 |
| Thời gian trung bình mỗi ảnh |      6.2 phút |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`): left_ear: 59%, right_eye : 38%,right_eye:  34%

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích: Các bộ phận đấy thường bị che khuất bởi tóc, mũ, hoặc do đối tượng quay đầu ra sau không thể xác định chính xác

<!-- Trả lời 2–4 câu. Phân biệt “hay bị che” với “khó xác định vị trí giải phẫu”; nêu bằng
chứng nhìn thấy thay vì chỉ nêu cảm giác. -->

## 2. Chấm với gold



<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

| Chỉ số                | Trước rework | Sau rework |
| ----------------------- | -------------: | ---------: |
| OKS trung bình         |          0.913 |      0.913 |
| OKS@0.50                |              1 |          1 |
| OKS@0.75                |              1 |          1 |
| Lỗi`dao_trai_phai`   |              0 |          0 |
| Lỗi`nham_nguoi`      |              0 |          0 |
| Lỗi`xoa_khop_bi_che` |              0 |          0 |

**(không rewwork, do kết quả đã tốt)**

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

<!-- Mỗi dòng phải có: tên ảnh + người thứ mấy + keypoint + thao tác sửa. Không viết “đã sửa
lại một số lỗi”. -->

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ,
bạn nghĩ vì sao mình vẫn sai?

<!-- Nếu không có lỗi, ghi rõ “Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh.” -->

## 3. Kiểm chéo

Bạn cùng nhóm: ______

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| ----- | ---: | --: | ----: | --------------------------------------- |
|       |      |     |       |                                         |
|       |      |     |       |                                         |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

<!-- Viết một rule kiểm chứng được: điều kiện nhìn thấy/căn cứ vị trí → chọn v=1 hoặc v=0.
Không chỉ ghi “cẩn thận hơn khi gán”. -->

## 4. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6. “Chênh” = sau fine-tune trừ baseline;
đây là quan sát trên tập test, không phải chất lượng sản phẩm. -->

| Chỉ số       | yolo26n-pose gốc | Sau fine-tune |  Chênh |
| -------------- | ----------------: | ------------: | ------: |
| pose_mAP50     |            0.8450 |        0.8450 |       0 |
| pose_mAP50-95  |            0.6853 |        0.6908 | +0.0055 |
| pose_precision |            0.9734 |        0.9792 | +0.0058 |
| pose_recall    |            0.8462 |        0.8462 |       0 |
| box_mAP50-95   |            0.8119 |        0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model
   điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?

* Chỉ số `pose_mAP50-95` tăng từ 0.6853 lên 0.6908 (tăng  0.0055 ).
* Việc tăng nhẹ cho thấy 20 ảnh bổ sung đã giúp model làm quen tốt hơn với các tư thế hoặc bối cảnh cụ thể trong tập dữ liệu của tmà COCO có thể chưa bao quát hết (ví dụ: góc chụp lạ hoặc trang phục đặc thù).
* Nếu `pose_mAP50-95` giảm,20 ảnh của tôi không dạy được model điều gì, nó có thể  làm hỏng khả năng tổng quát trên các pose/góc nhìn đa dạng của COCO , tức là model bị  overfit vào 20 ảnh .

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm
   *khớp* dễ hơn? Vì sao?
   box_mAP50 (0.96) cao hơn pose_mAP50 (0.845) khoảng 0.115.
   Model tìm người dễ hơn tìm khớp. Lý do là vì khung hình người (bounding box) là một vùng lớn, đặc điểm nhận dạng rõ ràng hơn so với các điểm khớp nhỏ lẻ thường bị che khuất hoặc dễ bị nhầm lẫn với môi trường.
3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43
   (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):
   Ảnh test_07: Model gặp lỗi trượt hẳn hoặc nhầm người (nó vẽ pose lên một vật thể không phải người rõ ràng hoặc sai lệch hoàn toàn so với cơ thể thật).
   Ảnh test_09: Xuất hiện lỗi lệch nhẹ ở các khớp chân do bị xe máy che khuất một phần.
4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?
   Ảnh có OKS thấp nhất là train_13 (0.463). Tôi nghĩ rằng tôi đúng vì ảnh khá mờ
5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó
   nói gì về bức ảnh đó?
   Ảnh train_04 xuất hiện trong cả cảnh báo nhãn (v=0 sai) và có OKS model-nhãn khá cao (0.865). Tuy nhiên, ảnh train_13 mới là nơi model lúng túng nhất. Nếu một ảnh cả tôi và model đều làm tệ, đó thường là ảnh có độ phân giải thấp, thiếu sáng hoặc bị che khuất cực nặng (heavy occlusion).

## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người,
khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

	Trong ảnh `train_04`, tôi đã phải quyết định trạng thái cho các khớp ở vùng thân dưới của người thứ nhất (bị che khuất bởi bàn ăn). Mặc dù các khớp đầu gối và cổ chân không nhìn thấy trực tiếp, tôi chọn nhãn v=1 thay vì v=0 vì người này đang ngồi ở trung tâm khung hình, không có phần thân nào nằm ngoài biên ảnh. Dựa vào cấu trúc cơ thể và vị trí hông, tôi vẫn có thể ước lượng được vị trí tương đối của chân dưới gầm bàn; việc đặt nhãn v=1 giúp model học được rằng thực thể người vẫn tồn tại toàn vẹn ngay cả khi bị vật thể khác che lấp, thay vì hiểu nhầm là người đó bị cắt cụt hoặc nằm ngoài ảnh.

<!-- Cấu trúc gợi ý: (1) train_XX + người thứ mấy + keypoint; (2) căn cứ thị giác như phần cơ
thể liền kề, trang phục hoặc vật che; (3) vì sao khớp còn trong khung (v=1) hay đã ra khỏi
khung (v=0). -->
