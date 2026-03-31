# 網站上線與綁定 Domain 教學

你已經有 domain，接下來要做的是兩件事：

1. 把網站放到一個可公開存取的主機
2. 把 domain 指向那個主機

---

## 我推薦的最簡單方式：Cloudflare Pages

這種純 HTML/CSS/JS 網站，非常適合用 Cloudflare Pages。

### 優點
- 免費
- 速度快
- 有 HTTPS
- 綁自訂 domain 很簡單
- 不需要自己架 Linux 主機

---

## 方式一：Cloudflare Pages 上線

### 第 1 步：準備網站檔案

把整個 `taisheng-farm-site` 資料夾保留好。

裡面至少要有：
- `index.html`
- `styles.css`
- `script.js`

---

### 第 2 步：建立 GitHub Repository

如果你有 GitHub：

1. 登入 GitHub
2. 新增一個 repository，例如：`taisheng-farm-site`
3. 把網站檔案上傳上去

如果你不會用 GitHub，我也可以下一步直接教你 git 指令。

---

### 第 3 步：登入 Cloudflare Pages

1. 到 <https://dash.cloudflare.com>
2. 建立帳號 / 登入
3. 點選 **Workers & Pages**
4. 選 **Create application**
5. 選 **Pages**
6. 連接你的 GitHub repository

---

### 第 4 步：部署設定

因為這是靜態網站：

- Framework preset：`None`
- Build command：留空
- Build output directory：`/`

然後按 Deploy。

部署完成後，Cloudflare 會給你一個預設網址，例如：

- `taisheng-farm-site.pages.dev`

---

### 第 5 步：綁定你買的 Domain

在 Cloudflare Pages 專案中：

1. 進入你的 Pages 專案
2. 點 **Custom domains**
3. 點 **Set up a custom domain**
4. 輸入你的 domain，例如：
   - `www.yourdomain.com`
   - 或 `yourdomain.com`
5. 按確認

如果你的 domain DNS 也在 Cloudflare，通常它會自動幫你設好。

如果你的 domain 不在 Cloudflare，你需要到 domain 註冊商那邊設定 DNS。

---

## DNS 怎麼設定？

常見會用到：

### 若要指向 `www`
新增一筆：

- 類型：`CNAME`
- 名稱：`www`
- 內容：Cloudflare 指定給你的 Pages 網址

例如：
- `www` → `taisheng-farm-site.pages.dev`

### 若要主網域也能開
例如 `yourdomain.com`

Cloudflare 會提示你如何設定，通常會是：
- CNAME flattening
- 或 A / CNAME 設定

這部分要看你的註冊商介面。

---

## 方式二：自己主機 + Nginx

如果你有自己的 VPS 或主機，也能自己架。

### 流程
1. 安裝 Nginx
2. 把 `taisheng-farm-site` 檔案放到 `/var/www/你的網站/`
3. 設定 nginx server block
4. 把 domain A record 指向主機 IP
5. 用 Certbot 安裝 HTTPS

### 簡易 nginx 範例

```nginx
server {
    listen 80;
    server_name yourdomain.com www.yourdomain.com;

    root /var/www/taisheng-farm-site;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

然後重新載入 nginx：

```bash
sudo nginx -t
sudo systemctl reload nginx
```

---

## Domain 設定觀念

你買 domain 只是「買網址」。
還需要：

- **DNS**：告訴網址要連去哪裡
- **Hosting**：真正放網站檔案的地方

所以流程就是：

`Domain` → `DNS 指向主機` → `主機提供網站內容`

---

## 你現在最適合的做法

如果你想要：
- 簡單
- 穩定
- 不想自己維護伺服器

我建議你直接用：

**Cloudflare Pages + 你的 domain**

這是目前最省事的組合。

---

## 我接下來還能幫你什麼

如果你願意，我下一步可以直接幫你：

1. 把這個網站再做得更像你貼的參考站
2. 幫你補上：
   - 真實電話
   - 真實地址
   - LINE 按鈕
   - Google 地圖
   - Facebook 粉專
   - 產品圖片區
   - SEO 標題與關鍵字
3. 如果你告訴我 domain 是哪一家買的（例如 GoDaddy / Namecheap / 中華電信 / Gandi / Cloudflare），我可以直接一步一步照你的註冊商介面教你設定 DNS

如果你要，我也可以下一則直接教你：

**「怎麼把這個資料夾上傳到 GitHub，再接到 Cloudflare Pages」**
