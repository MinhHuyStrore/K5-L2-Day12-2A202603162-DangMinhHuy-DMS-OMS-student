# Báo cáo QC — DMS/OMS · Đặng Minh Huy (2A202603162) — BẢN CHỐT

> Lab Ngày 12 · Guideline Cabin v1.0 · Kiểm bằng `scripts/cabin_check.py check`
> Vòng 1 (sáng): `OMS.zip`+`DMS.zip` — 10 ERROR · [không còn giữ file trung gian]
> Vòng 2 (17:35): bản sửa #1 — OMS còn 1 ERROR (box trùng S02_rgb chưa xóa)
> **Vòng 3 (19:50): `export_final_v3` — OMS 0 lỗi/3 WARN · DMS 0 lỗi/0 WARN → ĐẠT E1**
> Bộ nộp đã dựng đúng chuẩn SUBMISSION.md tại `K5-L2-DAY12-DangMinhHuy-2A202603162-DMSOMS/`

## 0. Kết luận cuối cùng

| Hạng mục | Trạng thái |
|---|---|
| E1 — `export_final.zip` (22 ảnh: S01–S08 + D01–D06, gộp từ 2 task CVAT, không kèm ảnh) | ✅ `check_final.txt`: **0 lỗi**, 2 WARN đã có dòng giải thích |
| E1 mở rộng — `export_extra.zip` (S09–S10) | ✅ `check_extra.txt`: 0 lỗi, 1 WARN có giải thích |
| E2 — `check_cpA.txt` / `check_cpB.txt` / `check_final.txt` | ✅ đủ 3, chạy trên bản nhãn hiện hành |
| E3 — `disagreement_log.csv` | ✅ **HOÀN THÀNH 22/11** — peer: Trương Tuấn Vũ (2A202603078). 26 dòng bất đồng, đã điền đủ verdict/rule/action: 10 dòng Vũ đúng (light→unknown, 4.2), 8 dòng Huy đúng (occupied=yes bị cấm, 4.8; seat S08 theo 4.4-3; mắt S01 unknown theo 4.4-1), 8 dòng ambiguous ghi chờ coach (face box S02_ir IoU=0,44; mắt/kính/gaze S06; occupied S07_ir). Bài Vũ cabin_check báo 4 ERROR occupied=yes — Vũ đã nhận sửa |
| E4 — `ir_log.csv` | 📝 6 quan sát — **bạn mở ảnh xác nhận từng dòng** |
| E5 — `class_list_v0.md` | 📝 7+7 lớp, 2 câu leo thang — rà lại trước nộp |
| Việc còn lại sau review chéo | ⚠️ Huy cần sửa trên CVAT: `light` → `unknown` ở D01/S02_rgb/S05_ir/S09_ir (kết luận peer đúng), export lại `export_final_v2.zip` + chạy `check_final_v2.txt` (rubric E3 yêu cầu nếu có sửa sau review) |

## 0a. Diễn biến sửa lỗi (tóm tắt)

OMS: 8 ERROR → 1 → 0. Đã sửa: đủ frame_meta 26/26 · `child_seat.occupied` hết giá trị `yes` · xóa toàn bộ box trùng S02_rgb (9→3 box) và S06_rgb · S05_rgb `driver`→`rear_right` · S03 object thống nhất `rear_right` · `light` SVIRO chuẩn hóa `unknown` còn 3 ảnh giải trình được.

## 1. Tổng quan lần 2 (lưu lịch sử)

| Tiêu chí | Lần 1 | **Lần 2** | Đạt? |
|---|---|---|---|
| DMS: `cabin_check` | 2 ERROR, 1 WARN | **0 lỗi, 0 cảnh báo** | ✅ **ĐẠT E1 (DMS)** |
| OMS: `cabin_check` | 8 ERROR, 16 WARN | **1 ERROR, 7 WARN** | ⚠️ còn 1 lỗi + box trùng |
| Mỗi ảnh 1 `frame_meta` | thiếu 5/26 | **26/26 đủ** | ✅ đã sửa hết |
| `child_seat.occupied='yes'` | 4 box | **còn 1 box** (S02_rgb) | ❌ |
| Box trùng lặp | S02_rgb 9 box, S06_rgb lặp | **vẫn còn nguyên** | ❌ chưa sửa |

**Kết luận: DMS đã đạt. OMS còn 1 ERROR + 3 điểm dữ liệu sai phải sửa lần 3** (mục 3). Sửa xong export `export_final_v3.zip`, chạy lại check đến khi `Kết quả: 0 lỗi`.

