# Mini guideline - làm cá nhân (không theo nhóm) | người gán: Cầm Vũ Ngọc Thạch-2A202602067 | ngày: 16/09/2026

> Điền file này **trong lúc** gán nhãn, không phải sau khi xong. Mỗi lần bạn dừng lại
> hơn 10 giây để phân vân, đó là một dòng phải ghi vào đây.

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

- Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file `.SVG` chung.
- Mọi người trong ảnh đều có **đủ 17 điểm**. Điểm không dùng được thì gắn cờ, không xoá.
- Trái/phải tính theo **cơ thể người**, không theo bức ảnh.
- Bị che, còn trong khung -> `v = 1`, **vẫn đặt chấm** ở vị trí ước lượng.
- Ra ngoài mép ảnh -> `v = 0`, **không** đặt chấm.
- Không dùng `Hidden` (`h`) - nó không được lưu vào file.

## 2. Luật của nhóm bạn (phải điền)

| Tình huống | Luật nhóm bạn chọn | Vì sao |
| --- | --- | --- |
| Hông của người mặc quần áo dài | `v=1`, đặt chấm ước lượng ở giữa hai đầu xương chậu theo tư thế thân/quần | Hông không bao giờ thấy được qua quần áo, nhưng vị trí giải phẫu vẫn tồn tại trong khung ảnh - đây là occluded, không phải outside |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Còn thấy rìa/dái tai → `v=2`; che hoàn toàn nhưng đầu còn trong khung → `v=1`, ước lượng theo đối xứng với tai kia; chỉ `v=0` khi đầu bị cắt ra khỏi mép ảnh | Vật che (tóc, mũ) vẫn nằm trong ảnh, khác với đầu bị cắt bởi khung hình |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | `v=0` cho mọi khớp từ đầu gối/mắt cá trở xuống nếu ảnh không còn phần khung nào bên dưới điểm cắt | Khớp không tồn tại trong vùng đã chụp - đây mới đúng là "ra ngoài mép ảnh", khác hẳn bị vật che |
| Cổ tay nằm sau tay lái / sau thân mình | `v=1`, đặt chấm ước lượng theo tư thế cầm lái - miễn là vật che (tay lái, thân xe) vẫn còn hiện diện trong khung ảnh | Vật cản nằm giữa camera và khớp, không phải khớp vượt ra khỏi biên ảnh - xe/tay lái là bằng chứng khung ảnh vẫn "chứa" vị trí đó |
| Hai người chồng lên nhau | Gán đủ 17 điểm riêng cho từng người theo đúng cơ thể của chính họ; điểm bị người kia che nhưng chủ nhân vẫn còn trong khung → `v=1` | Làm xong hẳn một người rồi mới sang người kế tiếp - tránh nhầm khớp của người này sang người khác |
| Người nhỏ đến mức nào thì không gán nữa | Gán đủ mọi người xuất hiện trong ảnh, không bỏ ai vì lý do kích thước | Bộ 20 ảnh core đã được chọn sẵn để mọi người trong ảnh đều đủ lớn để gán (README) |

Với mỗi luật, chèn **một ảnh mẫu** (screenshot từ CVAT) thay vì chỉ viết một câu.
Slide 12 nói rõ: khớp không có bề mặt nhìn thấy được thì phải có ảnh mẫu, không phải
một câu văn chung chung.

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_04`, người thứ `1 và 2`, khớp `hip, knee, ankle (cả hai bên)`

- Mơ hồ ở chỗ nào: ảnh chụp cận cảnh hai người ngồi trên xe motocross, cắt sát ở khoảng ngực/thắt lưng - không rõ chân bị vật che hay ảnh vốn không chụp tới đó.
- Bạn quyết thế nào: `v=0` (Outside), không đặt chấm.
- Vì sao: xem lại ảnh gốc, khung hình không còn phần nào bên dưới thắt lưng - không có mặt đất, không có phần xe nào tiếp tục xuống - nghĩa là chân thật sự nằm ngoài vùng đã chụp, không phải bị che bởi vật gì đó vẫn nằm trong ảnh.
- Nếu người khác quyết ngược lại thì model học sai cái gì: nếu gán `v=1` và tự bịa toạ độ chân không có căn cứ trong ảnh, model sẽ học một vị trí chân tưởng tượng ngẫu nhiên - nhiễu huấn luyện chứ không phải tín hiệu thật.

### Ca 2 - ảnh `train_03`, người thứ `1 (áo khoác xanh đậm, cầm ly nước)`, khớp `left_shoulder, left_elbow, left_wrist, right_wrist`

- Mơ hồ ở chỗ nào: người này đứng sát người đàn ông thứ hai phía trước, tay trái bị chính thân mình/người kia che khuất, còn cổ tay phải đang giơ lên cầm ly nước nên dễ bị bỏ sót khi gán vội.
- Bạn quyết thế nào: `v=1` (Occluded), đặt chấm ước lượng theo tư thế cầm ly và hướng vai.
- Vì sao: người kia (vật che) vẫn hiện diện rõ trong khung ảnh - đây là bị che, không phải ra ngoài mép ảnh. Cổ tay phải thực ra ước lượng được khá chính xác vì nhìn thấy được ly nước và một phần cánh tay.
- Nếu người khác quyết ngược lại thì model học sai cái gì: để `v=0` sẽ xoá khớp này khỏi phép tính OKS dù vị trí có thể ước lượng tốt - mất tín hiệu huấn luyện thật cho một tư thế tay phổ biến (tay giơ lên gần mặt).

### Ca 3 - ảnh `train_06` và `train_09`, người thứ `1 (lái xe máy, nhìn từ phía sau)`, khớp `hip, knee, ankle`

- Mơ hồ ở chỗ nào: nhìn từ phía sau, thân xe và yên xe che gần hết phần hông trở xuống của người lái, ban đầu dễ nhầm là "không thấy thì cho ra ngoài khung".
- Bạn quyết thế nào: `v=1` (Occluded), đặt chấm ước lượng dựa theo tư thế ngồi lái và vị trí bàn đạp/yên xe.
- Vì sao: toàn bộ chiếc xe và mặt đường vẫn hiện diện đầy đủ trong khung ảnh (thấy tới tận bánh sau, biển số) - vật che (thân xe) nằm trong ảnh, nên khớp bị che chứ không ra ngoài khung.
- Nếu người khác quyết ngược lại thì model học sai cái gì: đây đúng lỗi phổ biến "xoá khớp bị che" mà `check_pose_labels.py` cảnh báo - model sẽ không bao giờ học được tư thế ngồi lái xe máy từ phía sau, một góc chụp rất thường gặp trong ảnh giao thông thực tế.

## 4. Sau khi so visibility report với bạn cùng nhóm

Không áp dụng - làm bài cá nhân, không có bạn cùng nhóm để chạy `visibility_report.py --compare`.
