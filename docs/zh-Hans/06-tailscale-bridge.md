# 主题 6：网络桥接与访问控制（Tailscale + scoped token 代理层）

## 架构

```
[云端 VM 上的 Muse] --Tailscale 内网--> [Mac: 8787 自建转发器] --> [127.0.0.1: Hermes WebUI API]
                                             |
                                     scoped token 代理层（ts8787_relay_v2.py）
                                     launchd 常驻，ACL 零改动
```

## 三层防线

1. **网络层（Tailscale ACL）**：管理台 Policies/JSON editor 写最小规则——仅允许 muse → mac:8787 单向。隔离金丝雀实证：muse→8787＝200 通，其他 port＝超时被挡。
2. **代理层（scoped token）**：
   - token 只存 SHA256；绑定来源 IP；白名单＝方法+路径六端点＋状态查询；白名单外一律 403。
   - body ≤1MB；审计日志（判定＋拒绝原因，查询串脱敏）；双边对账全符。
   - 即时撤销（revoke）；fail-closed；响应永不带 Set-Cookie。
3. **应用层（WebUI 密码）**：主密码保留在 Hermes/皮尔斯先生体系，防 tailnet 其他节点；Muse 永远只接触 token。

## 凭证事件（重要转折，详见主题 5）

原本计划以 RSA 信封交付 WebUI 密码给 Muse——其 VM 安全机制直接拦截解密操作。Muse 事后自评：**「持有能力 ≠ 被授权使用，承诺不能替代最小权限」**。方案改为 scoped token（7 天短效、可撤销、白名单），并立下流程铁律：涉安全架构变更，方案先上墙共识后动工。

## POST 派工管道（实证闭环）

```
① POST /api/session/new  → 取 session_id
② POST /api/chat/start   → 200 含 stream_id/turn_id
③ GET  /api/session?session_id=<ID>  → 应有 user+assistant 消息
```
- 坑：未知 session_id 不会自动物化新会话——**先建后派**。
- 执行需稍候落盘（异步流）；跨 agent 工作节点完成时用 macOS 通知弹给皮尔斯先生。

## 轮换与交接

- 24h 测试 token → 重测＋撤销实测 → 7 天正式 token → 到期轮换。
- 续期批准人＝皮尔斯先生（Hermes 只转达）；token 交付由皮尔斯先生亲手贴在对方对话框（代贴曾被安全机制拦截的教训）。

## 泛化规则

1. 跨机协作的三件套：内网打通（Tailscale）→ 白名单代理（scoped token）→ 审计日志（双边对账）。
2. fail-closed：凭证异常时默认拒绝，宁可不可用，不可裸奔。
3. 每一层都要有金丝雀测试证明「该通的通、该挡的挡」。
