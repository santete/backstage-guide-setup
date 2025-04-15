# ✨ Hướng Dẫn Cấu Hình Backstage Toàn Diện

> Tài liệu này hướng dẫn chi tiết việc cài đặt và cấu hình Backstage bao gồm PostgreSQL, GitLab, TechDocs và Catalog cho User/Group.

---

## 1. 🔠 Cấu Hình Kết Nối PostgreSQL

### ⚙️ Thêm vào `app-config.yaml`
```yaml
backend:
  database:
    client: pg
    connection:
      host: localhost
      port: 5432
      user: backstage
      password: your_password
      database: backstage_db
```

### 🌐 Sư dụng Docker Compose cho PostgreSQL (tùy chọn)
```yaml
db:
  image: postgres:13
  environment:
    POSTGRES_USER: backstage
    POSTGRES_PASSWORD: your_password
    POSTGRES_DB: backstage_db
  ports:
    - "5432:5432"
```

> 🔍 ✨ **Hình minh họạ:** PostgreSQL container hoạt động song song với Backstage

---

## 2. 💰 Tích Hợp GitLab Source Control

### ✏️ Sửa `app-config.yaml`
```yaml
integrations:
  gitlab:
    - host: gitlab.com
      token: ${GITLAB_TOKEN}  # Token truy cập repo
```

### 🚫 Đặt Token:
```bash
# .env hoặc CI/CD
GITLAB_TOKEN=your_token
```

> 🔐 Lấy token tại: `https://gitlab.com/-/profile/personal_access_tokens`

---

## 3. 📚 Cấu Hình Tài Liệu TechDocs

### 📄 Cài package:
```bash
yarn add --cwd packages/techdocs-container @techdocs/cli
yarn add --cwd packages/backend @backstage/plugin-techdocs-backend
```

### 🔢 Sửa `app-config.yaml`
```yaml
techdocs:
  builder: 'local'
  generator:
    runIn: 'local'
  publisher:
    type: 'local'
```

### ✍️ backend/src/plugins/techdocs.ts:
```ts
import { createRouter } from '@backstage/plugin-techdocs-backend';
export default async function createPlugin({ logger, config, discovery }) {
  return await createRouter({ logger, config, discovery });
}
```

> 📸 **Hình minh họạ:** Quy trình sinh TechDocs local

---

## 4. 👥 Quản Lý User & Group trong Catalog

### 📁 `catalog-info.yaml` ví dụ:
```yaml
apiVersion: backstage.io/v1alpha1
kind: Group
metadata:
  name: engineering
spec:
  type: team
  profile:
    displayName: Engineering Team
  children: []
---
apiVersion: backstage.io/v1alpha1
kind: User
metadata:
  name: johndoe
spec:
  profile:
    displayName: John Doe
    email: johndoe@example.com
  memberOf: [engineering]
```

### 📑 Cấu hình `catalog.locations`
```yaml
catalog:
  locations:
    - type: url
      target: https://gitlab.com/my-org/my-repo/blob/main/catalog-info.yaml
```

---

## 5. 🔢 Tích hợp GitLab Discovery (quét toàn bộ repo)

### 📅 `backend/src/plugins/catalog.ts`
```ts
import { GitLabDiscoveryEntityProvider } from '@backstage/plugin-catalog-backend-module-gitlab';
builder.addEntityProvider(
  GitLabDiscoveryEntityProvider.fromConfig(env.config, {
    logger: env.logger,
    scheduler: env.scheduler,
  })
);
```

> 🔹 Giúc Backstage phát hiện repo tự động dựa vào nhóm GitLab

---

## ✨ Kết Luận

| Tính năng      | Trạng thái     |
|------------------|----------------|
| Database (PostgreSQL) | ✅ Đã thiết lập |
| GitLab Integration    | ✅ Hoàn tất         |
| TechDocs              | ✅ Sinh local      |
| Catalog User/Group    | ✅ Có mô tả rõ     |

> ☕ Tham khảo [Backstage Documentation](https://backstage.io/docs) để cấu hình nâng cao hệ thống!

