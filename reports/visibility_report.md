# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 29 skeleton, trung bình 16.14 khớp có v > 0 mỗi người
- Tổng: v=2 354 | v=1 114 | v=0 25

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 22 | 7 | 0 | 24% |
| 1 | left_eye | 22 | 7 | 0 | 24% |
| 2 | right_eye | 22 | 7 | 0 | 24% |
| 3 | left_ear | 13 | 16 | 0 | 55% |
| 4 | right_ear | 17 | 12 | 0 | 41% |
| 5 | left_shoulder | 26 | 3 | 0 | 10% |
| 6 | right_shoulder | 27 | 2 | 0 | 7% |
| 7 | left_elbow | 24 | 5 | 0 | 17% |
| 8 | right_elbow | 25 | 4 | 0 | 14% |
| 9 | left_wrist | 20 | 9 | 0 | 31% |
| 10 | right_wrist | 20 | 8 | 1 | 28% |
| 11 | left_hip | 23 | 6 | 0 | 21% |
| 12 | right_hip | 21 | 8 | 0 | 28% |
| 13 | left_knee | 21 | 5 | 3 | 17% |
| 14 | right_knee | 20 | 6 | 3 | 21% |
| 15 | left_ankle | 16 | 4 | 9 | 14% |
| 16 | right_ankle | 15 | 5 | 9 | 17% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
