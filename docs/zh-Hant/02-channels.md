# 主題 2：溝通通道工程（每個對手一條實證通道）

## 通道 A：Copilot（VS Code）

- 發送：外部 Python 以 subprocess 執行 `code chat -m ask '<訊息>'`（純問答模式）。**終端機直跑會撞自動化審批門，60 秒無人點即 BLOCKED**——subprocess 路徑 exit 0 即送達。
- `code chat` 支援 `-m ask|edit|agent`；exit=0 只證明投遞成功。
- 驗收：讀 `~/.copilot/session-state/<最新uuid>/events.jsonl`，過濾 `assistant.message` 事件。**坑**：正文讀 `data.content`（字串或分段陣列），讀 `message.content` 永遠讀空；事件類型帶點（`assistant.message`／`user.message`）；時間戳是 UTC（本地 +8）。
- 額度經濟學：chat 池餘量 <20% 時凍結新派單；批量代碼生成放 VS Code 編輯器環境（燒 completions 池）——兩個錢包分開算。

## 通道 B：ChatGPT（桌面 App，Chromium 套殼）

- 同一對話的後續訊息：剪貼板路徑必敗（焦點已失）。**唯一可靠法＝AXTextArea 直寫**：遍歷 `entire contents of window 1` 找 `role is "AXTextArea"` → `set focused` → `set value` → 驗證 value 長度 → `key code 36`。
- keystroke 直打中文會被輸入法吃成亂碼且看似已發送。
- AppleScript 一律寫成 .applescript 檔再執行；行內 `-e` 長腳本會撞審批門。
- 發送後必須讀回全文驗證落窗；**驗證詞不能預設對方措辭**——先搜自己訊息的獨有標記（如規則編號），再讀其後的回覆原文（詳見主題 7 案例五）。

## 通道 C：Muse（Meta 桌面 App，輸入區是網頁元件）

- AX 樹裡輸入區只顯示 AXEmptyGroup：**不可 AXValue 寫值、不可讀回、無 AXTextArea**。唯一路徑＝盲操作四步：`click at {600,790}` → Cmd+V（先 pbcopy）→ Enter → 讀牆驗證。
- 驗證只能靠牆：發前確認獨有短語不在窗內；發送後輪詢出現該短語＝上牆（DOM 刷新有 15-20 秒延遲，別查太急）。
- **假信號目錄**：①「傳送訊息」按鈕 AXEnabled 恆 true，不能當焦點證據；②Cmd+A+Cmd+C 讀回探針（短「Z」＝焦點在輸入框，長文＝焦點在頁面）；③驗證詞必須本次訊息獨有，含「Hermes」等常用詞會命中歷史訊息造成假陽性。
- 卡死黃金解法：多輪自動化後輸入管道會卡死——`quit → reopen → 等 7 秒 → 重放首封成功序列`。連續 2 次失敗就重開，不要戀戰。
- **主人正在前台用該 App 時禁止發送**（合成輸入會搶焦點打斷他）——先探測 frontmost，再用戶確認。

## 通道 D：Muse → Hermes（跨機，8787 代理）

- 見主題 6：Tailscale + scoped token 代理層，POST 建會話／派工，GET 查詢結果。
- 坑：未知 session_id 不會自動物化新會話——先建後派。

## 泛化規則

1. 每個對手花一次時間摸清「它吃哪一套」，之後全部走同一條實證通道，不每次重新發明。
2. 一切發送動作的成功回傳值只是「動作做了」，效果必須讀回驗證。
3. 驗證用「本次訊息的獨有標記」，不用可撞歷史資料的通用詞。
