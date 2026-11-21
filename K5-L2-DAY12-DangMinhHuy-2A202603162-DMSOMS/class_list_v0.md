# Danh sách lớp v0 — Đặng Minh Huy · 2A202603162 (bản nháp, cần bạn rà lại trước nộp)

Mỗi lớp trả lời: **model cần nhận ra cái này để làm gì?**

## DMS (mặt tài xế — ảnh NIR D01–D06)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng DMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | eye_state (eye_left, eye_right) | open / closed / unknown | Phát hiện buồn ngủ (PERCLOS), cảnh báo khi mắt khép lâu | Có, trừ khi bị tay/vật che hoặc kính phản xạ LED |
| 2 | glasses | none / clear / sun / unknown | Bù trừ phản xạ hồng ngoại khi ước lượng tâm mắt; sun glasses làm mất điểm mắt | Một phần — gọngclear thấy, sun glasses che mắt trong NIR |
| 3 | gaze_zone | road / mirror_l / mirror_r / mirror_c / cluster / center / lap / other | Phát tài xế mất tập trung (nhìn điện thoại, cluster) kéo dài | Có, nhờ hướng đồng tử + tư thế đầu |
| 4 | face13 (13 điểm mắt–mũi–miệng + cờ occluded/outside) | điểm 2D | Đầu vào hình học để tính mắt (EAR), yaw/pitch đầu; cờ occluded nói model "đừng tin điểm này" | Có với phần thấy được; phần bị che phải có cờ |
| 5 | hand + on_wheel | whose, side, yes / no / unknown | Giám sát tài xế có đặt tay vô-lăng (yêu cầu hands-on của xe bán tự lái) | Có, bàn tay NIR nổi bật |
| 6 | phone | held / mounted / lap | Phát hiện dùng điện thoại khi lái; mounted thì không tính là vi phạm | Có nếu thấy rõ vật; không suy đoán từ tư thế |
| 7 | phone_call (tổ hợp phone=held + miệng/gần mặt) | có / không | Cảnh báo gọi điện không rảnh tay | Một phần, dựa phone + gaze |

## OMS (khoang hành khách — ảnh SVIRO S01–S10)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng OMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | person + seat | driver / front_passenger / rear_left / rear_middle / rear_right | Đếm người theo ghế; bật nhắc thắt dây theo từng ghế, tắt túi khí ghế phụ trống | Có (SVIRO IR-sim giữ được hình người) |
| 2 | seatbelt | worn_correct / worn_incorrect / not_worn / unknown | Cảnh báo thắt dây; cần phân biệt đeo sai (dưới tay) với chưa đeo | Kém — dải dây tối màu dễ lẫn nếp áo, vì vậy unknown hợp lệ |
| 3 | child_seat | seat, occupied (no/unknown) | Nhắc trẻ quên trong xe; khóa cửa sổ/hàng ghế theo cấu hình ghế trẻ em | Có, theo hình dạng ghế |
| 4 | infant_seat | seat, facing (rear/front) | Tắt túi khí ghế trước nếu ghế infant quay ngược đặt ghế phụ | Có, hướng ghế thấy rõ qua khối |
| 5 | object + type | bag / box / cup / other | Phân biệt hành lý với người (chống báo nhầm reminder "còn người trong xe") | Có, hình khối |
| 6 | frame_meta.modality | ir / rgb | Routing sang model chuyên IR hay RGB; chuẩn hóa input | n/a |
| 7 | frame_meta.light + camera_blocked | day / night / unknown; yes / no | Loại khung camera che; chọn threshold ban đêm | camera_blocked thấy cả hai; light không kết luận được trên IR-sim → unknown |

## Câu hỏi leo thang ( guideline v1.0 chưa trả lời)

1. **Câu hỏi:** Ghế `child_seat` có người ngồi thật bên trong thì `occupied` gán thế nào, khi schema chỉ cho `no`/`unknown`? Người lớn hay trẻ em ngồi trong child_seat có phải vừa là `person` vừa là `child_seat occupied` không?
   **Khung ví dụ:** S02 (person + child_seat cùng rear_left).
   **Tạm gán:** child_seat.occupied=unknown, person tách box riêng theo guideline 4.4 — vì schema không cho `yes`.

2. **Câu hỏi:** Ảnh SVIRO IR-sim render có đèn nội thất, `light` nên là giá trị của scene dựng hay bắt buộc `unknown` vì không phải ảnh camera thật?
   **Khung ví dụ:** S02_rgb, S05_ir (cabin_check WARN).
   **Tạm gán:** giữ `day` khi scene thể hiện chiếu sáng rõ trong ảnh dựng, kèm dòng giải thích ở `check_final.txt`.
