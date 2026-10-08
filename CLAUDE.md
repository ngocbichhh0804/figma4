# Figma4 — Figma → HTML/CSS

Chuyển thiết kế Figma sang HTML semantic + CSS thuần. Không framework, không
bước build, **không một dòng JavaScript nào được gửi xuống trình duyệt**.

Chú thích trong code viết tiếng Việt cho dễ đọc lúc phát triển — chuyển sang
tiếng Anh trước khi bàn giao.

## Quy ước bắt buộc — đọc trước khi sửa bất cứ gì

- **Không flexbox, không grid, không `gap`.** Bố cục dựng bằng `float` +
  `calc()`, `clear`, `flow-root`, `inline-block` + `font-size: 0` ở cha,
  `line-height` + `vertical-align`, `position: absolute`.
- **Không JavaScript** trên trang giao cho khách. Trạng thái mở/đóng dùng
  `:target`, `<details>`, radio/checkbox + `:has()`. `dev/*-overlay.html` là
  công cụ nội bộ (cũng không JS).
- **CSS chia theo cascade layer**, khai báo một lần trong `<head>`:
  `reset, tokens, base, layout, components, utilities`. Không `!important`
  (trừ khối `prefers-reduced-motion`).
- **`css/tokens.css` chỉ chứa giá trị dùng chung** (màu, font, thang độ đậm,
  leading, chuyển động, bo góc). Số đo riêng của component ghi thẳng trong
  file component. Số dùng ở nhiều rule trong cùng component thì làm biến cục
  bộ trên selector gốc (vd. `.card { --card-title-gap: 87px; }`), không đưa lên
  `:root`. Mọi con số đều ghi nhãn nguồn gốc ngay bên cạnh:
  - `[FIGMA]` — đọc thẳng từ panel inspector
  - `[ĐO]` — đo bằng script quét pixel trên ảnh export 1:1
  - `[MẪU]` — lấy từ file asset được cung cấp
  - `[ƯỚC]` — ước lượng, chờ xác nhận lại
- Đặt tên BEM, specificity phẳng, không selector lồng quá một cấp, không dùng
  ID để style.
- **Pixel-perfect**: mỗi trang render đúng khổ khung Figma; đối chiếu bằng
  overlay + script so pixel. Đã chốt số đo thì không đổi.
- Font thương mại → thay bằng font OFL, tự lưu `.woff2` trong `assets/fonts/`
  (không gọi Google Fonts).

## Cấu trúc

```
*.html                   mỗi trang Figma một file
css/
├── reset.css / fonts.css / tokens.css / base.css / layout.css
├── components/          mỗi khối một file BEM
└── utilities.css        .visually-hidden, .skip-link, .at / .fs (toạ độ Figma ≥ 80rem)
assets/fonts|icon|img/   ảnh chụp: <tên>.png (1x) + <tên>@2x.png, dùng srcset
dev/pages.json           danh sách trang (nguồn sinh overlay)
dev/*-overlay.html       đè ảnh Figma lên bản code (Code / 50% / Figma / Slider)
docs/comparison/<slug>/  <slug>-figma.png, -build, -side-by-side, -overlay, -difference
```

## Toạ độ Figma (≥ 80rem)

- Mỗi section có `.frame` rộng 1920 đơn vị, chiều cao lấy từ `--frame-h`.
- Phần tử gắn lớp `.at` đặt toạ độ bằng `--x --y --w --h`. `.fs` đặt cỡ chữ bằng `--fs --lh`.
  - Các biến này không đơn vị, tính theo px của Figma, và đo từ mép trên section, hoặc từ cha nếu cha cũng là `.at`.
- `--u = min(1px, 100cqw / 1920)`. Không dùng `vw`, để thanh cuộn không làm lệch khung.
- Các biến toạ độ được đăng ký `@property … inherits: false`, nên phần tử con không vô tình nhận toạ độ của cha.

## Trạng thái các trang

- **Landing page** (`index.html`, 1920 × 6814): xong.
  - Sai lệch TB 1.8/765, 98.6 % pixel lệch ≤ 30, 56/73 vùng khớp 0px.
  - Component CSS: site-header, button, hero, brands, section-title, product-card, arrivals, payday, favourites, app-promo, newsletter, site-footer.
  - Overlay: `dev/home-overlay.html`. Vùng đo: `docs/comparison/home/regions.txt`.

## Chạy thử

```bash
npm install     # chỉ để có browser-sync
npm run dev     # http://localhost:3000
```

Không có npm: `python3 -m http.server 3000`.
