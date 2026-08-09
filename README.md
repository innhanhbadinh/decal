# Landing Page Decal — bản TĨNH (không cần server)

Chỉ 2 file: `admin.html` (chỉnh sửa) + `index.html` (trang thật). Không Node.js, không database,
không server nào cả — công thức tính giá + toàn bộ dữ liệu Excel (giá in/giấy/bế) đã **nhúng thẳng**
vào file, chạy hoàn toàn trên trình duyệt khách. Deploy ở đâu cũng được, **miễn phí vĩnh viễn**.

## Vì sao đổi sang bản này

Bản cũ (có server Node/Express + database) cần gói Render trả phí (~190k/tháng) để dữ liệu
không bị mất khi deploy lại. Nếu làm nhiều landing page, chi phí đó nhân lên nhiều lần. Bản tĩnh
này không cần server nào — deploy bao nhiêu trang cũng free.

**Đánh đổi:** không còn "sửa xong tự động lên web ngay". Sửa gì cũng phải qua đúng 1 bước:
sửa trong `admin.html` → bấm **"⬇ Xuất file HTML"** → thay file `index.html` → đẩy lên GitHub.

## Chạy thử ở máy mình

Mở thẳng `admin.html` bằng trình duyệt (double-click là được, không cần cài gì, không cần chạy lệnh nào).

- Sửa nội dung/giá ở panel bên trái, xem trước ngay bên phải.
- Mục **"📊 Dữ liệu Excel"**: tải file `.xlsx` mới lên để cập nhật giá — xử lý ngay trên trình
  duyệt (không gửi đi đâu cả), nhớ xuất file lại sau khi đổi.
- Bấm **"⬇ Xuất file HTML"** → tải về `decal-tem-nhan-landing.html` → đổi tên thành `index.html`.

## Đưa lên GitHub

```bash
git init
git add .
git commit -m "Landing page decal — bản tĩnh"
git branch -M main
git remote add origin https://github.com/<tên-tài-khoản>/<tên-repo>.git
git push -u origin main
```

## Deploy — 3 lựa chọn, đều FREE vĩnh viễn (không giới hạn thời gian, không bị "ngủ")

### GitHub Pages (đơn giản nhất)
Repo → **Settings → Pages** → Deploy from a branch → chọn `main` → `/ (root)`.
Có link ngay: `https://<tên>.github.io/<repo>/`

### Vercel
vercel.com → **Add New → Project** → chọn repo → không cần cấu hình gì (không có build step) → Deploy.

### Render Static Site
Render dashboard → **New → Static Site** → chọn repo → Build Command để trống, Publish Directory
để `.` (dấu chấm, nghĩa là thư mục gốc) → Deploy. **Lưu ý:** chọn đúng "Static Site", KHÔNG phải
"Web Service" — Static Site free vĩnh viễn, không cần thẻ, không bị ngủ.

## Làm nhiều landing page

Copy nguyên cặp `admin.html` + `index.html` sang thư mục/repo mới, mở `admin.html`, đổi nội dung/giá
cho sản phẩm khác, xuất file, đẩy lên 1 repo GitHub mới → deploy y hệt các bước trên. Mỗi landing page
là 1 repo độc lập, không đụng gì đến các trang khác.

## Cách hoạt động (để hiểu, không cần biết để dùng được)

- `pricingEngineClient.js` (đã nhúng sẵn vào `admin.html`) — công thức tính giá thuần JS, port lại
  y hệt logic đã kiểm chứng ở bản server trước đó — đã test đối chiếu ra đúng từng đồng.
- Dữ liệu 6 sheet cần thiết (GIAYDECAL, BETHUONG, KETP, BEKHO, INGIAY, INDECAL) từ `bangtinh.xlsx`
  được nhúng thẳng dạng JSON (~3KB) — không cần đọc file Excel lúc khách xem trang.
- Khi khách đổi lựa chọn trên trang, JS tính lại **ngay lập tức, không có độ trễ mạng** (khác bản
  server phải chờ gọi API).

## Giới hạn so với bản server

- Không còn nút "Lưu lên server" — mọi thay đổi phải xuất file + đẩy lên Git mới lên trang thật.
- `admin.html` tự lưu nháp vào bộ nhớ trình duyệt (`localStorage`) để không mất khi lỡ đóng tab —
  nhưng chỉ lưu ở **đúng trình duyệt, đúng máy** đó, không đồng bộ giữa các máy/trình duyệt khác nhau.
- Ảnh tải lên từ máy giờ **nhúng thẳng vào file HTML** (base64) — nên dùng ảnh dưới ~800KB, ảnh lớn
  hơn nên dán URL từ web thay vì tải trực tiếp, tránh file HTML quá nặng.
