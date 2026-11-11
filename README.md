# sing-box-rules

自维护的 sing-box 规则集（rule-set），供家里路由器以 remote rule_set 方式引用。按去向分三套：

| 名称 | 源文件 | 编译文件 | 用途（建议动作） |
|---|---|---|---|
| rui-ads | `rules/rui-ads.json` | `rule-set/rui-ads.srs` | 广告/追踪域名 → reject（初始为空，待填充） |
| rui-proxy | `rules/rui-proxy.json` | `rule-set/rui-proxy.srs` | 需走代理的域名 → Proxy 出站 |
| rui-direct | `rules/rui-direct.json` | `rule-set/rui-direct.srs` | 强制直连的域名 → direct（初始为空，待填充） |

## 当前内容

- **rui-proxy**：FLAI 全套域名（主站/API/关联站 flaibutter 及图片 CDN，共 11 条 domain_suffix；flaiapp.com 直连存在 SNI 干扰，必须走代理；清单整理自 Caeliger/Rule-Set 的 Surge/flai.list，2026-11-12 收录）+ 币安域名 5 条（binance.com、binance.me、bnbstatic.com、binance.vision、binance.org，2026-11-12 自路由器原内联规则合入）。
- **rui-ads / rui-direct**：空集占位，后续按需往源文件里加。

## 引用方式（路由器 sing-box 配置）

```json
{ "tag": "rui-ads",   "type": "remote", "format": "binary", "url": "https://raw.githubusercontent.com/catbreast/sing-box-rules/main/rule-set/rui-ads.srs",    },
{ "tag": "rui-proxy", "type": "remote", "format": "binary", "url": "https://raw.githubusercontent.com/catbreast/sing-box-rules/main/rule-set/rui-proxy.srs" },
{ "tag": "rui-direct","type": "remote", "format": "binary", "url": "https://raw.githubusercontent.com/catbreast/sing-box-rules/main/rule-set/rui-direct.srs" }
```

route 规则中以 `"rule_set": "<名称>"` 引用：rui-ads 用 `"action": "reject"`，rui-proxy route 到 Proxy，rui-direct route 到 direct。规则顺序：rui-ads → rui-proxy → rui-direct，放在既有规则之前或按需调整。

## 维护

1. 改 `rules/<name>.json`（sing-box rule-set 源格式，version 1）。
2. 用与路由器相同版本的 sing-box 重新编译：`sing-box rule-set compile rules/<name>.json -o rule-set/<name>.srs`（fleet 当前为 1.15.0-alpha.13）。
3. commit + push，路由器按各自的 update_interval 拉取新版。
