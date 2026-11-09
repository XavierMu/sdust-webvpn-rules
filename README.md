# SDUST WebVPN 公共分流规则

给 [SDUST WebVPN 桌面端](https://github.com/XavierMu/sdust-webvpn-desktop) 用的公共规则文件。
桌面端默认已经填好这里的地址，安装后不需要手动配置。

## 怎么用

桌面端「分流规则 → 在线规则…」里默认填的就是下面这三条（前面的拉不动会自动换下一条）：

```
https://fastly.jsdelivr.net/gh/XavierMu/sdust-webvpn-rules@main/rules.json
https://cdn.jsdelivr.net/gh/XavierMu/sdust-webvpn-rules@main/rules.json
https://raw.githubusercontent.com/XavierMu/sdust-webvpn-rules/main/rules.json
```

也可以直接订阅 `rules.json` 的原始地址：

```
https://raw.githubusercontent.com/XavierMu/sdust-webvpn-rules/main/rules.json
```

## 文件格式

`rules.json`：

```json
{
  "name": "SDUST 校内站点公共规则",
  "updated": "2026-11-09",
  "version": 1,
  "proxy": ["sdust.edu.cn", "10.0.0.0/8"],
  "direct": ["example.com"]
}
```

- `proxy`：命中就走学校 WebVPN（相当于白名单）
- `direct`：命中就直连（相当于黑名单，**优先级高于 `proxy`**）
- `updated`：`YYYY-MM-DD`。桌面端拿它判断新旧，**远端比本地缓存旧时会拒绝覆盖**，所以改完记得改这个日期
- `version`：可选。写了就以它为准，比日期更保险，推荐每次改动 +1

条目写法：

| 写法 | 含义 |
| --- | --- |
| `sdust.edu.cn` | 本级域名 + 所有子域 |
| `*.cnki.net` | 同上（`*.` 和前面的点只是写法糖） |
| `10.0.0.0/8` | IPv4 网段 |
| `172.18.129.250` | 单个 IPv4 |
| `*` | 全部（一般不要用） |

除了 JSON，桌面端也认纯文本格式（一行一条，`#` 开头是注释，`[proxy]` / `[direct]` 分段），
示例见 [rules.example.txt](rules.example.txt)。

## 更新后多久生效

桌面端默认每 24 小时自动同步一次，也可以在界面里点「立即同步」。

注意 jsDelivr 会缓存 `@main` 分支的文件（大约 12 小时），所以走 CDN 的那两条地址可能拿到旧版本。
桌面端对这种「远端比本地旧」的情况会直接跳过并换下一个地址，不会把规则回滚。
需要立刻生效的话：

1. 在 <https://www.jsdelivr.com/tools/purge> 提交这几条 URL 清缓存，或
2. 直接用 `raw.githubusercontent.com` 那条地址。

## 免责

这些规则是社区整理的校内/校内可访问站点清单，**不保证**随时与学校实际部署一致。
用不用、算不算数，以学校的规定为准。
