# 新番时间表 Bangumi-Timetable

用于获取新番更新时间，支持以 Bangumi ID、AniDB ID 或中日名称检索。

数据均来源于网络，有错误请提 issue 或 pr 反馈。

## 用法及说明

一般情况下，`titleCN` 来自 Bangumi，`titleJP` 来自 AniDB。

返回的 `begin` 值即为开始连载时间，请自行转换 UTC 时间。

`latest.json` 为当季的更新时间列表，`data` 目录内包含自 2022 年 4 月起的所有记录。

数据可能存在一些缺失值，通常以 -1 表示，表示未查询到该信息。

## 示例

```json
[
    {
        "titleJP": "サマータイムレンダ",
        "titleCN": "夏日重现",
        "begin": "2022-04-14T15:00:00.000Z",
        "bangumiId": 326895,
        "anidbId": 16033
    }
]
```

## 代码示例

### jQuery Ajax 请求

下方的代码给出了简单的请求示例。

```js

var timetableData

function getTimeByTitle(title) { // 以中文名称查询（必须完全匹配）
    var temp = timetableData.filter(function (x) { return x.titleCN == title }) // 查找
    if (!temp.length) return false
    if (temp[0].begin) return temp[0].begin
    return false
}

function getTimeByAnidbId(id) { // 以 AniDB ID 查询
    var temp = timetableData.filter(function (x) { return x.anidbId == id })
    if (!temp.length) return false
    if (temp[0].begin) return temp[0].begin
    return false
}

function getTimeByBangumiId(id) { // 以 Bangumi ID 查询
    var temp = timetableData.filter(function (x) { return x.bangumiId == id })
    if (!temp.length) return false
    if (temp[0].begin) return temp[0].begin
    return false
}

$.ajax({
    method: "get",
    async: false,
    url: "https://cdn.jsdelivr.net/gh/ming-yue0/Bangumi-Timetable/data/202204.json", // 注意：jsDelivr 在部分区域可能无法访问！
    success: (data) => { timetableData = data }
})

if (getTimeByTitle("夏日重现")) // 以中文名称查询（必须完全匹配）
    console.log(new Date(getTimeByTitle("夏日重现")).toLocaleTimeString().substring(0,5)) // 转换时间
else console.log("暂无时间")

if (getTimeByAnidbId(16033))  // 以 AniDB ID 查询
    console.log(new Date(getTimeByAnidbId(16033)).toLocaleTimeString().substring(0,5))
else console.log("暂无时间")

if (getTimeByBangumiId(326895))  // 以 Bangumi ID 查询
    console.log(new Date(getTimeByBangumiId(326895)).toLocaleTimeString().substring(0,5))
else console.log("暂无时间")

```

### Python

```python
import requests


def filter_by(info, titleCN, anidbId, bangumiId):  # 此段代码来源于网络
    params = locals()
    params.pop('info')
    for key in params:
        if params[key] is None:
            continue
        if info.get(key) != params[key]:
            return False
    return True


def getTime(titleCN=None, anidbId=None, bangumiId=None):
    import dateutil.parser
    import pytz
    from datetime import datetime

    temp = list(filter(lambda x: filter_by(x, bangumiId=bangumiId,
                anidbId=anidbId, titleCN=titleCN), timetableData))
    if(len(temp) == 0):
        return False
    timeRaw = temp[0]['begin']
    local_time = dateutil.parser.parse(timeRaw).astimezone(
        pytz.timezone('Asia/Shanghai'))   # 转换为北京时间
    time = datetime.strftime(local_time, '%H:%M')
    return time


url = "https://cdn.jsdelivr.net/gh/ming-yue0/Bangumi-Timetable/data/202204.json"
timetableData = requests.get(url).json()

print(getTime(titleCN="夏日重现"))
print(getTime(anidbId=16033))
print(getTime(bangumiId=326895))

```