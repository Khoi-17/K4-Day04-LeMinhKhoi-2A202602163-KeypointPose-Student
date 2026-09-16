# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Lê Minh Khôi  Nhóm: ______   Ngày: 16/09/2026

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

<!-- Lấy số từ reports/visibility_report.md hoặc outputs/visibility_report.json sau Chặng 4.
Số ảnh phải là 20; số skeleton là tổng số người trong 20 ảnh. Thời gian trung bình = tổng
thời gian gán / 20. -->

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| v=2 / v=1 / v=0 | 354 / 114 / 25 |
| Thời gian trung bình mỗi ảnh | |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. `left_ear` - 55%
2. `right_ear` - 41%
3. `nose`, `left_eye`, `right_eye` - 24% (đồng hạng)

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

Có, nhìn chung đây cũng là những khớp tôi thấy khó gán nhất. Hai tai thường bị tóc, mũ bảo hiểm hoặc góc quay che nên khó xác định vị trí giải phẫu, thể hiện qua tỷ lệ v=1 cao: tai trái 55% và tai phải 41%. Mũi và hai mắt ít bị che hơn nhưng ở một số người ảnh mờ, tôi vẫn phải dựa vào vị trí tương đối với khuôn mặt để ước lượng.


## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.9529 | 0.9442 |
| OKS@0.50 | 0.9655 | 1.0000 |
| OKS@0.75 | 0.9655 | 0.9655 |
| Lỗi `dao_trai_phai` | 0 | 1 |
| Lỗi `lech_nhe` | 4 | 7 |
| Lỗi `nham_nguoi` | 0 | 0 |
| Lỗi `xoa_khop_bi_che` | 0 | 0 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

<!-- Mỗi dòng phải có: tên ảnh + người thứ mấy + keypoint + thao tác sửa. Không viết “đã sửa
lại một số lỗi”. -->

- train_13.jpg + người thứ 3 + toàn bộ 17 keypoint: bổ sung skeleton `person` cho người bị thiếu.
-
-

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ,
bạn nghĩ vì sao mình vẫn sai?

Lỗi xảy ra ở `train_13.jpg`, người thứ 3; đây là người khá mờ và khó nhìn nên tôi đã đặt skeleton bị đảo trái/phải. Kết quả đánh giá cho thấy cần kiểm tra trái/phải theo cơ thể người, không theo phía của ảnh.

## 3. Kiểm chéo

Bạn cùng nhóm: ______

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| | | | | |
| | | | | |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

<!-- Viết một rule kiểm chứng được: điều kiện nhìn thấy/căn cứ vị trí → chọn v=1 hoặc v=0.
Không chỉ ghi “cẩn thận hơn khi gán”. -->

-

## 4. Model


| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450 | 0.8450 | +0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | +0.0000 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model
   điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?

   pose_mAP50-95 tăng từ 0.6853 lên 0.6908, tức thay đổi +0.0055 sau fine-tune. Vì chỉ số này không giảm nên trường hợp giả định trong câu hỏi không áp dụng cho kết quả của tôi. Trên tập test 10 ảnh, fine-tune giúp cải thiện nhẹ khả năng định vị keypoint ở các ngưỡng IoU/OKS chặt hơn.   

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm
   *khớp* dễ hơn? Vì sao?

   Sau fine-tune, box_mAP50-95 là 0.8041, trong khi pose_mAP50-95 là 0.6908. Chênh lệch là:

   0.8041 - 0.6908 = 0.1133.

   Điều này cho thấy trên tập test này, việc xác định người (bounding box) đạt điểm cao hơn việc xác định chính xác 17 khớp (pose/keypoints). Lý do là bounding box chỉ cần bao đúng vùng người, trong khi pose phải đặt từng keypoint đúng vị trí; các khớp có thể bị che khuất, khó nhìn hoặc nằm ở tư thế khó.

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43
   (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):

   Ở `train_03`, model dự đoán 4 người trong khi nhãn của tôi có 2 người. Đây là dấu
   hiệu model **nhầm người** (dự đoán thêm người không thuộc đối tượng cần gán), cần
   xem ảnh phủ để xác nhận các detection thừa là người thật hay false positive.

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?

   Ảnh có OKS thấp nhất là `train_14`, với OKS `0.666`. Chỉ số này cho biết model và
   nhãn của tôi bất đồng nhiều nhất, nhưng không đủ để kết luận bên nào đúng; cần đối
   chiếu ảnh gốc với ảnh phủ của model và nhãn COCO để kiểm tra vị trí từng keypoint.

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó nói gì về bức ảnh đó?

   Không. Ảnh tôi gán tệ nhất theo gold là `train_13`, người thứ 3, OKS `0.702`,
   trong khi ảnh model bất đồng nhiều nhất với nhãn của tôi là `train_14`, OKS `0.666`.
   Điều này cho thấy ảnh tôi gán tệ nhất và ảnh model gặp khó nhất không hoàn toàn
   trùng nhau; mỗi bên có thể mắc lỗi ở các ảnh khác nhau.

## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người,
khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.


   Ở ảnh `train_11`, người phụ nữ, keypoint `right_hip` bị con mèo và chiếc bàn che khuất.
   Tôi vẫn xác định được vị trí hông dựa vào thân người và hướng nối giữa vai với đầu gối.
   Vì khớp hông vẫn nằm trong khung ảnh nhưng không nhìn thấy rõ, tôi đặt điểm tại vị trí ước lượng
   và chọn `v=1` (Occluded), không chọn `v=0` vì hông không bị cắt khỏi mép ảnh.
