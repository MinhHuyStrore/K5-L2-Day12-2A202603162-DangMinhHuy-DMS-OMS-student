# Tự đánh giá — Đặng Minh Huy · G09

## 1. Tự rà soát 4 cảnh

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| S02_rgb | person/child_seat | extra | 6 box trùng do copy đối tượng giữa frame (không phải nhiều người) | có — xóa còn person×2 + child_seat×1 |
| S05_rgb | person | seat_flip | gán `driver` vì nhìn theo ảnh; camera SVIRO chỉ chiếu hàng ghế sau | có — đổi `rear_right` |
| S03_ir | object | missing/seat | thiếu frame_meta + object trái ảnh ghi `rear_middle` lệch với rgb | có — thêm tag, đổi `rear_right` |
| D06 | face13.right_eye_* | flag | điểm outside=1 nhưng eye_right vẫn `unknown` chưa giải trình | có — bật đủ cờ, check sạch warn |

## 2. Lỗi mình lặp lại nhiều nhất hôm nay

Lỗi: **quên tag `frame_meta`** (mắc 3 ảnh: S02_rgb, S03_ir, S06_rgb) và **box trùng** ở ảnh copy nhãn.

Vì sao mình mắc: gán vội cuối khung hình, nhảy sang ảnh khác khi CVAT chưa kịp lưu tag; dùng tính năng copy đối tượng sang frame mà không biết nó nhân bản box.

Lần sau tránh bằng cách: gắn `frame_meta` **ngay khi mở một ảnh mới** (thói quen mở-đầu), và sau mỗi lần copy/trước mỗi lần export chạy `cabin_check` với `--only` cho ảnh vừa làm thay vì đợi cuối buổi.

## 3. Ghi chú về disagreement_log

Dòng bạn ấy đúng và mình sai: `frame_meta.light` ở D01.png — mình ghi `night` vì ảnh NIR tối, Vũ để `unknown`. Theo guideline 4.2/4.4-1, ảnh hồng ngoại không đánh giá được sáng/tối thật bằng mắt thường nên `unknown` là đúng chuẩn; mình đã sửa D01, S02_rgb, S05_ir, S09_ir về `unknown`.

Mình học được: metadata phản ánh **khả năng quan sát của camera**, không phản ánh cảm nhận của người xem ảnh — thấy tối không đồng nghĩa là "đêm".

Ngược lại, Vũ sai ở 4 box `child_seat.occupied=yes` (guideline 4.8 cấm `yes`) — Vũ đã sửa; và seat object S08_ir (trái ảnh = `rear_right` theo 4.4-3). 8 dòng còn lại (mắt/kính S01, S06; face box S02_ir; occupied S07_ir) hai bạn chưa thống nhất, đã ghi ở `TEAM.md` để hỏi coach.
