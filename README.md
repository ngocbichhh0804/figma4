# Figma4 — Figma → HTML/CSS

Cắt tay từ Figma sang HTML semantic và **CSS thuần**. Không framework, không
bước build, **không JavaScript**.

**Live:** _điền URL_ · **Source:** _điền URL_ · **Thiết kế:** _điền link Figma_

## Độ khớp với bản thiết kế

| Trang | Khung Figma | Sai lệch màu TB (/765) | Pixel lệch ≤ 30 | Ghi chú |
| --- | --- | --- | --- | --- |
| Landing page | 1920 × 6814 | 1.8 | 98.6 % (≤ 60: 99.5 %) | 56/73 vùng khớp 0px, 17 vùng lệch 1px ở mép chữ (khác engine dựng chữ) |

Ảnh đối chiếu nằm trong `docs/comparison/<trang>/`. Tự kiểm trực tiếp: mở
`dev/<trang>-overlay.html` (bốn chế độ Code only / Overlay 50% / Figma only /
Slider — không JavaScript, nút là radio input đọc bằng `:has()`).

Danh sách vùng đo nằm trong `docs/comparison/home/regions.txt`.

## Không flexbox, không grid

Bố cục dựng bằng kỹ thuật trước thời flexbox: `float` + `calc()`, `flow-root`,
`inline-block` + `font-size: 0`, `line-height` + `vertical-align`,
`position: absolute`.

## Font

| Font Figma | Thay bằng (OFL) | Weight |
| --- | --- | --- |
| Poppins | Poppins (dùng nguyên bản, OFL) | 400, 500, 600, 700, 900 |
| Roboto | Roboto (dùng nguyên bản, OFL) | 400 |

## Responsive

- Đã kiểm 17 khổ từ 320 tới 2560px, cùng điện thoại xoay ngang (568/667/844 × 320). Không khổ nào cuộn ngang.
- **Dưới 80rem (1280px)**: bố cục chảy tự nhiên. Điện thoại 1 cột, menu thu vào nút ☰ (checkbox + `:has()`). Từ 48rem (768px) lên 2 cột.
- **Từ 80rem**: đặt đúng toạ độ Figma trong khung `.frame` 1920 đơn vị. Đơn vị `--u = min(1px, 100cqw / 1920)`, nên từ khổ 1920 trở lên khớp pixel, hẹp hơn thì cả khung thu theo tỉ lệ.
- **Trên 1920px**: khung giữ 1920px và canh giữa, nền section trải hết bề ngang.
- Số đo riêng cho khổ nhỏ đều gắn nhãn `[ƯỚC]`, vì Figma chỉ có khổ 1920.
- Cảnh báo còn lại, đã chấp nhận:
  - *Vùng bấm menu < 24px ở khổ 1280.* Script chỉ đo khung chữ; vùng bấm thật đã được nới bằng `::after` (inset −12px/−8px), và các link cách nhau hơn 24px.
  - *Nét quét dưới "NEW ARRIVALS" bị kéo giãn ~10%.* Xem mục dưới.

## Chỗ khác bản thiết kế

- **Ngôi sao trang trí ở payday** không có trong asset. Em tách từ ảnh Figma: lấy hiệu so với nền, rồi lưu thành `assets/img/star-payday-*.png` có kênh alpha.
- **Gradient nền payday** (21 điểm màu): cắt một cột 1px từ ảnh Figma (`payday-gradient.png`) rồi kéo giãn ngang, thay vì gõ tay 21 điểm.
- **Vector 8** (nét quét dưới "NEW ARRIVALS", 186×34.94) chưa được export. Đang tạm dùng Vector 9 (190×32), kéo giãn cho đủ khung.
- Chữ SIGN UP dùng 24px theo spec. Chữ các mục menu 22px.

## Ảnh

- Ảnh chụp lấy từ bản export PNG khổ lớn của Figma (hero 4x, còn lại 3x).
- Mỗi ảnh có 2 bản: `<tên>.png` 1x để khớp pixel với Figma, và `<tên>@2x.png` cho màn Retina. Trình duyệt tự chọn qua `srcset`.
- **Tạm thời:** `payday-girl.png` được cắt từ ảnh Figma 1:1 (vùng 0,2491 → 988,3459) vì chưa có file export `image 12`. Bản này chỉ có 1x nên hơi mờ trên Retina. Khi có PNG 3x thì tạo lại bản 1x và @2x như các ảnh khác.
- Ngôi sao hero dùng `Star 9.png` export từ Figma. Ngôi sao khối payday vẫn tách từ ảnh Figma.

## Còn cần từ phía khách

- Export **Vector 8** (nét quét dưới "NEW ARRIVALS") dạng SVG.
- Export **SVG** cho: logo-mark (Vector), nét quét (Vector 9), logo 6 hãng (Rectangle 36/38/41/43/44/45), icon mạng xã hội (Group 10–13), nút App Store / Google Play (Rectangle 55/18). Hiện chỉ có PNG 1x nên màn Retina sẽ hơi mờ.
- Ảnh **image 12** (người mẫu khối Payday), PNG 3x.
- **Ngôi sao trang trí ở payday**, nếu có layer riêng.
