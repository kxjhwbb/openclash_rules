# openclash_rules

**专为香港用户设计**的 Clash / OpenClash 规则仓库，基于 [ACL4SSR](https://github.com/ACL4SSR/ACL4SSR) 的 `Online_Mini_NoAuto` 精简，针对香港本地网络环境优化。

## 设计理念

香港本地网络环境下，默认所有流量直连。只针对以下两类特殊场景提供分流：

1. **禁海外IP访问的内地网站**（如天眼查、企查查等）需要走大陆节点
2. **限制香港IP的海外服务**（如 ChatGPT、Claude、Cursor、JetBrains、Google AI API 等）需要走非香港海外节点
3. **广告拦截** — 使用 ACL4SSR 的广告域名规则

其余流量全部香港本地直连，不走任何代理。

## 文件说明

```
Clash/
├── BanOverseaIP_Mainland.list   # 【禁海外IP之内地网站】天眼查/企查查/爱企查/向日葵等
├── BanHKIP_AITools.list         # 【禁香港IP之AI工具】ChatGPT/Claude/Cursor/JetBrains/Google AI API 等
├── GoogleDirect.list            # gemini.google.com / bard.google.com 直连
└── config/
    └── MyACL4SSR_Mini_NoAuto.ini   # subconverter 配置模板（使用 jsDelivr CDN 加速）
```

## 策略组

| 分组名 | 候选项 | 默认 | 说明 |
|---|---|---|---|
| 🛑 全球拦截 | REJECT + DIRECT | REJECT | 广告拦截 |
| 🇨🇳 禁海外IP之内地网站 | 大陆/回国节点 + DIRECT + REJECT | 首个大陆节点 | 仅限大陆IP访问的网站，**正则自动筛选大陆节点** |
| 🤖 禁香港IP之AI工具 | 海外可用节点 + DIRECT + REJECT | 首个海外节点 | 对香港IP限制的服务，**正则自动排除香港及大陆节点** |

> **提示**：两个自定义分组已将筛选后的有效节点排在最前，默认自动选中首个匹配节点（也可在面板里随时切换其他节点，选完后会自动保存）。
>
> **极简设计**：配置仅保留 5 条规则（广告拦截 + 2 个自定义分流 + GoogleDirect + FINAL），不加载冗余的直连规则集（LocalAreaNetwork、ChinaDomain 等），启动更快、内存占用更少。

## 用法

在自建 subconverter 的订阅链接上指定 `config` 参数为本仓库的配置链接（推荐使用 CDN 加速链接）：

```
http://你的subconverter地址/sub?target=clash&url=你的原始订阅&config=https://cdn.jsdelivr.net/gh/kxjhwbb/openclash_rules@main/Clash/config/MyACL4SSR_Mini_NoAuto.ini
```

或使用 GitHub Raw 链接：

```
http://你的subconverter地址/sub?target=clash&url=你的原始订阅&config=https://raw.githubusercontent.com/kxjhwbb/openclash_rules/main/Clash/config/MyACL4SSR_Mini_NoAuto.ini
```

## 维护

- 日常发现新的"仅限大陆IP访问"网站，加进 `BanOverseaIP_Mainland.list`
- 发现新的"香港IP受限"的 AI/开发工具，加进 `BanHKIP_AITools.list`
- 规则文件为纯域名列表（`DOMAIN-SUFFIX,xxx` / `DOMAIN-KEYWORD,xxx`），一行一条，`#` 开头为注释

## 适用场景

- ✅ 香港本地网络环境
- ✅ 偶尔需要访问限制海外IP的内地网站（天眼查等）
- ✅ 使用对香港IP有限制的海外服务（ChatGPT、Claude、Cursor、JetBrains、某些 AI API）
- ❌ 不适用于内地网络环境（该配置默认全部直连）

