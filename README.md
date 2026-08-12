# Tengri Qaghan · OpenData

Tengri Qaghan 桌面应用的**公开静态数据仓库**。应用启动时从这里拉取远程公告等数据，无需自建服务器。

## 文件

| 文件 | 用途 |
|---|---|
| `announcement.json` | 远程公告清单，应用启动时读取并弹窗（紧急 / 新闻 / 活动） |

## announcement.json 格式

```json
{
  "version": 1,
  "announcements": [
    {
      "id": "唯一标识，用于「24 小时不再提示」去重",
      "kind": "urgent | news | event",
      "title": "公告标题",
      "body": "纯文本正文，\\n 换行，http(s):// 链接自动可点击",
      "publishedAt": "2026-08-13T10:00:00Z",
      "actionLabel": "可选：操作按钮文案",
      "actionUrl": "可选：操作按钮跳转链接"
    }
  ]
}
```

### 字段说明

- **kind**
  - `urgent` 紧急通知（红色，强制触达，不提供「24 小时不再提示」）
  - `news` 新闻（蓝色）
  - `event` 活动（黄色）
- **id**：每条公告必须唯一，修改公告后若想让用户重新看到，请更换 id。
- **body**：纯文本，不支持 HTML（防注入）；`\n` 换行；正文中的网址会自动转成可点击链接。
- **publishedAt**：ISO 8601 时间，应用按时间倒序展示。
- 多条公告会依次弹出；普通公告关闭后 24 小时内不再提示。

## 应用如何读取

应用通过 jsDelivr CDN 读取：

```
https://cdn.jsdelivr.net/gh/65tiankehan/tengriqaghan-Opendata@main/announcement.json
```

推送到 `main` 分支后，CDN 可能有几分钟缓存；如需立即刷新，可访问
`https://cdn.jsdelivr.net/gh/65tiankehan/tengriqaghan-Opendata@main/announcement.json`
查看，或 purge 缓存。

## 发布公告流程

1. 编辑 `announcement.json`，新增一条公告（建议新 `id`）。
2. 提交并推送到 `main`。
3. 等待数分钟 CDN 生效，用户下次启动应用即可看到。
