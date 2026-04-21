# Kevin & Ady Wedding Website

Website cưới đầy đủ với admin panel để quản lý nội dung.

## Tech Stack

- **Backend**: Node.js + Express
- **Frontend**: HTML/CSS/Vanilla JS (không React)
- **Database**: JSON file
- **Auth**: JWT + bcrypt
- **Upload**: Multer + Sharp (auto resize)

## Cài đặt

### 1. Clone & Install

```bash
cd wedding-site
npm install
```

### 2. Cấu hình môi trường

```bash
cp .env.example .env
```

Mở `.env` và chỉnh:
```
DEFAULT_ADMIN_PASSWORD=MatKhauBienCuaBan123
JWT_SECRET=chuoi_ngau_nhien_dai_it_nhat_32_ky_tu
PORT=3000
```

Tạo JWT_SECRET ngẫu nhiên:
```bash
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
```

### 3. Chạy server

**Development** (auto-reload):
```bash
npm run dev
```

**Production**:
```bash
npm start
```

## Truy cập

| URL                          | Mô tả               |
|------------------------------|---------------------|
| `http://localhost:3000`      | Website cưới public |
| `http://localhost:3000/admin`| Admin panel         |

## Lần đầu sử dụng

1. Truy cập `http://localhost:3000/admin`
2. Đăng nhập bằng mật khẩu trong `.env` → `DEFAULT_ADMIN_PASSWORD`
3. **Đổi mật khẩu ngay** trong Admin → Đổi mật khẩu
4. Thêm nội dung từng section

## Cấu trúc thư mục

```
wedding-site/
├── server.js           # Express server + tất cả API endpoints
├── package.json
├── .env                # (tự tạo từ .env.example)
├── data/
│   ├── content.json    # Toàn bộ nội dung website
│   ├── rsvp.json       # Danh sách RSVP của khách
│   └── users.json      # Admin account (auto-tạo lần đầu)
├── public/
│   ├── index.html      # Website cưới
│   ├── admin.html      # Admin panel
│   ├── css/
│   │   ├── site.css
│   │   └── admin.css
│   ├── js/
│   │   ├── site.js
│   │   └── admin.js
│   └── images/         # Ảnh được upload
└── uploads/            # Temp folder multer
```

## API Endpoints

| Method | Path                     | Auth  | Mô tả                    |
|--------|--------------------------|-------|--------------------------|
| POST   | `/api/login`             | No    | Đăng nhập, trả JWT token |
| GET    | `/api/content`           | No    | Lấy toàn bộ nội dung     |
| GET    | `/api/content/:section`  | No    | Lấy 1 section            |
| PUT    | `/api/content/:section`  | Yes   | Cập nhật 1 section       |
| POST   | `/api/upload/:folder`    | Yes   | Upload ảnh               |
| DELETE | `/api/image`             | Yes   | Xóa ảnh                  |
| POST   | `/api/rsvp`              | No    | Gửi RSVP                 |
| GET    | `/api/rsvp`              | Yes   | Danh sách RSVP           |
| DELETE | `/api/rsvp/:id`          | Yes   | Xóa RSVP                 |
| GET    | `/api/rsvp/export`       | Yes   | Xuất CSV                 |
| POST   | `/api/change-password`   | Yes   | Đổi mật khẩu admin       |

## Backup định kỳ

Backup các file/folder sau:
```bash
# Nội dung website
data/content.json

# Danh sách khách RSVP
data/rsvp.json

# Ảnh đã upload
public/images/
```

## Deploy trên Railway/Render

### Railway
1. Push code lên GitHub
2. Tạo project mới trên Railway từ GitHub repo
3. Thêm Environment Variables: `DEFAULT_ADMIN_PASSWORD`, `JWT_SECRET`, `PORT`
4. Railway tự deploy

### Render
1. Push code lên GitHub
2. New Web Service → connect GitHub repo
3. Build Command: `npm install`
4. Start Command: `npm start`
5. Thêm Environment Variables

### VPS (Ubuntu/Debian)
```bash
# Cài Node.js
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs

# Clone và chạy
git clone <repo> wedding-site
cd wedding-site
npm install
cp .env.example .env
nano .env  # Điền mật khẩu và JWT_SECRET

# Chạy với PM2
npm install -g pm2
pm2 start server.js --name wedding
pm2 startup
pm2 save
```

## Lưu ý quan trọng

- `data/users.json` chứa hashed password — KHÔNG commit lên git
- `data/content.json` và `data/rsvp.json` — backup thường xuyên
- Ảnh trong `public/images/` — backup thường xuyên
- Đổi `JWT_SECRET` sẽ logout tất cả admin sessions
