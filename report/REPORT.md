# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Hoàng Trần Minh Đức     Nhóm: T003   Ngày: 16/9/2026

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

<!-- Lấy số từ reports/visibility_report.md hoặc outputs/visibility_report.json sau Chặng 4.
Số ảnh phải là 20; số skeleton là tổng số người trong 20 ảnh. Thời gian trung bình = tổng
thời gian gán / 20. -->

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 29 skeleton, trung bình 16.07 khớp có v > 0 mỗi người
- Tổng: v=2 355 | v=1 111 | v=0 27

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 23 | 6 | 0 | 21% |
| 1 | left_eye | 21 | 8 | 0 | 28% |
| 2 | right_eye | 22 | 7 | 0 | 24% |
| 3 | left_ear | 15 | 14 | 0 | 48% |
| 4 | right_ear | 23 | 6 | 0 | 21% |
| 5 | left_shoulder | 25 | 4 | 0 | 14% |
| 6 | right_shoulder | 28 | 1 | 0 | 3% |
| 7 | left_elbow | 23 | 6 | 0 | 21% |
| 8 | right_elbow | 25 | 4 | 0 | 14% |
| 9 | left_wrist | 19 | 10 | 0 | 34% |
| 10 | right_wrist | 20 | 8 | 1 | 28% |
| 11 | left_hip | 22 | 7 | 0 | 24% |
| 12 | right_hip | 22 | 7 | 0 | 24% |
| 13 | left_knee | 19 | 6 | 4 | 21% |
| 14 | right_knee | 21 | 4 | 4 | 14% |
| 15 | left_ankle | 15 | 5 | 9 | 17% |
| 16 | right_ankle | 12 | 8 | 9 | 28% |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1.  left_ear
2.  left_wrist
3.  right_wrist

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

<!-- Trả lời 2–4 câu. Phân biệt “hay bị che” với “khó xác định vị trí giải phẫu”; nêu bằng
chứng nhìn thấy thay vì chỉ nêu cảm giác. -->

Left_ear là phần tai khó xác định vị trí giải phẫu do hướng nhìn của người trong ảnh. Left_wrist và right_wrist trên có tỷ lệ v1 cao do trong các bức ảnh phần tay hay bị che.

## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

OKS trung bình : 0.954
OKS@0.50       : 1.000  (tỉ lệ người gán đúng ở mức 'nhận ra pose')
OKS@0.75       : 1.000  (tỉ lệ người gán đủ chính xác để train)
Người: gold 29 | ghép được 29 | thiếu 0 | thừa 0
Mức            : Xuất sắc

Danh sách lỗi theo loại:
-    3  Lệch nhẹ - sửa được, ít hại
-   44  Cờ khác gold (vị trí vẫn đúng) - không trừ điểm OKS
-   74  Gold để v=0 ở khớp bạn có gán - không trừ điểm

Không có skeleton nào cần rework: mọi người đều đạt OKS >= 0.75 và không có lỗi đã phân loại.

Chi tiết từng khớp: outputs\eval_vs_gold.json

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.954|0.98|
| OKS@0.50 |1|1|
| OKS@0.75 |1|1|
| Lỗi `dao_trai_phai` |0|0|
| Lỗi `nham_nguoi` |0|0|
| Lỗi `xoa_khop_bi_che` |0|0|

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

<!-- Mỗi dòng phải có: tên ảnh + người thứ mấy + keypoint + thao tác sửa. Không viết “đã sửa
lại một số lỗi”. -->

- Image 13 , person 2, right_wrist: sửa lại bị lệch nhẹ
- Image 13 , person 3, left_ankle: sửa lại bị lệch nhẹ
- Image 13 , person 3, right_ankle: sửa lại bị lệch nhẹ

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ,
bạn nghĩ vì sao mình vẫn sai?

Không có lỗi đảo trái phải trong toàn bộ 20 ảnh
<!-- Nếu không có lỗi, ghi rõ “Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh.” -->

## 3. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6. “Chênh” = sau fine-tune trừ baseline;
đây là quan sát trên tập test, không phải chất lượng sản phẩm. -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 |0.8450  |        0.8450 |  +0.0000|
| pose_mAP50-95 |0.6853   |       0.6908|   +0.0055|
| pose_precision | 0.9734    |      0.9792|   +0.0058|
| pose_recall |  0.8462     |     0.8462|   +0.0000|
| box_mAP50-95 |  0.9785      |   0.8041|   -0.0078|


### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model
   điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm
   *khớp* dễ hơn? Vì sao?

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43
   (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó
   nói gì về bức ảnh đó?

## 4. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người,
khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

<!-- Cấu trúc gợi ý: (1) train_XX + người thứ mấy + keypoint; (2) căn cứ thị giác như phần cơ
thể liền kề, trang phục hoặc vật che; (3) vì sao khớp còn trong khung (v=1) hay đã ra khỏi
khung (v=0). -->
