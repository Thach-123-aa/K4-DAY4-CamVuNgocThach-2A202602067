# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Cầm Vũ Ngọc Thạch - 2A202602067   Nhóm: làm cá nhân (không theo nhóm)   Ngày: 16/09/2026

## 1. Nhãn của tôi

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| v=2 / v=1 / v=0 | 318 / 146 / 29 |
| Thời gian trung bình mỗi ảnh | 5p |

Ba khớp có `%v=1` cao nhất (từ `reports/visibility_report.md`):

1. `left_ear` - 52%
2. `right_ear` - 48%
3. `left_hip` - 45%

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

Đúng một phần. Tai bị che cao vì nhiều ảnh trong bộ 20 người đội mũ bảo hiểm hoặc tóc phủ
một phần tai (ví dụ `train_12`, `train_15` - người đội mũ full-face) - đây là "hay bị che"
thật sự, có bằng chứng nhìn thấy (viền mũ/tóc đè lên vị trí tai). Hông thì khác loại: nó không
phải "bị che bởi vật cản" mà là "không có bề mặt nào để nhìn thấy" - hông luôn nằm dưới quần áo
nên phải ước lượng theo giải phẫu (giữa hai đầu xương chậu), bất kể ảnh dễ hay khó. Vì vậy hai
loại khớp này khó vì hai lý do khác nhau: tai khó vì *bị che ngẫu nhiên tuỳ ảnh*, hông khó vì
*luôn luôn phải ước lượng, không phụ thuộc ảnh nào*.

## 2. Chấm với gold

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.732 | 0.866 |
| OKS@0.50 | 0.828 | 1.000 |
| OKS@0.75 | 0.759 | 1.000 |
| Lỗi `dao_trai_phai` | 7 | 0 |
| Lỗi `nham_nguoi` | 6 | 1 |
| Lỗi `xoa_khop_bi_che` | 3 | 1 |

**Tôi đã sửa gì giữa hai lần chạy:**

- `train_06`, người #1: đổi lại `left_elbow`, `left_wrist` (đang đảo sang bên phải), kéo lại `left_ankle` về đúng khớp (đang trượt hẳn).
- `train_09`, người #1: cùng lỗi và cùng cách sửa như `train_06` (đảo trái/phải cả cánh tay trái, trượt `left_ankle`).
- `train_13`, người #1: đổi lại toàn bộ điểm trái/phải theo đúng hướng cơ thể (ảnh người mặc vest xem điện thoại).
- `train_14`, người #1: đổi `left_elbow` về đúng người (đang bị gán nhầm sang người bên cạnh), kéo `right_elbow` về đúng vị trí (trượt hẳn), gắn thêm `right_wrist` đang bị bỏ trống.
- `train_16`, người #1: đổi `left_elbow`, `left_wrist` về đúng người (nhầm người), kéo `right_elbow` về đúng vị trí (trượt hẳn).
- `train_16`, người #2: đổi `right_elbow`, `right_wrist` về đúng người (nhầm người), kéo `left_elbow`, `left_wrist` về đúng vị trí (trượt hẳn).
- `train_19`, người #2: kéo lại cả bốn điểm `left_elbow`, `right_elbow`, `left_wrist`, `right_wrist` về đúng vị trí và đúng bên (vừa đảo trái/phải vừa trượt hẳn).
- `train_08`, người #1: gắn lại `v=1` cho `left_hip`, `left_knee`, `left_ankle` - trước đó bị xoá nhầm thành `v=0` dù các khớp này chỉ bị che, không ra khỏi khung.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào? Ảnh đó dễ hay khó? Nếu là ảnh dễ, bạn nghĩ vì sao mình vẫn sai?**

Có hai nhóm rõ rệt. `train_06` và `train_09` là ảnh **khó**: người lái xe máy nhìn từ phía sau,
không thấy mặt nên không có mốc mắt/mũi để xác định hướng cơ thể - dễ lẫn trái/phải theo hướng
màn hình thay vì hướng cơ thể thật. `train_14`, `train_16`, `train_19` cũng khó vì nhiều người
đứng gần/chồng lên nhau, dễ lẫn cả bên lẫn người. Ngược lại `train_13` là ảnh **dễ**: một người
đứng thẳng, mặt nhìn rõ hoàn toàn, không có gì che khuất - vậy mà vẫn bị đảo trái/phải. Tôi nghĩ
nguyên nhân là thói quen gán nhanh theo "trái/phải của màn hình" thay vì dừng lại tưởng tượng
đứng vào đúng vị trí người trong ảnh rồi giơ tay trái lên - đúng cái bẫy `GUIDE.md` đã cảnh báo:
lỗi đảo trái/phải không xảy ra ở ảnh khó, nó xảy ra ở ảnh dễ, lúc làm nhanh và chủ quan.

## 3. Kiểm chéo

Không áp dụng - làm bài cá nhân, không có bạn cùng nhóm để chạy `visibility_report.py --compare`
(đã ghi rõ trong `GUIDELINE_MINI.md` mục 4).

## 4. Model

> Đã xác nhận bản chạy này dùng đúng nhãn sau rework: cell clone in ra "Đã clone fork vào
> /content/Day4-Lab" (không phải "dùng bản hiện có"), và cell kiểm nhãn in đúng
> `v=2 318 | v=1 146 | v=0 29` khớp với commit `08fc6ec`.

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450 | 0.8450 | +0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | +0.0000 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

