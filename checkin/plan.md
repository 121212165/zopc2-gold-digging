# plan.md — AdGenie 开发计划(worktree 并行)

> 版本:v1.0
> 日期:2026-08-15
> 目标:按 plan 自动开 worktree 并行开工,三天窗口内核心动线跑通。

---

## 阶段划分

### 阶段 A · 地基(独立可并行)
- **worktree:feat/scaffold**(T01 + T11)
  - 建项目、Tailwind、布局、首页落地页
- **worktree:feat/db**(T02 + T03)
  - Supabase 建表 + RLS + Auth 接入

> A 的两个 worktree 互不依赖,可同时开工。

### 阶段 B · 核心动线(A 完成后并行)
- **worktree:feat/generate**(T04 → T05 → T06)
  - 上传导入 → 生成编排 → 结果画布(串行,因为 T05 依赖 T04)
- **worktree:feat/pay**(T07 → T08 → T09 → T10)
  - 导出 → 额度 → Stripe → 用尽引导(串行)

> B 的两个 worktree 依赖同一个 A 的地基,但彼此独立,可并行。

### 阶段 C · 联调与验收
- 合并 B → main,跑 AC-1~4 全量验收
- 修 bug → 出内测版(10 个种子用户)

---

## 执行方式

1. `git worktree add` 按上述分支各自独立开发,互不干扰
2. 每个 worktree 完成即 PR,review 后合入 main
3. 冲突面很小:仅 T01 的基础布局与各分支的页面文件边界清晰
4. 每日一次 main 合流 + CHANGELOG 更新

## 检查点

| 时间 | 检查 |
|------|------|
| D1 结束 | 阶段 A 完成:项目可跑、库表就绪、可登录 |
| D2 结束 | 阶段 B 完成:上传→生成→导出闭环通(免费路径) |
| D3 结束 | 支付闭环通 + AC-1~4 全绿 + 内测版可发布 |

## 风险与兜底

- 图像 API 超时 → 生成编排加 60s 超时 + 重试 1 次 + 明确报错(AC-2.3)
- 三天做不完 → 砍导出尺寸切换(先单尺寸),其余标 later(见 tasks L 表)
- Stripe 沙箱测试 → 先接 test mode 验证 webhook,上线前切 live
