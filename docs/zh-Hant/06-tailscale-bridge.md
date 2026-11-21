# 主題 6：網路橋接與存取控制（Tailscale + scoped token 代理層）

## 架構

```
[雲端 VM 上的 Muse] --Tailscale 內網--> [Mac: 8787 自建轉發器] --> [127.0.0.1: Hermes WebUI API]
                                             |
                                     scoped token 代理層（ts8787_relay_v2.py）
                                     launchd 常駐，ACL 零改動
```

## 三層防線

1. **網路層（Tailscale ACL）**：管理台 Policies/JSON editor 寫最小規則——僅允許 muse → mac:8787 單向。隔離金絲雀實證：muse→8787＝200 通，其他 port＝超時被擋。
2. **代理層（scoped token）**：
   - token 只存 SHA256；綁定來源 IP；白名單＝方法+路徑六端點＋狀態查詢；白名單外一律 403。
   - body ≤1MB；審計日誌（判定＋拒絕原因，查詢串脫敏）；雙邊對賬全符。
   - 即時撤銷（revoke）；fail-closed；響應永不帶 Set-Cookie。
3. **應用層（WebUI 密碼）**：主密碼保留在 Hermes/主人體系，防 tailnet 其他節點；Muse 永遠只接觸 token。

## 憑證事件（重要轉折，詳見主題 5）

原本計畫以 RSA 信封交付 WebUI 密碼給 Muse——其 VM 安全機制直接攔截解密操作。Muse 事後自評：**「持有能力 ≠ 被授權使用，承諾不能替代最小權限」**。方案改為 scoped token（7 天短效、可撤銷、白名單），並立下流程鐵律：涉安全架構變更，方案先上牆共識後動工。

## POST 派工管道（實證閉環）

```
① POST /api/session/new  → 取 session_id
② POST /api/chat/start   → 200 含 stream_id/turn_id
③ GET  /api/session?session_id=<ID>  → 應有 user+assistant 訊息
```
- 坑：未知 session_id 不會自動物化新會話——**先建後派**。
- 執行需稍候落盤（異步流）；跨 agent 工作節點完成時用 macOS 通知彈給主人。

## 輪換與交接

- 24h 測試 token → 重測＋撤銷實測 → 7 天正式 token → 到期輪換。
- 續期批准人＝主人（Hermes 只轉達）；token 交付由主人親手貼在對方對話框（代貼曾被安全機制攔截的教訓）。

## 泛化規則

1. 跨機協作的三件套：內網打通（Tailscale）→ 白名單代理（scoped token）→ 審計日誌（雙邊對賬）。
2. fail-closed：憑證異常時預設拒絕，寧可不可用，不可裸奔。
3. 每一層都要有金絲雀測試證明「該通的通、該擋的擋」。