## 1a. Diễn biến sửa lỗi (lần 1 → lần 2)

| Lỗi lần 1 | Trạng thái |
|---|---|
| Thiếu frame_meta: S02_rgb, S03_ir, S06_rgb, D05, D06 | ✅ Đã thêm đủ, `light` đổi về `unknown` phần lớn ảnh SVIRO |
| `occupied='yes'`: S02_ir, S05_ir, S05_rgb | ✅ Đã đổi `no`/đúng |
| `occupied='yes'`: S02_rgb | ❌ **Còn 1 box** (box child_seat trùng thứ hai vẫn `yes`) |
| ERROR/WARN DMS (frame_meta D05/D06, cờ mắt D06) | ✅Sửa hết — DMS sạch sẽ hoàn toàn |
| Box trùng S02_rgb, S06_rgb | ❌ **Chưa xóa** |
| S05_rgb `seat=driver` | ❌ **Chưa sửa** |
| S03_ir/rgb lệch ghế object | ❌ **Chưa thống nhất** (ir vẫn `rear_middle`, rgb `rear_right`) |

## 2. Lỗi còn lại ở lần 2 (phải sửa trước khi nộp)

### OMS — `export_final_v2.zip` (1 ERROR + 3 vấn đề dữ liệu)

| # | Ảnh | Vấn đề | Mức độ | Cách sửa |
|---|---|---|---|---|
| 1 | **S02_rgb** | `child_seat.occupied='yes'` (box child_seat trùng thứ 2) | **ERROR** | Xóa box trùng này là hết error (xem #2) |
| 2 | **S02_rgb** | **Vẫn còn box trùng y hệt lần 1**: `person rear_left` ×**4**, `person rear_right` ×**2**, `child_seat rear_left` ×**2** (tổng 9 box, ảnh S02_ir chỉ 3 box) | WARN nhưng **là lỗi dữ liệu nghiêm trọng** | Trong CVAT: mở S02_rgb, chọn từng box trùng (Ctrl+A hoặc đếm trong list), **xóa** cho tới khi còn đúng `person×2 + child_seat×1` |
| 3 | **S06_rgb** | `person rear_middle (427,360,549,612)` trùng ×2 | WARN, lỗi dữ liệu | Xóa 1 box → khớp S06_ir |
| 4 | **S05_rgb** | `person seat=driver` (S05_ir đã đúng `rear_right`) | WARN, **sai nhãn theo guideline** | Đổi `seat` → `rear_right` (camera SVIRO chỉ chiếu hàng ghế sau) |
| 5 | S02_rgb, S05_ir, S09_ir | `light='day'` trong khi ảnh ir/rgb cùng cặp đã `unknown` | WARN | Đồng bộ `light=unknown` (SVIRO là IR-sim, không đánh giá được sáng/tối) |
| 6 | S03_ir vs S03_rgb | object (x≈141): ir=`rear_middle`, rgb=`rear_right` | WARN, lệch cặp | Bên trái ảnh = `rear_right` → sửa **S03_ir** thành `rear_right` |

### DMS — `export_final_v2_dms.zip`: ✅ **0 lỗi, 0 cảnh báo — ĐẠT, không cần sửa gì thêm**

- 6/6 ảnh có `frame_meta`, face/face13 đủ 13 điểm với cờ `occluded`/`outside`, 12 tay, 1 phone hợp lệ.
- Cờ mắt D06 (warn lần 1) đã được xử lý.

## 3. Bất nhất cặp rgb ↔ ir còn tồn tại (OMS)

Sau khi đối chiếu lại từng cặp ở lần 2, chỉ còn 2 cặp lệch:

| Cặp | Lệch | Kết luận |
|---|---|---|
| **S05** | rgb: person `seat=driver`; ir: `rear_right` | Sửa rgb → `rear_right` |
| **S03** | ir: object trái ảnh `rear_middle`; rgb: `rear_right` | Sửa ir → `rear_right` (bên trái ảnh = ghế phải xe) |

Các cặp S01, S04, S06 (trừ box trùng), S07–S10 đã **khớp nhau** sau lần sửa. `light` đã chuẩn hóa `unknown` ở 17/20 ảnh, còn 3 ảnh ở mục 2.5.

## 4. Kiểm theo tiêu chí RUBRIC (trạng thái lần 2)

| Tiêu chí rubric | Trạng thái | Ghi chú |
|---|---|---|
| **E1** (10đ): 0 lỗi, mỗi ảnh 1 `frame_meta` | ⚠️ frame_meta **26/26 đạt**; còn 1 ERROR ở S02_rgb | Sửa xong mục 2.1–2.3 là đạt |
| **OMS ghế + lớp** (20đ) | ⚠️ Còn S05_rgb `driver`, S03_ir lệch ghế, box trùng S02/S06 | `seatbelt` không còn `worn_correct` trên S## — đạt |
| **DMS** (25đ): face13, cờ, thuộc tính | ✅ **Đạt toàn bộ** (0 lỗi 0 warn) | `on_wheel=no` bình thường (xe tự lái); `phone held` D05 hợp lý |
| **IR / ir_log.csv** (E4, 15đ) | ❓ Chưa kiểm được — file `templates/ir_log.csv` chưa được điền | Nhắc nộp kèm, ≥4 quan sát, phân biệt IR-sim (S##) vs NIR thật (D##) |
| **Review–rework** (E2, E3 — 15đ) | ⚠️ `check_cpB.txt` đã có; cần `check_cpA.txt`, `check_final.txt` của file đạt, `disagreement_log.csv` | File này + check_*.txt là bằng chứng E2 |
| **E5 class list** (10đ) | ❓ Chưa thấy `class_list_v0.md` đã điền | Nhắc nộp |
| Tự đánh giá (5đ) | ❓ `self_review.md` chưa điền | Nhắc nộp |

## 5. Việc cần làm cho lần 3 (thứ tự ưu tiên)

1. **Xóa box trùng** trên CVAT: S02_rgb (9→3 box), S06_rgb (4→3 box). Việc này đồng thời xóa ERROR `occupied='yes'` còn lại.
2. **Sửa 2 ghế lệch**: S05_rgb `driver`→`rear_right`; S03_ir object `rear_middle`→`rear_right`.
3. **Đồng bộ `light=unknown`** cho S02_rgb, S05_ir, S09_ir.
4. Export `export_final_v3.zip` (gộp cả DMS nếu nộp chung task, hoặc 2 file riêng) và chạy:
   ```
   python scripts/cabin_check.py check export_final_v3.zip --out check_final_v3.txt
   ```
   Dòng cuối phải là `Kết quả: 0 lỗi`. Với 7 WARN còn lại: thêm dòng giải thích vào cuối file (CHECKPOINTS yêu cầu).
5. Chạy lệnh checkpoint chuẩn với `Gxx/` đã chép vào repo (bỏ comment `#` khi paste, chạy từng dòng `python`):
   ```
   python scripts/cabin_check.py check export_final_v3.zip --only S01,S02 --expect 4 --manifest Gxx/oms/manifest.csv --out check_cpA.txt
   ```
6. Hoàn thiện các bằng chứng còn thiếu: `ir_log.csv` (≥4 dòng), `class_list_v0.md`, `self_review.md`, `prompts.md`, `disagreement_log.csv` (diff với bạn cùng lớp).

## 6. Thống kê nhanh lần 2

- **OMS**: 20 ảnh · frame_meta 20/20 ✅ · 75 đối tượng (person=22, child_seat=13, object=14, infant_seat=10, face=16) · **1 ERROR, 7 WARN** (lần 1: 8/16)
- **DMS**: 6 ảnh · frame_meta 6/6 ✅ · face=6, face13=6, hand=12, phone=1 · **0 ERROR, 0 WARN** (lần 1: 2/1) ✅
- Tiến độ tổng thể: **10 lỗi → 1 lỗi**. Chỉ còn xử lý S02_rgb/S06_rgb là E1 đạt.

---

## Phụ lục A — Trạng thái lần 1 (lịch sử, đã xử lý)

**OMS.zip (8 ERROR):** S02_ir/S02_rgb×2/S05_ir/S05_rgb `child_seat.occupied='yes'`; thiếu frame_meta ở S02_rgb, S03_ir, S06_rgb. **DMS.zip (2 ERROR):** thiếu frame_meta D05, D06. **WARN đáng chú ý:** box trùng S02_rgb (person rear_left ×4, rear_right ×3), S05_rgb `seat=driver`, S03 lệch cặp, `light='day'` trên SVIRO, D06 cờ mắt chưa khớp.
→ Tất cả đã sửa ở lần 2, **ngoại trừ**: box trùng S02_rgb/S06_rgb, S05_rgb `driver`, S03_ir, `light` 3 ảnh, và 1 box `occupied='yes'` còn sót.
