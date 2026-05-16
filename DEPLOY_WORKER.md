# Hướng dẫn Deploy Cloudflare Worker

## Cách 1: Dùng Cloudflare Dashboard (Dễ nhất)

### Bước 1: Tạo tài khoản Cloudflare
1. Truy cập https://dash.cloudflare.com/sign-up
2. Đăng ký tài khoản miễn phí

### Bước 2: Tạo Worker
1. Vào Dashboard > **Workers & Pages**
2. Click **Create Application**
3. Chọn **Create Worker**
4. Đặt tên: `shopee-link-resolver`
5. Click **Deploy**

### Bước 3: Dán code
1. Sau khi deploy, click **Edit code**
2. Xóa code mẫu
3. Copy toàn bộ nội dung file `worker.js` và dán vào
4. Click **Save and deploy**

### Bước 4: Lấy URL
- URL Worker sẽ có dạng: `https://shopee-link-resolver.<your-subdomain>.workers.dev`
- Ví dụ: `https://shopee-link-resolver.abc123.workers.dev`

### Bước 5: Cập nhật Frontend
Mở file `index.html`, tìm dòng:
```javascript
const CONFIG = {
    affiliateId: "17352620178",
    subId: "product-fb"
};
```

Thêm dòng `workerUrl`:
```javascript
const CONFIG = {
    affiliateId: "17352620178",
    subId: "product-fb",
    workerUrl: "https://shopee-link-resolver.<your-subdomain>.workers.dev"
};
```

---

## Cách 2: Dùng Wrangler CLI (Cho dev)

### Cài đặt Wrangler
```bash
npm install -g wrangler
```

### Đăng nhập
```bash
wrangler login
```

### Deploy
```bash
cd /path/to/project
wrangler deploy
```

---

## Test Worker

Sau khi deploy, test bằng cách truy cập:
```
https://shopee-link-resolver.<your-subdomain>.workers.dev?url=https://vn.shp.ee/xKJarNWo
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

## Free Tier Limits

- 100,000 requests/ngày (miễn phí)
- Quá đủ cho sử dụng cá nhân
