# Danh sách lớp v0 — Đặng Trung Hiếu · G01

Viết tiếp từ slide 54 bài giảng. Mỗi lớp trả lời: **model cần nhận ra cái này để làm gì?**

## DMS (8 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng DMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `eye_left`, `eye_right` | open / closed / unknown | Phát hiện buồn ngủ: tỉ lệ thời gian nhắm mắt, nhắm mắt kéo dài | Có, trừ khi đốm phản xạ trên kính che khe mắt (D04) hoặc mặt quay khỏi đèn (D03) |
| 2 | `gaze_zone` | road, mirror_l/r/c, cluster, center, lap, other | Phát hiện mất tập trung (rời mắt khỏi đường quá lâu); kiểm tra tài xế đã nhìn gương trước khi chuyển làn | Có khi thấy mắt và hướng đầu; mất khi mắt bị che |
| 3 | `face13` (13 điểm mặt) | tọa độ + cờ occluded/outside | Ước lượng hướng đầu (head pose), độ mở mắt, phát hiện ngáp (khoảng cách hai môi) | Phần lớn có; điểm mắt mất dưới đốm phản xạ, điểm miệng mất khi tay hoặc chai che (D02) |
| 4 | `hand.on_wheel` | yes / no / unknown | Hands-on detection: tài xế có sẵn sàng nhận lại lái khi xe tự lái yêu cầu không | Có; vật cầm gần camera có thể cháy sáng làm mất mép tay (D06) |
| 5 | `phone.state` | held / mounted / lap | Phát hiện dùng điện thoại khi lái (gọi điện, nhắn tin), phân biệt với điện thoại để trên giá | Chỉ theo hình dạng: màn hình không sáng khác biệt trên NIR, máy tối áp tai dễ lẫn với tay |
| 6 | `glasses` | none / clear / sun / unknown | Chọn cách đọc trạng thái mắt; cảnh báo khi kính che mắt để không kết luận sai về buồn ngủ | Khó: kính râm có thể trông trong suốt trên NIR, nên hay phải ghi unknown |
| 7 | `person.seatbelt` (tài xế) | worn_correct / worn_incorrect / not_worn / unknown | Nhắc thắt dây; phát hiện thắt sai (dưới nách, sau lưng) | Có khi dây tối trên áo sáng (D04); mất trên áo tối hoặc khi đang thắt dở (D03) |
| 8 | `face` (box) | có / không | Xác nhận có tài xế trong ghế và định vị vùng đầu cho các lớp 1–3 | Có |

## OMS (8 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng OMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `person.seat` | driver, front_passenger, rear_left/middle/right | Đếm người theo từng ghế; bật nhắc dây an toàn và túi khí đúng ghế | Có (hình dạng người) |
| 2 | `person.seatbelt` (ghế sau) | not_worn / unknown (SVIRO) | Nhắc hành khách ghế sau thắt dây | Một phần: dây tối trên áo tối khó thấy (S02 IR-sim) |
| 3 | `infant_seat.facing` | rear / front | Ghế em bé quay ngược đặt ghế trước thì bắt buộc tắt túi khí; nhắc quên trẻ trong xe | Có (hình dạng nôi, quai xách) |
| 4 | `child_seat.occupied` | no / unknown / yes (coach cho thêm `yes`) | Biết ghế trẻ em có trẻ hay không để nhắc quên trẻ và chỉnh túi khí | Có hình dạng ghế; bên trong ghế tối khó thấy trên RGB tối, IR-sim rõ hơn (S01) |
| 5 | `object.type` | bag / box / cup / other | Nhắc quên đồ khi rời xe; phân biệt đồ vật với người để không báo nhầm | Hình dạng có, màu mất (ba lô đỏ S06 thành xám) |
| 6 | `face` (OMS, chỉ box) | có / không | Xác nhận đúng là người (không phải đồ vật hay áo treo), đếm người, ước lượng vị trí đầu cho túi khí | Có |
| 7 | `frame_meta.camera_blocked` | yes / no | Camera bị che thì hệ thống báo lỗi cảm biến, thay vì kết luận nhầm là "xe trống" | Có |
| 8 | `child` (đề xuất, chưa có trong v1.0) | có / không, theo ghế | Child presence detection: phát hiện trẻ bị bỏ quên trong xe; đây là chức năng OMS quan trọng nhất nhưng guideline v1.0 chưa có lớp | Có (S03, S05 thấy rõ trẻ trên IR-sim) |

## 2 câu hỏi leo thang

1. Câu hỏi: Khi ghế trẻ em hoặc ghế em bé **đang có trẻ ngồi** thì gán thế nào? Guideline v1.0 chỉ định nghĩa `child_seat` và `infant_seat` là ghế trống, `occupied` không có giá trị `yes`, còn §3 nói bộ ảnh không có trẻ ngồi. Nhưng trong bộ G01 có trẻ thật.
   Khung ví dụ: G01 S05 (hai trẻ ngồi trong hai `child_seat`), G01 S03 (trẻ ngồi trong `child_seat` rear_left), G01 S02 (em bé nằm trong `infant_seat` rear_right).
   Bạn tạm gán thế nào và vì sao: Đã hỏi coach. Coach cho thêm giá trị `yes` vào `occupied`, nên `child_seat` ở S03 và S05 ghi `occupied = yes`; không vẽ box riêng cho trẻ. Đề nghị guideline v1.x ghi chính thức giá trị `yes` (kèm định nghĩa "thấy rõ có trẻ trong ghế") và cho biết `infant_seat` có em bé (S02) cần thuộc tính tương tự hay không, vì hiện `infant_seat` chưa có trường nào ghi ghế có trẻ.

2. Câu hỏi: Trên ảnh NIR thật (D##) thì gán `frame_meta.light` thế nào? Guideline chỉ nói ảnh dựng SVIRO ghi `light = unknown`. Ảnh D## dùng đèn hồng ngoại chủ động nên không biết bên ngoài xe là ngày hay đêm.
   Khung ví dụ: G01 D01 và G01 D04 (cửa kính tối, chỉ mặt và áo tài xế được đèn IR chiếu sáng).
   Bạn tạm gán thế nào và vì sao: Ghi `light = unknown` theo quy tắc 4.4-1 (không thấy thì unknown), không đoán `night` chỉ vì ảnh tối, vì ảnh NIR tối không có nghĩa là trời tối.
