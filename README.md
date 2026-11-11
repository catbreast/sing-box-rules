# sing-box-rules

自维护的 sing-box 规则集（rule-set），供家里路由器以 remote rule_set 方式引用。

## 规则集列表

| 名称 | 源文件 | 编译文件 | 说明 |
|---|---|---|---|
| flai | `rules/flai.json` | `rule-set/flai.srs` | FLAI（www.flaiapp.com）全套域名：主站、API、关联站 flaibutter 及图片 CDN。该域名直连存在 SNI 干扰，需走代理。域名清单整理自 Caeliger/Rule-Set 的 Surge/flai.list（2026-11-12 收录）。 |

## 引用方式（路由器 sing-box 配置）

```json
{
  "tag": "flai",
  "type": "remote",
  "format": "binary",
  "url": "https://raw.githubusercontent.com/catbreast/sing-box-rules/main/rule-set/flai.srs",
  "download_detour": "anyreality-us-bwg"
}
```

route 规则中以 `"rule_set": "flai"` 引用，action route 到目标出站。

## 维护

1. 改 `rules/<name>.json`（sing-box rule-set 源格式，version 1）。
2. 用与路由器相同版本的 sing-box 重新编译：`sing-box rule-set compile rules/<name>.json -o rule-set/<name>.srs`（fleet 当前为 1.15.0-alpha.13）。
3. commit + push，路由器按各自的 update_interval 拉取新版。
