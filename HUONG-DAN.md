# Poker Log: cách cài lên iPhone

Đây là web app (PWA). Sau khi "Thêm vào Màn hình chính", nó mở toàn màn hình như app thật, chạy được khi không có mạng và không cần Mac hay tài khoản Apple Developer.

## 1. Đưa app lên mạng (làm 1 lần, miễn phí)

App cần một địa chỉ `https://` để iPhone cài được. Cách dễ nhất là **GitHub Pages**:

1. Tạo tài khoản ở github.com (nếu chưa có).
2. Bấm **New repository**, đặt tên ví dụ `poker-log`, chọn **Public**, rồi bấm **Create**.
3. Bấm **uploading an existing file** và kéo thả **tất cả file trong thư mục này** vào:
   `index.html`, `manifest.webmanifest`, `sw.js`, `icon-180.png`, `icon-192.png`, `icon-512.png`.
   Bấm **Commit changes**.
4. Vào **Settings → Pages**. Ở mục *Branch* chọn `main`, thư mục `/ (root)`, rồi bấm **Save**.
5. Đợi khoảng 1 phút. Địa chỉ app sẽ có dạng `https://<tên-github>.github.io/poker-log/`.

> Repo Public thì ai biết link cũng mở được trang, nhưng **dữ liệu của bạn không nằm trên đó**. Dữ liệu chỉ lưu trong iPhone của bạn.

Có thể dùng cách khác nếu muốn: Cloudflare Pages (Direct Upload) hoặc Netlify (kéo thả thư mục).

## 2. Cài lên iPhone

1. Mở link trên bằng **Safari** (bắt buộc dùng Safari).
2. Bấm nút **Chia sẻ** (ô vuông có mũi tên) rồi chọn **Thêm vào MH chính** (*Add to Home Screen*).
3. Mở app từ icon ♠ trên màn hình chính.

## 3. Sử dụng

- **Nhật ký**: bấm **+** để thêm tour. Mỗi tour gồm ngày, tên tour, buy-in, số lần re-buy/re-entry (nút − / +), tiền thu về và ghi chú. Tổng chi được tính bằng buy-in × (1 + số re-buy). Chạm vào một tour để sửa hoặc xoá.
  - Gõ lại tên tour đã từng chơi thì app tự điền buy-in cũ. Các mức buy-in gần đây hiện thành nút bấm nhanh.
- **Thống kê**: chuyển giữa *Theo ngày* (các ngày trong một tháng) và *Theo tháng* (các tháng trong một năm). Phần này có:
  - tổng chi, thu, lãi/lỗ, ROI và tỉ lệ ITM;
  - biểu đồ cột chi ra / thu về (chạm vào cột để xem số liệu, vuốt ngang để xem ngày khác);
  - biểu đồ lãi/lỗ luỹ kế;
  - bảng chi tiết;
  - tổng từ trước đến nay.
- **Cài đặt**: đổi đơn vị tiền, xuất file sao lưu (.json), xuất bảng tính (.csv), khôi phục dữ liệu từ file.

## 4. Đăng nhập

- Lần đầu mở app sẽ phải đăng nhập. Sau đó app nhớ trạng thái đăng nhập trên máy, tới khi bạn bấm **Cài đặt → Đăng xuất**.
- Tài khoản không phân biệt chữ hoa/thường. Mật khẩu thì có phân biệt.
- Trong `index.html` chỉ lưu **mã băm** (PBKDF2-SHA256, có salt, 200.000 vòng) của tài khoản + mật khẩu, không lưu mật khẩu thô. Vì vậy upload lên repo Public thì người khác cũng không đọc được mật khẩu.
- Đây là khoá ở phía giao diện. Nó đủ để người lạ mở link không dùng được app, nhưng không phải bảo mật cấp ngân hàng: người rành kỹ thuật vẫn có thể sửa code để bỏ qua bước đăng nhập. Dù vậy, họ cũng **không thấy được dữ liệu của bạn**, vì dữ liệu chỉ nằm trên iPhone của bạn.

**Đổi tài khoản / mật khẩu**: mở PowerShell (máy có cài Node.js) và chạy lệnh dưới đây, thay `TAIKHOAN` và `MATKHAU`:

```bash
node -e "const c=require('crypto');const s=c.randomBytes(16);console.log('salt:',s.toString('hex'));console.log('hash:',c.pbkdf2Sync(process.argv[1].trim().toLowerCase()+'\n'+process.argv[2],s,200000,32,'sha256').toString('hex'))" "TAIKHOAN" "MATKHAU"
```

Sau đó dán hai giá trị `salt` và `hash` vào phần `const AUTH = {...}` trong `index.html` rồi upload lại. Bạn sẽ phải đăng nhập lại bằng mật khẩu mới.

## 5. Lưu ý về dữ liệu

- Dữ liệu lưu **trên iPhone, trong app đã cài**. App không gửi dữ liệu đi đâu cả.
- Nếu **xoá app khỏi màn hình chính** hoặc **đổi sang link khác**, dữ liệu sẽ mất. Vì vậy nên vào **Cài đặt → Xuất file sao lưu** định kỳ và lưu file vào Files / iCloud Drive. Muốn lấy lại thì dùng **Khôi phục từ file sao lưu**.
- Khi sửa code và upload lại lên GitHub, app sẽ tự cập nhật ở lần mở tiếp theo (cần có mạng). Dữ liệu vẫn giữ nguyên.
