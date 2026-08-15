# tech-spec — AdGenie 技术方案(说人话版)

> 版本:v1.0
> 日期:2026-08-15
> 输入:spec.md(v1.0 定稿)
> 形式:先给人确认的"说人话版",确认后再出 tasks.md + plan.md。

---

## 1. 技术栈定稿(照 MVP 规划)

| 层 | 选择 | 为什么 |
|----|------|--------|
| 前端 | Next.js(App Router)+ Tailwind CSS | 快、SEO 友好、Vercel 一键部署 |
| 后端 | Next.js API Routes + Supabase | 无服务器零运维,数据库/认证/存储一把梭 |
| 图像生成 | 现成图像 API(按量付费) | 不造模型,成本可控 |
| 文案生成 | 现成 LLM API | 同上 |
| 支付 | Stripe(订阅) | 收入证据 A 级(TrustMRR 可接) |
| 部署 | Vercel | 绑定域名即上线 |
| 认证 | Supabase Auth(邮箱 + Google) | 免费、快 |

> 原则:**能抄的抄,能买的别造。** MVP 每省一小时开发都是赢。

---

## 2. 系统架构(一张图)

```
[浏览器] 
   │
   ├─ Next.js App(页面:首页 / 生成器 / 定价)
   │        │
   │        ├─ /api/generate  ← 编排:图 + 产品名 → 调图像 API + LLM API → 返回素材
   │        ├─ /api/export    ← 加水印 / 限尺寸 → 返回可下载文件
   │        └─ Supabase      ← 用户 / 额度(3次) / 生成记录
   │
   ├─ Stripe  ← 订阅支付(Pro),webhook 回调 → 更新额度
   └─ Vercel  ← 托管全部
```

## 3. 数据模型(第一版)

**users**:id, email, plan(free/pro), free_credits(剩余次数), created_at
**generations**:id, user_id, product_name, image_url, result_url, platform, size, created_at
**payments**:id, user_id, stripe_session_id, status, amount, created_at

> 第一版表就 3 张,不搞复杂。

## 4. 关键接口契约

| API | 入参 | 出参 | 备注 |
|-----|------|------|------|
| POST /api/generate | {image, product_name} | {results: [{platform, size, image_url, copy}]} | ≤60s 超时 |
| POST /api/export | {generation_id, is_pro} | {download_url, watermark: bool} | 免费带水印 |
| POST /api/webhook/stripe | 标准事件 | 200 | 支付成功 → 更新额度 |

## 5. 安全与合规(内置)

- 上传文件校验类型/大小(≤10MB),存 Supabase Storage 私有桶,仅本人可读
- API Key 全部走服务端环境变量,绝不下发前端
- 导出物带 AI 生成声明(应对 Meta/TikTok 政策)
- 额度扣减在服务端执行,防刷

## 6. 说人话总结(给你确认)

> 就是一个 **Next.js 网站 + 两个 AI API + Supabase 存数据 + Stripe 收钱**,核心动线"上传图 → 生成 → 导出"。
> 不做:登录后没用到的东西一律不做;页面就 3 个(首页/生成器/定价)。
> 预计工作量集中在**生成编排**(调两个 AI API 拼出最终素材)和**额度/支付闭环**。

✅ 请确认此技术方案,确认后出 tasks.md + plan.md。
