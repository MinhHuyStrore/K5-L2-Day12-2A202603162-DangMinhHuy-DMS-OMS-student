# Cá Nhân Ngày 12

Mã bộ ảnh: **G09** · Hình thức: **cá nhân**

| Họ tên | MSSV | Đã gán đủ 22 ảnh? | Review chéo với (peer diff) | Số dòng disagreement đã xử lý |
|---|---|---|---|---|
| Đặng Minh Huy | 2A202603162 | có | _Trương Tuấn Vũ_ | _6_ |


## Cá nhân

### DangMinhHuy-2A202603162
- Phần mình làm: gán toàn bộ 22 ảnh cốt lõi (S01–S08 cặp rgb/ir + D01–D06) và phần mở rộng S09–S10; tự QC ba vòng bằng `cabin_check` (vòng 1: 10 lỗi → vòng 2: 1 lỗi → vòng 3: 0 lỗi); sửa box trùng S02_rgb/S06_rgb, đổi seat S05_rgb, đồng bộ `light`/`frame_meta`.
- Điều mình học được hôm nay: ghế gọi theo xe chứ không theo ảnh (camera nhìn về sau nên trái ảnh = rear_right); export CVAT có thể sinh box trùng khi undo/copy giữa frame, phải soi bằng đếm box từng ghế chứ không chỉ nhìn ảnh.
