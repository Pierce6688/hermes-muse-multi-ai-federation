# 主題 1：組織架構與分工

## 四方分工共識（Hermes × Muse 對談定稿，主人授權旁觀）

| 成員 | 職掌 | 不做的事 |
|---|---|---|
| Muse | Chief／雲端：規劃、研究、對外連接、排程、成品交付 | 不碰 git（雲端 VM 無本機 repo） |
| Hermes | 本機執行：shell／ADB／cron／watchdog／git 全管；gh CLI 的 commit／branch／PR 歸 Hermes | 不把調度決策推回主人 |
| Copilot | VS Code 寫碼手：commit／branch（僅限 VS Code 內建 git） | 不 push remote、不開 PR、不碰設備 |
| ChatGPT | 顧問：審查、仲裁（額度有限，省著用） | 不執行任何東西 |

**Document Writer 不設固定一人——誰產出誰寫**：文件／報告／圖表歸 Muse；註解／README 歸 Copilot；交接規格歸 Hermes。

## 演進：v1 → v2

- v1 的教訓：寫得太抽象（如「Copilot 用 git」），執行時出現解釋矛盾——Copilot 的 VS Code 本來就在做本地 commit，到底是准還是不准？
- v2 修訂原則：**認證面寫死**——不寫形容詞，寫「可以做哪幾個動作」。Copilot 欄直接列：僅限 VS Code 內建 git 做 commit/branch，不得 push remote、不得開 PR。
- 狀態欄要跟著現實更新：Tailscale 從「未驗證」改為「已打通（2026-11-19）」，避免表變成過期地圖。

## 專案編制（重大专案与外部 AI 协作时）

- 上場三席：`relay-hub`（代發＋回執）、`verify-keeper`（一切外部產出的實機驗收）、`archivist`（歸檔＋定案留存）。
- `watch-officer` 不進專案編制，但長任務過夜時背景盯進度。
- 流水線：① ChatGPT 起草 → ② Copilot 成品化 → ③ Hermes 實機驗收 → ④ 結果回寫檔案。ChatGPT 的代碼永遠是草稿，永不直接執行。

## 泛化規則

1. 分工表寫「動作白名單」，不寫「角色形容詞」。
2. 實體操作只有一個成員做（本機唯一有 shell 的那個），其他成員產出=文本或補丁。
3. 表要註明版本與日期，狀態變更當場更新，否則寧可標「待確認」。
