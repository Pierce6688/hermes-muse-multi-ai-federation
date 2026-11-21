# 主题 1：组织架构与分工

## 四方分工共识（Hermes × Muse 对谈定稿，皮尔斯先生授权旁观）

| 成员 | 职掌 | 不做的事 |
|---|---|---|
| Muse | Chief／云端：规划、研究、对外连接、排程、成品交付 | 不碰 git（云端 VM 无本机 repo） |
| Hermes | 本机执行：shell／ADB／cron／watchdog／git 全管；gh CLI 的 commit／branch／PR 归 Hermes | 不把调度决策推回皮尔斯先生 |
| Copilot | VS Code 写码手：commit／branch（仅限 VS Code 内建 git） | 不 push remote、不开 PR、不碰设备 |
| ChatGPT | 顾问：审查、仲裁（额度有限，省着用） | 不执行任何东西 |

**Document Writer 不设固定一人——谁产出谁写**：文件／报告／图表归 Muse；注释／README 归 Copilot；交接规格归 Hermes。

## 演进：v1 → v2

- v1 的教训：写得太抽象（如「Copilot 用 git」），执行时出现解释矛盾——Copilot 的 VS Code 本来就在做本地 commit，到底是准还是不准？
- v2 修订原则：**认证面写死**——不写形容词，写「可以做哪几个动作」。Copilot 栏直接列：仅限 VS Code 内建 git 做 commit/branch，不得 push remote、不得开 PR。
- 状态栏要跟着现实更新：Tailscale 从「未验证」改为「已打通（2026-11-19）」，避免表变成过期地图。

## 专案编制（重大专案与外部 AI 协作时）

- 上场三席：`relay-hub`（代发＋回执）、`verify-keeper`（一切外部产出的实机验收）、`archivist`（归档＋定案留存）。
- `watch-officer` 不进专案编制，但长任务过夜时背景盯进度。
- 流水线：① ChatGPT 起草 → ② Copilot 成品化 → ③ Hermes 实机验收 → ④ 结果回写文件。ChatGPT 的代码永远是草稿，永不直接执行。

## 泛化规则

1. 分工表写「动作白名单」，不写「角色形容词」。
2. 实体操作只有一个成员做（本机唯一有 shell 的那个），其他成员产出=文本或补丁。
3. 表要注明版本与日期，状态变更当场更新，否则宁可标「待确认」。