1. **`pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, giải thích.**
   Tăng nhẹ +0.0055 (không giảm). Nhìn log huấn luyện: `EarlyStopping` chọn **epoch 9** làm
   checkpoint tốt nhất - từ epoch 13 trở đi, `pose_recall` và `pose_mAP50-95` sụp hẳn về 0
   (huấn luyện mất ổn định vì chỉ có 20 ảnh), rồi dao động hồi phục nhưng không bao giờ vượt
   qua mức của epoch 9. Vì epoch 9 mới chỉ vừa cập nhật vài bước từ trọng số pretrained trên
   COCO, model gần như chưa kịp "học" gì mới từ 20 ảnh trước khi bị chọn làm checkpoint cuối -
   nên kết quả trên 10 ảnh test gần như y hệt baseline. 20 ảnh có giúp `pose_precision` nhích
   lên (+0.0058) nhưng chưa đủ để thay đổi được gì lớn hơn.
   Điều đáng chú ý: tôi đã chạy thử nghiệm này **hai lần** - một lần trên bản nhãn còn 7 lỗi
   đảo trái/phải (trước rework) và một lần trên bản đã sửa hết (sau rework, OKS 0.866). Cả
   hai lần `EarlyStopping` đều dừng ở đúng epoch 9 với cùng một kiểu sụp ở epoch 13. Vậy hiện
   tượng "kết quả gần như không đổi" **không phải do lỗi nhãn** của tôi, mà là do bộ
   hyperparameter (đặc biệt `pose` loss weight = 12.0, `fliplr=0.5`) không hợp với một tập chỉ
   có 20 ảnh - dataset quá nhỏ khiến quá trình học dao động mạnh bất kể nhãn đúng hay sai.

2. **`box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm người hay tìm khớp dễ hơn?**
   Baseline: `box_mAP50-95` 0.8119 so với `pose_mAP50-95` 0.6853 - chênh 0.1266. Sau fine-tune:
   0.8041 so với 0.6908 - chênh 0.1133. Model luôn tìm **người (box)** dễ hơn định vị **khớp
   (pose)** ở cả hai mốc, vì bounding box chỉ cần bao đúng vùng thân, trong khi 17 điểm đòi
   hỏi định vị chính xác từng khớp nhỏ (cổ tay, mắt cá) - những điểm dễ lệch nhất khi bị che
   hoặc ở tư thế bất thường.

3. **Một ảnh test model đoán sai - gọi tên lỗi:**
   `test_07` (người phụ nữ ngồi sau bàn có chuông thuỷ tinh đựng bánh): model phát hiện đúng
   người (`person 0.86`) nhưng bộ khung xương lại kéo dài xuống tận chuông thuỷ tinh và mặt
   bàn phía trước - hoàn toàn không trùng vị trí thân dưới thật của người này (vốn bị bàn che
   khuất). Đây là lỗi **trượt hẳn** (chấm vào chỗ không có khớp) - model đoán bừa vị trí chân
   khi không nhìn thấy bằng chứng thật, thay vì giữ khớp ở vị trí hợp lý theo tư thế ngồi.

4. **Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng?**
   `train_13`, OKS model-vs-nhãn = 0.449 (thấp nhất trong 29 skeleton). Tôi cho rằng **nhãn của
   tôi đúng hơn**: ảnh này đã qua cổng gold sau rework (không còn lỗi đảo trái/phải, cùng nhóm
   ảnh đạt OKS trung bình 0.866 với gold), trong khi model chỉ được cập nhật 9 epoch trước khi
   `EarlyStopping` dừng lại - chưa đủ để học được tư thế đặc thù của ảnh này (người đứng thẳng,
   hai tay khoanh gần thân cầm điện thoại, nhìn xuống) trên nền dữ liệu pretrained COCO vốn ít
   gặp góc chụp này.

5. **Ảnh bạn gán tệ nhất có cũng là ảnh model đoán tệ nhất không?**
   Có. `train_13` từng là 1 trong 8 skeleton phải rework so với gold (lỗi đảo trái/phải, OKS
   0.593 trước khi sửa) - và sau khi tôi đã sửa đúng, nó **vẫn** là ảnh model bất đồng nhiều
   nhất (0.449). Điều này cho thấy `train_13` là một tư thế thật sự khó - cả người gán (lúc
   đầu) và model (dù đã fine-tune) đều gặp khó khăn với nó, không phải do tôi gán ẩu mà do bản
   thân bức ảnh có tư thế mơ hồ (tay gần thân, khó phân biệt trái/phải khi nhìn nhanh).

## 5. Một rule evidence bạn đã dùng

**Ảnh `train_03`, người #1 (áo khoác xanh đậm, đang cầm ly nước đỏ), khớp `right_wrist`.**
Người này đứng sát người thứ hai ngay phía trước, ban đầu tôi gán `right_wrist` là `v=0`
(Outside). Xem lại ảnh gốc thì thấy tay phải của anh ấy đang giơ lên gần miệng, cầm một ly
nước màu đỏ - bằng chứng nhìn thấy là chiếc ly và một phần cánh tay vẫn hiện diện rõ trong
ảnh. Vì bàn tay/cổ tay đó rõ ràng còn nằm trong khung hình (chỉ là dễ bị bỏ sót khi gán vội,
không phải bị cắt bởi mép ảnh), tôi đổi lại thành `v=1` (Occluded một phần bởi chính tư thế
tay) và ước lượng chấm theo vị trí cầm ly.
