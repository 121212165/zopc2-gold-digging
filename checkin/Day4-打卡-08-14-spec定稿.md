# 📅 打卡 Day 4 | 08.14 周五 | 想清楚,文件化

> 项目:ZOPC21Launch 掘金实战(第2期) · 阶段2 · 开始落地
> 目标:spec 定稿 + 设计稿第一版
> 说明:sdd-suite = ZOPC 精品 Skill 仓库的 **new-project skill**(高质量Spec文档生成Skill,用户上传 SKILL.md 确认)。已按其 Phase 5 模板校准产出 `SPEC.md`。

## ✅ 今日产出

### 1. SPEC.md 已定稿(按 sdd-suite = new-project skill 模板)

> 产物:`SPEC.md`(完整文件见同目录)。按 new-project skill 的 Phase 5 输出格式生成(Overview → Product Direction → Scope → User Journey → Tech Stack → Architecture → Directory Structure → Design Direction → Implementation Plan → Open Questions),并过了一遍 skill 的审阅检查项(无 TODO/TBD、struggling moment 具体、体验-架构-视觉一致、关键时刻保留、Implementation Plan 可直接变 commit)。

**核心内容摘要**:

| 项 | 内容 |
|----|------|
| 产品定义 | **AdGenie** — AI 电商广告素材生成器:上传商品图/粘贴商品链接 → 生成多平台( FB/IG/TikTok)广告创意图 + 文案 → 一键导出 |
| 给谁 | 独立电商卖家 / 小团队投放手(不会设计、手动做素材极耗时) |
| Struggling Moment | 卖家上新品当天就要三平台尺寸素材,手动做 3 小时以上,Canva 模板不贴合、外包贵慢 |
| 关键时刻 | 60 秒内看到 3 平台成品创意图 + 英文文案 |
| V1 | F1-F6(上传/生成/文案/导出/额度/AI声明),Not Doing 与 Future 已划界 |
| Implementation Plan | Phase 1-3 共 11 个 commit,粒度可直接执行 |

**验收标准(AC)摘要**:
- **AC-1 上传**:用户能上传商品图(≤10MB,JPG/PNG/WebP)或粘贴商品链接;链接可自动抓取主图 ✓
- **AC-2 生成**:输入 1 张图 + 产品名,≤60 秒返回 3 个平台各 1 张创意图 + 3 条文案(英文) ✓
- **AC-3 导出**:免费版可导出 1280px 带水印版,付费解锁 2048px 无水印 + 全部尺寸 ✓
- **AC-4 额度**:未登录用户被引导注册;注册用户 3 次免费额度,用尽弹付费引导,且不阻断已生成内容的下载 ✓

### 2. 设计稿第一版

> 产物:`design-v1.md`(完整文件见同目录),已把 spec 的功能清单落成页面结构与视觉规范。

**页面清单**:

| 页面 | 核心模块 | 状态 |
|------|---------|------|
| 首页(落地页) | Hero(一句话卖点) + 三步用法 + 定价 + 排队/注册 CTA | ✅ 线框完成 |
| 生成器(主工作流) | 左:上传/链接输入 · 右:生成结果画布 + 尺寸切换 + 导出按钮 | ✅ 线框完成 |
| 定价页 | 免费 3 次 / Pro 订阅两档 | ✅ 线框完成 |

**视觉规范(第一版)**:浅色主题、电商场景感(商品图为主视觉)、主色 #6C5CE7(紫)+ 强调绿 #00B894;字体 Inter;圆角 12px;卡片式布局。

> ⚠️ 待办:设计稿第二版需把线框喂给 Open Design / Awesome Design md 等工具出高保真图(需要时再执行)。

---

## 附:执行记录

- [x] 找到 sdd-suite:ZOPC 精品 Skill 仓库的 **new-project skill**(用户提供 SKILL.md 确认,非 npm 包)
- [x] 通读 SKILL.md,掌握其 6 道确认门(G1-G6)与 Phase 5 SPEC.md 输出模板
- [x] 按 skill 模板校准产出 `SPEC.md`(v2.0,含 Implementation Plan / Open Questions)
- [x] 设计稿第一版(页面结构 + 视觉规范,对应 skill 的 Design Direction 环节)
- [ ] 待办:如有 frontend-design skill 可补高保真视觉方向(G5 环节)
