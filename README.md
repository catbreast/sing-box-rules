# sing-box-rules

自维护的 sing-box 规则集（rule-set），供家里路由器以 remote rule_set 方式引用。按去向分三套：

| 名称 | 源文件 | 编译文件 | 用途（建议动作） |
|---|---|---|---|
| ads | `rules/ads.json` | `rule-set/ads.srs` | 广告/追踪域名 → reject（初始为空，待填充） |
| proxy | `rules/proxy.json` | `rule-set/proxy.srs` | 需走代理的域名 → Proxy 出站 |
| direct | `rules/direct.json` | `rule-set/direct.srs` | 强制直连的域名 → direct（初始为空，待填充） |

## 当前内容

- **proxy**：FLAI 全套域名（主站/API/关联站 flaibutter 及图片 CDN，共 11 条 domain_suffix）。flaiapp.com 直连存在 SNI 干扰，必须走代理。清单整理自 Caeliger/Rule-Set 的 Surge/flai.list（2026-11-12 收录）。
- **ads / direct**：空集占位，后续按需往源文件里加。

## 引用方式（路由器 sing-box 配置）

```json
{ "tag": "ads",   "type": "remote", "format": "binary", "url": "https://raw.githubusercontent.com/catbreast/sing-box-rules/main/rule-set/ads.srs",   "download_detour": "anyreality-us-bwg" },
{ "tag": "proxy", "type": "remote", "format": "binary", "url": "https://raw.githubusercontent.com/catbreast/sing-box-rules/main/rule-set/proxy.srs", "download_detour": "anyreality-us-bwg" },
{ "tag": "direct","type": "remote", "format": "binary", "url": "https://raw.githubusercontent.com/catbreast/sing-box-rules/main/rule-set/direct.srs","download_detour": "direct" }
```

route 规则中以 `"rule_set": "<名称>"` 引用：ads 用 `"action": "reject"`，proxy route 到 Proxy，direct route 到 direct。规则顺序：ads → proxy → direct，放在既有规则之前或按需调整。

## 维护

1. 改 `rules/<name>.json`（sing-box rule-set 源格式，version 1）。
2. 用与路由器相同版本的 sing-box 重新编译：`sing-box rule-set compile rules/<name>.json -o rule-set/<name>.srs`（fleet 当前为 1.15.0-alpha.13）。
3. commit + push，路由器按各自的 update_interval 拉取新版。
