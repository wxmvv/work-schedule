# 工作排期台

一个无需后端的单文件工作排期页面，支持：

- 添加、编辑、删除工作记录
- 月、周、年、列表四种时间视图
- 按关键词、状态和负责人筛选
- 浏览器本地自动保存
- 导入、导出 JSON 数据备份
- 导出包含当前数据的只读静态 HTML，下载后可直接分享或离线打开

## 使用

直接双击 `index.html` 即可使用。也可以在项目目录运行：

```bash
python3 -m http.server 4173
```

然后访问 `http://localhost:4173/`。

页面首次打开会提供 3 条当月示例记录。删除或修改后，数据会保存在当前浏览器的 `localStorage` 中。

## JSON 数据导入与导出

编辑页面右上角提供两个数据按钮：

- 「导出数据」会下载一个 `工作排期数据_YYYY-MM-DD.json` 文件，包含当前全部记录。
- 「导入数据」可选择此前导出的 JSON 文件。确认导入后，文件内的记录会替换当前浏览器中的全部记录。

JSON 文件采用以下顶层结构：

```json
{
  "schemaVersion": 1,
  "exportedAt": "2026-08-31T03:30:00.000Z",
  "records": [
    {
      "id": "0bbd8577-5f89-4ea0-9f42-c19a0de02a5d",
      "title": "完成第三季度项目复盘",
      "project": "经营分析",
      "owner": "王小明",
      "startDate": "2026-09-01",
      "endDate": "2026-09-03",
      "startTime": "09:30",
      "endTime": "18:00",
      "status": "doing",
      "priority": "high",
      "place": "线上会议",
      "notes": "整理关键数据并形成结论。",
      "color": "#2563eb",
      "createdAt": "2026-08-30T09:00:00.000Z",
      "updatedAt": "2026-08-31T03:30:00.000Z"
    }
  ]
}
```

顶层字段：

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `schemaVersion` | number | 数据格式版本，当前为 `1` |
| `exportedAt` | string | 导出时间，ISO 8601 格式 |
| `records` | array | 工作记录数组 |

每条工作记录的字段：

| 字段 | 类型 | 必填 | 格式或可选值 |
| --- | --- | --- | --- |
| `id` | string | 否 | 唯一标识；缺失时导入过程会自动生成 |
| `title` | string | 是 | 工作内容 |
| `project` | string | 否 | 项目或分类 |
| `owner` | string | 否 | 负责人 |
| `startDate` | string | 是 | `YYYY-MM-DD` |
| `endDate` | string | 是 | `YYYY-MM-DD`，不能早于开始日期 |
| `startTime` | string | 否 | `HH:mm` |
| `endTime` | string | 否 | `HH:mm` |
| `status` | string | 否 | `todo`、`doing`、`done`、`blocked` |
| `priority` | string | 否 | `normal`、`high`、`urgent` |
| `place` | string | 否 | 地点或线上渠道 |
| `notes` | string | 否 | 补充说明 |
| `color` | string | 否 | 页面支持的十六进制标记色 |
| `createdAt` | string | 否 | ISO 8601 格式的创建时间 |
| `updatedAt` | string | 否 | ISO 8601 格式的最后更新时间 |

导入时也兼容直接以记录数组作为顶层内容的旧格式。缺失的可选字段会使用默认值；缺少 `title`、`startDate` 或 `endDate` 的记录会被忽略。

## 分享

点击页面右上角的「导出静态 HTML」。下载得到的文件已经内嵌当前全部排期数据、样式与交互，可独立打开。

分享版会移除编辑页顶部的产品标题和导航栏，只保留排期内容；它默认只读，但仍可切换视图、时间范围和筛选条件。点击月视图、周视图或列表视图中的具体任务，会打开悬浮详情窗查看日期、时间、负责人、状态、优先级、地点和说明。
