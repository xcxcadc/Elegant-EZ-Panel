# 主题源码审计记录

审计日期：2026-10-02

## 本次检查和修复

- 站外 IP 定位等绝对地址请求不再经过中间件映射，也不再携带面板 `Authorization`，避免把登录凭据发送给第三方。
- 浏览器禁用或限制 `localStorage` 时，旧 v1 请求使用内存 IV 回退，不会在请求初始化阶段卡死。
- 已经是中间件 v2 地址的订阅链接不会被再次加密，避免订阅链接越套越长。
- Axios 取消请求会直接结束，不再进入读请求重试逻辑，减少页面切换时的重复请求。
- 清理未被源码使用的编辑器、客服和引导依赖；升级 Axios、DOMPurify、Markdown、ECharts，并锁定 nanoid 安全版本。

## V2Board 兼容检查

已检查登录、用户信息、订阅信息、订单支付状态、工单、公告、流量和订阅导入使用的 API 请求路径。加密请求继续使用中间件协议，站外辅助请求保留绝对地址。V2Board 返回的完整订阅 URL 在 v2 模式下会连同查询参数一起加密。

## 验证结果

```text
npm test -- --run  PASS（5 个测试）
npm run build      PASS
npm audit --omit=dev  0 vulnerabilities
```

构建仍可能提示 Vite 的 `.env` 中设置了 `NODE_ENV=production`，这是 Vite 的提示，不影响构建结果；生产模式由 `vite build` 自动确定。

## 发布前检查

部署前请在本地私有配置中填写真实 API 和中间件密钥，再重新构建。不要把 `src/config/index.js`、`.env`、真实后端地址或带 token 的订阅链接提交到公开仓库。

