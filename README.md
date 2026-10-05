# openclash_rules

自用 Clash / OpenClash 规则仓库，基于 [ACL4SSR](https://github.com/ACL4SSR/ACL4SSR) 的 `Online_Mini_NoAuto` 精简，叠加两个自定义分组。

## 文件说明

```
Clash/
├── BanOverseaIP_Mainland.list   # 【禁海外IP之内地网站】天眼查/企查查/爱企查/向日葵等，走大陆节点
├── BanHKIP_AITools.list         # 【禁香港IP之AI工具】JetBrains/Antigravity/Google AI API 等，走海外节点
├── GoogleDirect.list            # gemini.google.com / bard.google.com 直连，不代理
└── config/
    └── MyACL4SSR_Mini_NoAuto.ini   # subconverter 配置模板，整合以上规则 + ACL4SSR 基础直连/拦截规则
```

## 策略组

| 分组名 | 候选节点 | 说明 |
|---|---|---|
| 🚀 节点选择 | 全部节点，自由选择 | 主选择分组 |
| 🎯 全球直连 | DIRECT | 国内域名/IP 直连 |
| 🛑 全球拦截 | REJECT | 广告拦截（BanAD + BanProgramAD） |
| 🇨🇳 禁海外IP之内地网站 | 全部节点，自由选择 | 仅限大陆IP访问的网站，手动选一个大陆节点 |
| 🤖 禁香港IP之AI工具 | 全部节点，自由选择 | 对香港出口限制的 AI 工具，手动选一个海外节点 |
| 🐟 漏网之鱼 | 🚀 节点选择 | 兜底规则 |

> 这两个自定义分组不预设任何具体节点名，候选列表包含订阅里的全部节点，需要自己在 OpenClash 面板里手动选一次要用的节点（选完会保留，不会每次重置）。

## 用法

在自建 subconverter 的订阅链接上指定 `config` 参数为本仓库的 raw 链接：

```
http://你的subconverter地址/sub?target=clash&url=你的原始订阅&config=https://raw.githubusercontent.com/kxjhwbb/openclash_rules/main/Clash/config/MyACL4SSR_Mini_NoAuto.ini
```

## 维护

- 日常发现新的"仅限大陆IP访问"网站，加进 `BanOverseaIP_Mainland.list`
- 发现新的"香港IP受限"的 AI/开发工具，加进 `BanHKIP_AITools.list`
- 规则文件为纯域名列表（`DOMAIN-SUFFIX,xxx` / `DOMAIN-KEYWORD,xxx`），一行一条，`#` 开头为注释
