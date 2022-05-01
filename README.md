# Bangumi-Timetable

# 新番时间表

用于获取新番更新时间，支持以 Bangumi ID、AniDB ID 或名称检索。

## 用法

一般情况下，`titleCN` 来自 Bangumi，`titleJP` 来自 AniDB。

返回的 `begin` 值即为开始连载时间，得到的时间请自行转换。

`latest.json` 为当季的更新时间列表，`data` 目录内包含自 2022 年 4 月起所有的记录。

数据均来源于网络，有错误请提 issue 或 pr 反馈。

## 示例

```json
[
    {
        "titleJP": "パリピ孔明",
        "titleCN": "派对浪客诸葛孔明",
        "begin": "2022-04-05T14:00:00.000Z",
        "bangumiId": "356756",
        "anidbId": "16970"
    },
]
```