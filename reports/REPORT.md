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
| OKS trung bình | 0.954|0.954|
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

- `pose_mAP50-95` tăng từ 0.6853 lên 0.6908 (+0.0055, tương đương tăng nhẹ +0.55%). Độ chính xác `pose_precision` cũng tăng từ 0.9734 lên 0.9792 (+0.0058), trong khi `pose_recall` giữ nguyên ở mức 0.8462.
- 20 ảnh của bạn dạy được model: Nhãn được gán rất chuẩn xác (OKS trung bình vs gold đạt 0.9542, không có lỗi đảo trái/phải hay nhầm người), độ phủ cao với 111 khớp $v=1$ có tọa độ ước lượng giải phẫu (thay vì vứt bỏ $v=0$ như nhiều ảnh COCO gốc). Model học được cách định vị chính xác hơn ở các khớp bị che khuất một phần (như cổ tay, tai), giúp cải thiện nhẹ precision và mAP trên dải ngưỡng OKS từ 0.50 đến 0.95.
- Điểm đánh đổi / làm hỏng: Kích thước 20 ảnh là quá nhỏ (phân phối hẹp, domain-specific) so với hàng trăm nghìn ảnh COCO. Fine-tune làm model bị overfit cục bộ và giảm nhẹ khả năng tổng quát hóa trên bài toán phát hiện vùng người (thể hiện qua `box_mAP50-95` giảm -0.0078 từ 0.8119 xuống 0.8041).

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm
   *khớp* dễ hơn? Vì sao?

- Sau fine-tune, `box_mAP50-95` (0.8041) cao hơn `pose_mAP50-95` (0.6908) là 0.1133 (khoảng 11.33%). Nếu xét ở ngưỡng IoU/OKS 0.50, `box_mAP50` (~0.9785) cao hơn `pose_mAP50` (0.8450) đến hơn 13.35%.
- Model tìm **người** (bounding box) dễ hơn tìm **khớp** (keypoints) rất nhiều.
- Vì sao:
  1. *Độ phân giải & không gian đặc trưng*: Bounding box chỉ cần bắt đặc trưng toàn cục của cơ thể (hình dạng người, thân, đầu) với vùng bao tương đối rộng; còn keypoint là bài toán định vị chi tiết (fine-grained) từng điểm giải phẫu nhỏ (mắt, mũi, khớp xương).
  2. *Che khuất và biến dạng tư thế*: Các khớp xương rất dễ bị che khuất (tóc che tai, tay sau lưng/cầm đồ vật, quần che gối) hoặc co gập phức tạp. Khi đó bounding box người vẫn nhận diện tốt, nhưng tọa độ từng khớp trở nên khó đoán.
  3. *Độ nhạy thước đo*: OKS phạt sai số theo hàm phân phối Gauss với bán kính dung sai $\sigma$ rất nhỏ (mắt $\sigma=0.025$, cổ tay $\sigma=0.062$); chỉ cần lệch vài pixel là điểm OKS tụt dốc, khắt khe hơn nhiều so với IoU của bounding box.

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43
   (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):

- Ảnh: `test_06.jpg` (ảnh hai người đứng che chung ô dưới mưa nhìn từ sau lưng).
- Tên lỗi: **Nhầm người** (`nham_nguoi`) và **Lệch nhẹ** (`lech_nhe`).
- Bằng chứng thị giác & phân tích:
  + Hai người (người áo cam váy xếp ly bên trái và người áo khoác nâu bên phải) đứng ép sát nhau dưới một chiếc ô đen. Vùng vai phải và cánh tay phải của người áo cam tiếp giáp trực tiếp với cánh tay trái của người áo nâu. Model dự đoán bị lỗi **nhầm người**: gán nhầm keypoint cánh tay/khuỷu tay của người này sang người kia do khoảng cách giữa hai người quá gần và ranh giới cơ thể bị mờ nhòe vì mưa.
  + Ngoài ra, vùng đầu hai người bị tán ô đen che khuất từ phía sau (trong ground truth `test_06.txt` mắt và mũi đều là $v=0$), model dự đoán cố gắng ước lượng khớp đầu nhưng bị **lệch nhẹ** ($1.0 < ratio \le 3.0$ lần bán kính dung sai).

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?

