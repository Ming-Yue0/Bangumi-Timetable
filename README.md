# 新番时间表 Bangumi-Timetable

用于获取新番更新时间，支持以 Bangumi ID、AniDB ID 或中日名称检索。

## 用法及说明

一般情况下，`titleCN` 来自 Bangumi，`titleJP` 来自 AniDB。

返回的 `begin` 值即为开始连载时间，得到的时间请自行转换。

`latest.json` 为当季的更新时间列表，`data` 目录内包含自 2022 年 4 月起的所有记录。

数据可能存在一些缺失值，表示未查询到该信息。

数据均来源于网络，有错误请提 issue 或 pr 反馈。

## 示例

```json
[
    {
        "titleJP": "サマータイムレンダ",
        "titleCN": "夏日重现",
        "begin": "2022-04-14T15:00:00.000Z",
        "bangumiId": "326895",
        "anidbId": "16033"
    }
]
```