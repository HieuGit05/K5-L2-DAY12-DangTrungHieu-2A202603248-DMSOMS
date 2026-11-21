# Tự đánh giá — Đặng Trung Hiếu · G01

## 1. Tự rà soát 4 cảnh

Chọn 4 ảnh (2 S##, 2 D##), đọc như người lạ.

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| S02_rgb | tag `frame_meta` | `wrong_class` | `modality = ir` trên ảnh `_rgb` (cả 8 ảnh S##_rgb đều sai như vậy) | Có (v2) |
| S04_rgb | `face` của người ở rear_middle | `missing` | Chỉ vẽ `person`, quên box `face` (OMS: người lớn kèm box face) | Có (v2) |
| D04 | `face`: `eye_left`, `eye_right` | `lr_flip` | Gán ngược mắt trái/phải trên ảnh xoay 90°; mắt trái là mắt có y lớn hơn. `gaze_zone = road` trong khi đầu ngửa lên (`guess`) | Có (v2) |
| D06 | `face`: `eye_left`, `eye_right` | `guess` | Ghi `open` trong khi hai mí khép rõ | Có (v2) |

Phát hiện thêm sau review, chưa sửa trong v2:
- D03 `person.seatbelt`: tài xế đang thắt dây dở, theo 4.7 phải là `unknown`, export v2 đang là `worn_incorrect` (`guess`).
- D03 `face13`: 4 điểm `right_eye_*` đặt gần ngang với mắt trái (y ≈ 505 so với 518), chưa theo trục dọc của ảnh xoay; cả 8 điểm mắt đã để `outside` (`box`/vị trí điểm).

## 2. Lỗi mình lặp lại nhiều nhất hôm nay

Lỗi: Để giá trị thuộc tính theo mặc định hoặc theo bản copy mà không kiểm lại: `modality = ir` lặp ở cả 8 ảnh `_rgb`; mắt để `open` ở D03, D04, D06 dù không thấy hoặc mắt đang nhắm.

Vì sao mình mắc: Mình copy shape và tag giữa ảnh `_ir` và `_rgb` rồi chỉ chỉnh box, không mở lại từng thuộc tính. Ở DMS, mình chọn `open` theo thói quen trước khi phóng to vùng mắt. `cabin_check` không kiểm `modality` hay trạng thái mắt nên báo 0 lỗi và mình tưởng đã đúng.

Lần sau tránh bằng cách: Sau mỗi lần Ctrl+V, mở `frame_meta` đầu tiên và đọc tên file (`_rgb` → rgb). Với mỗi mắt, phóng to rồi mới chọn, mặc định nghĩ `unknown` trước, chỉ đổi sang `open`/`closed` khi thấy rõ khe mắt hoặc hai mí. Trước khi export, lọc theo label `face` và rà riêng cột thuộc tính mắt của 6 ảnh D##.

## 3. Ghi chú về disagreement_log

Một dòng bạn của mình đúng và mình sai: D06 `eye_left`/`eye_right`, mình `open`, bạn `closed`. Mình học được: `closed` chỉ cần thấy rõ hai mí khép trong khung này. Mình đã gán theo cảm giác "tài xế đang ngồi bình thường" thay vì nhìn vào mắt. Mỗi khung phải đọc độc lập, không suy từ tư thế hay từ khung khác.
