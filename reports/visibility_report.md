# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 29 skeleton, trung bình 16.0 khớp có v > 0 mỗi người
- Tổng: v=2 318 | v=1 146 | v=0 29

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 24 | 5 | 0 | 17% |
| 1 | left_eye | 22 | 7 | 0 | 24% |
| 2 | right_eye | 22 | 7 | 0 | 24% |
| 3 | left_ear | 13 | 15 | 1 | 52% |
| 4 | right_ear | 15 | 14 | 0 | 48% |
| 5 | left_shoulder | 26 | 3 | 0 | 10% |
| 6 | right_shoulder | 26 | 3 | 0 | 10% |
| 7 | left_elbow | 22 | 6 | 1 | 21% |
| 8 | right_elbow | 23 | 5 | 1 | 17% |
| 9 | left_wrist | 19 | 9 | 1 | 31% |
| 10 | right_wrist | 19 | 9 | 1 | 31% |
| 11 | left_hip | 15 | 13 | 1 | 45% |
| 12 | right_hip | 17 | 11 | 1 | 38% |
| 13 | left_knee | 15 | 11 | 3 | 38% |
| 14 | right_knee | 14 | 12 | 3 | 41% |
| 15 | left_ankle | 12 | 9 | 8 | 31% |
| 16 | right_ankle | 14 | 7 | 8 | 24% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
