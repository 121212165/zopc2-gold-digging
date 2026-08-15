# AdGenie — Spec

> AI 电商广告素材生成器:一张商品图,十分钟,一套多平台广告素材
> 生成方式:按 sdd-suite(new-project skill)Phase 5 模板产出,G1-G5 各门已确认

## 1. Overview

**项目背景**

阶段1(掘金实战 Day 1-3)用数据验证出方向:AI 电商广告素材。标杆 Promer AI 增长率 +1712%、自然/付费双轮、AI 搜索可见(29 提及);需求侧 Reddit 5 条真实痛点声音 + Semrush 163 个自然关键词验证。

**Struggling Moment(具体场景和步骤)**

独立电商卖家在准备一次新品广告投放时,到了「做素材」这一步卡住:
- 场景:上新品 → 当天就要 FB/IG/TikTok 三套尺寸的广告创意图 + 文案
- 步骤:打开 Canva/PS → 找模板 → 改尺寸 → 写文案 → 导出 → 发现平台尺寸不对 → 重做
- 痛点:手动做一套素材要 3 小时以上;卖家不会设计,做出来的效果自己都不满意
- 现状变通:用 Canva 模板(不贴合广告尺寸)、外包(贵且慢)、或干脆拿产品图硬发(效果差)

**差异化切入点**

现有工具(ChatCut 类)纯投放驱动无自然护城河;通用 AI 生图(如 DALL-E 套壳)不垂直、产出「不电商」;Promer 只做英文。切入:**垂直电商 + 多平台尺寸一键闭环 + 先打小语种市场**(西语/葡语/中文),大厂没盯上的缝隙。

**Why Now**

1. 图像生成 + 文案 LLM 的 API 成本已降到个人可承担(按量付费)
2. AI Overviews 在 SERP 占比快速上升(Clumi 已吃到 20.9%),垂直工具站吃 AI 搜索红利的窗口正在打开
3. 标杆 Promer 已验证商业模式(3,000+ 商家),但多语言/批量等瑕疵明显——瑕疵即机会

## 2. Product Direction

**体验质感**

「快、专业、不用学」。用户打开就知道怎么用:传图 → 点生成 → 拿素材。产品给人的感觉是一个懂电商广告的资深设计师,而不是一个通用 AI 画图工具。

**关键时刻(aha moment)**

第一次生成:上传商品图、输入产品名,60 秒内看到 3 个平台尺寸的成品创意图 + 英文文案,并意识到「这比我一下午做的还好」。

**商业意图**

(B) → (C):先免费获取用户,验证付费转化率 ≥10% 后转收费订阅(Stripe)。收入证据走 A 级(TrustMRR 可接)。

## 3. Scope

### V1(只做这些)

| 功能 | 说明 | 为什么必须有 |
|------|------|-------------|
| F1 商品图上传 / 商品链接导入 | 上传 ≤10MB 图片,或粘贴链接自动抓主图 | 旅程第一步 |
| F2 AI 生成创意图 | 输入图+产品名,生成 FB/IG/TikTok 各 1 张电商风广告图 | **关键时刻** |
| F3 文案生成 | 每条配 1 条英文广告文案(卖点+CTA) | 与图同时交付,闭环 |
| F4 多尺寸导出 | 免费:1280px 带水印;付费:2048px 无水印+全部尺寸 | 变现钩子 |
| F5 免费额度 3 次 | 注册送 3 次,用尽弹付费引导,不阻断已生成内容下载 | 获客+转化 |
| F6 AI 生成声明 | 导出物附 "AI-generated" 合规标注 | Meta/TikTok 政策合规 |

### Not Doing(明确不做)

- 多语言素材(英文验证模式后再加)
- 批量上传/批量导出(上线后按需)
- 团队协作 / 工作流管理
- 自建模型(用现成图像 + 文案 API)
- 社媒一键发布(只导出,不代发)

### Future

- 批量处理(有量级用户需求)
- 多语言(西语/葡语/中文)——差异化蓝海
- 生成历史记录页(留存)
- 社媒一键发布

## 4. User Journey

1. 打开 → 落地页(一句话卖点 + 免费试用 CTA)
2. 上传商品图 / 粘贴商品链接 → 右侧画布显示原图
3. 输入产品名 → 点击生成 ← **关键时刻(60s 出成品)**
4. 平台切换 FB/IG/TikTok + 尺寸查看 + 文案确认
5. 导出下载(免费带水印 / Pro 无水印高清)
6. 额度用尽 → 付费引导(不阻断已生成内容)

## 5. Tech Stack

