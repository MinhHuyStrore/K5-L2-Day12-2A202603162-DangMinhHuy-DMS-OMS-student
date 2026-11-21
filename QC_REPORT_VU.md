# Báo cáo QC chéo — Bài Trương Tuấn Vũ (2A202603078)

> Lab Ngày 12 · Guideline Cabin v1.0 · Người kiểm: Đặng Minh Huy (2A202603162)
> Ngày kiểm: 22/11/2026 · Nguồn: `export_final.zip` (22 ảnh) + `export_extra (1).zip` (4 ảnh) do Vũ gửi
> File kiểm đã đổi tên trong repo: `qc_vu_final_src.zip`, `qc_vu_extra_src.zip`
> Kết quả thô: [qc_vu_final.txt](qc_vu_final.txt) · [qc_vu_extra.txt](qc_vu_extra.txt)
> Đây là bằng chứng E3 (review chéo). File KHÔNG nằm trong folder nộp của Huy — folder nộp Huy là `K5-L2-DAY12-DangMinhHuy-2A202603162-DMSOMS/`.

## 1. Tổng quan

| Hạng mục | Kết quả | Đạt? |
|---|---|---|
| Task chính 22 ảnh (S01–S08 + D01–D06) | **4 ERROR, 0 WARN** | ❌ chưa đạt E1 |
| Task mở rộng S09–S10 | **0 lỗi, 0 cảnh báo** | ✅ |
| frame_meta | 26/26 ảnh, mỗi ảnh đúng 1 tag | ✅ |
| Số đối tượng (task chính) | person=18, face=18, face13=6, hand=12, child_seat=10, infant_seat=8, object=14, phone=1 | — |

**Kết luận: bài Vũ sạch về cấu trúc (frame_meta đủ, không box trùng, face13 đủ 13 điểm), nhưng mắc đúng 1 loại lỗi: `child_seat.occupied='yes'` — giá trị bị guideline 4.8 cấm.**

## 2. Lỗi ERROR phải sửa (4 box)

| Ảnh | Lỗi | Cách sửa (guideline 4.8) |
|---|---|---|
| S02_ir | `child_seat occupied='yes'` | Ghế có trẻ ngồi trong → đổi `unknown` (schema không cho `yes`) |
| S02_rgb | `child_seat occupied='yes'` | Như trên |
| S05_ir | `child_seat occupied='yes'` | Như trên |
| S05_rgb | `child_seat occupied='yes'` | Như trên |

Vũ chỉ cần sửa 4 thuộc tính này, export lại, chạy `cabin_check` → sẽ 0 lỗi.

## 3. Điểm mạnh của bài Vũ

- **`light` nhất quán `unknown`** trên toàn bộ ảnh SVIRO + DMS — đúng guideline 4.2 (ảnh dựng/NIR không đánh giá sáng tối). Đây là điểm **Vũ đúng hơn Huy** (10 dòng trong `disagreement_log.csv`).
- `seatbelt`: không có `worn_correct` trên SVIRO — đạt.
- Không phát hiện box trùng (khác bài Huy ở vòng 1–2).
- face13 D01–D06: đủ 13 điểm, có cờ occluded/outside.

## 4. Bất đồng giữa hai bài (đã ghi trong disagreement_log.csv của folder nộp Huy)

- **Vũ đúng / Huy sai (10 dòng):** `frame_meta.light` (D01, S02_rgb, S05_ir, S09_ir…) — Huy giữ `day`/`night`, Vũ để `unknown` theo 4.2. **Huy sẽ sửa.**
- **Huy đúng / Vũ sai (8 dòng):** 4 box `occupied='yes'` (mục 2); `object seat` S08_ir (trái ảnh = `rear_right` theo 4.4-3, Vũ ghi `rear_left`); mắt S01 `open/clear` trong khi 4.4-1 yêu cầu `unknown` khi IR-sim không thấy rõ.
- **Ambiguous — 2 bạn chưa thống nhất, cần coach (8 dòng):** vị trí box `face` S02_ir (IoU=0,44); `eye/glasses/gaze_zone` S06 (Vũ: open/none/center — Huy: unknown); `child_seat occupied` S07_ir (no vs unknown).

## 5. Kiến nghị gửi Vũ

1. Sửa 4 `occupied` → `unknown`, 1 `seat` S08_ir → `rear_right`, cân nhắc 6 thuộc tính mắt/kính S01 → `unknown` (4.4-1).
2. Chạy lại: `python scripts/cabin_check.py check export_final_v2.zip --expect 22 --out check_final_v2.txt` cho tới khi `0 lỗi`.
3. 3 ca ambiguous: hỏi chung coach trước 12:00 hôm nay.
4. Vũ cũng cần nộp `disagreement_log.csv` của **phía Vũ** (diff ngược bài Vũ × bài Huy) — nhắc bạn ấy.

## 6. Thống kê

- Vũ: 26 ảnh · 4 ERROR (toàn bộ cùng 1 loại) · 0 WARN
- Huy (bản chốt): 0 ERROR · 3 WARN đã giải trình
- Diff hai bài: 26 điểm khác → 10 Vũ đúng / 8 Huy đúng / 8 chờ coach
