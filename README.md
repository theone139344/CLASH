# CLASH — 自用规则真源（WindShadow777）

单一信源仓库。OpenClash / 订阅转换只**拉取**，不在路由上再抄一份同名名单。

## 文件分工

| 文件 | 用途 | 现网消费方 |
|------|------|------------|
| `theone-china.list` | 自用**国内/直连**域名与进程 | YAML：`TheOneChina / Domain` → 组「直连」；ini：`国内自用` |
| `theone-Proxy.list` | 自用**国外/代理**域名 | YAML：`Other AI / Domain` 等；ini：`国外自用` |
| `theone-ClashAll.ini` | subconverter（订阅转换）整包模板 | 本地 sub `10.39.3.1:25500` 等 |
| `CloudFlare.list` | 其它地址集 | 按需挂 ruleset |

## 维护约定（Hermes 特许可直接执行）

授权范围（用户 2026-07-22）：上述自用 list / ini / 后续同仓地址集，**可直接改、commit、经可写通道推送**，无需每次口头审批。

| 可直接做 | 仍须人类 / 另闸 |
|----------|----------------|
| 增删改 list 条目、整理注释、修明显笔误 | 软路由 `10.39.3.1` 上改运行中 YAML / 重启 OpenClash（网络写守卫） |
| 调整 ini 中自用 ruleset **顺序**（自用优先于大盘） | 改 MATCH/出口组、HUMAN-NET、密钥 |
| fork→PR 或上游 push（有权限时） | 公网 DNS 把域名指到内网 IP |

### 写法

- 整棵自有域：一条 `DOMAIN-SUFFIX,example.com`，**不必**枚举子域  
- 国内 vs 国外：**同一域名只出现在一侧**  
- 拉取 URL：ini 已用 `github.moeyy.xyz`；手写 YAML 的 provider 建议同样套加速前缀（与仓库内其它规则一致）

### 工作副本

本机：`D:\ClaudeProjects\ops\CLASH`（clone；推送视 GitHub 权限走 upstream 或 fork+PR）

### 路由手写 YAML 对齐（人类一次）

`/etc/openclash/config/自用clash-fallback-smart-std.yaml` 中自用 provider 建议：

```yaml
TheOneChina / Domain: {<<: *class, url: "https://gh-proxy.com/raw.githubusercontent.com/WindShadow777/CLASH/refs/heads/main/theone-china.list"}
Other AI / Domain: {<<: *class, url: "https://gh-proxy.com/raw.githubusercontent.com/WindShadow777/CLASH/refs/heads/main/theone-Proxy.list"}
```

规则序保持：`RULE-SET,TheOneChina / Domain,直连` 在大盘 Proxy/China **之前**（与 ini「自用优先」同构）。
