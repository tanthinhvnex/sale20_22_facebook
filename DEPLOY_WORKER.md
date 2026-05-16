# Hướng dẫn Deploy Cloudflare Worker qua GitHub

## Bước 1: Tạo tài khoản Cloudflare

1. Truy cập https://dash.cloudflare.com/sign-up
2. Đăng ký tài khoản miễn phí

---

## Bước 2: Lấy Account ID

1. Đăng nhập Cloudflare Dashboard
2. Vào **Workers & Pages** (menu bên trái)
3. Ở góc phải, bạn sẽ thấy **Account ID** - copy giá trị này

---

## Bước 3: Tạo API Token

1. Vào **My Profile** (góc trên phải) → **API Tokens**
2. Click **Create Token**
3. Chọn template **Edit Cloudflare Workers**
4. Click **Continue to summary** → **Create Token**
5. **Copy token** ngay lập tức (chỉ hiển thị 1 lần!)

---

## Bước 4: Thêm Secrets vào GitHub

1. Vào repo GitHub: https://github.com/tanthinhvnex/shopee-affiliate
2. Vào **Settings** → **Secrets and variables** → **Actions**
3. Click **New repository secret** và thêm 2 secrets:

| Name | Value |
|------|-------|
| `CLOUDFLARE_ACCOUNT_ID` | Account ID từ bước 2 |
| `CLOUDFLARE_API_TOKEN` | API Token từ bước 3 |

---

## Bước 5: Trigger Deploy

### Cách 1: Push code
```bash
git add .
git commit -m "Deploy worker"
git push
```

### Cách 2: Chạy thủ công
1. Vào repo GitHub → **Actions**
2. Chọn workflow **Deploy Cloudflare Worker**
3. Click **Run workflow**

---

## Bước 6: Lấy URL Worker

Sau khi deploy thành công:

1. Vào Cloudflare Dashboard → **Workers & Pages**
2. Click vào worker **shopee-link-resolver**
3. URL sẽ có dạng: `https://shopee-link-resolver.<account>.workers.dev`

---

## Bước 7: Cập nhật Frontend

Mở file `index.html`, sửa CONFIG:

```javascript
const CONFIG = {
    affiliateId: "17352620178",
    subId: "product-fb",
    workerUrl: "https://shopee-link-resolver.<account>.workers.dev"
};
```

Commit và push:
```bash
git add index.html
git commit -m "Add Worker URL"
git push
```

---

## Test Worker

Truy cập URL sau để test:
```
https://shopee-link-resolver.<account>.workers.dev?url=https://vn.shp.ee/xKJarNWo
```

Kết quả mong đợi:
```json
{
  "success": true,
  "canonicalUrl": "https://shopee.vn/product/118938080/23087700521",
  "shopId": "118938080",
  "itemId": "23087700521"
}
```

---

## Troubleshooting

### Lỗi "Authentication error"
- Kiểm tra lại API Token đã đúng chưa
- Đảm bảo token có quyền "Edit Cloudflare Workers"

### Lỗi "Account not found"
- Kiểm tra Account ID đã đúng chưa
- Account ID là chuỗi 32 ký tự hex

### Worker không hoạt động
- Kiểm tra GitHub Actions có chạy thành công không
- Xem logs trong tab Actions của repo
