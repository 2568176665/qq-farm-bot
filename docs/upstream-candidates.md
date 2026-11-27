# 候选上游仓库与合并记录

原上游 [liyangpengs/qq-farm-bot](https://github.com/liyangpengs/qq-farm-bot) 已于 **2026-11-25** 停更（最后提交 `5049f69`）。当前仓库（`2568176665/qq-farm-bot`）已关闭定时自动合并，改由本地按需审查并手动合入来自活跃维护分支的更新。

## 已记录的仓库与 Git Remote

| Remote 名称 | 仓库地址 | 默认分支 | 定位与特点 |
| --- | --- | --- | --- |
| `origin` | `https://github.com/2568176665/qq-farm-bot.git` | `master` | 本仓库推送目标 |
| `upstream` | `https://github.com/liyangpengs/qq-farm-bot.git` | `master` | 原上游（已停更于 `5049f69`），仅作历史参考 |
| `ccpopy` | `https://github.com/ccpopy/qq-farm-bot-upstream.git` | `master` | 原上游 PR #112–#188 主要贡献者维护；侧重协议版本对齐、双端 TSDK/WASM 更新、完整游戏配置与贴图资源、基础活动接口 |
| `shufflewzc` | `https://github.com/shufflewzc/qq-farm-bot.git` | `master` | 基于旧上游最后版本 `5049f69` 继续维护；侧重「以薯换福」好友比价与自动挂机、回归测试修复、专题文档与个人自用部署脚本 |

## 2026-11-27 组合合并清单

### 合自 `ccpopy/qq-farm-bot-upstream`（基线 `2563ff9` / `d6a3b25`）

- **协议与运行时**：`core/src/config/config.ts`（QQ 与微信版本同步至 `1.17.1.12_20261117`）、`core/src/utils/tsdk-runtime.ts`、`core/src/utils/tsdk.wasm`（`v3.9.0.1795598781`）、`core/src/utils/tsdk-wx.wasm`（`v3.9.0.1795598186`）、`docs/tsdk-runtime.md`、`core/tests/tsdk-runtime.test.js`、`core/tests/wechat-connection.test.js`
- **游戏配置与贴图**：`core/src/activity-data/pending-content.json`、`core/src/gameConfig/ItemInfo.json`、`core/src/gameConfig/Plant.json`、`core/src/gameConfig/Illustrated.json`，以及 `core/src/gameConfig/seed_images_named/seed_images/` 下的 9 张以薯换福相关 PNG 贴图
- **通用能力**：`core/src/services/share.ts`（导出 `checkCanShare` 并改用 `sendMsgAsync` 返回 `ReportShareReply`）、`web/src/components/commerce/PurchaseDialog.vue` 与 `web/src/stores/commerce.ts`（支持自定义动作文案 `verb` 与商品 `description`）
- **未合入部分**：`maple-market`（与 `shufflewzc` 的 `yishuhuanfu` 互斥，二者同属活动组 `2026111707` / 主活动 `2026111708`）

### 合自 `shufflewzc/qq-farm-bot`（基线 `bd76e15`）

- **以薯换福活动与自动化**：`core/src/services/activity-center/yishuhuanfu.ts`、`core/src/services/yishuhuanfu-auto.ts`、`core/src/config/yishuhuanfu-window.ts`、协议与路由接入、前端面板 `YishuHuanfuActivityPanel.vue`、活动静态图与设置表单，以及对应前后端测试（含红薯背包优先种植回归测试）
- **核心测试修复**：`592b8cd` 中对原有 9 个核心测试文件的断言与桩数据修复（`tsdk-runtime.test.js` / `wechat-connection.test.js` 保持 `ccpopy` 最新版，`network-keepalive.test.js` 版本断言对齐至 `1.17.1.12_20261117`）
- **专题文档**：`docs/code-architecture.md`、`docs/ranch-automation-20261116.md`
- **未合入部分**：个人 fork 专属的 `README.md` / `Sidebar.vue` 仓库链接、`docs/fork-sync.md` / `docs/local-changes.md` / `docs/local-change-guide.md`，以及 VPS 部署脚本 `deploy/`、`scripts/deploy/`、`redeploy.bat`（如后续改用自建 VPS 非 GHCR 部署，可按需从 `shufflewzc/master` 提取）

## 后续手动对比与合并命令

```bash
# 拉取两个候选仓库最新提交（不改写当前工作区）
git fetch --no-tags ccpopy
git fetch --no-tags shufflewzc

# 查看相对当前分支的新增提交
git log --oneline HEAD..ccpopy/master
git log --oneline HEAD..shufflewzc/master

# 查看指定提交或指定文件差异后按需 cherry-pick / checkout
git show <commit-sha>
git cherry-pick -x <commit-sha>
```
