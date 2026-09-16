# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 29 skeleton, trung bình 15.69 khớp có v > 0 mỗi người
- Tổng: v=2 313 | v=1 142 | v=0 38

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 22 | 7 | 0 | 24% |
| 1 | left_eye | 21 | 8 | 0 | 28% |
| 2 | right_eye | 21 | 8 | 0 | 28% |
| 3 | left_ear | 10 | 18 | 1 | 62% |
| 4 | right_ear | 16 | 13 | 0 | 45% |
| 5 | left_shoulder | 25 | 4 | 0 | 14% |
| 6 | right_shoulder | 27 | 2 | 0 | 7% |
| 7 | left_elbow | 20 | 6 | 3 | 21% |
| 8 | right_elbow | 25 | 3 | 1 | 10% |
| 9 | left_wrist | 18 | 8 | 3 | 28% |
| 10 | right_wrist | 21 | 5 | 3 | 17% |
| 11 | left_hip | 13 | 14 | 2 | 48% |
| 12 | right_hip | 19 | 9 | 1 | 31% |
| 13 | left_knee | 13 | 12 | 4 | 41% |
| 14 | right_knee | 16 | 10 | 3 | 34% |
| 15 | left_ankle | 10 | 10 | 9 | 34% |
| 16 | right_ankle | 16 | 5 | 8 | 17% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
