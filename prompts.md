# Khai báo dùng AI — Đặng Trung Hiếu

Nhắc lại: không đưa ảnh cabin hay ảnh tài xế (kể cả ảnh chụp màn hình) cho bất kỳ công cụ AI nào.

| # | Công cụ | Bạn hỏi gì (tóm tắt) | Bạn dùng kết quả thế nào | Bạn có kiểm lại không? Bằng gì? |
|---|---|---|---|---|
| 1 | Claude Code (chạy trên máy, trong VS Code) | Hướng dẫn CP7: phân xử `disagreement_log.csv` theo guideline | Dùng danh sách sửa để tự sửa trên CVAT, export `export_final_v2.zip`. Trong lúc phân tích, công cụ tự mở một số ảnh D## trên máy (không tải lên đâu) | Có: chạy lại `cabin_check` (`check_final_v2.txt`), đối chiếu GUIDE |
| 2 | Claude Code | Giải thích quy tắc 4.2, 4.3 (box `face` cho người lớn OMS, `modality` ảnh `_rgb`) | Hiểu quy tắc rồi tự sửa trên CVAT | Có, đối chiếu GUIDE §4.2, §4.3 |
| 3 | Claude Code | Nháp `ir_log.csv`, `class_list_v0.md` (công cụ có mở ảnh ghép `ir_pairs/` trên máy) | Đọc lại, sửa theo quyết định của coach (cho thêm `occupied=yes`) | Có, đọc lại từng dòng |
| 4 | Claude Code | Nháp `self_review.md`, `TEAM.md` | Tự rà và sửa câu chữ | Có, đọc lại từng dòng |