| 技术 | 用途 | 理由 |
|------|------|------|
| Next.js(App Router)+ Tailwind | 前端 + 后端 | 快、SEO 友好、Vercel 一键部署 |
| Supabase(Auth + Postgres + Storage) | 认证 / 3 张表 / 图片私有存储 | 免费、零运维 |
| 图像生成 API(按量付费) | 生成创意图 | 不造模型,成本可控 |
| LLM API | 生成广告文案 | 同上 |
| Stripe | 订阅支付 | 收入证据 A 级(TrustMRR 可接) |
| Vercel | 部署 | 绑定域名即上线 |

## 6. Architecture

**系统结构**

```
[浏览器]
   ├─ Next.js App(首页 / 生成器 / 定价)
   │     ├─ /api/generate  ← 编排:图+产品名 → 图像 API + LLM API → 素材
   │     ├─ /api/export    ← 水印/尺寸限制 → 下载
   │     └─ Supabase      ← users / generations / payments + 私有存储桶
   ├─ Stripe ← 订阅 + webhook 更新额度
   └─ Vercel ← 托管
```

**数据流**

上传/导入 → 生成请求(服务端调双 API,≤60s)→ 结果存 generations → 导出(按 plan 加水印/限尺寸)→ Stripe webhook → 更新 credits

**数据模型**

- `users`: id, email, plan(free/pro), free_credits, created_at
- `generations`: id, user_id, product_name, image_url, result_url, platform, size, created_at
- `payments`: id, user_id, stripe_session_id, status, amount, created_at

**体验保障**

- 生成中显示进度(第一步构图 / 第二步写文案),不白屏(AC-2.3)
- 额度用尽弹窗带「仍可下载已生成内容」安抚文案(AC-4.1)
- 全部服务端扣减额度,防刷

## 7. Directory Structure

```
adgenie/
├── app/
│   ├── page.tsx              # 首页落地页
│   ├── generator/page.tsx    # 生成器(主工作流)
│   ├── pricing/page.tsx      # 定价页
│   └── api/
│       ├── generate/route.ts # 生成编排
│       ├── export/route.ts   # 导出(水印/尺寸)
│       └── webhook/stripe/route.ts
├── lib/
│   ├── supabase.ts           # 客户端
│   ├── server/supabase.ts    # 服务端(RLS 安全)
│   ├── image-api.ts          # 图像 API 封装
│   ├── llm.ts                # 文案 LLM 封装
│   └── watermark.ts          # 水印逻辑
├── components/               # 上传区/画布/定价卡等
├── supabase/migrations/      # 建表 SQL
├── .env.local                # API Key(服务端)
├── SPEC.md
└── CLAUDE.md
```

## 8. Design Direction(前端)

**调性**:干净、可信、电商感——用户第一眼就知道是给卖家用的工具,不是通用画图玩具。

- **字体**:Inter(Display/Body);Mono 用于尺寸标注(1200×628 等)
- **配色**:主色紫 #6C5CE7(CTA/生成按钮)· 强调绿 #00B894(免费额度/成功)· 背景白 #FFFFFF / 浅灰 #F7F8FA · 文字 #1A1A2E / #6B7280
- **布局**:卡片式;生成器左右分栏(左输入,右画布),商品图为视觉主角
- **记忆点**:生成结果画布直接呈现"平台尺寸标签"(FB 1200×628 / IG 1080×1080 / TK 1080×1920)——一屏看懂价值
- **如何呼应体验质感**:「快」= 生成进度可见、「专业」= 尺寸/平台信息醒目、「不用学」= 三步流程页内可见

## 9. Implementation Plan(粒度 = commit)

**Phase 1:地基**

1. `init next.js app + tailwind + base layout`
2. `supabase: tables + RLS + auth (email + google)`
3. `landing page (hero + 3-step + pricing entry)`

**Phase 2:核心动线**

4. `upload: drag/click upload + link import with og-image`
5. `api/generate: orchestrate image api + llm (60s timeout + retry)`
6. `canvas: platform tabs + size display + copy`
7. `api/export: watermark free / hd pro + download`

**Phase 3:变现闭环**

8. `credits: register 3 free + server-side decrement + UI badge`
9. `stripe: subscription + webhook → credits update`
10. `paywall: exhausted modal (keep downloads open)`
11. `e2e: AC-1~4 all green + seed test`

## 10. Open Questions

1. 图像 API 具体选型与单次成本(需实测三家报价,预算 $0.05/次以内)
2. 免费额度 3 次是否够「关键时刻」完整体验(内测验证后可能调)
3. Pro 定价 $9/月 是否对标 Promer(上线前查竞品定价最终确认)
4. 域名 `adgenie.xxx` 可用性待确认(影响品牌名微调)

---

*SPEC v2.0(按 new-project skill 模板校准)。G1-G5 基于阶段1/2 材料确认,G6 审阅待用户批准。*
