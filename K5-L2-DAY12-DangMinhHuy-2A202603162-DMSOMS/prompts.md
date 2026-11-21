# Khai báo dùng AI — Đặng Minh Huy (2A202603162)

Nhắc lại: không đưa ảnh cabin hay ảnh tài xế (kể cả ảnh chụp màn hình) cho bất kỳ công cụ AI nào. **Tuân thủ: chỉ đưa file XML export nhãn (không chứa ảnh) và tài liệu lab cho công cụ.**

| # | Công cụ | Bạn hỏi gì (tóm tắt) | Bạn dùng kết quả thế nào | Bạn có kiểm lại không? Bằng gì? |
|---|---|---|---|---|
| 1 | GitHub Copilot (VS Code) | Đọc `annotations.xml` của 2 file export + tài liệu lab, chỉ ra lỗi so với Guideline Cabin v1.0 | Nhận diện 10 lỗi (thiếu frame_meta, `occupied='yes'`, box trùng, seat sai) rồi tự sửa trên CVAT | Có — bằng `scripts/cabin_check.py check` (máy chấm chuẩn của lab), không tin kết quả suy luận của AI |
| 2 | GitHub Copilot (VS Code) | Soạn cấu trúc thư mục nộp + bản nháp `ir_log.csv`, `class_list_v0.md` | Dùng làm dàn ý, tự kiểm tra lại từng dòng với ảnh trên CVAT trước khi nộp | Có — đối chiếu nhãn thực tế trong XML |
