# Chonglangban 主题与加密中间件使用方法

## 安装主题

```bash
npm install
npm run dev
```

生产构建：

```bash
npm run build
```

将 `dist/` 的内容部署到静态 Web 服务器根目录。Vite 已配置相对资源路径，适合放在域名根目录或普通静态目录。

## 配置面板连接

站点名称、后端地址、V2Board 类型、菜单、公告和中间件参数集中在 `src/config/index.js`。这个文件包含部署私有配置，真实后端域名、密钥和图床 Key 不要提交到公开仓库。

推荐使用 v2 AES-256-GCM：

```js
API_MIDDLEWARE_ENABLED: true,
API_MIDDLEWARE_URL: 'https://middleware.example.com',
API_MIDDLEWARE_PATH: '/clb/clb',
API_MIDDLEWARE_PROTOCOL: 'aead',
API_MIDDLEWARE_AEAD_KEY: '与中间件 AEAD_KEY 相同的64位十六进制字符串',
```

中间件端对应：

```dotenv
ENCRYPTION_PROTOCOL=aead
AEAD_KEY=同一份64位十六进制字符串
PATH_PREFIX=/clb/clb
API_PREFIX=/api/v1
ALLOWED_ORIGINS=https://你的主题域名
ALLOW_PLAIN_SUBSCRIPTIONS=false
```

旧 EZ 兼容模式使用 `API_MIDDLEWARE_PROTOCOL: 'legacy'`，并让 `API_MIDDLEWARE_KEY` 与服务端 `AES_KEY` 完全一致。迁移期间服务端可以使用 `ENCRYPTION_PROTOCOL=auto`。

## 订阅、订单和支付

主题会根据面板返回的数据展示套餐、流量、到期时间和订阅入口。v2 模式会把订阅 URL 的路径和查询参数整体加密，避免把 `token` 直接放在公开订阅地址中；已经是 v2 中间件地址的链接不会重复加密。

支付通知由支付平台直接访问中间件白名单地址。中间件文档中的 `PAYMENT_NOTIFY_PATHS` 必须与实际回调路径匹配，前端的支付状态检测仍然通过普通加密 API 完成。

## 常用检查

```bash
npm test -- --run
npm run build
```

浏览器检查时确认：登录后 API 请求的 Host 是中间件地址；v2 请求路径以 `v2.` 开头；站外 IP 定位请求不会携带面板 Authorization；取消的请求不会再次发出。

## 修改与发布

1. 复制并保留本地 `src/config/index.js`，只在本地填写真实配置。
2. 修改公开代码或文档后运行测试和构建。
3. 将 `dist/` 内容部署到静态服务器。
4. 不要提交 `.env`、真实密钥、后端地址或带 token 的订阅链接。

