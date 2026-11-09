# Agent Note: 2026-11-09 皮肤收录轮

Status: implemented

## Problem

2026-11-09 的皮肤收录轮按 pr-maintenance 的默认范围处理 dsh-skins 中「球在维护者一侧」的两个 PR：#77（新增轻色专用的水彩皮肤 `whale-watercolour`）与 #105（`remiel-starlit` 亮暗对比度修复）。两者上一轮均以 CHANGES_REQUESTED 退回，本轮分别有 2026-11-09 与 2026-11-08 的新提交，需要逐项核对退回项是否落实、复核三项内容闸门（用户可见证据、第三方素材版权、美学鉴赏），并裁定是否合入 `main` 与上架。

## Decision

- **#105 合入。** 三项退回项全部落实：`skin.json` 从 0.1.0 升到 0.1.1（`main` 上确认仍为 0.1.0，商店会当作更新）；`NOTICE` 新增「Z·Z·Z / zenless zone zero 官方 logo 水印属于米哈游、素材不裁剪不遮盖」的中英说明；颠倒构图由作者说明为官方原画本身的倾斜构图（`assets/remiel-nocturne.jpg` 是 3840×2160 等比缩小、无旋转步骤），本轮未改任何素材，按原样接受。CSS 改动与描述一致：深色块不再镜像 `--dsw-static-neutral-*`（`neutral-850` 回落到 `#3d3144`），`neutral-00` 保持 `#ffffff` 以维持外壳白叠加语义，亮态半透明面 alpha 上调（`bg-base` 0.42→0.62、`sidebar-fill` 0.60→0.74），`label-tertiary` / `caption` / `dimmed` 加深，并补声明 `--dsw-alias-link` 与 `--dsw-alias-toast-label`。亮暗两张截图里卡片正文、次级文字与链接均可读。
- **#77 合入。** 三项退回项落实：会话态截图不再有桌宠浮窗；两侧画面列（左 x40-260、右 x1060-1280）在会话态下与首页态一致地保留发丝、花瓣与云边细节，纱边界可见；`preview/light.jpg`、`preview/dark.jpg`、`evidence/whale-watercolour-home.jpg` 三份字节相同（sha256 `f586c5c2…edf705`），`README.md` 与 `README.zh.md` 记录的 sha 与文件一致。来源闸门齐：底图由贡献者在本机 ComfyUI 用开源图像模型生成、非转载，角色为贡献者自有的「鲸鱼娘」角色线，`LICENSE` 为 CC BY-NC-SA 4.0，`skin.json` 的 `license` / `licenseUrl` / `attribution` 与 README 归属节一致，合规责任由贡献者承担。美学鉴赏通过：构图完整、无裁切畸变与多余肢体，配色与外壳融合，1280×800 原样截图无锯齿、无水印、无明显 AI 伪影；亮暗同值是轻色专用皮肤的刻意取舍，README 已写明。
- **视觉闸门本轮由主会话执行。** `magpie-anthropic` 路由（Sonnet 5.5 与 Opus 5.5）两次正式派发加两次探针均静默失败，属该 provider 的基础设施问题；为不阻塞收录，改由多模态主会话按同一套闸门判据完成，逐图观感、区域级定位与 sha256 比对记入本轮评审评论。

## Alternatives considered

- **要求 #105 补「修复前」截图再合入。** 否决：本仓模板的证据节只要求亮/暗双态截图且与 `evidence/` 字节一致，修复前后的量值已由作者写在描述与 README 变更节（1.28:1 → 11.07:1）；再要一组修复前截图不改变任何准入判断，只会多拖一轮。
- **以 #77 首页标题与输入卡压脸为由继续退回。** 否决：上一轮已判定该项可不改，本轮作者把局部纱收窄并在正文写明取舍，属于美术决定而非缺陷；以构图偏好阻塞会侵占作者的美术裁量。
- **因视觉子 agent 路由不可用而暂停本轮皮肤收录。** 否决：路由故障不是内容缺陷，主会话本身具备多模态判据能力；改为主会话执行并把证据写进评论，既不降低闸门标准，也不把基础设施问题转嫁给贡献者。

## Consequences

- 两个 PR 已 squash 合入 `main`（#105 于 15:53:43Z、#77 于 15:53:52Z），`skins/whale-watercolour` 与 `skins/remiel-starlit@0.1.1` 随本仓内容进入市场构建；上架还需在 dsh-web 移动 `satellites/dsh-skins` 钉版并重建 `market/dist`。
- 「既有皮肤修复必须递增 `skin.json` 版本」已连续三轮被退回，本轮 #105 已按此执行；下一轮可在审查模板里前置该条以减少往返。
- 轻色专用皮肤以亮暗同值满足双态证据要求，本轮确立判例：只要 README 写明取舍、两份 preview 为有意同值，不因缺暗态专图阻塞。