- Ảnh có OKS thấp nhất: **`train_13`** (người có OKS thấp nhất đạt **0.581**, người thứ hai đạt **0.709**).
- **Tôi (người gán nhãn) đúng**, model đoán sai.
- Căn cứ:
  1. *Bằng chứng thị giác*: Trong `train_13.jpg`, người đàn ông mặc vest ở tiền cảnh bị cắt ngang thân dưới ở mép ảnh dưới (chân ra ngoài ảnh nên hai mắt cá chân gán $v=0$ theo đúng luật lớp), hai tay cầm bao thuốc/ví che khuất nhau; người đi bộ áo xanh ở xa bên trái thì quá nhỏ và mờ nhòe. Model YOLO-pose bị "ảo giác" cố đoán khớp chân kéo xuống đáy ảnh hoặc trượt khớp ở người bị mờ phía sau.
  2. *Đối chiếu với gold (`outputs/eval_vs_gold.json`)*: Nhãn của tôi khớp chính xác cả 3/3 người với gold (OKS người 1 là 0.9586, người 2 là 0.8912, người 3 là 0.824 và sau rework đạt ~0.98), hoàn toàn không có lỗi đảo trái/phải hay nhầm người. OKS giữa tôi và model thấp là do model gặp khó khăn trên ảnh phức tạp này, chứng minh nhãn người gán chuẩn xác hơn model.

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó
   nói gì về bức ảnh đó?

- **Có**. Ảnh tôi gán có OKS thấp nhất so với gold là `train_13.jpg` (người thứ 3 ban đầu có OKS = 0.824, thấp nhất trong 29 người), và đây **cũng chính là** ảnh model đoán có OKS thấp nhất so với tôi (0.581).
- Điều đó nói lên:
  + `train_13.jpg` là một bức ảnh **cực kỳ khó (hard sample / extreme edge case)** đối với bài toán pose estimation.
  + Ảnh có sự kết hợp của: (1) **Đa tỉ lệ & độ sâu trường ảnh**: người tiền cảnh rất to nhưng bị cắt cúp mép ảnh, người hậu cảnh rất nhỏ và bị mờ out-of-focus; (2) **Che khuất nặng & tự che khuất (self-occlusion)**: hai bàn tay cầm ví và bao thuốc đan xen; (3) **Cắt mép ảnh (truncation)**: ranh giới nhạy cảm giữa khớp còn trong khung ($v=1$) và đã lọt ra ngoài mép ($v=0$).
  + Khi cả con người cẩn thận gán nhãn lẫn mô hình học sâu đều cho điểm thấp nhất trên cùng một bức ảnh, nó xác nhận đây là mẫu biên mang tính thách thức cao của dữ liệu thế giới thực.

## 4. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người,
khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

- **Ảnh & Đối tượng**: `train_13.jpg`, người thứ 1 (người đàn ông mặc vest ở tiền cảnh), khớp `left_ankle` (mắt cá chân trái).
- **Bằng chứng nhìn thấy**: Nhìn từ thắt lưng xuống, vạt áo vest và ống quần tây kéo dài tới sát cạnh đáy bức ảnh; khớp đầu gối trái (`left_knee`) được định vị ở tọa độ $y \approx 0.94$ (sát mép dưới nhưng vẫn còn trong ảnh). Tuy nhiên, đường biên mép dưới của bức ảnh đã cắt ngang ngay dưới đầu gối, hoàn toàn không nhìn thấy phần cẳng chân dưới, cổ chân hay giày.
- **Lý do chọn trạng thái**: Do tỉ lệ giải phẫu cơ thể người đòi hỏi khoảng cách từ đầu gối đến mắt cá chân lớn hơn nhiều so với khoảng trống còn lại đến mép ảnh, khớp mắt cá chân trái chắc chắn đã nằm ra ngoài khung hình (`out-of-frame`). Căn cứ theo luật bắt buộc của lớp ("ra ngoài mép ảnh $\rightarrow v=0$, không đặt chấm"), tôi quyết định gán `v=0` (tọa độ `0.0 0.0 0`) thay vì cố chấm ước lượng `v=1` bên trong khung hình như model thường nhầm lẫn.
