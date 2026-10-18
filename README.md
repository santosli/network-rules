# Network Rules

Quantumult X 远程分流规则。只包含域名、公开服务网段和路由策略，不包含节点、订阅地址、密码或设备信息。

## 引用

在现有配置的 `[filter_remote]` 中添加下列一行，不要重复创建配置段：

```ini
https://raw.githubusercontent.com/santosli/network-rules/main/quantumultx/routing.list, tag=Desktop-Routing, enabled=true
```

资源地址：<https://raw.githubusercontent.com/santosli/network-rules/main/quantumultx/routing.list>

这是一份混合策略资源，必须保留规则原有的 `proxy` / `direct` / `reject`。**不要设置 `force-policy`，也不需要开启资源解析器。**

在现有 `[filter_local]` 末尾保留一条默认规则：

```ini
final, proxy
```

`proxy` 是 QX 内置代理策略，请在手机上选择实际要使用的节点。使用规则分流模式，不是全局直连或全局代理。

## 分流范围

- Claude / Anthropic、OpenAI、Microsoft / Outlook、GitHub、Google、老虎证券等走代理。
- Apple、国内站点，以及 B 站、抖音、小红书的已列出 CDN 走直连。
- 少量广告域名拦截；中国大陆 GeoIP 直连；未命中流量由本地 `final, proxy` 处理。

Microsoft 的 GEOSITE 条目展开为 751 条原生域名规则，并保持原有位置。当前资源共 906 条规则，为 2026-10-19 的静态快照；个人自有域名例外未公开，默认规则留在本地。

## 使用和验证边界

1. 先备份现有 QX 配置，再添加资源；不要替换整份配置或删除原有节点订阅。
2. 现有本地规则、远程资源排序、插入式资源和匹配优化可能影响实际命中。避免更高优先级的宽泛国内直连规则覆盖 Microsoft 等例外；以 QX 的实际匹配记录为准。
3. 添加后检查 Claude / Outlook 命中 `proxy`，Apple / 国内视频 CDN 命中 `direct`。不要为了测试而将 Claude 请求改为直连。
4. 撤销时停用或删除这条远程资源即可；其他配置恢复到备份。

规则格式和代表性域名已离线检查；尚未在真实 iPhone QX 中完成导入或运行验证。QX 与 Clash 的 GeoIP 数据和匹配机制不同，不能保证完全等价。

这是分流资源，**不修改 DNS、不提供断网保护，也不复制 macOS 的防火墙、IPv6 阻断或开机保护**。浏览器自带 DoH 等路径仍需单独检查。无需安装证书或开启 HTTPS 解密。

QX 可以更新 GitHub 上的这份资源，但电脑上的后续修改不会自动同步到仓库；只有仓库内容更新后，手机刷新资源才会获取新规则。

## 来源与许可

- 规则语法及配置方式：[Quantumult X 官方示例](https://github.com/crossutility/Quantumult-X/blob/master/sample.conf)。
- Microsoft 展开项来自本地 GEOSITE 数据快照；相关上游项目包括 [MetaCubeX/meta-rules-dat](https://github.com/MetaCubeX/meta-rules-dat) 和 [v2fly/domain-list-community](https://github.com/v2fly/domain-list-community)。无法仅从本地数据文件确定其具体构建提交。
- 本仓库的规则整理及文档按 GPL-3.0-or-later 提供，详见 `LICENSE`；上游 V2Ray 的 MIT 声明保留于 `LICENSE-MIT-V2RAY`。第三方数据的原始权利和许可不变。
