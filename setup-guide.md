
# 🚀 Hướng Dẫn Cài Đặt Backstage (Node.js 18 – Local Dev)

> Phiên bản khuyến cáo: **Node.js 18.x**  
> Tránh dùng Node.js 20+ vì dễ gây lỗi khi build plugin.

---

## ✅ Yêu cầu hệ thống

| Thành phần     | Phiên bản     |
|----------------|---------------|
| Node.js        | **18.x**      |
| Yarn           | >= 1.22.x     |
| Git            | >= 2.0        |
| Python         | >= 3.6 (chỉ Windows) |

set NODE_OPTIONS=--no-node-snapshot nếu node>=20
---

## 🔧 Bước 1: Cài Node.js 18

- Tải tại: https://nodejs.org/en/download/releases
- Cài bản LTS 18.x

```bash
node -v  # Kết quả nên là v18.x.x
```

---

## 🔧 Bước 2: Cài Yarn

```bash
npm install -g yarn
yarn -v  # Nên >= 1.22

yarn set version 4.4.1 #Backstage gợi ý version này cho yarn
yarn add dotenv        #Plugin giúp yarn đọc được file .env trong backend
```

---

## 🧱 Bước 3: Tạo ứng dụng Backstage

```bash
npx @backstage/create-app@latest
```

- Nhập tên project, ví dụ: `my-portal`

---

## 🚀 Bước 4: Chạy ứng dụng

```bash
cd my-portal
yarn dev      #Node 18
yarn start    #Node 20
```

- Truy cập: [http://localhost:3000](http://localhost:3000)

---

## 📂 Cấu trúc thư mục chuẩn

```
my-portal/
├── app-config.yaml               # ✅ Cấu hình chính của Backstage
├── app-config.local.yaml         # (tùy chọn) override local
├── app/                          # React frontend app
├── backend/                      # Express backend
│   └── src/index.ts              # Điểm khởi động backend
├── packages/                     # Các module nội bộ re-export
│   ├── app/
│   ├── backend/
├── plugins/                      # Các plugin tự viết thêm
│   └── example/
├── yarn.lock
└── package.json
```

---

## 🔧 Các lệnh quan trọng

| Lệnh                     | Mục đích                         |
|--------------------------|----------------------------------|
| `yarn dev`               | Chạy cả frontend + backend       |
| `yarn start-backend`     | Chạy chỉ backend                 |
| `yarn tsc`               | Kiểm tra TypeScript              |

---

## ⚠️ Lưu ý

- ✅ Chỉ dùng **Node.js 18**
- ❌ Không dùng npm để cài package — luôn dùng `yarn`
- 🧪 Tránh dùng Windows nếu chưa quen, Linux/macOS/WSL ổn định hơn
